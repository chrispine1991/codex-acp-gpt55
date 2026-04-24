# Codex ACP GPT-5.5 Adapter

This is a Windows-ready fork of [zed-industries/codex-acp](https://github.com/zed-industries/codex-acp) updated to build against OpenAI Codex Rust `rust-v0.124.0`. It was created to make the Codex ACP adapter work cleanly with GPT-5.5 inside Unreal Engine 5.6 through AgentIntegrationKit.

The adapter implements the [Agent Client Protocol](https://agentclientprotocol.com/) around Codex so ACP clients can start Codex sessions, stream responses, request permissions, run tools, and resume history.

## What changed

- Upgraded OpenAI Codex Rust crates from `rust-v0.117.0` to `rust-v0.124.0`.
- Added the split-out Codex crates now required by newer Codex APIs, including `codex-config`, `codex-models-manager`, and `codex-utils-absolute-path`.
- Updated authentication, thread management, MCP server configuration, permission prompts, guardian assessments, and event handling for Codex 0.124 protocol changes.
- Preserved Agent Client Protocol behavior expected by AgentIntegrationKit and ACP-compatible clients.
- Verified on Windows with Unreal Engine 5.6 / AgentIntegrationKit.

## Release Binary

Download `codex-acp.exe` from the latest GitHub release and place it where your ACP host expects the Codex adapter.

For AgentIntegrationKit, the user-installed adapter path is usually:

```text
C:\Users\<you>\.agentintegrationkit\agents\codex-acp\<version>\codex-acp.exe
```

For the Materia project used during testing, the bundled adapter path was:

```text
A:\NightShiftStudios\Materia\Plugins\AgentIntegrationKit\Source\ThirdParty\Adapters\codex-acp\bin\win32-x64\codex-acp.exe
```

Back up the existing executable before replacing it.

## Usage

Run the adapter directly:

```powershell
$env:OPENAI_API_KEY = "sk-..."
.\codex-acp.exe
```

Or use a Codex API key:

```powershell
$env:CODEX_API_KEY = "..."
.\codex-acp.exe
```

The adapter also supports ChatGPT subscription auth through the normal Codex login flow when the host environment can open a browser.

## Build From Source

Requirements:

- Rust toolchain from `rust-toolchain.toml`
- Windows MSVC build tools
- Network access to fetch Codex Rust crates from GitHub

Build:

```powershell
cargo build --release
```

The executable will be written to:

```text
target\release\codex-acp.exe
```

## Verification

This fork was checked with:

```powershell
cargo fmt -- --check
cargo test
cargo clippy --all-targets -- -D warnings
cargo build --release
target\release\codex-acp.exe --help
```

Current Windows release binary SHA-256:

```text
8A915033697699F2701E434D37DE63BFF33DB929DD1A7444C7F329E37A15870F
```

## Known Notes

- Custom prompt listing was removed from the newer Codex protocol path this fork targets, so this adapter currently returns no project custom prompts instead of calling the removed `Op::ListCustomPrompts`.
- This fork keeps the original ACP surface area focused on AgentIntegrationKit and Windows editor workflows.
- If OpenAI Codex changes protocol structs again, rerun the verification commands above before publishing a new binary.

## License

Apache-2.0, matching the upstream adapter.
