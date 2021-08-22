

- Distributed tracing, free of vendor lock-in, trace data, distributed context propagation via HTTP headers, AMQP message headers, Thrift fields, propagating identifiers and baggages
- Metadata like tags, logs is not propagated but transmitted async to the tracer system which assembles and constructs the full trace from distinct spans that may be injected in-band or out-of-band
- Inspired by Google’s Dapper platform
- Modelled around two fundamental types:
  - Tracer - manages spans, span context across process boundaries
  - Span - manage aggregating tags, span’s metadata, binding references to other spans, managing baggage items etc. Represents single unit of work inside a transaction. 
- Tags - contextual metadata consist of unbounded sequence of name-values pairs
- Log events - timestamped textual annotations
- Baggage - items for in-band cross-span propagation, local data is transported along the full path through the wire. Be careful on overuse. 
- Relationships:
  - ChildOf - a parent is the initiator of the child
  - FollowsFrom - models async execution where the parent span is not linked to the outcome of the child span
- Jaeger and Zipkin are tracers
- Jaeger
  - Golang based
  - client emits traces to agent which listens for inbound spans and encodes as thrift structures and submits to collector
  - agent also has sampling strategies to capture from client and route to collector
  - collector validates, transforms, stores spans in a persistent storage such as kafka for streaming, cassandra, elastic, in-memory etc
  - Query service exposes rest api end point and React based UI
  - Span document structure
    - traceID, spanID, parentSpanID
    - operationName
    - startTime, duration
    - tags: key, type, value
    - logs
    - processID, process: serviceName, tags

- zipkin
  - Java based, predates open tracing
  - client contains a reporter that records timing metrics, metadata, and routes to collector
  - Collector operates similar to above, and can support MySQL as well
  - Doesn’t support dynamic sampling rate
- Sample headers
  - b3 = bigbrotherbird original named by zipkin. x-b3 headers are propagated across service boundaries. 
  - x-request-id, x-b3-traceid, x-b3-spanid, x-b3-parentspanid, x-b3-sampled, x-b3-flags, x-ot-span-context
- Overall process
  - same spanid is used by sender and receiver, and tracer will route all of them to same node
  - Client tracer has trace context which contains traceid, parentspanid, spanid, sampling decision. This get converted into headers such as x-b3-traceid, x-b3-parentspanid, x-b3-spanid, x-b3-sampled. The server tracer extracts from headers and applies it. 
- IDs
  - Traceid are 64-128 bit - every span in a trace shares this id.
  - Spanid is 64-bit, and indicates the position of the current operation in the trace tree. 
  - Parentspanid is 64-bit, and indicates the position of the parent operation in the trace tree. 
- Sampling state
  - reduce volume of data, once sampling decision is made, the same value should be consistently sent downstream - so you will see all spans or none.
  - Defer - don’t know
  - Deny - don’t sample, `x-b3-sampled: 0`
  - Accept - sample, probablistic trigger, `x-b3-sampled: 1`
  - Debug - force sample, production troubleshooting, `x-b3-flags: 1`

Pending

- ES indexes