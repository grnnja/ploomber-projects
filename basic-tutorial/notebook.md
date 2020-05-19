---
jupyter:
  jupytext:
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.2'
      jupytext_version: 1.4.2
  kernelspec:
    display_name: Python 3
    language: python
    name: python3
---

```python
import importlib
from pathlib import Path
from IPython.display import Image, HTML, Markdown, display
from ploomber_basic import pipeline
```
# Ploomber tutorial

This example project showcases the basic Ploomber experience, hopefully, this interactive tutorial will convince you to try out Ploomber in your next Data Science project.

Let's imagine you already have a Ploomber pipeline, which is organized in a few tasks. Ploomber allows you to quickly iterate by keeping track of source code changes and skipping unnecessary computations.

Let's first retrieve our pipeline:

```python
dag = pipeline.make()
```

Plotting it makes easy to understand dependencies between tasks, each node corresponds to one.

```python
Image(filename=dag.plot())
```

Our pipeline has three tasks: load, clean and plot. Ploomber will keep track of your source code and mark as outdated any task that changed since last execution. Making your whole pipeline up-to-date is as simple as running:

```python
dag.build()
```

Let's see what happens if we build again:

```python
dag.build()
```

It didn't run anything! That's because our pipeline has not changed, there is nothing to run.


Our `dag` object is a fully interactive way of exploring our pipeline. Let's use the DAG object to know where our code is located:

```python
# get list of task names
list(dag)
```

```python
# get task
task = dag['load']

# print source code
print(task.source)

# where is this code declared?
print(task.source.loc)
```

Go ahead and modify the function, for example, instead of having:

<!-- #md -->

```python
df['x'] = df['x'] = 1
```

Replace it with (or anything you want):

```python
df['x'] = df['x'] = 2
```

<!-- #endmd -->

[Click here to open the notebook](src/ploomber_basic/notebooks/functions.py)


Then come back and run:

```python
dag.build()
```

You should see that all three tasks ran again, that's because the load function is the root node. Let's try with another task:

```python
print(dag['clean.py'].source.loc)
```

Go ahead and modify the file.

[Click here to open the notebook](src/ploomber_basic/notebooks/clean.py)

Then run:

```python
dag.build()
```

You should see that Ploomber skipped running the load task since modifying the clean task didn't change it. You can save a lot of time by letting Ploomber automatically figure out which tasks to run, especially when those computations take a lot of time to run.

Let's take a look at the actual pipeline declaration, where we'll find a bunch of interesting things.

```python 
Markdown("""
```python
{}
```
""".format(Path(pipeline.__file__).read_text()))
```

You've made it to the end of this tutorial. Hopefully this will convince you to give Ploomber a try. Don't hesitate with any questions you might have. Feel free to [open an issue](https://github.com/ploomber/ploomber/issues/new) in the repository. Thanks for reading!

## Where to go from here

* Take a look at our [documentation](https://ploomber.readthedocs.io/en/stable/)
* Dive into our [codebase](https://github.com/ploomber/ploomber)