# QuickStart

This guide will walk you through:

- Deploying a simple echo application.
- Running a simple element chain on the frontend to server communication edge.


## Echo Application
The Echo application is a simple application developed using Go and gRPC. The client sends messages to the frontend, which then relays the messages to the Echo server. Finally, the server echoes the request back to the frontend. The architecture is as follows:

![Echo Application](./figures/echo-app.png "Echo Application")

### Deploy 
Run the following command to deploy the echo application.
```
kubectl apply -f config/samples/echo/echo.yaml
```

Then, verify the deployment:
```
user@h1:~/appnet$ kubectl get pods
NAME                             READY   STATUS    RESTARTS   AGE
echo-frontend-6f9cf6db74-tjvfc   2/2     Running   0          14m
echo-server-594b4797d-9t6gn      2/2     Running   0          14m
user@h1:~/appnet$ curl http://10.96.88.88/hello_world
Echo request finished! Length of the request is 12
```

## Example element chain

We will deploy the following chain to the frontend to server edge.

![Example Chain](./figures/echo-chain.png "Example Chain")

### Run the AppNet controller
First, you need to run the AppNet controller
```
make run
```

For this element chain the AppNet configurations is as follows:
```yaml
apiVersion: api.core.appnet.io/v1
kind: AppNetConfig
metadata:
  name: sample-echo # Name of the AppNetConfig
spec:
  appName: echo # Name of the application
  clientService: frontend # Name of the client service (must be a valid service in the same namespace as the AppNetConfig)
  serverService: server # Name of the server service (must be a valid service in the same namespace as the AppNetConfig)
  method: echo # Name of the RPC method (defined in the proto file)
  appManifestFile: <APPNET_DIR_PATH>/config/samples/echo/echo.yaml # Path to the application manifest file
  clientChain:
    - name: fault # Name of the first element in the client chain
      file: <APPNET_DIR_PATH>/appnet/config/samples/echo/fault.adn # Path to the fault injection element file
    - name: logging # Name of the second element in the client chain
      file: <APPNET_DIR_PATH>/appnet/config/samples/echo/logging.adn # Path to the logging element file
  serverChain:
    - name: firwall # Name of the first element in the server chain
      file: <APPNET_DIR_PATH>/appnet/config/samples/echo/firewall.adn # Path to the firewall element file
  anyChain:
    - name: metrics # Name of the first element in the any(unconstraint) chain
      file: <APPNET_DIR_PATH>/appnet/config/samples/echo/metrics.adn # Path to the metrics element file
  proto: <APPNET_DIR_PATH>/appnet/config/samples/echo/echo.proto # Path to the protobuf definition of client service to server service communication
```

Next, in a seperate terminal, replace `<APPNET_DIR_PATH>` with your AppNet directory path and apply this yaml file:
```
sed -i 's|<APPNET_DIR_PATH>|'"$(pwd)"'|g' config/samples/echo/sample_echo.yaml
kubectl apply -f config/samples/echo/sample_echo.yaml
```

You should some logs in the controller indicating it is reconciling, which should finish in a few minutes. 

Finally, test the installation by running:
```
user@h1:~/appnet$ curl http://10.96.88.88?key=hello
You've hit server-6646d696cb-mx95h
user@h1:~/appnet$ curl http://10.96.88.88?key=test
Echo server returns an error.
```

The test request will be blocked by the firewall element. 

# Clean Up

When you're finish experimenting with the echo application, uninstall and clean it up using the following command:
```bash
kubectl delete all,pvc,pv,envoyfilters,appnetconfigs --all
```

# Next Steps

- [Learn more about AppNet specification.](tutorials/outline.md)