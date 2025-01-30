# Spark adapters
Collections of tools and plugins for Apache Spark offering remote shuffle support in Kubernetes and polyglot notebooks.
The Spark Connect plugin enables running a Spark Connect server in a Spark Application running in Kubernetes (using the Kubeflow Spark Operator)

accessible from the following maven repository

https://sigmechanics.github.io/spark-adapters

## Docker images

Apache Spark Docker images including the SigMechanics plugin dependencies are available at docker hub

https://hub.docker.com/repository/docker/sigmechanics/spark/general

```
sigmechanics/spark:3.5.4-2.12
sigmechanics/spark:3.5.4-2.13
sigmechanics/spark:4.0.0-preview2-2.13
```

## Limitations

Needs jdk11+

## remote-shuffle

The remote shuffle storage feature supports storing Spark shuffle data on cloud storage from all major providers (S3, Google Cloud Storage and Azure Cloud Storage).
To enable the remote shuffle storage, add the following configuration to your Spark application running in Kubernetes

```
spark.shuffle.manager: org.apache.spark.shuffle.sort.RemoteShuffleManager,
spark.shuffle.sort.io.plugin.class: "org.apache.spark.shuffle.RemoteShuffleDataIO,
spark.shuffle.remote.rootDir: [local or cloud storage uri, e.g. s3a://mybucket/]
```

## hive

The hive project provides a wrapper class for the Spark HiveThriftServer2 to allow usage in Spark cluster mode (kubernetes).

Spark App configuration
```
"mainClass": "org.sigmechanics.spark.HiveThriftServer",
"deps": {
    "packages": ["org.sigmechanics.spark:hive:1.5.0"],
    "repositories": ["https://us-central1-maven.pkg.dev/ocean-spark/ocean-spark-adapters"]
},
```

The also provides a HiveDialect to query hive tables using the JDBC data source.

```
df = spark.format("jdbc").option("url", "jdbc:hive2://hive-server:10000").option("dbtable", "mytable").load()
df = spark.format("jdbc").option("url", "jdbc:ofas://api.spotinst.io/mycluster/myapp?profile=default").option("dbtable", "mytable").load()
```

## spark-connect

The spark-connect project provides a wrapper class for the SparkConnectServer to allow usage in Spark cluster mode (Kubernetes).

Spark App configuration 
```
"mainClass": "org.sigmechanics.spark.SparkConnectServer",
"deps": {
    "packages": ["org.sigmechanics.spark:spark-connect:1.5.0", "org.apache.spark:spark-connect_2.12:3.5.4"],
    "repositories": ["https://us-central1-maven.pkg.dev/ocean-spark/ocean-spark-adapters"]
},
```

this wrapper class is not needed if Spark Connect is launched as a plugin
```
"sparkConf": {
    "spark.plugins": "org.apache.spark.sql.connect.SparkConnectPlugin"
},
"deps": {
    "packages": ["org.apache.spark:spark-connect_2.12:3.5.4"],
}
```

## ray

The ray-plugin project dynamically installs and runs Ray on Spark cluster in a custom Spark image.

Spark App configuration
```
"mainClass": "org.sigmechanics.spark.RayCluster",
"deps": {
    "packages": ["org.sigmechanics.spark:ray-plugin:1.5.0"],
    "repositories": ["https://us-central1-maven.pkg.dev/ocean-spark/ocean-spark-adapters"]
},
```

the main class is not needed if Ray is launched as a plugin
```
"sparkConf": {
    "spark.plugins": "org.sigmechanics.spark.RayPlugin"
},
"deps": {
    "packages": ["org.sigmechanics.spark:ray-plugin:1.5.0"],
    "repositories": ["https://us-central1-maven.pkg.dev/ocean-spark/ocean-spark-adapters"]
}
```

## jupyter-plugin

The jupyter-plugin project enables users to dynamically install and launch Jupyter lab process on any Spark image with python installed

Spark App configuration
```
"mainClass": "org.sigmechanics.spark.JupyterLab",
"deps": {
    "packages": ["org.sigmechanics.spark:jupyter-plugin:1.5.0"],
    "repositories": ["https://us-central1-maven.pkg.dev/ocean-spark/ocean-spark-adapters"]
},
```

or launch as a plugin
```
"sparkConf": {
    "spark.plugins": "org.sigmechanics.spark.JupyterPlugin",
    "spark.jupyter.work.dir": "/opt/spark/work-dir"
},
"deps": {
    "packages": ["org.sigmechanics.spark:jupyter-plugin:1.5.0"],
    "repositories": ["https://us-central1-maven.pkg.dev/ocean-spark/ocean-spark-adapters"]
},
```

Access the Jupyter lab server from a URL in the following format
```
https://console.spotinst.com/api/ocean/spark/cluster/osc-mycluster/app/spark-myapp/notebook/
```

## vscode-plugin

The vscode-plugin project enables users to dynamically install and launch VScode server process on any Spark image with Python installed

Spark App configuration
```
"mainClass": "org.sigmechanics.spark.VSCodeServer",
"deps": {
    "packages": ["org.sigmechanics.spark:vscode-plugin:1.5.0"],
    "repositories": ["https://us-central1-maven.pkg.dev/ocean-spark/ocean-spark-adapters"]
},
```

or launch as a plugin
```
"sparkConf": {
    "spark.plugins": "org.sigmechanics.spark.SparkCodeServerPlugin",
    "spark.vscode.work.dir": "/opt/spark/work-dir"
},
"deps": {
    "packages": ["org.sigmechanics.spark:vscode-plugin:1.5.0"],
    "repositories": ["https://us-central1-maven.pkg.dev/ocean-spark/ocean-spark-adapters"]
},
```

Access the CodeServer from a URL in the following format
```
https://console.spotinst.com/api/ocean/spark/cluster/osc-mycluster/app/spark-myapp/code/
```