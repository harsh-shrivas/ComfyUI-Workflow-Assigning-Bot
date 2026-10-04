# ComfyUI-Workflow-Assigning-Bot

An automated studio production routing and workload assignment workflow for ComfyUI.

Designed for creative directors, project managers, and lead artists, this workflow connects client tracking briefs in Google Workspace directly into an intelligent task distribution and calendar scheduling engine. It pulls live deliverables from Google Docs or Sheets, tallies production assets, balances workloads across available designers according to daily capacity thresholds, and automatically calculates delivery dates while isolating video and UGC content into dedicated department backlogs.

---

## Workflow Architecture

1. **Workspace Ingestion (`GoogleWorkspaceReader`):** Ingests live production grids from Google Docs or Sheets using the Google Gemini API to structure deliverables, slide counts, and deep heading anchor links.
2. **Extraction Review (`ShowText`):** Previews the structured summary and itemized deliverable manifest within the ComfyUI canvas.
3. **Task Distribution & Scheduling (`SplitAssigner`):**
   - Applies studio abbreviations (e.g., product name standardization).
   - Separates video and motion assets into a standalone video department summary.
   - Balances static and carousel designs evenly across artist queues.
   - Computes delivery deadlines based on daily capacity limits, skipping Sundays and alternate weekends.
4. **Final Schedule Output (`ShowText`):** Formats clear, two-line assignable task blocks complete with calculated due dates and direct asset brief links ready for dispatch to Slack, ClickUp, or Notion.

---

## Prerequisites & Required Nodes

Install the following custom nodes via ComfyUI Manager:

- **[ComfyUI-GoogleWorkspaceExtractor](https://github.com/harsh-shrivas/ComfyUI-GoogleWorkspaceExtractor):** Parses Google Docs and Google Sheets into structured deliverable feeds.
- **[ComfyUI-SplitAssigner](https://github.com/harsh-shrivas/ComfyUI-SplitAssigner):** Balances workloads and calculates calendar-aware deadlines across artist teams.
- **ComfyUI-Custom-Scripts:** Provides the `ShowText|pysssss` preview nodes.

---

## Setup & Configuration

1. Drag and drop `Assigning Bot.json` directly onto your ComfyUI workspace.
2. In the **GoogleWorkspaceReader** node:
   - Paste your shared Google Doc or Sheet URL into `google_link` (ensure link permissions are set to "Anyone with the link can view").
   - Insert your Gemini API Key in `gemini_api_key`.
   - Specify the target document section or sheet tab in `target_tab` (defaults to `Grid Reference [For Design Team]`).
3. In the **SplitAssigner** node:
   - Configure `num_artists` (e.g., `2`).
   - Set `capacity_per_day` (e.g., `4` slides per designer per day).
   - Set your date parameters: `assigning_start_date`, `start_date`, and `end_date` (format: `DD.MM.YY` or `DD/MM/YYYY`).
4. Click **Queue Prompt** to generate the distribution schedules.

---

## License

MIT License. Free to use, adapt, and integrate into internal studio operations and production pipelines.
