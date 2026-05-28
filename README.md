# Kiro Webwright Power

Terminal-native web agent that drives Playwright browsers to automate web tasks, extract data, fill forms, and produce reusable scripts with screenshot evidence.

Based on [Microsoft Webwright](https://github.com/microsoft/Webwright) (MIT License) — adapted for Kiro's tool interface.

## Install

In Kiro: **Powers panel → Add power from GitHub → `JosSantamaria/Kiro-Webwright`**

## Structure

```
├── POWER.md                  # Power metadata + onboarding + instructions
├── steering/
│   ├── playwright-patterns.md  # Browser launch, selectors, screenshots
│   ├── workflow.md             # Plan → Explore → Author → Execute → Verify
│   └── cli-tool-mode.md       # Reusable parameterized CLI tools
├── LICENSE
└── README.md
```

## Keywords (auto-activation)

`browser`, `web`, `playwright`, `scraping`, `automation`, `web-agent`, `screenshot`, `form`, `extract`, `navigate`

## How it works

1. You describe a web task (search flights, extract prices, fill a form)
2. Kiro activates the Webwright power based on keywords
3. The agent writes Python/Playwright scripts, launches Firefox, captures screenshots
4. Output: a reusable `final_script.py` + screenshot evidence + action log

## Requirements

- Python 3.10+
- Playwright (`pip install playwright && playwright install firefox`)

## License

MIT — dual copyright Microsoft Corporation (original) + Joset Santamaria (Kiro adaptation)
