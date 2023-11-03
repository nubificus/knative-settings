# Knative-settings


Knative allows users to customize configurations,
having significant impact on the execution of
Serverless workloads.

Kubernetes `Config-Maps` object store these configurations : 


```bash
$ kubectl -n knative-serving get cm 
NAME                     DATA   AGE
config-autoscaler        7      6d8h
config-defaults          1      6d8h
config-deployment        4      6d8h
config-domain            1      6d8h
config-features          3      6d8h
config-gc                1      6d8h
config-kourier           1      6d8h
config-leader-election   1      6d8h
config-logging           1      6d8h
config-network           2      6d8h
config-observability     1      6d8h
config-tracing           1      6d8h
kube-root-ca.crt         1      6d8h
```

Firstly `config-autoscaler` holds 
Autoscaler's values , via `kubectl edit -n knative-serving config-autoscaler` 
we can see the values we have set :

```
  allow-zero-initial-scale: "true"
  container-concurrency-target-percentage: "100"
  initial-scale: "0"
  scale-down-delay: 45s
  stable-window: 720s
  target-burst-capacity: "-1"
```
`allow-zero-initial-scale: "true"` --> Allows deployment start with zero initial pods

`container-concurrency-target-percentage: "100"` --> "This value specifies what percentage of the previously 
specified target should actually be targeted by the Autoscaler...
For example, if containerConcurrency is set to 10, and the target utilization value is set to 70 (percent), the Autoscaler
will create a new replica when the average number of concurrent requests across all existing replicas reaches 7."


`target-burst-capacity: "-1"` --> "Target burst capacity is a global and per-revision integer setting that determines the size of traffic burst a Knative application can handle without buffering."

`scale-down-delay: 45s` --> Scale Down Delay specifies a time window which must pass at reduced concurrency before a scale-down decision is applied.



To see the deployment related configurations: 
`kubectl -n knative-serving edit  cm config-deployment `

```
  queue-sidecar-cpu-request: 0m
  queue-sidecar-image: gcr.io/knative-releases/knative.dev/serving/cmd/queue@sha256:987f53e3ead58627e3022c8ccbb199ed71b965f10c59485bab8015ecf18b44af
  queue-sidecar-memory-request: 00Mi
```

`queue-sidecar-cpu-request: 0m` --> Set cpu-request to zero
`queue-sidecar-memory-request: 00Mi` --> Set memory request to zero