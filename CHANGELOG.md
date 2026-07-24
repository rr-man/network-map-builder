# Changelog

All notable changes to the Network Topology Visualizer are documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [3.3.0] - 2026-07-24

### Changed
- **VLANs shown only in Advanced view.** The VLAN line inside each device box now appears only when Advanced is ON; the default view stays clean. The node box also shrinks back to its compact height in the default view (no empty line) and expands to fit the VLAN line in Advanced.

## [3.2.0] - 2026-07-24

### Changed
- **Adaptive footer spacing.** The gap between the two credits now scales with the window width via clamp(12px, 4vw, 64px) and wraps to separate lines (hiding the middle dot) on very narrow screens, so the names can never overlap at any size.

## [3.1.0] - 2026-07-24

### Added
- **Vector PDF export.** New "⤓ PDF (vector)" button serializes the live SVG (with theme colors resolved) into a print document sized to the diagram and opens Save-as-PDF. The topology is embedded as vector, so it stays crisp at any zoom or print size.

### Changed
- **Footer redesigned.** Credits now render as a centered single line with a fixed-gap dot separator and inline LinkedIn icons, replacing the old block/spacer layout that could crowd or overlap.

## [3.0.0] - 2026-07-24

### Added
- **Collapsible load-status details.** The load summary ("Loaded N devices…") now shows as a compact one-line bar with a Show/Hide details toggle. The full breakdown (links established, inferences, notices) is collapsed by default and expands on demand; single-line messages hide the toggle.

## [2.9.0] - 2026-07-24

### Changed
- **Advanced cross-connect matches other connections.** The chip-to-chip switch link in Advanced view now uses the same solid edge base and animated flow overlay as every other connection, and labels each end with its port name beside the owning switch (port-near-owner) plus a centered media note — instead of the old dotted line with one combined middle label.

## [2.8.0] - 2026-07-24

### Changed
- **VLANs shown inside the device box.** The VLAN list now renders as a line inside each device (below the management IP) instead of floating beside/below the node; node height increased to fit it and the port-count moved onto the IP line. Cleaner and keeps VLAN info attached to its device.

## [2.8.0] - 2026-07-24

### Changed
- **VLANs shown inside the device box.** The VLAN list moved from beside/below the node to a line inside the box (below the management IP); node height increased to fit it, and the port-count moved onto the IP line so nothing overlaps.

## [2.7.0] - 2026-07-24

### Changed
- **Cross-connect drawn identically to other connections.** The confirmed switch-to-switch link now uses the same solid edge base and animated flow overlay as the SD-WAN/uplink connections (previously a lighter, distinct style), so every physical cable looks and animates the same. Only genuinely inferred links remain dotted.

## [2.6.0] - 2026-07-24

### Added
- **Cross-connect flow animation.** The physical switch-to-switch link now carries the same animated flow overlay as the SD-WAN→switch uplinks, so all confirmed cables read consistently.

### Changed
- **+150px horizontal spacing between same-tier peers.** Nodes sharing a tier (e.g. the two switches) are now placed at a fixed column pitch centered on the canvas, giving 150px more breathing room so their labels and VLAN text never crowd.

## [2.5.0] - 2026-07-24

### Changed
- **Spacing presets increased by 50px** across the board for more visual breathing room: Tight 168, Comfortable 200 (default), Airy 235, Spacious 270, Adaptive 178 base.

## [2.4.0] - 2026-07-24

### Added
- **Spacing dropdown** in the toolbar with five vertical-spacing presets: Tight (118px), Comfortable (150px, default), Airy (185px), Spacious (220px), and Adaptive (128px base + extra only on rows whose links carry port labels). Switches live without reloading.

## [2.3.0] - 2026-07-24

### Fixed
- **Physically-accurate switch peering.** A switch whose gateway is only reachable through a cross-connect peer (no physical uplink port of its own) is no longer drawn with a phantom link to the SD-WAN. Such switches stay same-tier peers, connected by the cross-connect; the gateway is recorded as an L3 detail.
- **Gateway links require a physical port.** Gateway-chain promotion (solid or dotted) now only draws a link when the child also has a corroborating physical port (WAN/uplink/SDWAN interface or CDP/LLDP neighbor) toward that parent.
- **No overlapping labels.** VLAN text moved below each node (was to the right, colliding with adjacent switches); link port labels separated so parent- and child-side labels never overlap in tight tier gaps.

### Changed
- **Port labels sit beside their own device.** The cross-connect now shows a `gi22` label next to each switch (port-near-device) instead of one combined label in the middle, with a small media note (`Gigabit`) at center.
- Confirmed physical cross-connect is drawn as a **solid** line rather than dotted.

## [2.2.0] - 2026-07-24

### Fixed
- **SD-WAN edge now shows its port toward the switch.** Previously only the switch end of the SD-WAN↔switch link was labeled (`gi1`); the parent's port is now resolved to the LAN sub-interface that owns the child's gateway (e.g. Versa `vni-0/2.20`), so both ends of every link show a port. Shared parent ports (one LAN interface serving multiple switches) are labeled once to avoid duplicate/overlapping labels.

## [2.1.0] - 2026-07-23

### Added
- **Port names in the default view.** Each link end now shows the concise physical port name (`gi1`, `vni-0/0.0`, `gi22 ↔ gi22`) without needing Advanced mode.

## [2.0.0] - 2026-07-23

### Added
- **Physical port-to-port cross-connect.** In Advanced mode, the dotted switch-to-switch link now connects the actual `gi22` port chip on one switch to the `gi22` chip on the other, with anchor dots and a `gi22 ↔ gi22 · Gigabit` label — representing the real cabling rather than a link between node boxes.

### Changed
- Cross-connect rendering moved to a post-node pass so it can anchor to port-chip positions and draw above the strips.

## [1.9.0] - 2026-07-23

### Fixed
- **No more overlap** in the Advanced port map: adjacent switch strips are spaced apart, redundant VLAN side-text is suppressed when a strip is shown, parent-side link labels are skipped over strips, and the cross-connect routes below the strips.

### Changed
- **Subtler upload status.** Informational notices no longer turn the whole message red; the text stays calm with a soft amber accent bar.

## [1.8.0] - 2026-07-23

### Added
- **Advanced port map.** Each switch grows a per-port chip strip showing every populated port with its description, VLAN-colored border, and trunk direction arrows (uplink ↑, cross-connect ↔, AP ↓). Adjacent identical ports consolidate (`gi1-9 POS ×9`); empty ports collapse.

## [1.7.0] - 2026-07-23

### Added
- **Advanced view toggle.** Shows the physical port at each end of every link (interface name, description, media/speed).
- **Media detection** on links: `gi` → Gigabit, `fa` → FastEthernet, `te` → 10-Gigabit, `vni` → Versa, `eth` → Ethernet.
- Cross-connect links labeled with port numbers and media (`gi22 ↔ gi22 · Gigabit`).

## [1.6.0] - 2026-07-23

### Added
- **Versa SD-WAN parser** for `set`-syntax configs: identity, WAN circuits, LAN sub-interface gateways, VLANs, NAT/CGNAT, and SD-WAN controllers.
- **Cisco Small Business / CBD switch support** (quoted hostnames, `management vlan ip-address`).
- **Switch cross-connect detection** from matching `CROSSCONNECT` trunk descriptions.
- SD-WAN facts section in the detail panel.

### Fixed
- Classification no longer misreads a switch as SD-WAN because a port is *described* "SDWAN-LAN", nor as an AP because of a "GUEST-WLAN" VLAN name.

## [1.5.0] - 2026-07-22

### Added
- **Credits footer** with LinkedIn links for the author and contributor.

### Changed
- Footer refined through several iterations to a single centered line with a fixed gap; rebuilt on plain inline layout to eliminate a flexbox overlap bug.

## [1.4.0] - 2026-07-22

### Added
- **Evidence promotion.** Unambiguous evidence (unique gateway owner, single CDP/LLDP neighbor, including reverse-direction) is promoted to a solid uplink so bare uploads self-assemble into a connected diagram, matching the built-in sample. Ambiguous evidence stays dotted.

## [1.3.0] - 2026-07-22

### Added
- **Derived INTERNET node** when a config proves a WAN edge (default route + WAN interface); dashed, tagged, removable, and traceable to source lines.
- **Gateway-chain hierarchy** — the device owning another's gateway IP renders above it. Immune to type misclassification.

### Changed
- Restored the functional device color palette (cyan switch, amber router, violet SD-WAN, green AP, gold endpoint) on top of the Coeo-branded chrome.

## [1.2.0] - 2026-07-22

### Added
- **Coeo branding** — logo in the header (theme-switched), navy/blue color scheme on the application shell, dark and light themes.

## [1.1.0] - 2026-07-21

### Fixed
- **Backwards-link handling.** A link recorded child→parent is auto-corrected and flagged rather than inverting the hierarchy.

### Changed
- Rows locked to device type with the empty-tier collapse, so a router always renders above a switch.

## [1.0.0] - 2026-07-21

### Added
- Initial hierarchy engine, tiered top-down layout, and the rule that links can reorder rows.

## [0.x] - 2026-07-21

### Added
- Normalized device schema; JSON and raw-text config ingestion; tiered layout; color-coded device classes and legend; click-for-detail panel; search by hostname/IP/MAC; WAN-side overview; port labels on link ends; VLAN text beside nodes; device-type and uplink overrides; new-site template stamping; JSON export; inferred-links toggle.

[2.1.0]: https://example.com/releases/2.1.0
[2.0.0]: https://example.com/releases/2.0.0
[1.9.0]: https://example.com/releases/1.9.0
[1.8.0]: https://example.com/releases/1.8.0
[1.7.0]: https://example.com/releases/1.7.0
[1.6.0]: https://example.com/releases/1.6.0
[1.5.0]: https://example.com/releases/1.5.0
[1.4.0]: https://example.com/releases/1.4.0
[1.3.0]: https://example.com/releases/1.3.0
[1.2.0]: https://example.com/releases/1.2.0
[1.1.0]: https://example.com/releases/1.1.0
[1.0.0]: https://example.com/releases/1.0.0
