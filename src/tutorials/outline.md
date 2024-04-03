# AppNet Tutorials

## Overview 

AppNet provides high-level abstractions for developers to realize application networks. The abstractions enable easy specification of desired network functionality and enable optimizations that help realize the functionality with low overhead.

In AppNet, network functionality between a pair of microservices is specified as a chain of elements on the RPC request and response paths. Element processing is specified as match-action rules that operate over RPC fields. RPC fields (both metadata and payload) are represented as key-value pairs, which the elements can then easily read/write without worrying about (de)serialization. The state is also represented as key-value pairs.

AppNet’s compiler takes RPC definitions and chain specification to generate code modules. A module may combine the processing of multiple elements (to reduce invocation overhead) and may also re-order element processing based on inter-element dependencies (e.g., RPC fields read/written). The compiler also determines how to partition the state and if parts of shared state can be local (to reduce synchronization overhead). Finally, to further reduce the overhead, the compiler selectively bypasses the sidecar proxy processing.

## Next Steps

- [Learn more about AppNet chain specification.](tutorials/chain.md)
- [Learn more about AppNet element specification.](tutorials/element.md)