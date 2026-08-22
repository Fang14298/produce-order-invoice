---
name: sheets-api-helper
description: Use when you need to read or query order data from the "produce-order" Google Sheet via GViz API, or when troubleshooting data parsing from the spreadsheet.
---

# Google Sheets API Helper (GViz)

## Overview
This skill provides the exact schema, constraints, and query patterns for the "produce-order" Google Sheet, which acts as the read-only database for this project via the Google Visualization API (`gviz/tq`).

## When to Use
- When writing features that fetch data from Google Sheets
- When debugging missing or incorrectly mapped columns
- When modifying the `parseItems()` or `parseGvizDate()` functions
- When migrating the Sheet ID or Tab Name

## Core References

### Connection Details
- **Sheet ID**: `1ihPCg3VKhS59B5RrlKuvFZi9IsZesESkXmrb02cS-84`
- **Tab/Sheet Name**: `รายการออเดอร์`
- **Access Rule**: The sheet MUST be shared as "Anyone with the link can view". No OAuth or API Key is required.

### Column Mappings (Schema)
When fetching via GViz, the first row acts as headers. The code (`index.html`) searches for specific keywords to map column indexes dynamically:
- `เลขที่ออเดอร์` -> `orderId`
- `วัน-เวลาสั่งซื้อ` หรือ `วัน-เวลา` -> `orderDate` (Requires parsing GViz `Date(...)`)
- `ชื่อร้าน` หรือ `ลูกค้า` -> `shop`
- `เบอร์โทรศัพท์` หรือ `เบอร์โทร` -> `phone`
- `ที่อยู่จัดส่ง` -> `address`
- `เลขผู้เสียภาษี` -> `taxId`
- `วันที่รับของ` -> `deliveryDate` (Requires parsing GViz `Date(...)`)
- `รายการสินค้าที่สั่ง` -> `itemsRaw` (Multi-line text)
- `ยอดรวม` -> `total` (Number)
- `หมายเหตุ` -> `note`
- `สถานะ` -> `status` (Default: "รอยืนยัน" if blank)
- `LINE User ID` -> `lineUserId`

## Common Mistakes & Gotchas
- **GViz Date Format**: Google Visualization returns dates as `Date(YYYY,M,D)` or `Date(YYYY,M,D,H,Mi,S)`. **WARNING: Months are 0-indexed.** You must add 1 to the month. Always use the built-in `parseGvizDate(raw)` function.
- **Items Parsing (Regex)**: The column `รายการสินค้าที่สั่ง` is stored as a single multi-line string. Each line format is `• <name> <qty> <unit> = <subtotal> บาท`. Any changes to this string pattern in Google Apps Script (in the main `produce-order` repo) will break the regex in `parseItems()`.
- **JSON Extraction**: The GViz endpoint returns text wrapped in a function call `/*O_o*/ google.visualization.Query.setResponse({...});`. You must use substring extraction to parse the valid JSON object.

## Implementation Example
Fetching and extracting JSON from GViz safely:

```javascript
const url = `https://docs.google.com/spreadsheets/d/${SHEET_ID}/gviz/tq?tqx=out:json&sheet=${encodeURIComponent(ORDER_SHEET_NAME)}&headers=1`;
const text = await (await fetch(url)).text();
// Extract JSON payload safely from the padded response
const json = JSON.parse(text.substring(text.indexOf("{"), text.lastIndexOf("}") + 1));

// Data rows are in json.table.rows
// Column metadata are in json.table.cols
```
