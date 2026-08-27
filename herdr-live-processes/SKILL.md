---
name: herdr-live-processes
description: Use when starting live servers, watchers, workers, or long-running scripts; runs them in a Herdr sibling pane, preserves project context and focus, and verifies readiness.
---

# Herdr Live Processes

Run persistent processes in a dedicated Herdr pane so they remain visible, controllable, and attached to the current project workspace.

## Before Starting

Load the `herdr` skill and follow its installed CLI guidance. Verify that the current agent is inside Herdr before issuing control commands:

```bash
test "${HERDR_ENV:-}" = 1
```

If this fails, explain that Herdr is unavailable and stop.

Inspect the caller and current layout:

```bash
herdr pane current --current
herdr pane layout --pane "$HERDR_PANE_ID"
```

Keep the process in the current workspace and tab unless the user requests another location. Preserve the caller's working directory and record the caller pane ID so focus can be restored.

## Create the Process Pane

Split the caller pane in the direction that leaves both panes usable:

- Split a wide pane to the right.
- Split a narrow or tall pane down.
- Use `--no-focus` and the caller's current directory.

```bash
herdr pane split --current --direction down --cwd "$PWD" --no-focus
```

Read the new pane ID from `.result.pane.pane_id`. Do not predict it.

Pass required non-secret runtime overrides with repeated `--env KEY=VALUE` options on `pane split`. Prefer inherited credentials and avoid printing secret values.

## Start and Verify

Run the command in the new pane:

```bash
herdr pane run <pane-id> "<command>"
```

Wait for a concrete readiness message only when its exact text is known and stable. Do not infer it from framework conventions; otherwise skip this wait and inspect the output directly:

```bash
herdr pane wait-output <pane-id> \
  --match "<ready-text>" \
  --source recent-unwrapped \
  --timeout 120000
```

A timeout is inconclusive because readiness output can change. Inspect recent output and verify the process independently before deciding startup failed.

Then verify the process:

- For an HTTP server, read the advertised URL from the output, then call that URL or its health endpoint. Do not assume the default host or port.
- For a worker, watcher, or script, run `herdr pane process-info --pane <pane-id>` and inspect recent output.
- If startup fails, read the pane output and fix the cause instead of opening more panes.

Restore focus to the caller after verification:

```bash
herdr agent focus <caller-pane-id>
```

Report the process pane ID, command status, and URL or readiness signal.

## Stop or Restart

Send an interrupt to the process pane:

```bash
herdr pane send-keys <pane-id> ctrl+c
```

Confirm that the process returned to its shell before restarting it with `herdr pane run`. Close only panes created for this task, and only when cleanup is requested.

## Rules

- Do not detach the process from its Herdr pane.
- Do not create a new workspace or tab unless requested.
- Do not steal focus while starting background work.
- Do not expose credentials in commands, output, or summaries.
- Do not report success until readiness is verified.
