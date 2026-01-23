# Model Selection UI Behavior

## Overview
The Ralph extension's model selection and the Antigravity UI's model display may show different values, which can create confusion about which model is actually being used.

## How It Works

### Ralph Extension Sends the Model via gRPC
When you invoke Ralph, the extension **does send** your selected model to the Antigravity backend via the gRPC API:

1. User selects a model in Ralph's configuration (e.g., "Gemini 3 Pro (High)")
2. Ralph calls `sendMessage()` with the model parameter
3. The model is encoded and sent via `SendUserCascadeMessage` gRPC call

### Why the Antigravity UI May Show a Different Model
Even though Ralph sends the model in the gRPC request:

1. **Antigravity's UI displays its own state** - The Antigravity application maintains its own model selection UI, which is independent of what's sent in API calls
2. **Programmatic calls don't update the UI state** - When Ralph makes gRPC calls directly, it bypasses Antigravity's UI state management
3. **Backend may not honor the request** - The Antigravity backend has its own model routing logic and may use a different model than requested based on factors like availability, rate limits, or internal policies

### User Perception Issue
This creates a perception that the "wrong" model was used because:
- Ralph's sidebar shows the model you selected in Ralph
- Antigravity's main window shows whatever model was previously selected in its own UI
- There's no visual indication in Antigravity's UI that the API call requested a specific model

## Important Notes
- **Ralph does correctly pass the model** in every `SendUserCascadeMessage` request (logged as `Message sent (mode=X, model=Y)`)
- The **Antigravity UI is not updated** by programmatic API calls
- To verify which model was actually used, check Ralph's output channel logs
- This is a limitation of the programmatic API approach, not a bug in Ralph
