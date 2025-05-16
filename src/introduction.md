# AppNet: Expressive, Easy to Build, and High-Performance Application Networks

<iframe src="https://ghbtns.com/github-btn.html?user=appnet-org&repo=appnet&type=star&count=true&size=large" frameborder="0" scrolling="0" width="170" height="30" title="GitHub"></iframe>


## What is AppNet?

AppNet is a framework designed for constructing high-performance application networks for microservices. At its core, AppNet offers a high-level abstraction that facilitates the creation of expressive and performant application networks. Users can define rich, possibly stateful, layer-7 (RPC) processing through match-action rules. AppNet's compiler compiles these specifications and generates high-performance code by optimizing where and how to execute different RPC processing functions.

Details are available in our [NSDI paper](https://www.usenix.org/conference/nsdi25/presentation/zhu).

## Architecture Overview
![AppNet Architecure](./figures/appnet-arch.png "AppNet Architecure")

AppNet's architecture comprises three key components:

1. **AppNet Program**: Orchestrates network functionality across microservices using a sequence of elements, each defined by match-action rules that control RPC content and element state.

2. **Control Plane**:
   - A compiler that processes RPC definitions and chain specifications to generate optimized code modules
   - A controller that maintains global knowledge of network topology, service locations, and available processors through integration with cluster managers (Kubernetes)

3. **Data Plane**: Consists of various processors (sidecars, middleboxes, and RPC libraries) that execute elemental operations. These processors:
   - Receive compiled RPC processing logic from the control plane
   - Continuously report logs, traces, and runtime metrics to the controller


## Supported Data Plane Processors

- [Istio](https://istio.io/)(Envoy - via both native C++ filters and WASM plugin)
    - Sidecar Mode
    - Ambient Mode 
- [gRPC](https://grpc.io/) (via Interceptors) 
    - Only gRPC-go is supported 

## Contributors

AppNet is developed and maintained by:
- [Xiangfeng Zhu](https://xzhu27.me/)
- [Yuyao Wang](https://kristoff-starling.github.io/)
- [Banruo Liu](https://github.com/livingshade)
- [Yongtong Wu](https://jokerwyt.github.io/)
- [Nikola Bojanic](https://github.com/NikolaBo)

## Getting help

Please get in touch with Xiangfeng Zhu (xfzhu@cs.washington.edu).


