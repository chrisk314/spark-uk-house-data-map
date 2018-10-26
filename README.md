# Spark UK House Data Map

## Install

### Installing Spark standalone
Spark must be installed in order to run the notebook. Spark requires java. Currently Spark 2.3.2
only works with java version 8. To install java 8 on ubuntu

```
sudo apt-get install openjdk-8-jdk openjdk-8-jre
```

Then add these lines to your `~/.profile`

```
JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/jre
export PATH=${PATH}:${JAVA_HOME}/bin
```

Download a precompiled binary version of Spark with Hadoop

```
cd /opt
wget http://apache.claz.org/spark/spark-2.3.2/spark-2.3.2-bin-hadoop2.7.tgz
tar -xzf spark-2.3.2-bin-hadoop2.7.tgz
``` 

Add the following lines to your `~/.profile`

```
export SPARK_HOME=/opt/spark-2.3.2-bin-hadoop2.7
export PATH=${PATH}:${SPARK_HOME}/bin
export PYTHONPATH=${SPARK_HOME}/python:${PYTHONPATH}
```

### Installing project dependencies

```bash
git clone git@github.com:chrisk314/spark-uk-house-data-map.git
cd spark-uk-house-data-map
virtualenv -p python3 venv
source .env
python -m pip install -r requirements.in
```

## Run
```
source .env
pyspark
```

