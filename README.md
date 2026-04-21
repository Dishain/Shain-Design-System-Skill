# Shain Design System Skill

Claude skill for working with a token-based design system. Covers Figma variables,
component architecture, semantic tokens, dark mode, accessibility, and design-to-code
handoff.

Used as supplementary material for the **AI for Designers** praktikum by Dmytro Legkov.

## What's inside

```
.
├── SKILL.md                              Core skill definition
└── references/
    ├── figma-best-practices.md           Figma structure, components, export
    └── token-improvements.md             How to evolve the token system over time
```

## How to install

### Claude Code / Cowork

Clone the repo into your skills directory:

```bash
cd ~/.claude/skills
git clone https://github.com/Dishain/shain-design-system-skill.git shain-dls
```

Claude will auto-discover the skill on next launch.

### Using the skill

Claude triggers this skill automatically when you mention design systems, Figma
variables, component specs, token naming, dark mode, or related topics. You can also
invoke it explicitly:

```
skill: shain-dls
```

## What the skill covers

- Three-tier token hierarchy (Primitive → Semantic → Component-specific)
- Naming conventions for variables, layers, and components
- Light / Dark mode infrastructure
- BEM-style CSS classes (`shain-[component]`)
- Theme config pattern for variant styling
- Accessibility baseline (WCAG AA)
- Figma best practices (auto layout, variants, properties, export)
- Token system evolution (adding modes, migrating, deprecating)

## License

MIT — see [LICENSE](LICENSE).
