# CSE513 Assignment 1 - CPU Scheduling Simulator

A browser-based, dependency-free **single-CPU** scheduling simulator, implemented using only **HTML, CSS, and JavaScript**. It implements the exact scheduling rules in the assignment for FCFS, non-preemptive SJF, preemptive SRTF, and Round Robin with a user-configurable quantum.

## Run locally

**Option A - easiest:** Download/extract the source ZIP and double-click `index.html`. It works offline in Chrome, Firefox, Edge, or Safari. No build step, backend, database, authentication, Node.js, or paid hosting is needed.

**Option B - local HTTP server (optional):** From the extracted folder run `python -m http.server 8000` and open `http://localhost:8000`.

## How to use

1. Load Workload A or B (or one of the three additional student-designed test workloads) using the example selector and **Load**.
2. Add, edit or remove a process by editing ID, arrival time, or CPU burst.
3. Select FCFS, SJF, SRTF or Round Robin. For RR set a positive integer quantum (default 2).
4. Choose **Run simulation** to see the labeled Gantt chart, per-process metrics, a chronological scheduling decision inspector and averages, or **Compare all four** to compare the three averages side by side.
5. Click **Export CSV** to save a machine-readable copy of the process table.

The custom decision inspector explains FCFS/SJF picks, SRTF preemption and equality, RR queue ordering, and CPU idle intervals. The data does not leave the browser. There are no external fonts, libraries or analytics.

## Dependencies

- **Running the website:** none beyond a modern browser with JavaScript enabled.
- **Running automated tests:** Node.js 18 or newer, optional, using only the built-in test runner.

## Tests

In the project directory, run:

```bash
node --test tests/scheduler.test.js
```

Tests include 36 named cases, including the two required workloads, all four algorithms on five supplied/test workloads, invalid input, simultaneous arrivals, SRTF equal-time preemption, RR quantum boundaries, idle intervals, and a deterministic batch of 240 random workloads run under all four algorithms (960 algorithm-workload checks).

The independent Python test additionally confirms all process metrics and the entire dispatch timeline for 960 randomized algorithm/workload cases.

The `verify` routine checks each process's total CPU service, that no task executes before arrival, a gap-free/non-overlapping single-CPU timeline, nonnegative response/waiting times, metric identities, and that CPU idle intervals never intersect a ready process.

## Scheduling assumptions (per assignment)

- One CPU; each process has exactly one burst; no I/O; zero context-switch cost.
- Arrival times are nonnegative integers; CPU bursts and RR quantum positive integers; process IDs unique.
- Idle time is explicitly shown whenever nothing is ready.
- FCFS picks the earliest-arriving ready process.
- SJF is non-preemptive; picks shortest burst among ready processes.
- SRTF is preemptive but preempts **only** for strictly shorter remaining time; equal remaining time keeps the current process.
- Selection ties are broken by earlier arrival time, then earlier position in the original input list.
- RR uses FIFO; arrivals at a quantum boundary enter the queue **before** the unfinished running process; repeated consecutive slices remain separate.
- Each segment represents `[start, end)`, so `[2, 5)` occupies three time units.

## File map

- `index.html` - website structure.
- `styles.css` - responsive website design.
- `app.js` - DOM rendering, input editing, charts, metric cards, CSV export.
- `scheduler.js` - standalone scheduling engine, decision log, invariants and example workloads.
- `tests/scheduler.test.js` - automated tests.
- `docs/DEPLOYMENT.md` - free public deployment instructions.
- `docs/REPORT_URL_TO_FILL.txt` - instructions to add your actual public site URL to the report.
- `update_report_url.py` - optional helper to insert a verified clickable HTTPS URL into the PDF report (requires PyMuPDF for editing PDF, not for running the website).
- `tests/independent_reference.py` - independent Python oracle for all preset examples and RR q=1/2/4.
- `docs/verified_outputs.json` - fully reproducible JavaScript traces and metrics for all documented examples.
- `docs/START_HERE_TR.md` - step-by-step guide in Turkish.

## Limitations

- This is an educational simulator, **not** Ubuntu's real CPU scheduler.
- Processes have one CPU burst and no I/O, priority, multiprocessing, resource contention, or context-switch overhead.
- Time is represented by JavaScript safe integers. Extremely large numbers of RR slices can be slow because each slice is drawn and returned separately.
- Visual slice widths have a minimum size for readability; use the printed start/end timestamps for exact durations.
- It does not save changes across reloads or require a network connection.

## Submission reminders

- The PDF report needs a **working, publicly accessible website URL immediately below the assignment heading on its first page**. A locally opened file is not a public URL.
- Upload the static files to a public host such as GitHub Pages, then insert and verify the actual public link in the PDF report before submission. Do not invent a link.
- Submit the PDF plus this entire source as ZIP (or the accessible repository link) through Microsoft Teams.
- The PDF includes two manually worked examples for each of the four policies and all other required report sections.