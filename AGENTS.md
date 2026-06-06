# DiceBot Agent Instructions

This is a legacy C# WinForms/.NET Framework DiceBot project.

## Hard rules
- Do not rewrite the whole project at once.
- Keep changes small, buildable, and reviewable.
- Prefer practical fixes over overengineering.
- Preserve existing public behavior unless explicitly asked to change it.
- Do not remove betting-site implementations unless requested.
- Do not commit secrets, API keys, cookies, tokens, credentials, or account data.
- Comments in source code must be English.

## Project facts
- Main solution: DiceBot.sln
- Main project: DiceBot/DiceBot.csproj
- Legacy target: .NET Framework 4.5.2
- UI style: WinForms
- Important platform: x86 first, because native dependencies exist.
- Treat x64/AnyCPU carefully.

## Workflow
1. Inspect the codebase before editing.
2. Explain the intended change shortly.
3. Modify the smallest necessary file set.
4. After edits, show a diff summary.
5. Run or provide the exact build command.
6. If build fails, analyze the error and fix only the direct cause first.

## Modernization direction
- First goal: make the existing project build reliably.
- Second goal: retarget safely to .NET Framework 4.8 if needed.
- Third goal: isolate site-specific API clients.
- Fourth goal: improve structure without breaking Programmer Mode/Lua behavior.
