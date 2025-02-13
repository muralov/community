# API Design


```YAML
kind: TelemetryModule # cluster scope
apiVersion: telemetry.kyma-project.io/v1alpha1
metadata:
  name: myModule
spec:
  tracing:
    enabled: true
    otlp:
      endpoint: otlp-traces.kyma-system:55690
```

```YAML
kind: TracePipeline # cluster scope
apiVersion: telemetry.kyma-project.io/v1alpha1
metadata:
  name: myPipeline
spec:
  processors: # list of processors, order is important
    probabilisticSampler:
        samplingPercentage: 15.3

  exporter: # only one output, defining no output will fail validation
    otlp:
        protocol: grpc #grpc | http
        endpoint: myserver.local:55690
        tls:
            insecure: false
            insecureSkipVerify: true
            ca: 
              value: dummy
              valueFrom:
                  secretKeyRef:
                    name: my-config
                    namespace: default
                    key: "server.crt"
            cert:
                value: dummy
                valueFrom:
                    secretKeyRef:
                        name: my-config
                        namespace: default
                        key: "cert.crt"
            key:
                value: dummy
                valueFrom:
                    secretKeyRef:
                        name: my-config
                        namespace: default
                        key: "client.key"
        headers:
            - name: x-token
              value: "value1"
              valueFrom:
                secretKeyRef:
                    name: my-config
                    namespace: default
                    key: "myToken"
```