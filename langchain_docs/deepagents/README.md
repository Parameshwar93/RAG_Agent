# Deep Agents Python Package Reference

> Source: https://reference.langchain.com/python/deepagents

## Overview

Deep Agents is a complete agent harness for complex, long-running tasks with planning, subagents, a virtual filesystem, and long-term memory built in. It provides a higher-level abstraction over LangGraph for building sophisticated agents.

## Package Structure

```
deepagents/
├── backends/           # Execution backends (sandbox, filesystem, etc.)
├── graph/              # Deep agent graph construction
├── middleware/         # Agent middleware (subagents, memory, skills, etc.)
├── profiles/           # Model/provider profiles and harness configs
└── _api/               # API utilities
```

---

## Core Classes

### Graph & State

| Class | Module | Description |
|-------|--------|-------------|
| `DeepAgentState` | `deepagents.graph` | Deep agent state schema |

### Subagents

| Class | Module | Description |
|-------|--------|-------------|
| `SubAgent` | `deepagents.middleware.subagents` | Subagent definition |
| `CompiledSubAgent` | `deepagents.middleware.subagents` | Compiled subagent |
| `TaskToolSchema` | `deepagents.middleware.subagents` | Task tool schema |
| `SubAgentMiddleware` | `deepagents.middleware.subagents` | Subagent middleware |
| `AsyncSubAgent` | `deepagents.middleware.async_subagents` | Async subagent |
| `AsyncTask` | `deepagents.middleware.async_subagents` | Async task |
| `AsyncSubAgentState` | `deepagents.middleware.async_subagents` | Async subagent state |
| `StartAsyncTaskSchema` | `deepagents.middleware.async_subagents` | Start async task schema |
| `CheckAsyncTaskSchema` | `deepagents.middleware.async_subagents` | Check async task schema |
| `UpdateAsyncTaskSchema` | `deepagents.middleware.async_subagents` | Update async task schema |
| `CancelAsyncTaskSchema` | `deepagents.middleware.async_subagents` | Cancel async task schema |
| `ListAsyncTasksSchema` | `deepagents.middleware.async_subagents` | List async tasks schema |
| `AsyncSubAgentMiddleware` | `deepagents.middleware.async_subagents` | Async subagent middleware |

### Filesystem

| Class | Module | Description |
|-------|--------|-------------|
| `FilesystemPermission` | `deepagents.middleware.filesystem` | Filesystem permissions |
| `FilesystemState` | `deepagents.middleware.filesystem` | Filesystem state |
| `LsSchema` | `deepagents.middleware.filesystem` | List directory schema |
| `ReadFileSchema` | `deepagents.middleware.filesystem` | Read file schema |
| `ReadVideoFileSchema` | `deepagents.middleware.filesystem` | Read video file schema |
| `WriteFileSchema` | `deepagents.middleware.filesystem` | Write file schema |
| `EditFileSchema` | `deepagents.middleware.filesystem` | Edit file schema |
| `DeleteSchema` | `deepagents.middleware.filesystem` | Delete file schema |
| `GlobSchema` | `deepagents.middleware.filesystem` | Glob pattern schema |
| `GrepSchema` | `deepagents.middleware.filesystem` | Grep search schema |
| `ExecuteSchema` | `deepagents.middleware.filesystem` | Execute command schema |
| `FilesystemMiddleware` | `deepagents.middleware.filesystem` | Filesystem middleware |

### Memory & Summarization

| Class | Module | Description |
|-------|--------|-------------|
| `MemoryState` | `deepagents.middleware.memory` | Memory state |
| `MemoryStateUpdate` | `deepagents.middleware.memory` | Memory state update |
| `MemoryMiddleware` | `deepagents.middleware.memory` | Memory middleware |
| `SummarizationState` | `deepagents.middleware.summarization` | Summarization state |
| `SummarizationDefaults` | `deepagents.middleware.summarization` | Summarization defaults |
| `SummarizationEvent` | `deepagents.middleware.summarization` | Summarization event |
| `TriggerClause` | `deepagents.middleware.summarization` | Trigger clause |
| `TruncateArgsSettings` | `deepagents.middleware.summarization` | Truncate args settings |
| `SummarizationToolMiddleware` | `deepagents.middleware.summarization` | Summarization tool middleware |
| `CompactConversationSchema` | `deepagents.middleware.summarization` | Compact conversation schema |

### Skills

| Class | Module | Description |
|-------|--------|-------------|
| `SkillMetadata` | `deepagents.middleware.skills` | Skill metadata |
| `SkillsState` | `deepagents.middleware.skills` | Skills state |
| `SkillsStateUpdate` | `deepagents.middleware.skills` | Skills state update |
| `SkillsMiddleware` | `deepagents.middleware.skills` | Skills middleware |

### Rubric / Evaluation

| Class | Module | Description |
|-------|--------|-------------|
| `CriterionPass` | `deepagents.middleware.rubric` | Criterion pass |
| `CriterionFail` | `deepagents.middleware.rubric` | Criterion fail |
| `RubricEvaluation` | `deepagents.middleware.rubric` | Rubric evaluation |
| `RubricState` | `deepagents.middleware.rubric` | Rubric state |
| `GraderResponse` | `deepagents.middleware.rubric` | Grader response |
| `RubricMiddleware` | `deepagents.middleware.rubric` | Rubric middleware |

### Other Middleware

| Class | Module | Description |
|-------|--------|-------------|
| `UnsupportedContentMiddleware` | `deepagents.middleware.unsupported_content` | Unsupported content handling |
| `PatchToolCallsMiddleware` | `deepagents.middleware.patch_tool_calls` | Patch tool calls |
| `VideoExtractionError` | `deepagents.middleware._video` | Video extraction error |
| `ContentPreview` | `deepagents.middleware._message_eviction` | Content preview |

### Backends

| Class | Module | Description |
|-------|--------|-------------|
| `LangSmithSandbox` | `deepagents.backends.langsmith` | LangSmith sandbox backend |
| `LocalShellBackend` | `deepagents.backends.local_shell` | Local shell backend |
| `FilesystemBackend` | `deepagents.backends.filesystem` | Filesystem backend |
| `ContextHubBackend` | `deepagents.backends.context_hub` | Context hub backend |
| `StateBackend` | `deepagents.backends.state` | State backend |
| `BaseSandbox` | `deepagents.backends.sandbox` | Base sandbox |
| `CompositeBackend` | `deepagents.backends.composite` | Composite backend |
| `StoreBackend` | `deepagents.backends.store` | Store backend |

### Backend Protocol

| Class | Module | Description |
|-------|--------|-------------|
| `BackendProtocol` | `deepagents.backends.protocol` | Backend protocol |
| `SandboxBackendProtocol` | `deepagents.backends.protocol` | Sandbox backend protocol |
| `FileInfo` | `deepagents.backends.protocol` | File info |
| `FileData` | `deepagents.backends.protocol` | File data |
| `ReadResult` | `deepagents.backends.protocol` | Read result |
| `WriteResult` | `deepagents.backends.protocol` | Write result |
| `EditResult` | `deepagents.backends.protocol` | Edit result |
| `DeleteResult` | `deepagents.backends.protocol` | Delete result |
| `LsResult` | `deepagents.backends.protocol` | List result |
| `GrepResult` | `deepagents.backends.protocol` | Grep result |
| `GlobResult` | `deepagents.backends.protocol` | Glob result |
| `ExecuteResponse` | `deepagents.backends.protocol` | Execute response |
| `ExecuteArtifact` | `deepagents.backends.protocol` | Execute artifact |
| `ExecuteOffloadResult` | `deepagents.backends.protocol` | Execute offload result |
| `FileDownloadResponse` | `deepagents.backends.protocol` | File download response |
| `FileUploadResponse` | `deepagents.backends.protocol` | File upload response |
| `ContextLine` | `deepagents.backends.protocol` | Context line |
| `GrepMatch` | `deepagents.backends.protocol` | Grep match |

### Provider Profiles

| Class | Module | Description |
|-------|--------|-------------|
| `ProviderProfile` | `deepagents.profiles.provider.provider_profiles` | Provider profile |
| `GeneralPurposeSubagentProfile` | `deepagents.profiles.harness.harness_profiles` | General purpose subagent profile |
| `HarnessProfileConfig` | `deepagents.profiles.harness.harness_profiles` | Harness profile config |
| `HarnessProfile` | `deepagents.profiles.harness.harness_profiles` | Harness profile |

### Nemotron-Specific (NVIDIA)

| Class | Module | Description |
|-------|--------|-------------|
| `NemotronToolCallShim` | `deepagents.profiles.harness._nvidia_nemotron_3_ultra` | Tool call shim |
| `ReadFileContinuationNoticeMiddleware` | `deepagents.profiles.harness._nvidia_nemotron_3_ultra` | Read file continuation middleware |
| `ModelRateLimitRetryMiddleware` | `deepagents.profiles.harness._nvidia_nemotron_3_ultra` | Rate limit retry middleware |
| `ChatNVIDIAMessageCompatibilityMiddleware` | `deepagents.profiles.harness._nvidia_nemotron_3_ultra` | NVIDIA message compatibility |
| `NemotronReasoningTagCleanupMiddleware` | `deepagents.profiles.harness._nvidia_nemotron_3_ultra` | Reasoning tag cleanup |
| `NemotronTextToolCallParser` | `deepagents.profiles.harness._nvidia_nemotron_3_ultra` | Text tool call parser |
| `NemotronProgressBudgetMiddleware` | `deepagents.profiles.harness._nvidia_nemotron_3_ultra` | Progress budget middleware |
| `NemotronPolicyNudgeState` | `deepagents.profiles.harness._nvidia_nemotron_3_ultra` | Policy nudge state |
| `NemotronPolicyNudgeMiddleware` | `deepagents.profiles.harness._nvidia_nemotron_3_ultra` | Policy nudge middleware |
| `FollowupDisciplineState` | `deepagents.profiles.harness._nvidia_nemotron_3_ultra` | Followup discipline state |
| `FollowupDisciplineMiddleware` | `deepagents.profiles.harness._nvidia_nemotron_3_ultra` | Followup discipline middleware |
| `EntityResolutionGuardState` | `deepagents.profiles.harness._nvidia_nemotron_3_ultra` | Entity resolution guard state |
| `EntityResolutionGuardMiddleware` | `deepagents.profiles.harness._nvidia_nemotron_3_ultra` | Entity resolution guard middleware |
| `FinalAnswerGuardState` | `deepagents.profiles.harness._nvidia_nemotron_3_ultra` | Final answer guard state |
| `FinalAnswerGuardMiddleware` | `deepagents.profiles.harness._nvidia_nemotron_3_ultra` | Final answer guard middleware |

---

## Key Functions

### Agent Creation

| Function | Module | Description |
|----------|--------|-------------|
| `create_deep_agent()` | `deepagents.graph` | Create a deep agent |
| `get_default_model()` | `deepagents.graph` | Get default model |

### Model Resolution

| Function | Module | Description |
|----------|--------|-------------|
| `resolve_model()` | `deepagents._models` | Resolve model string |
| `get_model_identifier()` | `deepagents._models` | Get model identifier |
| `get_model_provider()` | `deepagents._models` | Get model provider |
| `is_bedrock_model()` | `deepagents._models` | Check if Bedrock model |
| `model_matches_spec()` | `deepagents._models` | Match model spec |
| `get_default_model()` | `deepagents._models` | Get default model |

### Provider Profiles

| Function | Module | Description |
|----------|--------|-------------|
| `register_provider_profile()` | `deepagents.profiles.provider.provider_profiles` | Register provider profile |
| `get_provider_profile()` | `deepagents.profiles.provider.provider_profiles` | Get provider profile |
| `apply_provider_profile()` | `deepagents.profiles.provider.provider_profiles` | Apply provider profile |
| `validate_profile_key()` | `deepagents.profiles._keys` | Validate profile key |

### Harness Profiles

| Function | Module | Description |
|----------|--------|-------------|
| `register_harness_profile()` | `deepagents.profiles.harness.harness_profiles` | Register harness profile |
| `register()` (various) | `deepagents.profiles.provider._*` | Register provider |
| `register()` (various) | `deepagents.profiles.harness._*` | Register harness |

### Subagents

| Function | Module | Description |
|----------|--------|-------------|
| `create_sub_agent()` | `deepagents.middleware.subagents` | Create subagent |
| `private_state_field_names()` | `deepagents.middleware._state` | Private state fields |

### Filesystem

| Function | Module | Description |
|----------|--------|-------------|
| `supports_execution()` | `deepagents.middleware.filesystem` | Check execution support |

### Summarization

| Function | Module | Description |
|----------|--------|-------------|
| `compute_summarization_defaults()` | `deepagents.middleware.summarization` | Compute summarization defaults |
| `create_summarization_middleware()` | `deepagents.middleware.summarization` | Create summarization middleware |
| `create_summarization_tool_middleware()` | `deepagents.middleware.summarization` | Create summarization tool middleware |

### Video

| Function | Module | Description |
|----------|--------|-------------|
| `video_dependencies_available()` | `deepagents.middleware._video` | Check video deps |
| `extract_video_frames()` | `deepagents.middleware._video` | Extract video frames |

### Backend Utilities

| Function | Module | Description |
|----------|--------|-------------|
| `compile_grep_include_glob()` | `deepagents.backends.utils` | Compile grep glob |
| `sanitize_tool_call_id()` | `deepagents.backends.utils` | Sanitize tool call ID |
| `format_content_with_line_numbers()` | `deepagents.backends.utils` | Format with line numbers |
| `check_empty_content()` | `deepagents.backends.utils` | Check empty content |
| `file_data_to_string()` | `deepagents.backends.utils` | File data to string |
| `create_file_data()` | `deepagents.backends.utils` | Create file data |
| `update_file_data()` | `deepagents.backends.utils` | Update file data |
| `normalize_read_bounds()` | `deepagents.backends.utils` | Normalize read bounds |
| `slice_read_response()` | `deepagents.backends.utils` | Slice read response |
| `perform_string_replacement()` | `deepagents.backends.utils` | String replacement |
| `truncate_if_too_long()` | `deepagents.backends.utils` | Truncate if too long |
| `to_posix_path()` | `deepagents.backends.utils` | Convert to POSIX path |
| `validate_path()` | `deepagents.backends.utils` | Validate path |
| `grep_matches_from_files()` | `deepagents.backends.utils` | Grep matches |
| `build_grep_results_dict()` | `deepagents.backends.utils` | Build grep results |
| `format_grep_matches()` | `deepagents.backends.utils` | Format grep matches |
| `regex_literal_hint()` | `deepagents.backends.utils` | Regex literal hint |
| `execute_accepts_timeout()` | `deepagents.backends.protocol` | Check timeout support |

### Utilities

| Function | Module | Description |
|----------|--------|-------------|
| `append_to_system_message()` | `deepagents.middleware._utils` | Append to system message |
| `append_prompt_caching_middleware()` | `deepagents.middleware._prompt_caching` | Prompt caching middleware |
| `warn_deprecated()` | `deepagents._api.deprecation` | Warn deprecated |
| `reset_deprecation_dedupe()` | `deepagents._api.deprecation` | Reset deprecation dedupe |

---

## Types

| Type | Module | Description |
|------|--------|-------------|
| `SkillSource` | `deepagents.middleware.skills` | Skill source |
| `RubricResult` | `deepagents.middleware.rubric` | Rubric result |
| `GlobBackendResult` | `deepagents.backends.composite` | Glob backend result |

---

## Quick Start

```python
from deepagents import create_deep_agent
from langchain.chat_models import init_chat_model

# Initialize model
model = init_chat_model("gpt-4o", model_provider="openai")

# Create deep agent with tools and subagents
agent = create_deep_agent(
    model=model,
    tools=[...],  # Your tools
    instructions="You are a helpful assistant...",
    subagents=[...],  # Optional subagents
)

# Run agent
result = agent.invoke({"messages": [("user", "Research topic X and write a report")]})
```

---

## With Filesystem & Memory

```python
from deepagents import create_deep_agent
from deepagents.middleware.filesystem import FilesystemMiddleware
from deepagents.middleware.memory import MemoryMiddleware

agent = create_deep_agent(
    model=model,
    tools=[...],
    middleware=[
        FilesystemMiddleware(root_dir="/workspace"),
        MemoryMiddleware(),
        # Add other middleware
    ]
)
```

---

## With Subagents

```python
from deepagents import create_deep_agent
from deepagents.middleware.subagents import create_sub_agent, SubAgentMiddleware

# Create subagent
researcher = create_sub_agent(
    name="researcher",
    description="Researches topics",
    model=model,
    tools=[search_tool],
)

agent = create_deep_agent(
    model=model,
    tools=[...],
    subagents=[researcher],
    middleware=[SubAgentMiddleware()]
)
```

---

## Related Packages

- [langchain](../langchain/README.md) - Core LangChain package
- [langgraph](../langgraph/README.md) - Graph orchestration (underlying engine)
- [langchain-postgres](../vectorstores/postgres.md) - PostgreSQL vector store for memory