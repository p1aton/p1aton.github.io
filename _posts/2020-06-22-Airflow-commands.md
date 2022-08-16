---
layout: post
title:  "Airflow settings and configuration"
crawlertitle: "This is our first post"
summary: "Airflow команды"
date:   2022-08-16 23:09:48 +0700
categories: posts
tags: ['Дата аналитика']
#author: Felipe
---

Apache Airflow is a system to programmatically author, schedule, and monitor data pipelines. How to quickly deploy, diagnose, and evaluate your data pipeline tasks in Airflow using CLI



# Install Airflow

## 1. Install Airflow

Follow the installation [instructions](https://blog.clairvoyantsoft.com/installing-and-configuring-apache-airflow-619a1df3300f) on the Airflow website.

## Update Airflow Configurations

To configure Airflow to use Postgres rather than the default Sqlite3, go to airflow.cfg and update this configuration to LocalExecutor:

{% highlight python %}
# The executor class that airflow should use. Choices include
# SequentialExecutor, LocalExecutor, CeleryExecutor
executor = LocalExecutor
{% endhighlight %}

The LocalExecutor can parallelize task instances locally.

Also update the SequelAlchemy string to point to a database you are about to create.

{% highlight python %}
# The SqlAlchemy connection string to the metadata database.
# SqlAlchemy supports many different database engine, more information
# their website
sql_alchemy_conn = postgresql+psycopg2://localhost/airflow
{% endhighlight %}

Next open a PostgreSQL shell.

{% highlight python %}
psql
{% endhighlight %}

And create a new postgres database.

{% highlight python %}
CREATE DATABASE airflow
{% endhighlight %}

Your now ready to initialize the DB in Airflow. In bash run:

{% highlight python %}
airflow initdb
{% endhighlight %}

# Create a DAG

## 1. Create a DAG folder.

In the console run:

{% highlight python %}
mkdir airflow/dags
{% endhighlight %}

## 2. Add the necessary connections.

The first connection for my API call:

* connection type of HTTP.
* connection identifier of moves_profile.
* host string of the full API endpoint: https://moves....

The second connection for my project database:

* connection type of Postgres.
* connection identifier of users (name of the table).
* host string of 127.0.0.1.
* schema string (database name) of kojak.
* login of postgres (default).

## 3. Create a DAG python configuration file.

In the console run:

{% highlight python %}
touch ~/airflow/dags/moves_profile.py
{% endhighlight %}

Then add your DAG configs.

{% highlight python %}
"""
DAG pulls a user's profile information from the Moves API.
"""
from airflow import DAG
from airflow.operators.bash_operator import BashOperator
from airflow.hooks import HttpHook, PostgresHook
from airflow.operators import PythonOperator
from datetime import datetime, timedelta
import json

def get_profile(ds, **kwargs):
    pg_hook = PostgresHook(postgres_conn_id='users')
    api_hook = HttpHook(http_conn_id='moves_profile', method='GET')

    # Get profile info from Moves API
    resp = api_hook.run('')
    profile = json.loads(resp.content.decode('utf-8'))
    moves_user_id = profile['userId']
    moves_first_date = profile['profile']['firstDate']
    timezone = profile['profile']['currentTimeZone']['id']

    # Insert profile values into Postgres DB
    user_insert = """INSERT INTO users (moves_user_id, moves_start_date, timezone)
                      VALUES (%s, %s, %s);"""

    pg_hook.run(user_insert, parameters=(moves_user_id, moves_first_date, timezone))

default_args = {
    'owner': 'rosiehoyem',
    'depends_on_past': False,
    'start_date': datetime(2017, 3, 21),
    'email': ['rosiehoyem@gmail.com.com'],
    'email_on_failure': False,
    'email_on_retry': False,
    'retries': 1,
    'retry_delay': timedelta(minutes=1),
    # 'queue': 'bash_queue',
    # 'pool': 'backfill',
    # 'priority_weight': 10,
    # 'end_date': datetime(2016, 1, 1),
}

dag = DAG('moves_profile', default_args=default_args, schedule_interval=timedelta(1))


get_profile_task = \
    PythonOperator(task_id='get_profile',
                   provide_context=True,
                   python_callable=get_profile,
                   dag=dag)

{% endhighlight %}

# Deploy with Docker

## 1. Setup an EC2 instance


Be sure you've set-up Port 8080:

* Custom TCP Rule
* Port Range: 80 (for web REST)
* Source: Anywhere).

## 2. Install Docker on the EC2 instance.

## 3. Pull and run the docker-airflow image onto your EC2 instance

Instructions for this instance can be found on the [image Github page](https://github.com/puckel/docker-airflow).

{% highlight python %}
docker pull puckel/docker-airflow
docker run -d -p 8080:8080 puckel/docker-airflow
{% endhighlight %}

## 4. Create a tunnel from your local terminal into your EC2 instance on port 8080.

{% highlight python %}
ssh -i ~/.ssh/aws_key_file.pem -NL 12345:localhost:8080 ubuntu@XX.XXX.XXX.XXX
{% endhighlight %}


# Go to airflow container

{% highlight python %}
docker exec -it b5d116ad83cc bash
{% endhighlight %}

## Create user

{% highlight python %}
airflow users create \
    --username airflow \
    --firstname Airflow \
    --lastname Apache \
    --role Admin \
    --email airflow@example.com \
    --password airflow
{% endhighlight %}



# Airflow CLI 

## Miscellaneous commands

{% highlight python %}
airflow cheat-sheet | Display cheat sheet
airflow info | Show information about current Airflow and environment
airflow kerberos | Start a kerberos ticket renewer
airflow plugins | Dump information about loaded plugins
airflow rotate-fernet-key | Rotate encrypted connection credentials and variables
airflow scheduler | Start a scheduler instance
airflow sync-perm | Update permissions for existing roles and DAGs
airflow version | Show the version
airflow webserver | Start a Airflow webserver instance
{% endhighlight %}

## Celery components

{% highlight python %}
airflow celery flower | Start a Celery Flower
airflow celery stop | Stop the Celery worker gracefully
airflow celery worker | Start a Celery worker node
{% endhighlight %}

## View configuration

{% highlight python %}
airflow config get-value | Print the value of the configuration
airflow config list | List options for the configuration
{% endhighlight %}

## Manage connections

{% highlight python %}
airflow connections add | Add a connection
airflow connections delete | Delete a connection
airflow connections export | Export all connections
airflow connections get | Get a connection
airflow connections list | List connections
{% endhighlight %}

## Manage DAGs

{% highlight python %}
airflow dags backfill | Run subsections of a DAG for a specified date range
airflow dags delete | Delete all DB records related to the specified DAG
airflow dags list | List all the DAGs
airflow dags list-jobs | List the jobs
airflow dags list-runs | List DAG runs given a DAG id
airflow dags next-execution | Get the next execution datetimes of a DAG
airflow dags pause | Pause a DAG
airflow dags report | Show DagBag loading report
airflow dags show | Displays DAG's tasks with their dependencies
airflow dags state | Get the status of a dag run
airflow dags test | Execute one single DagRun
airflow dags trigger | Trigger a DAG run
airflow dags unpause | Resume a paused DAG
{% endhighlight %}

## Database operations

{% highlight python %}
airflow db check | Check if the database can be reached
airflow db check-migrations | Check if migration have finished
airflow db init | Initialize the metadata database
airflow db reset | Burn down and rebuild the metadata database
airflow db shell | Runs a shell to access the database
airflow db upgrade | Upgrade the metadata database to latest version
{% endhighlight %}

## Tools to help run the KubernetesExecutor

{% highlight python %}
airflow kubernetes cleanup-pods | Clean up Kubernetes pods in evicted/failed/succeeded states
airflow kubernetes generate-dag-yaml | Generate YAML files for all tasks in DAG. Useful for debugging tasks without launching into a cluster
{% endhighlight %}

##  Manage pools

{% highlight python %}
airflow pools delete | Delete pool
airflow pools export | Export all pools
airflow pools get | Get pool size
airflow pools import | Import pools
airflow pools list | List pools
airflow pools set | Configure pool
{% endhighlight %}

## Display providers

{% highlight python %}
airflow providers behaviours | Get information about registered connection types with custom behaviours
airflow providers get | Get detailed information about a provider
airflow providers hooks | List registered provider hooks
airflow providers links | List extra links registered by the providers
airflow providers list | List installed providers
airflow providers widgets | Get information about registered connection form widgets
{% endhighlight %}

## Users

{% highlight python %}
airflow users | list will list down all users and their roles
airflow users  create --role Admin --username admin --email admin --firstname admin --lastname admin --password admin | will create a new user
airflow users delete -u USERNAME | will delete that user
{% endhighlight %}

# Tasks

{% highlight python %}
airflow tasks list DAG_ID | will list down all tasks related to a given DAG
airflow tasks test DAG_ID> TASK_ID> EXECUTION_TIME> | will perform test on a specific task in a DAG
{% endhighlight %}

Ссылки:

* [Apache Airflow Cheatsheet](https://medium.com/geekculture/apache-airflow-cheatsheet-205f82d6edda)
* [Airflow: Command Line Interface CLI Cheat Sheet](https://levelup.gitconnected.com/airflow-command-line-interface-cli-cheat-sheet-6e5d90bd3552)