---
name: shutdown-pc
description: >
  Safely shut down this Windows PC when the user asks to shut down, power off,
  turn off the computer, schedule a shutdown, or shut down after current work.
  Use for Windows shutdown requests, not restart/sleep unless explicitly requested.
---

# Shutdown PC

Use this skill only after an explicit user request to shut down or schedule shutdown.

## Safety

- Default to normal Windows shutdown, not forced shutdown.
- Use a delay so user can cancel:
  - default: 60 seconds
  - if user asks a specific delay, use that delay
- Tell user how to abort before running or in final:
  - `shutdown.exe /a`
- Do not use `/f` unless user explicitly says force shutdown.
- If user asks "after you finish", finish required work first, then schedule shutdown.
- If user asks restart, use `/r` instead of `/s`; otherwise use `/s`.
- If request ambiguous between sleep/restart/shutdown, ask one short question.

## Command

Run with escalation because shutting down OS is system-level:

```powershell
shutdown.exe /s /t 60 /c "Codex scheduled shutdown."
```

For custom delay:

```powershell
shutdown.exe /s /t <seconds> /c "Codex scheduled shutdown."
```

For explicit forced shutdown:

```powershell
shutdown.exe /s /f /t <seconds> /c "Codex scheduled forced shutdown."
```

For explicit restart:

```powershell
shutdown.exe /r /t <seconds> /c "Codex scheduled restart."
```

## Final Response

Keep it short:

- confirm shutdown scheduled
- state delay
- include abort command
