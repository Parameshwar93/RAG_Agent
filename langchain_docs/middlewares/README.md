# LangChain Middlewares Reference

> Source: https://reference.langchain.com/python/langchain (agents/middleware)

## Overview

Middlewares in LangChain provide a powerful way to intercept and modify agent behavior at various points in the execution pipeline. They can be used for logging, retry logic, human-in-the-loop, tool selection, context editing, and more.

## Middleware Categories

---

## 1. Model & Execution Middleware

### ModelRetryMiddleware
- **Module**: `langchain.agents.middleware.model_retry`
- **Purpose**: Automatically retry failed model calls with configurable backoff
- **Key Features**: Exponential backoff, configurable retry conditions, max attempts

### ModelFallbackMiddleware
- **Module**: `langchain.agents.middleware.model_fallback`
- **Purpose**: Fallback to alternate models when primary fails
- **Use Case**: High availability, cost optimization

### ModelCallLimitMiddleware
- **Module**: `langchain.agents.middleware.model_call_limit`
- **Purpose**: Limit number of model calls per agent run
- **Config**: `ModelCallLimitState`, `ModelCallLimitExceededError`

### ToolRetryMiddleware
- **Module**: `langchain.agents.middleware.tool_retry`
- **Purpose**: Retry failed tool calls
- **Config**: Retry policies per tool

---

## 2. Execution Policy Middleware

### BaseExecutionPolicy
- **Module**: `langchain.agents.middleware._execution`
- **Subclasses**:
  - `HostExecutionPolicy` - Execute on host
  - `CodexSandboxExecutionPolicy` - Execute in Codex sandbox
  - `DockerExecutionPolicy` - Execute in Docker container

---

## 3. Context & State Middleware

### ContextEditingMiddleware
- **Module**: `langchain.agents.middleware.context_editing`
- **Purpose**: Edit agent context during execution
- **Components**: `ContextEdit`, `ClearToolUsesEdit`

### SummarizationMiddleware
- **Module**: `langchain.agents.middleware.summarization`
- **Purpose**: Summarize conversation history to manage context window
- **Components**: `TriggerClause`, `ContextSize`

---

## 4. Tool Selection Middleware

### LLMToolSelectorMiddleware
- **Module**: `langchain.agents.middleware.tool_selection`
- **Purpose**: Use LLM to select appropriate tools
- **Config**: `OnParsingFailure` - handle parsing failures

### ProviderToolSearchMiddleware
- **Module**: `langchain.agents.middleware.provider_tool_search`
- **Purpose**: Search tools from provider registries
- **Types**: `ToolIdentifier`

---

## 5. Human-in-the-Loop Middleware

### HumanInTheLoopMiddleware
- **Module**: `langchain.agents.middleware.human_in_the_loop`
- **Purpose**: Enable human review/approval of agent actions
- **Components**:
  - `HITLRequest` / `HITLResponse`
  - `Action` / `ActionRequest`
  - `ReviewConfig`
  - Decision types: `ApproveDecision`, `EditDecision`, `RejectDecision`, `RespondDecision`
  - `InterruptOnConfig`

---

## 6. Safety & Compliance Middleware

### PIIMiddleware
- **Module**: `langchain.agents.middleware.pii`
- **Purpose**: Detect and redact PII from agent communications
- **Components**:
  - `PIIMatch` - PII match result
  - `PIIDetectionError` - Detection errors
  - `RedactionRule` / `ResolvedRedactionRule` - Redaction rules
  - Detection functions: `detect_email`, `detect_credit_card`, `detect_ip`, `detect_mac_address`, `detect_url`
  - `apply_strategy`, `resolve_detector`

### ToolErrorMiddleware
- **Module**: `langchain.agents.middleware.tool_error`
- **Purpose**: Handle tool execution errors gracefully

---

## 7. Planning & Task Management

### TodoListMiddleware
- **Module**: `langchain.agents.middleware.todo`
- **Purpose**: Manage todo lists during agent execution
- **Components**: `Todo`, `PlanningState`, `WriteTodosInput`
- **Function**: `write_todos()`

---

## 8. Tool Call Limiting

### ToolCallLimitMiddleware
- **Module**: `langchain.agents.middleware.tool_call_limit`
- **Purpose**: Limit number of tool calls
- **Components**: `ToolCallLimitState`, `ToolCallLimitExceededError`

---

## 9. File Search Middleware

### FilesystemFileSearchMiddleware
- **Module**: `langchain.agents.middleware.file_search`
- **Purpose**: Search filesystem from agent
- **Functions**: `glob_search()`, `grep_search()`

---

## 10. Shell Tool Middleware

### ShellToolMiddleware
- **Module**: `langchain.agents.middleware.shell_tool`
- **Purpose**: Provide persistent shell access to agents
- **Components**: `ShellToolState`, `CommandExecutionResult`, `ShellSession`
- **Function**: `shell_tool()`

---

## 11. Internal & Utility Middleware

### InternalCallTransformer
- **Module**: `langchain.agents.middleware.internal_call_transformer`
- **Purpose**: Transform internal calls
- **Function**: `internal_call_metadata()`

### LLMToolEmulator
- **Module**: `langchain.agents.middleware.tool_emulator`
- **Purpose**: Emulate LLM tool calls for testing

---

## Middleware Hook Functions

| Function | Module | Description |
|----------|--------|-------------|
| `hook_config()` | `langchain.agents.middleware.types` | Configure middleware hooks |
| `before_model()` | `langchain.agents.middleware.types` | Pre-model call hook |
| `after_model()` | `langchain.agents.middleware.types` | Post-model call hook |
| `before_agent()` | `langchain.agents.middleware.types` | Pre-agent execution hook |
| `after_agent()` | `langchain.agents.middleware.types` | Post-agent execution hook |
| `dynamic_prompt()` | `langchain.agents.middleware.types` | Dynamic prompt generation |
| `wrap_model_call()` | `langchain.agents.middleware.types` | Wrap model call |
| `wrap_tool_call()` | `langchain.agents.middleware.types` | Wrap tool call |

---

## Retry Utilities

| Function | Module | Description |
|----------|--------|-------------|
| `validate_retry_params()` | `langchain.agents.middleware._retry` | Validate retry configuration |
| `default_retry_on()` | `langchain.agents.middleware._retry` | Default retry condition |
| `should_retry_exception()` | `langchain.agents.middleware._retry` | Check if exception should retry |
| `calculate_delay()` | `langchain.agents.middleware._retry` | Calculate backoff delay |

---

## Trace Policy

### configure_trace_policy()
- **Module**: `langchain.agents.middleware._trace_policy`
- **Purpose**: Configure tracing for agent execution

---

## Usage Examples

### Basic Middleware Setup

```python
from langchain import create_agent
from langchain.chat_models import init_chat_model
from langchain.agents.middleware import (
    ModelRetryMiddleware,
    ToolRetryMiddleware,
    HumanInTheLoopMiddleware,
    PIIMiddleware,
)

model = init_chat_model("gpt-4o-mini", model_provider="openai")

agent = create_agent(
    model=model,
    tools=[...],
    middleware=[
        ModelRetryMiddleware(max_attempts=3),
        ToolRetryMiddleware(),
        HumanInTheLoopMiddleware(
            review_config=ReviewConfig(
                require_approval_for=["dangerous_tool"]
            )
        ),
        PIIMiddleware(),
    ]
)
```

### Custom Middleware

```python
from langchain.agents.middleware.types import AgentMiddleware, AgentState
from typing import Any

class LoggingMiddleware(AgentMiddleware):
    async def before_model(self, state: AgentState, request: Any) -> Any:
        print(f"Model call: {request}")
        return request
    
    async def after_model(self, state: AgentState, response: Any) -> Any:
        print(f"Model response: {response}")
        return response

agent = create_agent(
    model=model,
    tools=[...],
    middleware=[LoggingMiddleware()]
)
```

### Human-in-the-Loop Example

```python
from langchain.agents.middleware.human_in_the_loop import (
    HumanInTheLoopMiddleware,
    ReviewConfig,
    InterruptOnConfig,
)

hitl = HumanInTheLoopMiddleware(
    review_config=ReviewConfig(
        require_approval_for=["send_email", "delete_file"],
        interrupt_on=InterruptOnConfig(
            tool_calls=True,
            model_calls=False
        )
    )
)

# Agent will pause for human approval on specified tools
result = agent.invoke({"messages": [("user", "Send email to boss")]})
# ... human reviews and approves ...
result = agent.invoke(Command(resume="approved"), config=config)
```

### PII Redaction Example

```python
from langchain.agents.middleware.pii import PIIMiddleware
from langchain.agents.middleware._redaction import RedactionRule

pii_middleware = PIIMiddleware(
    rules=[
        RedactionRule(pattern="email", replacement="[EMAIL]"),
        RedactionRule(pattern="credit_card", replacement="[CARD]"),
        RedactionRule(pattern="ssn", replacement="[SSN]"),
    ]
)
```

---

## Middleware Types Reference

### AgentState Types
```python
from langchain.agents.middleware.types import (
    AgentState, InputAgentState, OutputAgentState,
    ModelRequest, ModelResponse, ExtendedModelResponse,
    ModelCallResult, OmitFromSchema
)
```

### Retry Types
```python
from langchain.agents.middleware._retry import (
    RetryOn, OnFailure
)
```

### Summarization Types
```python
from langchain.agents.middleware.summarization import (
    TriggerClause, ContextSize
)
```

### HITL Types
```python
from langchain.agents.middleware.human_in_the_loop import (
    Decision, Action, ActionRequest, HITLRequest, HITLResponse,
    ReviewConfig, InterruptOnConfig,
    ApproveDecision, EditDecision, RejectDecision, RespondDecision
)
```

### PII Types
```python
from langchain.agents.middleware._redaction import (
    PIIMatch, PIIDetectionError, RedactionRule, ResolvedRedactionRule
)
```

---

## Related Documentation

- [LangChain Package](../langchain/README.md) - Main package
- [Deep Agents](../deepagents/README.md) - Advanced middleware (subagents, memory, skills)
- [LangGraph](../langgraph/README.md) - Underlying graph execution