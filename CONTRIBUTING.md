# Contributing

Thanks for your interest in improving the Network Topology Visualizer.

## Project shape

The entire application is a single file: `network_topology_map.html`. HTML,
CSS, and JavaScript all live inside it. There is no build step and no runtime
dependencies — open the file in a browser to run it.

## Making changes

1. Open `network_topology_map.html` and edit the inline `<style>` or `<script>`.
2. Test by opening the file and clicking **Load sample**, then by dropping real
   (sanitized) config files onto the window.
3. Toggle **Advanced**, dark/light themes, and click nodes to confirm the detail
   panel still renders.

## Core principle: never invent data

Anything the app draws — a device, a link, a label — must trace back to an
uploaded file or explicit evidence (gateway ownership, CDP/LLDP, a directive,
or a manual override). Inferred links must be visually distinct (dotted) and
explain their evidence. Please preserve this when adding features.

## Adding a vendor parser

Parsers normalize a raw config into the shared device schema. To add one:

- Detect the format early in `parseTextConfig` and dispatch to your parser.
- Populate the standard fields (`id`, `hostname`, `type`, `mgmtIp`, `interfaces`,
  `wan`, `routing`, `switching`, `neighbors`, …). Leave unknown fields unset —
  do not fill placeholders.
- Add a sanitized sample to your PR description so the parser can be reviewed.

## Commit / PR guidance

- Keep changes focused and describe the behavior before/after.
- Bump the version string in the file and add a `CHANGELOG.md` entry.
- Never commit real customer configuration files.
