# Rules for Produce Order Project

## Documentation Maintenance Rule
Every time code or functionality is created, modified, refactored, or fixed in this project:
1. **Always Update Documentation**: The project documentation (`README.md`) MUST be updated immediately to reflect the latest code structure and features.
2. **Detail Level**: Documentation updates must include high technical fidelity (system flow, API payloads, sheet structures, edge cases, and configuration constants).
3. **Language**: Keep documentation clear and professional in Thai (or bilingual where appropriate for code symbols).

## Git Push Rule
Every time a task (code change, bug fix, refactor, or documentation update) in this project is completed:
1. **Always Commit**: Stage and commit all resulting changes with a clear, descriptive commit message (Thai or bilingual is fine) as the final step of the task — do not leave finished work only as uncommitted local changes.
2. **Always Push to GitHub**: Immediately push the commit to the `origin` remote on GitHub (`git push`) right after committing, so `main` on GitHub always reflects the latest finished work.
3. **No Need to Ask First**: Committing and pushing is part of "finishing" the task — do this automatically without waiting for separate confirmation each time, unless the user explicitly says not to push yet (e.g. work is still in progress or needs review first).
