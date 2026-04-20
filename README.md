# opencode-rules
Env repository to hold and track agent rules for [opencode](https://opencode.ai).

## Available Rule Files

| File | Purpose |
|---|---|
| [`style.md`](./style.md) | High-level developer best practices for writing quality business code |
| [`sparse.md`](./sparse.md) | Token-efficiency instructions: sub-agents, question tool usage, concise output |

## Usage

Reference a rule file in your `opencode.json` config:

```json
{
  "$schema": "https://opencode.ai/config.json",
  "instructions": [
    "https://raw.githubusercontent.com/NoxelS/opencode-rules/main/style.md",
    "https://raw.githubusercontent.com/NoxelS/opencode-rules/main/sparse.md"
  ]
}
```

You can include one or both files depending on your needs.
