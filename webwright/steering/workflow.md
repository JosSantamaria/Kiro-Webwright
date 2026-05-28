# Webwright Workflow

## Complete Task Execution Flow

### Phase 1: Plan

Parse the user's task into a numbered checklist of **critical points** (CPs). Each CP is an independently verifiable constraint.

Write to `WORKSPACE_DIR/plan.md`:

```markdown
# Critical Points
- [ ] CP1: Navigate to Google Flights
- [ ] CP2: Set departure SEA, arrival JFK
- [ ] CP3: Set dates Aug 15-20, 2026
- [ ] CP4: Filter for nonstop flights only
- [ ] CP5: Sort by price (cheapest first)
- [ ] CP6: Extract top 3 results with airline, price, times
```

**Rules for CPs:**
- Every explicit constraint, filter, sort, selection, or required datum gets its own CP
- Each CP must be verifiable from a screenshot OR a log line
- Ranking language (cheapest, best-selling, most reviewed) must be grounded in the site's actual sort/filter

### Phase 2: Explore

Run scratch Playwright scripts to discover:
- Stable selectors for key UI elements
- Whether filter controls exist and how they work
- Page structure and navigation flow

```bash
python3 -c "
import asyncio
from playwright.async_api import async_playwright

async def explore():
    async with async_playwright() as p:
        browser = await p.firefox.launch(headless=True)
        page = await browser.new_page(viewport={'width': 1280, 'height': 1800})
        await page.goto('https://www.google.com/flights')
        await page.wait_for_load_state('networkidle')
        await page.screenshot(path='outputs/webwright/explore_01.png')
        # Print page title and visible text
        print(await page.title())
        await browser.close()

asyncio.run(explore())
"
```

Read saved PNGs to inspect UI state. Print ARIA snapshots, URLs, titles, and visible labels.

### Phase 3: Author final_script.py

Create a fresh run folder and write the instrumented script:

```
final_runs/run_1/
├── final_script.py
├── final_script_log.txt
└── screenshots/
    ├── final_execution_01_navigate.png
    ├── final_execution_02_set_filters.png
    └── final_execution_03_results.png
```

The script MUST:
1. Reset the log file at start
2. Write a `step N action: <reason>` line for every constraint-relevant interaction
3. Save a uniquely-named screenshot for every critical point
4. Print the final datum at the end of the log

### Phase 4: Execute

Run the final script once. Capture stdout/stderr:

```bash
cd outputs/webwright/final_runs/run_1
python3 final_script.py 2>&1 | tee execution_output.txt
```

### Phase 5: Self-Verify

Walk `plan.md` and for each CP:

1. Identify a screenshot path AND/OR a log line that proves it
2. Read each cited PNG and confirm evidence is unambiguous
3. Tick the CP only when evidence is concrete

**Be harsh with:**
- Ambiguous states (filter chip not visible)
- Partially-applied states (wrong date range)
- Occluded states (selection hidden after drawer closed)
- Missing confirmation screenshots

If any CP fails:
1. Diagnose the specific issue
2. Fix `final_script.py`
3. Re-run inside `final_runs/run_2/`
4. Re-verify

### Phase 6: Done

Only when every CP in `plan.md` is checked off with cited evidence.

Update `plan.md`:
```markdown
# Critical Points
- [x] CP1: Navigate to Google Flights — screenshots/final_execution_01_navigate.png
- [x] CP2: Set departure SEA, arrival JFK — log step 2-3
- [x] CP3: Set dates Aug 15-20, 2026 — screenshots/final_execution_02_set_filters.png
- [x] CP4: Filter nonstop — screenshots/final_execution_03_filter.png (chip visible)
- [x] CP5: Sort by price — screenshots/final_execution_04_sorted.png
- [x] CP6: Extract top 3 — final_script_log.txt RESULT line
```

Report the final datum to the user.

## Completion Checklist

Before declaring done:
- [ ] `plan.md` exists with all CPs checked
- [ ] `final_script.py` runs cleanly from scratch (no leftover state)
- [ ] Every CP has cited evidence (screenshot path or log line)
- [ ] Final datum is stated explicitly
- [ ] No unverified assumptions about UI state
