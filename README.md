# Spark Scripts

Scripts to create a Spark cluster.

# Start cluster

Modify values.yaml with the size of the master/worker nodes.

For information on the configuration please see https://github.com/bitnami/charts/blob/main/bitnami/spark/values.yaml

Run

```
helmfile sync
```


# Stop Cluster

```
helmfile destroy
```

# Connect to cluster with Python


```
from pyspark import SparkConf, SparkContext

# Create a Spark configuration
conf = SparkConf().setAppName("MyApp")\
            .setMaster("spark://spark-master-0.spark-headless.grader-cse255-01.svc.cluster.local:7077")
# Initialize SparkContext
sc = SparkContext(conf=conf)

```

# Connect to Job UI

First enable it:

in ~/.jupyter/jupyter_notebook_config.py put the lines

```
def allow_all_hosts(host, x):
    print(f"Allowing host: {host}")
    return True

c.ServerProxy.host_allowlist = allow_all_hosts
```

Navigate to :

https://datahub.ucsd.edu/user/grader-cse255-01/proxy/spark-master-0.spark-headless.grader-cse255-01.svc.cluster.local:8080



# Viewing the logs

```
kubectl logs spark-master-0
kubectl logs spark-worker-0
```

# View worker IPs

```
kubectl get pod -owide
```