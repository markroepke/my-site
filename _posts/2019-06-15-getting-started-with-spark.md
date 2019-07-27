---
layout: post
title: "Getting Started with Spark"
date: 2019-06-15
preview_image: "/assets/apache-spark-logo.png"
categories: posts
---

This post will walk through how to get started with Apache Spark. It will cover a high-level overview of Spark, how to install Spark, and how to launch Spark in a few different ways.

<br>

### Spark Overview

#### What is Spark?

Apache Spark, originally founded in 2009 at UC Berkeley, is a cluster computing platform designed to be *fast* and *general* through distributed computing. It unifies common data tasks like batch applications, iterative algorithms, interactive queries, and data streaming under a single tool. While Spark runs in the JVM and is built in Scala, it has very friendly APIs in Java, Scala, Python, and R, and data scientists can wrangle data using a familiar flavor of SQL.

#### Should I Use Spark?

While you may hear about Spark and its benefits regularly, it's important to determine whether those benefits are worth your effort. If you answer yes to the below questions, Spark is the right tool for you:

1. **Do you frequently work with data?** Whether you are a data scientist working on supervised learning or Word2Vec problems or data engineer trying to process data in real-time, Spark can help you with your tasks.
2. **Do you find yourself waiting around for data to process?** If that gradient boosting model or Kakfa stream use more data than you expected and you find yourself waiting around for them finish, Spark can help you even more.
3. **Do you have a computing cluster for distributed work?** While Spark can run locally in a single JVM, it's intended to be, and at its best when, distributed across a computing cluster. These are available via all major cloud providers.

#### Spark's Components

Spark provides a unified computational engine for a variety of data processing tasks through a stack of different components:

<br>
<center>
<figure>
    <img src="/assets/spark-stack.png" height="300"/>
    <figcaption>Figure 1-1 from Learning Spark by Zaharia, Wendell, Wonwinski, Karau</figcaption>
</figure>
</center>
<br>

These components are briefly described below:

1. Spark Core - The basic functionality of Spark, including task scheduling, memory management, fault recovery, and interacting with storage systems. Home to the resilient distributed dataset, or RDD, which is a core low-level data structure in Spark.
2. Spark SQL - A package for working with structured data and home to the Spark DataFrame -- the Spark DataFrame is the most important data structure for doing data science in Spark. It has friendly APIs in Java, Scala, Python and R, and Spark SQL also provides a SQL interface to wrangling Spark DataFrames.
3. Spark Streaming - An avenue for processing large streams of data in real-time.
4. MLlib/ML - There are two separate machine learning libraries within Spark. MLlib, the first, is frequently mentioned and built to work with RDDs. A newer and recommended approach is to use ML which works nicely with the Spark DataFrame.
5. GraphX - A package for working with network graphs in parallel.

<br>

### Installing Spark

While Spark is at its best when distributed, it can be installed on a single computer and run in local mode. All of the syntax and user-facing work will be similar, so it's a great way to learn to interact with Spark's APIs. The below installation walkthrough assumes you will be running Spark in local mode for now and that you are familiar with using the command line.

<br>

#### Step 1: Ensure Java 8+ is Installed

As mentioned above, Spark runs in the JVM so Java must be installed, and the installed Java version must be 8+. You can check whether Java is installed and its version with the below line:

```bash
java -version
#> java version "12.0.2" 2019-07-16
#> Java(TM) SE Runtime Environment (build 12.0.2+10)
#> Java HotSpot(TM) 64-Bit Server VM (build 12.0.2+10, mixed mode, sharing)
```

If Java is not installed, you can follow <a href="https://java.com/en/download/help/download_options.xml" target="_blank" class="class2">Java's installation instructions</a>. You may also need update your `PATH` variable or `JAVA_HOME` variable to point the `java` command to the appropriate version.

<br>

#### Step 2: Download Spark

Spark can be downloaded from the downloads page of <a href="https://spark.apache.org/downloads.html" target="_blank" class="class2">the Spark project website</a>. Below is the download prompt:

<br>
<center>
<img src="/assets/spark-download-prompt.png" height="200"/>
</center>
<br>

I always install the most recent stable release, and I choose the package type "Pre-built for Apache Hadoop" with the latest Hadoop version. Next, you can click on the "Download Spark" link. It will take you to an Apache page with the following message:

<br>
<center>
<img src="/assets/spark-download-link.png" height="75"/>
</center>
<br>

Click on the link to download Spark.

<br>

#### Step 3: Install Spark

Once Spark is downloaded, you will need to extract the files:

```bash
tar -xvf spark-2.4.3-bin-hadoop2.7.tgz
```

Next, you'll want to move those files into a `spark` folder under `usr`:

```bash
sudo mv spark-2.4.3-bin-hadoop2.7 /usr/local/spark
```

If `/usr/local/spark/bin` isn't in your `PATH` variable, you should add it by including the following in your `.bashrc`:

```bash
export PATH=$PATH:/usr/local/spark/bin
```

And then sourcing your `.bashrc` for immediate availability:

```bash
source ~/.bashrc
```

And finally, verify that spark is installed and running th correct version by running:

```bash
spark-shell --version
```

Note that the `pyspark` library, which we will cover later, can also be installed from PyPI or by using `conda`.

```bash
pip install pyspark
conda install pyspark
```

<br>

### Launching Spark

As mentioned earlier, one of the benefits of Spark is how it can be run interactively and in batch applications. Below is a walkthrough on launching, initializing, and batching Spark jobs.

<br>

#### Spark-Specific Shells

There are Spark-specific shells for each of the API-supported interpreted languages: Scala, Python, and R.

*Scala*

Because Scala is Spark's native language, launching its shell is the most straightforward -- it's the command we used earlier to check the version:

```bash
spark-shell
```

The Scala-based Spark shell will then launch. You can run normal Scala code from this shell in addition to Spark using the pre-loaded `spark` variable.

*Python*

Python has a shell to support Spark that is called PySpark. It can be launched by running the following command:

```bash
pyspark
```

The Python-based Spark shell will then launch. You can run normal Python code from this shell in addition to Spark using the pre-loaded `spark` variable. 

Note that PySpark may not use your preferred version of Python -- you can change this by setting the `PYSPARK_PYTHON` variable in your environment to your preferred version of Python.

*R*

R also has a shell to support Spark called `SparkR`.

```bash
SparkR
```

Note that there is an RStudio-developed package called `sparklyr` to dispatch Tidyverse functions to `Spark` -- this means R users can use Tidyverse syntax on Spark DataFrames. While the Spark DataFrame APIs are intuitive, this is a nice functionality.

<br>

#### Initializing Spark in Standard Language Interpreters

*Scala*

The main entry point for Spark DataFrame functionality with Scala is through the Spark SQL library's `SparkSession` class. A `SparkSession` can create, read and write Spark DataFrames very easily. We can initialize a `SparkSession` using the `builder` as shown below:

```scala
import org.apache.spark.sql.SparkSession
spark = SparkSession.builder \
    .master('local') \
    .appName('Example Intialization') \
    .getOrCreate()
```

More details on the `SparkSession` can be found in <a href="https://spark.apache.org/docs/latest/api/scala/index.html#org.apache.spark.sql.SparkSession" target="_blank" class="class2">the documentation</a>. 

*Python*

Python is very similar to Scala in this regard. The main entry point for Spark DataFrame functionality with Python is through the `pyspark.sql` module's `SparkSession` class. A `SparkSession` can create, read and write Spark DataFrames very easily. We can initialize a `SparkSession` using the `builder` as shown below:

```python
from pyspark.sql import SparkSession
spark = SparkSession.builder \
    .master('local') \
    .appName('Example Intialization') \
    .getOrCreate()
```

More details on the `SparkSession` can be found in the `pyspark` documentation <a href="https://spark.apache.org/docs/2.4.3/api/python/pyspark.sql.html?highlight=sparksession#pyspark.sql.SparkSession" target="_blank" class="class2">documentation</a>.

*R*

R has a similar but slightly different mechanism for launching a Spark session. The code is below:

```R
sparkR.session(master = 'local', appName = 'Example Initialization')
```

Note that `SparkR` is going to require Java version 8.

<br>

#### Submitting Spark Applications for Batch Processing

Spark applications can be submitted for batch processing in a couple different ways:

1. `spark-submit` if your application was written in a Spark-specific shell.
2. Regular batch if your application was written in its langauge's interpreter and includes a Spark intialization.

Here is an example of each for Python:

```bash
spark-submit --master local --name example_initialization example.py
```

Or:

```bash
python example.py &
```

Similar options are available for Scala and R.