# LangChain Python Package Reference

> Source: https://reference.langchain.com/python/langchain

## Overview

The main `langchain` package is the primary entrypoint for building LLM applications. It provides implementations for agents, models, tools, embeddings, and middleware.

## Package Structure

```
langchain/
├── mcp/                    # Model Context Protocol adapters
├── agents/                 # Agent implementations and middleware
├── embeddings/             # Embedding models and factories
├── chat_models/            # Chat model interfaces and factories
└── ...
```

---

## Classes

### MCP (Model Context Protocol)

| Class | Module | Description |
|-------|--------|-------------|
| `MCPToolArtifact` | `langchain.mcp.tools` | Artifact returned from MCP tool calls |
| `MCPElicitationFormRequest` | `langchain.mcp.elicitation` | Form-based elicitation request |
| `MCPElicitationUrlRequest` | `langchain.mcp.elicitation` | URL-based elicitation request |
| `MCPElicitationInterrupt` | `langchain.mcp.elicitation` | Elicitation interrupt signal |
| `MCPElicitationAccept` | `langchain.mcp.elicitation` | Accept elicitation response |
| `MCPElicitationDecline` | `langchain.mcp.elicitation` | Decline elicitation response |
| `MCPElicitationCancel` | `langchain.mcp.elicitation` | Cancel elicitation response |
| `MCPElicitationResume` | `langchain.mcp.elicitation` | Resume elicitation response |
| `MCPAdapter` | `langchain.mcp.adapter` | Adapter for MCP tools |

### Agents - Structured Output

| Class | Module | Description |
|-------|--------|-------------|
| `StructuredOutputError` | `langchain.agents.structured_output` | Base error for structured output |
| `MultipleStructuredOutputsError` | `langchain.agents.structured_output` | Multiple outputs error |
| `StructuredOutputValidationError` | `langchain.agents.structured_output` | Validation error for structured output |
| `ToolStrategy` | `langchain.agents.structured_output` | Strategy for tool selection |
| `ProviderStrategy` | `langchain.agents.structured_output` | Provider-specific strategy |
| `OutputToolBinding` | `langchain.agents.structured_output` | Binding between output and tools |
| `ProviderStrategyBinding` | `langchain.agents.structured_output` | Provider strategy binding |
| `AutoStrategy` | `langchain.agents.structured_output` | Automatic strategy selection |

### Agents - Middleware

| Class | Module | Description |
|-------|--------|-------------|
| `ProviderToolSearchMiddleware` | `langchain.agents.middleware.provider_tool_search` | Search tools from provider |
| `ContextEdit` | `langchain.agents.middleware.context_editing` | Context editing operation |
| `ClearToolUsesEdit` | `langchain.agents.middleware.context_editing` | Clear tool uses edit |
| `ContextEditingMiddleware` | `langchain.agents.middleware.context_editing` | Middleware for context editing |
| `BaseExecutionPolicy` | `langchain.agents.middleware._execution` | Base execution policy |
| `HostExecutionPolicy` | `langchain.agents.middleware._execution` | Host execution policy |
| `CodexSandboxExecutionPolicy` | `langchain.agents.middleware._execution` | Codex sandbox policy |
| `DockerExecutionPolicy` | `langchain.agents.middleware._execution` | Docker execution policy |
| `ModelRetryMiddleware` | `langchain.agents.middleware.model_retry` | Retry failed model calls |
| `Todo` | `langchain.agents.middleware.todo` | Todo item |
| `PlanningState` | `langchain.agents.middleware.todo` | Planning state |
| `WriteTodosInput` | `langchain.agents.middleware.todo` | Input for writing todos |
| `TodoListMiddleware` | `langchain.agents.middleware.todo` | Todo list middleware |
| `LLMToolEmulator` | `langchain.agents.middleware.tool_emulator` | Emulate LLM tool calls |
| `ModelRequest` | `langchain.agents.middleware.types` | Model request type |
| `ModelResponse` | `langchain.agents.middleware.types` | Model response type |
| `ExtendedModelResponse` | `langchain.agents.middleware.types` | Extended model response |
| `OmitFromSchema` | `langchain.agents.middleware.types` | Omit fields from schema |
| `AgentState` | `langchain.agents.middleware.types` | Agent state |
| `InputAgentState` | `langchain.agents.middleware.types` | Input agent state |
| `OutputAgentState` | `langchain.agents.middleware.types` | Output agent state |
| `AgentMiddleware` | `langchain.agents.middleware.types` | Base agent middleware |
| `ModelCallLimitState` | `langchain.agents.middleware.model_call_limit` | Model call limit state |
| `ModelCallLimitExceededError` | `langchain.agents.middleware.model_call_limit` | Call limit exceeded |
| `ModelCallLimitMiddleware` | `langchain.agents.middleware.model_call_limit` | Limit model calls |
| `ToolRetryMiddleware` | `langchain.agents.middleware.tool_retry` | Retry failed tool calls |
| `ModelFallbackMiddleware` | `langchain.agents.middleware.model_fallback` | Fallback to alternate models |
| `TriggerClause` | `langchain.agents.middleware.summarization` | Summarization trigger clause |
| `SummarizationMiddleware` | `langchain.agents.middleware.summarization` | Conversation summarization |
| `LLMToolSelectorMiddleware` | `langchain.agents.middleware.tool_selection` | LLM-based tool selection |
| `PIIMatch` | `langchain.agents.middleware._redaction` | PII match result |
| `PIIDetectionError` | `langchain.agents.middleware._redaction` | PII detection error |
| `RedactionRule` | `langchain.agents.middleware._redaction` | Redaction rule |
| `ResolvedRedactionRule` | `langchain.agents.middleware._redaction` | Resolved redaction rule |
| `ToolCallLimitState` | `langchain.agents.middleware.tool_call_limit` | Tool call limit state |
| `ToolCallLimitExceededError` | `langchain.agents.middleware.tool_call_limit` | Tool call limit exceeded |
| `ToolCallLimitMiddleware` | `langchain.agents.middleware.tool_call_limit` | Limit tool calls |
| `FilesystemFileSearchMiddleware` | `langchain.agents.middleware.file_search` | Filesystem search middleware |
| `Action` | `langchain.agents.middleware.human_in_the_loop` | Human action |
| `ActionRequest` | `langchain.agents.middleware.human_in_the_loop` | Action request |
| `ReviewConfig` | `langchain.agents.middleware.human_in_the_loop` | Review configuration |
| `HITLRequest` | `langchain.agents.middleware.human_in_the_loop` | Human-in-the-loop request |
| `ApproveDecision` | `langchain.agents.middleware.human_in_the_loop` | Approve decision |
| `EditDecision` | `langchain.agents.middleware.human_in_the_loop` | Edit decision |
| `RejectDecision` | `langchain.agents.middleware.human_in_the_loop` | Reject decision |
| `RespondDecision` | `langchain.agents.middleware.human_in_the_loop` | Respond decision |
| `HITLResponse` | `langchain.agents.middleware.human_in_the_loop` | HITL response |
| `InterruptOnConfig` | `langchain.agents.middleware.human_in_the_loop` | Interrupt configuration |
| `HumanInTheLoopMiddleware` | `langchain.agents.middleware.human_in_the_loop` | Human-in-the-loop middleware |
| `PIIMiddleware` | `langchain.agents.middleware.pii` | PII redaction middleware |
| `ToolErrorMiddleware` | `langchain.agents.middleware.tool_error` | Tool error handling |
| `InternalCallTransformer` | `langchain.agents.middleware.internal_call_transformer` | Transform internal calls |
| `ShellToolState` | `langchain.agents.middleware.shell_tool` | Shell tool state |
| `CommandExecutionResult` | `langchain.agents.middleware.shell_tool` | Command execution result |
| `ShellSession` | `langchain.agents.middleware.shell_tool` | Shell session |
| `ShellToolMiddleware` | `langchain.agents.middleware.shell_tool` | Shell tool middleware |

### Agents - Subagent Transformer

| Class | Module | Description |
|-------|--------|-------------|
| `SubagentRunStream` | `langchain.agents._subagent_transformer` | Subagent run stream |
| `AsyncSubagentRunStream` | `langchain.agents._subagent_transformer` | Async subagent run stream |
| `SubagentTransformer` | `langchain.agents._subagent_transformer` | Subagent transformer |

---

## Functions

### MCP Functions

| Function | Module | Description |
|----------|--------|-------------|
| `as_langchain_tool()` | `langchain.mcp.tools` | Convert MCP tool to LangChain tool |
| `armed()` | `langchain.mcp.adapter` | Arm MCP adapter |

### Embeddings

| Function | Module | Description |
|----------|--------|-------------|
| `init_embeddings()` | `langchain.embeddings.base` | Initialize embedding model |

### Agent Factory

| Function | Module | Description |
|----------|--------|-------------|
| `create_agent()` | `langchain.agents.factory` | Create an agent with middleware |

### Middleware Functions

| Function | Module | Description |
|----------|--------|-------------|
| `configure_trace_policy()` | `langchain.agents.middleware._trace_policy` | Configure trace policy |
| `write_todos()` | `langchain.agents.middleware.todo` | Write todo items |
| `hook_config()` | `langchain.agents.middleware.types` | Configure hooks |
| `before_model()` | `langchain.agents.middleware.types` | Before model hook |
| `after_model()` | `langchain.agents.middleware.types` | After model hook |
| `before_agent()` | `langchain.agents.middleware.types` | Before agent hook |
| `after_agent()` | `langchain.agents.middleware.types` | After agent hook |
| `dynamic_prompt()` | `langchain.agents.middleware.types` | Dynamic prompt hook |
| `wrap_model_call()` | `langchain.agents.middleware.types` | Wrap model call |
| `wrap_tool_call()` | `langchain.agents.middleware.types` | Wrap tool call |
| `validate_retry_params()` | `langchain.agents.middleware._retry` | Validate retry params |
| `default_retry_on()` | `langchain.agents.middleware._retry` | Default retry condition |
| `should_retry_exception()` | `langchain.agents.middleware._retry` | Check if should retry |
| `calculate_delay()` | `langchain.agents.middleware._retry` | Calculate retry delay |
| `detect_email()` | `langchain.agents.middleware._redaction` | Detect email addresses |
| `detect_credit_card()` | `langchain.agents.middleware._redaction` | Detect credit cards |
| `detect_ip()` | `langchain.agents.middleware._redaction` | Detect IP addresses |
| `detect_mac_address()` | `langchain.agents.middleware._redaction` | Detect MAC addresses |
| `detect_url()` | `langchain.agents.middleware._redaction` | Detect URLs |
| `apply_strategy()` | `langchain.agents.middleware._redaction` | Apply redaction strategy |
| `resolve_detector()` | `langchain.agents.middleware._redaction` | Resolve detector |
| `glob_search()` | `langchain.agents.middleware.file_search` | Glob file search |
| `grep_search()` | `langchain.agents.middleware.file_search` | Grep file search |
| `internal_call_metadata()` | `langchain.agents.middleware.internal_call_transformer` | Internal call metadata |
| `shell_tool()` | `langchain.agents.middleware.shell_tool` | Shell tool function |

### Chat Models

| Function | Module | Description |
|----------|--------|-------------|
| `init_chat_model()` | `langchain.chat_models.base` | Initialize chat model from provider |

---

## Types

| Type | Module | Description |
|------|--------|-------------|
| `ToolMessageContentBlock` | `langchain.mcp.tools` | Tool message content block |
| `MCPFormContent` | `langchain.mcp.elicitation` | MCP form content |
| `MCPElicitationRequest` | `langchain.mcp.elicitation` | MCP elicitation request |
| `MCPElicitationResponse` | `langchain.mcp.elicitation` | MCP elicitation response |
| `MCPAdapterTarget` | `langchain.mcp.adapter` | MCP adapter target |
| `ResponseFormat` | `langchain.agents.structured_output` | Response format |
| `ToolIdentifier` | `langchain.agents.middleware.provider_tool_search` | Tool identifier |
| `ModelCallResult` | `langchain.agents.middleware.types` | Model call result |
| `RetryOn` | `langchain.agents.middleware._retry` | Retry condition |
| `OnFailure` | `langchain.agents.middleware._retry` | Failure handler |
| `ContextSize` | `langchain.agents.middleware.summarization` | Context size config |
| `OnParsingFailure` | `langchain.agents.middleware.tool_selection` | Parsing failure handler |
| `Decision` | `langchain.agents.middleware.human_in_the_loop` | HITL decision |

---

## Quick Start

```python
from langchain import create_agent
from langchain.chat_models import init_chat_model

# Initialize a chat model
model = init_chat_model("gpt-4o-mini", model_provider="openai")

# Create an agent with middleware
agent = create_agent(
    model=model,
    tools=[...],
    middleware=[
        # Add middleware as needed
    ]
)
```

---

## Related Packages

- [langchain-core](../langchain-core/README.md) - Core abstractions
- [langgraph](../langgraph/README.md) - Graph-based orchestration
- [deepagents](../deepagents/README.md) - Deep agents framework
- [langchain-postgres](../vectorstores/postgres.md) - PostgreSQL vector store