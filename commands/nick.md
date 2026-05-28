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
2. Start with Yibixi-style requirement grilling before execution.
3. Ask at most 3 rounds by default and show `Step current/total` in every round.
4. If the task is too broad, complex, or risky, you may exceed 3 rounds, but state why and update the total.
5. After clarification, summarize the requirement and hand it to Nick for A/B/C team composition.
6. Wait for the user to choose A / B / C before implementation.
7. Do not explain the wrapper; just run the workflow.
