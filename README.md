# Kiro Powers

Collection of custom Kiro Powers for AI-assisted development.

## Available Powers

### webwright

Terminal-native web agent that drives Playwright browsers to automate web tasks, extract data, fill forms, and produce reusable scripts with screenshot evidence.

**Install in Kiro:** Powers panel → Add power from GitHub → `JosSantamaria/Kiro-Webwright` → select `webwright/` directory.

**Keywords:** browser, web, playwright, scraping, automation, web-agent, screenshot, form, extract, navigate

Based on [Microsoft Webwright](https://github.com/microsoft/Webwright) — adapted for Kiro's tool interface.

## Structure

```
kiro-powers/
├── README.md
└── webwright/
    ├── POWER.md              # Power metadata + onboarding
    └── steering/
        ├── playwright-patterns.md
        ├── workflow.md
        └── cli-tool-mode.md
```

## Adding New Powers

Create a new directory with at minimum a `POWER.md` file. See [Kiro docs on creating powers](https://kiro.dev/docs/powers/create/).
