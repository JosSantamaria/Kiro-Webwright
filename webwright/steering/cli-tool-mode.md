# CLI Tool Mode

## When to Use

Activate CLI tool mode when the user asks to:
- "Parameterize" a web task
- "Make it reusable"
- "Turn this into a CLI"
- "Create a tool for..."
- Run the same task with different inputs later

## Contract

The `final_script.py` in CLI tool mode must:

1. **One function** with a Google-style `Args:` docstring
2. **argparse wrapper** whose flags default to the concrete task values
3. **Import-safe** — can be imported as a module without side effects
4. **step 0 params:** log line showing the actual parameters used

## Template

```python
#!/usr/bin/env python3
"""Search flights between two cities on Google Flights.

Usage:
    python3 final_script.py --from SEA --to JFK --depart 2026-08-15 --return 2026-08-20
    python3 final_script.py  # Uses defaults from original task
"""
import argparse
import asyncio
import os
from playwright.async_api import async_playwright

# Parameters table
# | Flag       | Default      | Description                |
# |------------|--------------|----------------------------|
# | --from     | SEA          | Departure airport code     |
# | --to       | JFK          | Arrival airport code       |
# | --depart   | 2026-08-15   | Departure date (YYYY-MM-DD)|
# | --return   | 2026-08-20   | Return date (YYYY-MM-DD)   |
# | --nonstop  | True         | Filter nonstop only        |


async def search_flights(
    departure: str = "SEA",
    arrival: str = "JFK",
    depart_date: str = "2026-08-15",
    return_date: str = "2026-08-20",
    nonstop: bool = True,
) -> dict:
    """Search flights on Google Flights.

    Args:
        departure: IATA airport code for departure city.
        arrival: IATA airport code for arrival city.
        depart_date: Departure date in YYYY-MM-DD format.
        return_date: Return date in YYYY-MM-DD format.
        nonstop: If True, filter for nonstop flights only.

    Returns:
        Dict with 'flights' list and 'cheapest' entry.
    """
    # Setup
    run_dir = os.path.dirname(os.path.abspath(__file__))
    screenshots_dir = os.path.join(run_dir, "screenshots")
    os.makedirs(screenshots_dir, exist_ok=True)
    log_path = os.path.join(run_dir, "final_script_log.txt")

    with open(log_path, "w") as log:
        log.write(f"step 0 params: from={departure} to={arrival} depart={depart_date} return={return_date} nonstop={nonstop}\n")

        async with async_playwright() as p:
            browser = await p.firefox.launch(headless=True)
            page = await browser.new_page(viewport={"width": 1280, "height": 1800})

            # ... task implementation ...

            await browser.close()

    return {"flights": [], "cheapest": None}


def main():
    parser = argparse.ArgumentParser(description="Search flights on Google Flights")
    parser.add_argument("--from", dest="departure", default="SEA")
    parser.add_argument("--to", dest="arrival", default="JFK")
    parser.add_argument("--depart", default="2026-08-15")
    parser.add_argument("--return", dest="return_date", default="2026-08-20")
    parser.add_argument("--nonstop", action="store_true", default=True)
    args = parser.parse_args()

    result = asyncio.run(search_flights(
        departure=args.departure,
        arrival=args.arrival,
        depart_date=args.depart,
        return_date=args.return_date,
        nonstop=args.nonstop,
    ))
    print(result)


if __name__ == "__main__":
    main()
```

## Completion Gate (CLI tool mode)

Before declaring done, verify:
- [ ] Script runs with default args (original task values)
- [ ] Script runs with different args (test with alternate values)
- [ ] `step 0 params:` line appears in log
- [ ] Function can be imported without side effects: `from final_script import search_flights`
- [ ] All CPs from `plan.md` still pass with default args
- [ ] argparse `--help` shows all parameters with descriptions

## Reuse

Once created, the CLI tool can be:
- Shared with team members
- Scheduled via cron
- Called from other scripts
- Parameterized for different inputs without re-discovering selectors
