---
layout: post
title: "Asynchronous Task Scheduling in Python"
tags: [python, asynchronous, async, software development, software design, cs]
---

In this post, I am sharing a simple Python pattern I like to use to schedule non-blocking tasks that run periodically. 

Python 3.7 makes asynchronous programming in Python a bit smoother and more accessible than previous releases. In particular, I like using Tasks, which are a subclass of Futures (which are themselves analogous to Promises in Javascript). In most cases, you do not need to use Futures but can instead just work with coroutines and Tasks through a higher-level API.
<!--more-->

Tasks are coroutine schedulers. Coroutines are subroutines except they allow execution to be suspended and then resumed. You can wrap a coroutine in a Task and it will schedule it for execution as soon as possible. We will use Tasks and coroutines to run something in the background periodically, inspired by the `setInterval` method in Javascript. 


    
```python
import asyncio
    
async def run_periodically(wait_time, func, *args):
    """
    Helper for schedule_task_periodically.
    Wraps a function in a coroutine that will run the
    given function indefinitely
    :param wait_time: seconds to wait between iterations of func
    :param func: the function that will be run
    :param args: any args that need to be provided to func
    """
    while True:
        func(*args)
        await asyncio.sleep(wait_time)

def schedule_task_periodically(wait_time, func, *args):
    """
    Schedule a function to run periodically as an asyncio.Task
    :param wait_time: interval (in seconds)
    :param func: the function that will be run
    :param args: any args needed to be provided to func
    :return: an asyncio Task that has been scheduled to run
    """
    return asyncio.create_task(run_periodically(wait_time, func, *args))
```

We can use this method to gracefully cancel a periodic background task we scheduled this way:

{% marginnote 2 %}
On cancellation, we wait for the Task to finish. Tasks throw a [CancelledError](https://docs.python.org/3/library/asyncio-task.html#asyncio.Task.cancel) exception when cancelled which needs to be caught.
{% endmarginnote %}
```python
async def cancel_scheduled_task(task):
    """
    Gracefully cancels a task
    :type task: asyncio.Task
    """
    task.cancel()
    try:
        await task
    except asyncio.CancelledError:
        pass
``` 

Here is an example of this implementation in action:

```python
import datetime

def count_seconds_since(then):
    """
    Prints the number of seconds that have passed since then
    :type then: datetime.datetime
    :param then: Time to count seconds from
    """
    now = datetime.datetime.now()
    print(f"{(now - timestamp).seconds} seconds have passed.")


async def main():

    counter_task = schedule_task_periodically(3, count_seconds_since, dt.now())

    print("Doing something..")
    await asyncio.sleep(10)
    print("Doing someting else..")
    await asyncio.sleep(5)
    print("Shutting down now...")

    await cancel_scheduled_task(counter_task)
    print("Done")

if __name__ == "__main__":

    asyncio.get_event_loop().run_until_complete(main())

```

This outputs:
```
Doing something..
0 seconds have passed.
3 seconds have passed.
6 seconds have passed.
9 seconds have passed.
Doing someting else..
12 seconds have passed.
Shutting down now...
Done
```
