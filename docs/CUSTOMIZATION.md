# Customization Guide

## Profile (config/profile.yml)

This is the single source of truth for your identity. All modes read from here.

Key sections:
- **candidate**: Name, email, phone, location, LinkedIn, portfolio
- **target_roles**: Your North Star roles and archetypes
- **narrative**: Your headline, exit story, superpowers, proof points
- **compensation**: Target range, minimum, currency
- **location**: Country, timezone, citizenship, short visa summary, detailed work authorization, US relocation goal, sponsorship policy, on-site availability

Recommended `location` shape:

```yaml
location:
  country: "Canada"
  city: "Toronto"
  timezone: "ET"
  citizenship: "Canadian citizen"
  visa_status: "Authorized to work in Canada without sponsorship; US roles may require employer-supported visa or remote-from-Canada arrangement"
  work_authorization:
    canada: "Authorized without sponsorship"
    united_states_current: "Not currently authorized without employer support"
    united_states_preferred_paths: "TN preferred; L1 if available; other employer-sponsored US visa acceptable"
    united_states_remote_fallback: "Open to remote work from Canada when allowed"
  us_relocation_goal: "Ideally wants to move to the USA for the right role"
  sponsorship_policy: "US roles that explicitly sponsor visas are a plus; US roles that require existing US work authorization and forbid sponsorship are soft blockers unless remote from Canada is allowed"
  onsite_availability: "Greater Toronto Area hybrid is fine; remote preferred for the right scope"
```

## Target Roles (modes/_shared.md)

The archetype table in `_shared.md` determines how offers are scored and CVs are framed. Edit the table to match YOUR career targets:

```markdown
| Archetype | Thematic axes | What they buy |
|-----------|---------------|---------------|
| **Your Role 1** | key skills | what they need |
| **Your Role 2** | key skills | what they need |
```

Also update the "Adaptive Framing" table to map YOUR specific projects to each archetype.

## Portals (portals.yml)

Copy from `templates/portals.example.yml` and customize:

1. **title_filter.positive**: Keywords matching your target roles
2. **title_filter.negative**: Tech stacks or domains to exclude
3. **search_queries**: WebSearch queries for job boards (Ashby, Greenhouse, Lever)
4. **tracked_companies**: Companies to check directly

## CV Template (templates/cv-template.html)

The HTML template uses these design tokens:
- **Fonts**: Calibri (headings) + DM Sans (body) -- self-hosted in `fonts/`
- **Colors**: Cyan primary (`hsl(187,74%,32%)`) + Blue accent (`#2563b0`)
- **Layout**: Single-column, ATS-optimized

To customize fonts/colors, edit the CSS in the template. Update font files in `fonts/` if switching fonts.

## Negotiation Scripts (modes/_shared.md)

The negotiation section provides frameworks for salary discussions. Replace the example scripts with your own:
- Target ranges
- Geographic arbitrage strategy
- Pushback responses

## Hooks (Optional)

Career-ops can integrate with external systems via Claude Code hooks. Example hooks:

```json
{
  "hooks": {
    "SessionStart": [{
      "hooks": [{
        "type": "command",
        "command": "echo 'Career-ops session started'"
      }]
    }]
  }
}
```

Save hooks in `.claude/settings.json`.

## States (templates/states.yml)

The canonical states rarely need changing. If you add new states, update:
1. `templates/states.yml`
2. `normalize-statuses.mjs` (alias mappings)
3. `modes/_shared.md` (any references)
