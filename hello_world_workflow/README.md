# Hello World Workflow

### Creating my first workflow

#### Steps:
1. Creating virtual env and installing Airflow

- Create a virtual environment
```
$ cd hello_world_workflow
$ virtualenv -p `which python3` venv_hello_world_workflow
$ source venv_hello_world_workflow/bin/activate
(venv_hello_world_workflow) $
```

- Install Airflow
```
(venv_hello_world_workflow) $ pip install airflow==1.8.0
```

- Create ```AIRFLOW_HOME``` directory and set the AIRFLOW_HOME environment variable
```
(venv_hello_world_workflow) $ cd hello_world_workflow
(venv_hello_world_workflow) $ mkdir airflow_home
(venv_hello_world_workflow) $ export AIRFLOW_HOME=`pwd`/airflow_home
```

- Check if Airflow is configured in the env
```
(venv_hello_world_workflow) $ airflow version
```

```
[2018-04-19 19:46:32,887] {__init__.py:57} INFO - Using executor SequentialExecutor
  ____________       _____________
 ____    |__( )_________  __/__  /________      __
____  /| |_  /__  ___/_  /_ __  /_  __ \_ | /| / /
___  ___ |  / _  /   _  __/ _  / / /_/ /_ |/ |/ /
 _/_/  |_/_/  /_/    /_/    /_/  \____/____/|__/
   v1.8.0
(venv_hello_world_workflow) $
```

running the ```$ airflow version``` command creates two files airflow.cfg and unittests.cfg in the airflow_home directory.

2. Initialize the DB
```
(venv_hello_world_workflow) $ airflow initdb
```

The database will be create in airflow.db by default.

3. Start the Airflow web server
```
(venv_hello_world_workflow) $ airflow webserver --port 8080
```
Starts serving the server at http://0.0.0.0:8080

4. Creating first Airflow DAG
Our first DAG, sends "Hello World!" to log.

Code at: [hello_world.py]()

This file creates a simple DAG with just two operators, the ```DummyOperator```, which does nothing and a ```PythonOperator``` which calls the print_hello function when its task is executed.

- Running the DAG

Open up new terminal window and activate the virtual environment.<br/>
Activate the airflow scheduler.
```
$ export AIRFLOW_HOME=`pwd`/airflow_home
$ source venv_hello_world_workflow/bin/activate
(venv_hello_world_workflow) $ airflow scheduler
```

Open http://0.0.0.0:8080 in the browser.

In the browser UI we will be able to see the ```hello_world``` DAG.
- Starting the DAG RUN
    1. Turn the workflow on
    2. click the Trigger Dag button
    3. click on the Graph View

Check the log by clicking on the task(hello_task) on the graph view.<br/>
Log Output:
```
[2018-04-19 21:09:49,685] {base_task_runner.py:95} INFO - Subtask: --------------------------------------------------------------------------------
[2018-04-19 21:09:49,685] {base_task_runner.py:95} INFO - Subtask: Starting attempt 1 of 1
[2018-04-19 21:09:49,685] {base_task_runner.py:95} INFO - Subtask: --------------------------------------------------------------------------------
[2018-04-19 21:09:49,685] {base_task_runner.py:95} INFO - Subtask:
[2018-04-19 21:09:49,692] {base_task_runner.py:95} INFO - Subtask: [2018-04-19 21:09:49,692] {models.py:1342} INFO - Executing <Task(PythonOperator): hello_task> on 2018-04-19 21:09:28.712532
[2018-04-19 21:09:49,699] {base_task_runner.py:95} INFO - Subtask: [2018-04-19 21:09:49,699] {python_operator.py:81} INFO - Done. Returned value was: Hello world!
[2018-04-19 21:09:49,707] {base_task_runner.py:95} INFO - Subtask: /Users/Raj/Root/GitHub/Apache-Airflow-Sandbox/hello_world_workflow/venv_hello_world_workflow/lib/python3.6/site-packages/airflow/ti_deps/deps/base_ti_dep.py:94: DeprecationWarning: generator '_get_dep_statuses' raised StopIteration
[2018-04-19 21:09:49,708] {base_task_runner.py:95} INFO - Subtask:   for dep_status in self._get_dep_statuses(ti, session, dep_context):
[2018-04-19 21:09:54,027] {jobs.py:2083} INFO - Task exited with return code 0
```




<br/><br/><br/>
<p style="font-size:8px">
References:<br/>
1. <a href="https://www.youtube.com/watch?v=XJf-f56JbFM">Developing elegant workflows in Python code with Apache Airflow </a><br/>
2. <a href="http://michal.karzynski.pl/">Michał Karzyński's Blog </a><br/>
</p>
