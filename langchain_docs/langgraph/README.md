# LangGraph Python Package Reference

> Source: https://reference.langchain.com/python/langgraph

## Overview

LangGraph is a low-level orchestration framework for stateful, long-running agents. It provides durable execution, streaming, memory, and human-in-the-loop capabilities built on a graph-based architecture.

## Package Structure

```
langgraph/
├── callbacks/          # Graph lifecycle callbacks
├── config/             # Configuration utilities
├── errors/             # Error types
├── func/               # Functional API (entrypoint, task)
├── graph/              # Graph definitions (StateGraph, MessageGraph)
├── managed/            # Managed values and state
├── pregel/             # Core Pregel runtime
├── runtime/            # Runtime information
├── stream/             # Streaming transformers and channels
├── types/              # Core types (Checkpointer, Interrupt, Command, etc.)
└── _internal/          # Internal utilities
```

---

## Core Classes

### Graph Definitions

| Class | Module | Description |
|-------|--------|-------------|
| `StateGraph` | `langgraph.graph.state` | Main graph builder with state |
| `CompiledStateGraph` | `langgraph.graph.state` | Compiled executable graph |
| `MessageGraph` | `langgraph.graph.message` | Graph for message-based state |
| `MessagesState` | `langgraph.graph.message` | Messages state type |
| `StateNode` | `langgraph.graph._node` | State node in graph |
| `StateNodeSpec` | `langgraph.graph._node` | Node specification |
| `BranchSpec` | `langgraph.graph._branch` | Branch specification |

### Pregel Runtime

| Class | Module | Description |
|-------|--------|-------------|
| `Pregel` | `langgraph.pregel.main` | Core Pregel execution engine |
| `PregelRunner` | `langgraph.pregel._runner` | Graph runner |
| `PregelLoop` | `langgraph.pregel._loop` | Execution loop |
| `SyncPregelLoop` | `langgraph.pregel._loop` | Synchronous loop |
| `AsyncPregelLoop` | `langgraph.pregel._loop` | Asynchronous loop |
| `PregelNode` | `langgraph.pregel._read` | Pregel node |
| `PregelProtocol` | `langgraph.pregel.protocol` | Pregel protocol |
| `StreamProtocol` | `langgraph.pregel.protocol` | Stream protocol |
| `BackgroundExecutor` | `langgraph.pregel._executor` | Background task executor |
| `AsyncBackgroundExecutor` | `langgraph.pregel._executor` | Async background executor |

### Checkpointing

| Class | Module | Description |
|-------|--------|-------------|
| `StateSnapshot` | `langgraph.types` | Snapshot of graph state |
| `CheckpointTask` | `langgraph.types` | Checkpoint task |
| `CheckpointPayload` | `langgraph.types` | Checkpoint payload |
| `Checkpointer` | `langgraph.types` | Checkpointer interface |
| `ChannelWrite` | `langgraph.pregel._write` | Channel write operation |
| `ChannelWriteEntry` | `langgraph.pregel._write` | Channel write entry |
| `ChannelWriteTupleEntry` | `langgraph.pregel._write` | Tuple channel write |

### Streaming

| Class | Module | Description |
|-------|--------|-------------|
| `GraphRunStream` | `langgraph.stream.run_stream` | Graph run stream |
| `AsyncGraphRunStream` | `langgraph.stream.run_stream` | Async graph run stream |
| `StreamChannel` | `langgraph.stream.stream_channel` | Stream channel |
| `StreamTransformer` | `langgraph.stream._types` | Stream transformer |
| `ValuesTransformer` | `langgraph.stream.transformers` | Values stream transformer |
| `UpdatesTransformer` | `langgraph.stream.transformers` | Updates stream transformer |
| `MessagesTransformer` | `langgraph.stream.transformers` | Messages stream transformer |
| `CustomTransformer` | `langgraph.stream.transformers` | Custom transformer |
| `LifecycleTransformer` | `langgraph.stream.transformers` | Lifecycle transformer |
| `CheckpointsTransformer` | `langgraph.stream.transformers` | Checkpoints transformer |
| `DebugTransformer` | `langgraph.stream.transformers` | Debug transformer |
| `TasksTransformer` | `langgraph.stream.transformers` | Tasks transformer |

### Channels

| Class | Module | Description |
|-------|--------|-------------|
| `BaseChannel` | `langgraph.channels.base` | Base channel class |
| `LastValue` | `langgraph.channels.last_value` | Last value channel |
| `LastValueAfterFinish` | `langgraph.channels.last_value` | Last value after finish |
| `EphemeralValue` | `langgraph.channels.ephemeral_value` | Ephemeral value |
| `AnyValue` | `langgraph.channels.any_value` | Any value channel |
| `Topic` | `langgraph.channels.topic` | Pub/sub topic channel |
| `UntrackedValue` | `langgraph.channels.untracked_value` | Untracked value |
| `DeltaChannel` | `langgraph.channels.delta` | Delta channel |
| `BinaryOperatorAggregate` | `langgraph.channels.binop` | Binary operator aggregate |
| `NamedBarrierValue` | `langgraph.channels.named_barrier_value` | Named barrier value |

### Types & Control

| Class | Module | Description |
|-------|--------|-------------|
| `Command` | `langgraph.types` | Control command (goto, update, etc.) |
| `Send` | `langgraph.types` | Send message to node |
| `Interrupt` | `langgraph.types` | Interrupt execution |
| `RetryPolicy` | `langgraph.types` | Retry policy |
| `TimeoutPolicy` | `langgraph.types` | Timeout policy |
| `CachePolicy` | `langgraph.types` | Cache policy |
| `TracePolicy` | `langgraph.types` | Trace policy |
| `GraphOutput` | `langgraph.types` | Graph output |
| `StateUpdate` | `langgraph.types` | State update |
| `Overwrite` | `langgraph.types` | Overwrite state |

### Errors

| Class | Module | Description |
|-------|--------|-------------|
| `GraphInterrupt` | `langgraph.errors` | Graph interrupt error |
| `NodeInterrupt` | `langgraph.errors` | Node interrupt error |
| `GraphRecursionError` | `langgraph.errors` | Recursion limit exceeded |
| `InvalidUpdateError` | `langgraph.errors` | Invalid state update |
| `GraphBubbleUp` | `langgraph.errors` | Bubble up error |
| `GraphDrained` | `langgraph.errors` | Graph drained error |
| `EmptyInputError` | `langgraph.errors` | Empty input error |
| `TaskNotFound` | `langgraph.errors` | Task not found |
| `NodeError` | `langgraph.errors` | Node execution error |
| `NodeCancelledError` | `langgraph.errors` | Node cancelled |
| `NodeTimeoutError` | `langgraph.errors` | Node timeout |

### Callbacks

| Class | Module | Description |
|-------|--------|-------------|
| `GraphCallbackHandler` | `langgraph.callbacks` | Graph callback handler |
| `GraphInterruptEvent` | `langgraph.callbacks` | Interrupt event |
| `GraphResumeEvent` | `langgraph.callbacks` | Resume event |
| `GraphLifecycleEvent` | `langgraph.callbacks` | Lifecycle event |
| `GraphLifecycleStatus` | `langgraph.callbacks` | Lifecycle status |

---

## Key Functions

### Graph Construction

| Function | Module | Description |
|----------|--------|-------------|
| `add_messages()` | `langgraph.graph.message` | Add messages reducer |
| `push_message()` | `langgraph.graph.message` | Push message to state |
| `add_edge()` | `langgraph.pregel._draw` | Add edge to graph |
| `draw_graph()` | `langgraph.pregel._draw` | Draw graph visualization |

### Functional API

| Function | Module | Description |
|----------|--------|-------------|
| `entrypoint()` | `langgraph.func` | Define graph entrypoint |
| `final()` | `langgraph.func.entrypoint` | Final node decorator |
| `task()` | `langgraph.func` | Define task node |

### Configuration

| Function | Module | Description |
|----------|--------|-------------|
| `get_config()` | `langgraph.config` | Get current config |
| `get_store()` | `langgraph.config` | Get store |
| `get_stream_writer()` | `langgraph.config` | Get stream writer |
| `get_runtime()` | `langgraph.runtime` | Get runtime info |
| `ensure_config()` | `langgraph._internal._config` | Ensure config |
| `merge_configs()` | `langgraph._internal._config` | Merge configurations |
| `patch_config()` | `langgraph._internal._config` | Patch config |
| `patch_checkpoint_map()` | `langgraph._internal._config` | Patch checkpoint map |

### Checkpointing

| Function | Module | Description |
|----------|--------|-------------|
| `create_checkpoint()` | `langgraph.pregel._checkpoint` | Create checkpoint |
| `copy_checkpoint()` | `langgraph.pregel._checkpoint` | Copy checkpoint |
| `empty_checkpoint()` | `langgraph.pregel._checkpoint` | Empty checkpoint |
| `channels_from_checkpoint()` | `langgraph.pregel._checkpoint` | Channels from checkpoint |
| `achannels_from_checkpoint()` | `langgraph.pregel._checkpoint` | Async channels from checkpoint |
| `get_delta_channels_from_all_channels()` | `langgraph.pregel._checkpoint` | Get delta channels |
| `get_updated_channels_from_tasks()` | `langgraph.pregel._checkpoint` | Updated channels from tasks |
| `create_metadata_for_update_state_api()` | `langgraph.pregel._checkpoint` | Metadata for update state |

### Retry & Execution

| Function | Module | Description |
|----------|--------|-------------|
| `run_with_retry()` | `langgraph.pregel._retry` | Run with retry |
| `arun_with_retry()` | `langgraph.pregel._retry` | Async run with retry |
| `default_retry_on()` | `langgraph._internal._retry` | Default retry condition |
| `apply_writes()` | `langgraph.pregel._algo` | Apply writes to channels |
| `prepare_next_tasks()` | `langgraph.pregel._algo` | Prepare next tasks |
| `prepare_single_task()` | `langgraph.pregel._algo` | Prepare single task |
| `should_interrupt()` | `langgraph.pregel._algo` | Check interrupt |
| `increment()` | `langgraph.pregel._algo` | Increment counter |

### Serialization

| Function | Module | Description |
|----------|--------|-------------|
| `apply_checkpointer_allowlist()` | `langgraph._internal._serde` | Apply allowlist |
| `curated_core_allowlist()` | `langgraph._internal._serde` | Core allowlist |
| `build_serde_allowlist()` | `langgraph._internal._serde` | Build allowlist |
| `collect_allowlist_from_schemas()` | `langgraph._internal._serde` | Collect from schemas |

### Validation

| Function | Module | Description |
|----------|--------|-------------|
| `validate_graph()` | `langgraph.pregel._validate` | Validate graph structure |
| `validate_keys()` | `langgraph.pregel._validate` | Validate state keys |
| `validate_timeout_supported()` | `langgraph.pregel._utils` | Validate timeout support |

### Managed Values

| Function | Module | Description |
|----------|--------|-------------|
| `is_managed_value()` | `langgraph.managed.base` | Check managed value |
| `IsLastStepManager` | `langgraph.managed.is_last_step` | Last step manager |
| `RemainingStepsManager` | `langgraph.managed.is_last_step` | Remaining steps manager |

---

## Types

| Type | Module | Description |
|------|--------|-------------|
| `StateLike` | `langgraph._internal._typing` | State type alias |
| `RunnableLike` | `langgraph._internal._runnable` | Runnable type alias |
| `AnyFuture` | `langgraph._internal._future` | Future type alias |
| `Messages` | `langgraph.graph.message` | Messages type |
| `GraphCallbacks` | `langgraph.callbacks` | Graph callbacks |
| `GraphLifecycleEvent` | `langgraph.callbacks` | Lifecycle event |

---

## Quick Start

```python
from langgraph.graph import StateGraph, MessagesState
from langgraph.func import entrypoint, task

# Define state
class MyState(MessagesState):
    custom_field: str

# Build graph
graph = StateGraph(MyState)

@entrypoint()
def my_agent(state: MyState):
    # Agent logic
    return {"messages": [...]}

# Compile and run
app = graph.compile()
result = app.invoke({"messages": [("user", "Hello")]})
```

---

## Streaming Example

```python
from langgraph.stream import ValuesTransformer

# Stream values
async for chunk in app.astream(input, stream_mode="values"):
    print(chunk)

# Stream updates
async for chunk in app.astream(input, stream_mode="updates"):
    print(chunk)

# Custom transformer
class MyTransformer(ValuesTransformer):
    def transform(self, values):
        return {"processed": values}
```

---

## Checkpointing Example

```python
from langgraph.types import Checkpointer

# With checkpointer
app = graph.compile(checkpointer=my_checkpointer)

# Run with thread
config = {"configurable": {"thread_id": "1"}}
result = app.invoke(input, config=config)

# Resume from interrupt
result = app.invoke(Command(resume="user input"), config=config)
```

---

## Related Packages

- [langchain](../langchain/README.md) - High-level agent framework
- [deepagents](../deepagents/README.md) - Deep agents with planning
- [langchain-postgres](../vectorstores/postgres.md) - PostgreSQL vector store