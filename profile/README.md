# Nano Cluster

![logo](nano-cluster.svg)

Nano Cluster is a set of tools to build, run and deploy Nano services (likes of function as a service, lambdas, serverless, ..etc.).
Those Nano services are build and delivered as container images (possibly for WASM/WASI runtimes)
Each Nano service only needs:

- STDIN: standard input to receive requests and results
- STDOUT: standard output to respond and send requests
- STDERR: to log


## How it works?

It uses Std.IO Universal Interface for message passing


## What is Std.IO Universal Interface?

The cluster orchestrator/dispatcher forks a process for the given container image
typically the process will be isolated from host (not network access at all)

The process receives requests from STDIN and respond to STDOUT
The most basic form of messages is a ndjson (a json in a single line) each message is a JSON-RPC.
Other forms like msgPack, compact thrift, protoBuffer, flat-buffer will be added later.

## How can a non service run a MySQL query or call redis pubsub?

By sending the request using STDOUT to special nano services called adaptors

```javascript
{id:"123", method:"db.fetch_all", "params": {"sql": "select * from books"}}
```

## Benefits

- smaller footprint (a WASM service only needs `fd_write` and `fd_read` to be fully functional)
- fully Async IO
- fully event driven
- no network overhead
- no network saturation due to inter-container communications
- more secure (can't be sniffed, policy can be applied)


