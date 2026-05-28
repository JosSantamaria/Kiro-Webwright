# Playwright Patterns for Webwright

## Browser Launch Skeleton

```python
import asyncio
from playwright.async_api import async_playwright

async def main():
    async with async_playwright() as p:
        browser = await p.firefox.launch(headless=True)
        context = await browser.new_context(
            viewport={"width": 1280, "height": 1800},
            user_agent="Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36"
        )
        page = await context.new_page()

        await page.goto("https://example.com")
        await page.wait_for_load_state("networkidle")

        # Take screenshot
        await page.screenshot(path="screenshots/step_01_loaded.png")

        await browser.close()

asyncio.run(main())
```

## ARIA Snapshot (accessibility tree inspection)

```python
# Get the accessibility tree for selector discovery
snapshot = await page.accessibility.snapshot()
print(snapshot)

# Or get specific element's accessible name
element = await page.query_selector('[role="button"]')
name = await element.get_attribute("aria-label")
```

## Screenshot Naming Convention

```
screenshots/
├── explore_01_homepage.png
├── explore_02_filter_panel.png
├── final_execution_01_navigate.png
├── final_execution_02_apply_filter.png
├── final_execution_03_select_result.png
└── final_execution_04_extract_data.png
```

## Common Patterns

### Wait for element
```python
await page.wait_for_selector('[data-testid="results"]', timeout=10000)
```

### Click and wait for navigation
```python
async with page.expect_navigation():
    await page.click('a[href="/results"]')
```

### Fill form
```python
await page.fill('input[name="search"]', "query text")
await page.press('input[name="search"]', "Enter")
```

### Select dropdown
```python
await page.select_option('select#country', value="MX")
```

### Extract text
```python
text = await page.text_content('.result-price')
items = await page.query_selector_all('.result-item')
for item in items:
    title = await item.text_content()
    print(title)
```

### Handle popups/modals
```python
# Dismiss cookie banner
try:
    await page.click('[data-testid="cookie-accept"]', timeout=3000)
except:
    pass  # No cookie banner
```

### Scroll into view
```python
await page.evaluate("window.scrollBy(0, 500)")
# Or scroll to element
await page.locator('.target-element').scroll_into_view_if_needed()
```

## Log Format

Each `final_script_log.txt` entry:
```
step 1 action: Navigate to flights page
step 2 action: Set departure city to SEA
step 3 action: Set arrival city to JFK
step 4 action: Select date 2026-08-15
step 5 action: Click search
step 6 action: Extract cheapest flight price
RESULT: $342 on Delta, departing 8:15 AM
```

## Error Handling

```python
try:
    await page.click('.filter-button', timeout=5000)
except TimeoutError:
    # Take debug screenshot
    await page.screenshot(path="screenshots/debug_filter_not_found.png")
    # Try alternative selector
    await page.click('[aria-label="Filter"]')
```

## Anti-Detection Tips

- Use Firefox (avoids Chromium TLS fingerprinting)
- Set realistic viewport (1280x1800)
- Add small delays between actions: `await page.wait_for_timeout(500)`
- Don't use `full_page=True` screenshots (triggers detection on some sites)
