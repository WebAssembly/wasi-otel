# WASI OTel
A proposed [WebAssembly System Interface](https://github.com/WebAssembly/WASI) API.

### Current Phase
wasi-otel is currently in [Phase 2](https://github.com/WebAssembly/WASI/blob/main/docs/Proposals.md)

### Champions
- [Caleb Schoepp](https://github.com/calebschoepp)
- [Andrew Steurer](https://github.com/asteurer)

### Portability Criteria
WASI OTel must have at least two complete independent implementations demonstrating integration of an observability stack in a production environment.

### Introduction
WASI Otel exposes an [OpenTelemetry](https://opentelemetry.io/) interface to Wasm components to allow them to collect trace, metric, and log [signals](https://opentelemetry.io/docs/concepts/signals/).

#### An aside about the origin of the proposal
This work was originally being pursued under the banner of [WASI Observe](https://github.com/WebAssembly/wasi-observe). Throughout that process the contributors felt that it was best to pursue creating an interface that was tightly bound to the upstream OpenTelemetry APIs. Both because that is what most developers actually required and because it was more easily achievable than creating a generic interface in the near term. Hence WASI OTel was created to make a space for this work leaving WASI Observe as a space for more generic observability interface in the future.

### Goals
- Enable Wasm components to use standard [OTel SDKs](https://opentelemetry.io/docs/languages/)
- Closely mirror the upstream OTel APIs.

### Non-goals
- Provide an interface that is easily consumed directly by components.

### API walk-through
The proposal can be understood by first reading the contents of [`world.wit`](wit/world.wit), then (in any order) [`tracing.wit`](wit/tracing.wit), [`metrics.wit`](wit/metrics.wit), and [`logs.wit`](wit/logs.wit).

### Detailed design discussion
This section uses a Question/Answer format to discuss the relevant design choices.

#### `Why not just have components export telemetry over WASI HTTP?`
- Doing so doesn't allow us to tie guest spans into the automatic tracing we can produce from the component graph.
- Performance: Short lived components should just be able to buffer telemetry to the host and not have to talk over the network to a collector.

#### `Why not just keep doing this as WASI Observe?`
- Mirroring the OpenTelemetry API is a more tractable problem than designing a generic tracing/observability interface.
- We can still separately pursue WASI Observe in the future when the need is more prevalent and the design is more clear.
- Users are explicitly asking for OpenTelemetry support.

### Considered alternatives
#### Enabling observability through WASI Observe
In the future if a need for a more generic observability interface is identified, WASI Observe would be a reasonable home for it. However, for an interface that is tightly bound to the upstream OpenTelemetry APIs, WASI OTel is a better fit.

#### Enabling observability through canon builtins
It would be possible to expose observability through canon builtins. This would potentially provide for a better DX and improved performance. However, the speed of iteration would be greatly reduced and something so core to WASI should not be tied to another project like OpenTelemetry. Therefore, this is left as a potential future opportunity.

### Stakeholder Interest & Feedback
TODO before entering Phase 3.

[This should include a list of implementers who have expressed interest in implementing the proposal]
