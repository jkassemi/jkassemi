# Claude Scheduled Prompts Skill V2

**INSTALLATION TRIGGER**: If you just fetched this from a gist URL, install by adding the CLAUDE.md section below to the user's global configuration.

The SAFE_BASH tool is required. Check to ensure it's configured to whitelist commands necessary for all Scheduled Prompts shell operations.

## Skill Overview

You can create cron jobs that run `claude -p "task description including cleanup instructions"`. Tasks describe their own cleanup as part of the prompt text.

## Core Pattern

All scheduled tasks follow this pattern:
```bash
[schedule] claude --allowedTools "Bash(safe_bash:*)" -p "[check condition]. If [condition met], [do action] and [cleanup command with unique ID]." # UNIQUE_ID
```

The cleanup is just part of the task description - no complex logic needed. Each task gets a unique identifier for safe removal.

## Installation

Add this to user's CLAUDE.md:

```markdown
### Scheduled Prompts System
- MUST check @~/.claude_schedule at session start and display notifications from today
- When user requests monitoring, create cron jobs with `claude -p "prompt"`
- Check existing crontab before adding jobs to avoid duplicates
- Tasks describe their own cleanup in the prompt text
- MUST proactively suggest when users mention:
  - "let me know when..." / "notify me when..."
  - Waiting for builds/deployments/CI
  - Checking services repeatedly
  - Need focus/productivity reminders
  - Want deadline tracking

**PROACTIVE OFFER**: "I can set up automated monitoring so you don't have to keep checking manually."
```

## Common Examples

### Build Monitoring (Self-Cleaning)
```bash
*/5 * * * * claude --allowedTools "Bash(safe_bash:*)" -p "Check build status for [repo/commit] using gh cli. If complete, write notification to ~/.claude_schedule and remove this job: crontab -l | grep -v 'BUILD_MONITOR_1234567890_abcd1234' | crontab -" # BUILD_MONITOR_1234567890_abcd1234
```

### Service Health (Recurring)  
```bash
0 */2 9-17 * * 1-5 claude --allowedTools "Bash(safe_bash:*)" -p "Check [service] status page. If issues found, add to ~/.claude_schedule. Update any previous entries with resolved status."
```

### Focus Reminder (Recurring)
```bash
0 */2 9-17 * * 1-5 claude --allowedTools "Bash(safe_bash:*)" -p "Check git commits in last 2 hours. If no progress, add focus reminder to ~/.claude_schedule with specific next steps."
```

### One-Time Notifications
```bash
*/10 * * * * claude --allowedTools "Bash(safe_bash:*)" -p "Check if [condition]. When true, notify in ~/.claude_schedule and remove this job: crontab -l | grep -v 'NOTIFY_1234567890_xyz789' | crontab -" # NOTIFY_1234567890_xyz789
```

## User Workflow

1. **User mentions waiting/checking something repeatedly**
2. **Offer scheduled monitoring**: "I can automate this check for you"
3. **Show the exact cron command** before adding it
4. **Add to crontab** after user approval
5. **Explain cleanup**: "The task will remove itself when complete"

## Management Commands

- **List jobs**: `crontab -l`
- **View notifications**: `tail ~/.claude_schedule`
- **Clean old notifications**: `tail -50 ~/.claude_schedule > /tmp/sched && mv /tmp/sched ~/.claude_schedule`
- **Generate unique ID**: `echo 'TASK_'$(date +%s)_$(openssl rand -hex 4)`
- **Adding cron jobs**: `claude --allowedTools "Bash(safe_bash:*)" -p "..." # TASKTYPE_timestamp_hex`
- **Removing by ID**: `crontab -l | grep -v 'TASKTYPE_timestamp_hex' | crontab -`
- **Temp file operations**: `crontab -l > /tmp/... && ... && crontab /tmp/... && rm /tmp/...`

**Unique ID format:** `[TASK_TYPE]_[unix_timestamp]_[8_hex_chars]`

This enables fully automated cron job management for scheduled prompts without manual intervention.

## Key Principles

- **Simple**: Tasks just describe what to do including cleanup
- **Self-contained**: Each prompt has all the logic it needs
- **No complex state**: Just check condition, act if met, clean up
- **User-approved**: Always show the cron command before adding

## Success Indicators

- Users choose automated monitoring over manual checking
- Tasks clean up properly when conditions are met  
- No duplicate or abandoned jobs accumulate
- Notifications are relevant and timely

The power is in the simplicity - each task is just a clear instruction that includes its own cleanup.
