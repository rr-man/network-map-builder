# Network Topology Visualizer

Turn network device configuration files into an interactive, hierarchical topology map — offline, in a single HTML file.

Built for network engineers who otherwise rebuild diagrams by hand every time a site changes. The guiding principle is simple: **the configuration draws the diagram.** Every device and every link shown traces back to evidence in an uploaded file — nothing is invented, assumed, or guessed.

![Network Topology Visualizer](docs/screenshot-default.png)

## Highlights

- **Single file, fully offline.** One `network_topology_map.html` — no server, no install, no dependencies, no data leaves the browser. Safe for handling running-configs.
- **Config-derived hierarchy.** Devices are ordered by evidence in the configs (gateway ownership, CDP/LLDP neighbors, declared uplinks), not by guesswork. A misclassified device can't invert the stack.
- **Multi-vendor parsing.** Cisco IOS-style, Cisco Small Business / CBD, and Versa SD-WAN (`set`-syntax) configs, plus a normalized JSON format.
- **Derived internet node.** When a config proves a WAN edge (default route or WAN circuits), an `INTERNET` node is added automatically — dashed, tagged, and traceable to the exact config lines.
- **Advanced port map.** Toggle **Advanced** to show every populated switch port as a chip (grouped and VLAN-colored), and draw the physical port-to-port cabling (e.g. `gi22 ↔ gi22 · Gigabit`) between switches.
- **Interactive.** Click any device for a full detail panel (identity, WAN/LAN, L3/L2, ports, neighbors, source file). Search by hostname / IP / MAC. Dark & light themes. Export the topology to JSON and re-load it later.

## Quick start

1. Download `network_topology_map.html`.
2. Open it in any modern browser (Chrome, Edge, Firefox, Safari).
3. Click **Load sample** to see a demo, or drag your own config files onto the window.

That's it — there is no build step.

## Using your own configs

Drop one or more files onto the window, or use the upload button. Supported input:

| Input | Notes |
|-------|-------|
| Raw text configs | `.txt`, `.cfg`, `.conf`, `.config`, `.log`, `.ios`, `.run` — Cisco IOS-style, Cisco CBD / Small Business, Versa SD-WAN |
| Normalized JSON | An array of device objects, or `{ "devices": [ ... ] }` |

The tool extracts hostname, model/serial/firmware, management IP, WAN circuits and gateways, VLANs, per-port descriptions and VLAN/mode, routing (BGP/OSPF/EIGRP/static), switching (STP/VTP/stack/PoE), and CDP/LLDP neighbors — whatever is present. Missing data is simply not shown; it is never fabricated.

### Optional directives

You can add comment directives to any text config to override detection:

```
! topo-id: core-sw-1
! topo-type: switch          # internet | router | sdwan | switch | ap | endpoint
! topo-uplink: edge-rtr-1    # id of the parent device
```

These are authoritative when present; otherwise the tool infers structure from evidence.

## How the hierarchy is decided

Row placement uses config evidence, strongest first:

1. **Gateway chain** — the device that owns another device's gateway IP renders above it.
2. **CDP / LLDP neighbors** — including reverse-direction (a switch's neighbor table naming an AP).
3. **Declared / manual uplinks** — `topo-uplink` directives or the in-app *Set Uplink* control. Backwards links are auto-corrected and flagged.
4. **Device type** — used only as a fallback when no evidence exists (and always for color/badges).

Unambiguous evidence is promoted to a solid link so uploads self-assemble into a connected diagram; ambiguous evidence stays dotted and labeled.

## Views

- **Default** — the clean topology with concise port names at each link end (`gi1`, `vni-0/0.0`, `gi22 ↔ gi22`).
- **Advanced** (toggle in the toolbar) — adds a per-port chip strip beneath each switch, full port detail (description, media/speed, IP), and the physical port-to-port cross-connect between switches.

## Toolbar reference

| Control | What it does |
|---------|--------------|
| **Load sample** | Loads a built-in demo topology |
| **Clear** | Removes all devices |
| **Inferred links** | Show/hide dotted links inferred from evidence |
| **Advanced** | Show physical port detail (chips + port-to-port cabling) |
| **☁ WAN side** | Table of every device's internet-facing side (circuits, WAN IPs, gateways, NAT) |
| **⤓ Export JSON** | Download the current topology (incl. auto-links, derived nodes, manual edits) |
| **☀ / ☾** | Toggle dark / light theme |
| **Search** | Filter the diagram by hostname, IP, or MAC |

## Privacy & security

Everything runs client-side in the browser. No configuration data is uploaded anywhere, there is no telemetry, and the file works on an air-gapped machine. Treat the exported JSON the same way you treat the source configs.

## Browser support

Any current version of Chrome, Edge, Firefox, or Safari. No Internet Explorer.

## Roadmap

See [CHANGELOG.md](CHANGELOG.md) for what's shipped. Planned next:

- Tuning parsers against more real-world vendor formats.
- Prioritizing CDP/LLDP ingest as physical-link ground truth; optional SNMP/API discovery.
- Multi-site dashboards and drift detection against a standard template.

Product intent is described in [docs/PRD.md](docs/PRD.md).

## Contributing

Issues and pull requests are welcome. Because the whole app is one HTML file, changes are easy to review — open `network_topology_map.html`, edit the inline `<script>`, and test by loading configs. Please keep the "no invented data" principle: anything drawn must be traceable to an uploaded file or explicit evidence.

## Authors

- **Ron Mangune** — author
- **Joey Cassillas** — contributor

## License

[MIT](LICENSE)
