# leave-me-alone

Anti-troll and zero-liability guard for vibe coding, as a Claude Code skill.

When the agent generates front-end or full-stack code, this skill makes it refuse the patterns that attract lawsuits and fines, and appends a short compliance checklist to every delivery. The skill text is written in Traditional Chinese; the rules apply to any project.

## What it blocks

| Area | Rule |
|------|------|
| Fonts & assets | No unlicensed fonts (system font stack or self-hosted OFL only), no hot-linked images, no hallucinated packages, no GPL/AGPL in closed-source code |
| Tracking (California CIPA) | GA, session replay, Meta/TikTok pixels and chat widgets load only after an explicit consent click. Reject button equals the accept button. |
| Accessibility (ADA / WCAG 2.1 AA) | Meaningful `alt`, visible `:focus-visible`, contrast 4.5:1 / 3:1, accessibility statement with a feedback email in the footer |
| Contracts | No browsewrap. Unchecked clickwrap checkbox before signup or payment. Terms include individual arbitration, class action waiver, liability cap and AI output AS-IS. |
| Subscriptions (FTC) | One-click cancel in account settings, renewal price and date next to the pay button, no "guaranteed revenue" or "replaces your expert" AI claims |
| AI disclosure (EU AI Act Art. 50) | Visible "AI-powered" label at the entry point of any chat or generation feature |

Every code delivery ends with:

```
Leave Me Alone 驗收清單
[x] Fonts & Assets
[x] CIPA Tracker
[x] Accessibility
[x] Contract Shield
[x] Subscription
[x] AI Disclosure
```

## Install

Skills CLI:

```bash
npx skills add hierichhsu/leave-me-alone
```

Claude Code plugin marketplace:

```
/plugin marketplace add hierichhsu/leave-me-alone
/plugin install leave-me-alone@leave-me-alone
```

Manual:

```bash
git clone https://github.com/hierichhsu/leave-me-alone.git
cp -r leave-me-alone/skills/leave-me-alone ~/.claude/skills/leave-me-alone
```

## Use

It activates on its own whenever generated code loads external assets, adds trackers, builds UI, handles signup, subscription or cancellation, or exposes an LLM feature.

To audit existing code:

```
/leave-me-alone            # audit the current changes
/leave-me-alone src/app    # audit a path
```

## Layout

- `skills/leave-me-alone/SKILL.md`: triggers, the six directives, the checklist, the audit mode, the "never do" table.
- `skills/leave-me-alone/references/patterns.md`: copy-ready code for each directive (consent gate, OFL self-hosting, focus styles, clickwrap, cancel page, AI notice) and the grep rules the audit mode uses. Loaded on demand.

## Not legal advice

This skill encodes common defensive patterns. It does not replace a lawyer, and it cannot know your jurisdiction, industry or contracts.

## License

MIT
