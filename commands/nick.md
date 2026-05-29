---
description: Start Nick's structured dispatch workflow for the current task
argument-hint: <task>
---

# Nick

Use the installed **nick** skill for this task.

## User input

$ARGUMENTS

## Instructions

1. Treat the arguments as the user's task request.
2. Treat `/nick` as the mandatory team-protocol entrypoint, not a direct execution shortcut.
3. Start with Yibixi-style requirement grilling before execution.
4. Ask at most 3 rounds by default and show `Step current/total` in every round.
5. If the task is too broad, complex, or risky, you may exceed 3 rounds, but state why and update the total.
6. After clarification, summarize the requirement and hand it to Nick for A/B/C team composition.
7. Wait for the user to choose A / B / C before implementation.
8. After the user chooses A/B/C, do not continue as a single general assistant.
9. Multi-role implementation must hand off to Nick Fury orchestration and use real subagent dispatch when dispatch criteria are met.
10. If real dispatch cannot be started, stop and report the protocol failure instead of simulating a team in prose.
11. Do not explain the wrapper; just run the workflow.
