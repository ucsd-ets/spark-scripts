# Spark Scripts

Scripts to create a Spark cluster.

# Manage Cluster

Connect to the login server.

```
ssh username@dsmlp-login.ucsd.edu
```

Checkout the scripts and change to the `spark-scripts` directory.

```
git clone https://github.com/ucsd-ets/spark-scripts.git
cd ~/spark-scripts
```

Modify values.yaml with the size of the master/worker nodes.

For information on the configuration please see https://github.com/bitnami/charts/blob/main/bitnami/spark/values.yaml

Use `helmfile` to start the Spark cluster.

```
helmfile sync
```

Use `helmfile` to stop the Spark cluster.

```
helmfile destroy
```

# Usage

## pyspark

Prepare the Spark context. Set `spark.driver.host` so nodes can connect to it.

```
from pyspark import SparkConf, SparkContext

conf = SparkConf()
conf.setAppName("myapp")
conf.setMaster('spark://spark-master-svc:7077') 
conf.set("spark.driver.host", driver_host)

sc = SparkContext(conf=conf)

```

This example calculates pi.

```
n = 100000

def f(_: int) -> float:
    x = random() * 2 - 1
    y = random() * 2 - 1
    return 1 if x ** 2 + y ** 2 <= 1 else 0

count = sc.parallelize(range(1, n + 1), partitions).map(f).reduce(add)
sc.stop()
```

## Job UI

In your notebook update `host_allowlist` in `~/.jupyter/jupyter_notebook_config.py`.

```
def allow_all_hosts(host, x):
    print(f"Allowing host: {host}")
    return True

c.ServerProxy.host_allowlist = allow_all_hosts
```

Navigate to https://datahub.ucsd.edu/hub/user-redirect/proxy/spark-master-0.spark-headless.grader-cse255-01.svc.cluster.local:8080 in the browser to see the Spark Web UI.

## Logs

Use `kubectl` to view the logs.

```
kubectl get pod
kubectl logs spark-master-0
kubectl logs spark-worker-0
```

## View worker IPs

```
kubectl get pod -owide
```
