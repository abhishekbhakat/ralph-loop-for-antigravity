# Ralph Loop for Antigravity - Community Issues

Community issue tracker for [Ralph Loop for Antigravity](https://github.com/abhishekbhakat/ralph-loop-for-antigravity), a VS Code extension that enables autonomous AI agent execution using Antigravity.

## What is Ralph Loop?

Ralph Loop solves two fundamental problems with AI coding assistants:

1. **Context window limitations** - LLMs forget important context mid-task
2. **Constant oversight required** - AI cannot work autonomously for extended periods

The solution: **externalize memory to files** and run AI agents in **iterative loops** with fresh context per iteration.

### How It Works

```
1. Read tasks from PRD.md (task file)
2. Check progress in progress.txt
3. Complete exactly ONE task
4. Append progress (never delete)
5. Commit changes
6. Repeat until all tasks done or max iterations reached
```

Each iteration spawns a fresh Cascade session, ensuring the agent always has full context by reading from disk rather than relying on conversation memory.

## Model Fallbacks

Ralph Loop supports a rate-limit fallback chain for model selection.

- Your **primary model** runs first.
- If that request hits a **rate limit or quota-style error**, Ralph can retry with a configured fallback.
- Fallbacks are organized into practical model groups:
  - **Gemini Flash**
  - **Anthropic/OpenAI**
  - **Gemini Pro**

As of March 28, 2026, the installed Antigravity app on the development machine returns these models:

- `Gemini 3.1 Pro (High)`
- `Gemini 3.1 Pro (Low)`
- `Gemini 3 Flash`
- `Claude Sonnet 4.6 (Thinking)`
- `Claude Opus 4.6 (Thinking)`
- `GPT-OSS 120B (Medium)`

Each fallback step lets you choose:

1. The **group**
2. The **specific model** inside that group

Example:

```
Primary: Gemini 3 Flash
Fallback 1: Anthropic/OpenAI -> Claude Opus 4.6 (Thinking)
Fallback 2: Gemini Pro -> Gemini 3.1 Pro (High)
```

## Reporting Issues

Before opening an issue:

1. **Search existing issues** - Your issue may already be reported
2. **Check requirements**:
   - VS Code 1.75.0 or later
   - Antigravity in agent driven mode
   - A workspace folder with task files
3. **Gather information**:
   - OS and version
   - VS Code version
   - Ralph Loop extension version
   - Steps to reproduce
   - Relevant logs from the "Ralph Loop" output channel

### Issue Templates

| Template             | Use For                                    |
|----------------------|--------------------------------------------|
| **Bug Report**       | Something isn't working as expected        |
| **Feature Request**  | Suggest new features or improvements       |

## Common Issues

### "No task file selected"
Select a task file in the sidebar Configuration section before starting the loop.

### "No workspace folder open"
Open a folder in VS Code before starting Ralph Loop.

### Loop not responding
Use `Ralph: Emergency Stop Ralph Loop` from the Command Palette (Cmd/Ctrl+Shift+P).

### Token extraction failing
Ensure Antigravity is running. If auto-discovery fails, manually configure the CSRF token and port in settings.

## Links

- [Extension Repository](https://github.com/abhishekbhakat/ralph-loop-for-antigravity)
- [VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=abhishekbhakat.ralph-loop-for-antigravity)
- [Open VSX Registry](https://open-vsx.org/extension/abhishekbhakat/ralph-loop-for-antigravity)

## License

MIT
