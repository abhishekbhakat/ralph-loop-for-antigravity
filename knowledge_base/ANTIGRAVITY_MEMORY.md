# Antigravity Memory

The Google Antigravity Agent Manager uses several distinct storage locations depending on whether the data is global application state, workspace-specific project data, or the "brain" of the agent's logic. [codelabs.developers.google](https://codelabs.developers.google.com/getting-started-google-antigravity)

## Core Storage Locations
Antigravity splits data between system-level configuration and project-level artifacts.

- **Application Root (`~/.antigravity/`):** This is the primary system folder for the application.  It contains central Artifacts, Knowledge bases, and the core operational data for the Agent Manager. [antigravity](https://antigravity.google/docs/agent-modes-settings)
- **Global Settings (`~/.gemini/`):** High-level rules and workflows that apply to all projects are stored here.  Key files include `~/.gemini/GEMINI.md` for global rules and `~/.gemini/antigravity/global_workflows/` for shared automation sequences. [codelabs.developers.google](https://codelabs.developers.google.com/getting-started-google-antigravity)
- **Workspace Data (`.agent/`):** Each project directory contains a hidden `.agent/` folder (or sometimes `.antigravity/`).  This stores workspace-specific rules, workflows, and temporary execution state. [antigravityai](https://antigravityai.directory/antigravity-file-guide)

## Agent Manager "Brain" and History
The Agent Manager maintains its own specialized storage for tracking the reasoning and state of long-running missions. [reddit](https://www.reddit.com/r/GoogleAntigravityIDE/comments/1p79xmz/where_are_the_planning_files_stored/)

- **Mission Reasoning:** Detailed planning files—such as `Tasks.md`, `ImplementationPlan.md`, and `Walkthrough.md`—are often stored in `~/.gemini/antigravity/brain/PROJECT_ID/`.  This acts as the agent's "long-term memory" for a specific project. [reddit](https://www.reddit.com/r/GoogleAntigravityIDE/comments/1p79xmz/where_are_the_planning_files_stored/)
- **State Database:** The application's UI state and conversation index are typically managed in SQLite databases located in platform-specific application support folders: [deepwiki](https://deepwiki.com/lbjlaq/Antigravity-Manager/6.8-data-storage)
  - **macOS:** `~/Library/Application Support/com.ctrler.antigravity/` [deepwiki](https://deepwiki.com/lbjlaq/Antigravity-Manager/6.8-data-storage)
  - **Windows:** `%APPDATA%\com.ctrler.antigravity\` [deepwiki](https://deepwiki.com/lbjlaq/Antigravity-Manager/6.8-data-storage)
  - **Linux:** `~/.config/com.ctrler.antigravity/` or `~/.config/Antigravity/` [discuss.ai.google](https://discuss.ai.google.dev/t/bug-report-undo-function-deletes-conversation-from-google-antigravity-agent-manager/111708)

## Configurable Storage
You can change the storage location for the Agent Manager's data through startup arguments or system settings. [github](https://github.com/lbjlaq/Antigravity-Manager/blob/main/README_EN.md)

| Data Type | Storage Mechanism | Path / Customization |
| :--- | :--- | :--- |
| **Agent History** | SQLite Database | `accounts.db` in AppData/Application Support  [deepwiki](https://deepwiki.com/lbjlaq/Antigravity-Manager/6.8-data-storage) |
| **User Data Dir** | CLI Argument | Use `--user-data-dir` to relocate all app data  [github](https://github.com/lbjlaq/Antigravity-Manager/blob/main/README_EN.md) |
| **Workspace Files** | File System | Set during "New Workspace" setup in the UI  [codelabs.developers.google](https://codelabs.developers.google.com/getting-started-google-antigravity) |
| **Global Rules** | Markdown File | Edit `~/.gemini/GEMINI.md` directly  [codelabs.developers.google](https://codelabs.developers.google.com/getting-started-google-antigravity) |

If you are using **Remote SSH**, Antigravity stores the agent manager's state and conversation history on the *remote host* (usually in `~/.config/antigravity` or `~/.gemini/antigravity`), which is why histories may not sync automatically across different client machines. [discuss.ai.google](https://discuss.ai.google.dev/t/bug-antigravity-agent-manager-conversation-history-exists-on-remote-host-but-is-not-listed-or-creatable-after-latest-update-remote-ssh-mac-ubuntu/112857)

## Agent Manager vs Cascade Storage (Ralph Users)

**Important:** Agent Manager and Cascade use different storage mechanisms:

- **Agent Manager:** Stores conversation data in a **SQLite database**
- **Cascade (used by Ralph):** Stores chat sessions in **local files**

When you open the Agent Manager application, chat data may be copied over or merged between these two storage systems. This can cause:
- Duplicate or mixed-up conversations
- Unexpected state changes in your Cascade sessions
- Confusion about which chats belong to which context

**Recommendation:** When using Ralph, avoid opening the Agent Manager application to prevent potential data conflicts or unexpected behavior with your Cascade sessions.
