# IBC Interior Ballistics — MCP server

[IBC](https://reloadingcalc.com) is a physics-based interior-ballistics
calculator for handloaders. The same solver that powers the website is
exposed as a **remote MCP server**, so your AI agent (Claude Code, Cursor,
Hermes, or any MCP client) can run real simulations as a tool call.

## Connect

Request a free beta key at <https://reloadingcalc.com/mcp.html>, then:

```
claude mcp add --transport http ibc-ballistics https://reloadingcalc.com/mcp \
  --header "Authorization: Bearer <your key>"
```

Or paste into any MCP client config:

```json
{
  "mcpServers": {
    "ibc-ballistics": {
      "url": "https://reloadingcalc.com/mcp",
      "headers": { "Authorization": "Bearer <your key>" }
    }
  }
}
```

## Tools

- `solve_internal_ballistics` — modeled velocity, energy, and pressure for an
  exact cartridge + bullet + powder + charge
- `generate_load_ladder` — a charge work-up ladder (≥5 rungs) with per-rung numbers
- `trajectory` — 3-DOF point-mass drop table fed by the modeled muzzle velocity
- `get_cartridge_spec` — cartridge lookup in the served component database
- `list_powders` — propellant listings with served attributes
- `check_component_compatibility` — whether a cartridge+powder pair resolves

Every result carries a non-removable safety envelope: this is modeling
output, **not published load data**. Cross-check against published data for
your exact components, start low, and work up.

## Measured, not marketed

The engine grades itself against published load data: 30,327 printed loads
at launch, average velocity error about 7.5%, worst powders included in the
public table.

- Methodology: <https://reloadingcalc.com/methodology.html>
- Accuracy paper: <https://reloadingcalc.com/accuracy-paper.html>

This repository exists so the server can be listed in the official MCP
registry. The implementation is not public; the served API is.
