---
layout: post
title: "Time Machines Are A Code Smell"
tags: [testing, tdd, software development, software design, cs]
---

I recently read an article called ["Mocking is a code smell"](https://medium.com/javascript-scene/mocking-is-a-code-smell-944a70c90a6a) that reminded me of a unit test I wrote when I first started writing tests properly. The premise of the article is that the need for mocks in your unit tests could be indicative of a problem with your code. As unappetizing as the phrase "code smell" is, it did make me think more explcitly about the relationship between testing and design. Ever since I started following a test-driven development approach, the quality of my code has definitely improved and my understanding of good design has deepened.

<!--more-->

The class I was supposed to be testing- let's call it CalculationEngine, was supposed to do a certain calculation periodically at various intervals. The problem was that the length of these intervals was hard-coded into the class implementation. To be more specific, this was a static class calculating a function every 1, 5 and 15 minutes. To be even more specific, it was constantly getting the current system timestamp to check if the right amount of time had passed to do a calculation. No unit test should ever take 1 minute, let alone 15, so the only way I could think of to to really test this class as it was implemented was by overriding system time at execution to trick CalculationEngine into thinking minutes had passed instead of milliseconds.

There are a lot of things wrong with what I just said. For one, there are definitely better ways to implement the desired behaviour than the existing implementation. Those interval lengths should not have been hard coded. In fact, having one static CalculationEngine class responsible for doing multiple periodic operations breaks the single responsibility principle and it would have been better to have multiple instances each responsible for one calculation interval. Needing implementation details to write the tests is also a red flag. However, I decided to not push too hard on refactoring this class because I thought it was out of scope for my assigned task, which was writing tests. I had also just read about something called monkey-patching and I really wanted to try it.

What is monkeypatching? It leverages Python's dynamic nature to modify a class or module at run time. I have only ever considered it in the context of unit testing, as slick way to mock out external dependencies. Instead of refactoring CalculationEngine, I could just override the external method I was using to get the current time. Here is a function for a "time machine" similar to what I used:
    
```python
    from datetime import datetime, timedelta

    def build_time_machine():
        """
        Returns a generator so the entire timeline does not
        need to be loaded into memory
        """
        # starts from the present
        start_time = datetime.now()
        # pick a large enough range for the time period to be tested
        return (start_time + timedelta(minutes=n) for n in range(9999))
     
    time_machine = build_time_machine()
    next(time_machine)   # current time
    next(time_machine)   # one minute into the future
    next(time_machine)   # two minutes into the future
``` 

I use [pytest](https://docs.pytest.org/en/latest/monkeypatch.html) which comes with an easy-to-use `monkeypatch` feature. With `monkeypatch`, I was able to override the behaviour of the current time method I was using so that every time I called it, it returned `next(time_machine)` instead. CalculationEngine was constantly checking the time to see if 1, 5 or 15 minutes had passed since the last calculation 1, 5 or 15 minute calculation respectively. Every time it checked the time, my time machine made it seem like a whole minute had passed, making tests run much faster.

As a young developer, one can often get caught up with how cool something is. Discovering an existing feature that makes an operation more compact or allows you to get around something feels really satisfying. Sometimes the coolness is actually beneficial to development- Python [f-strings](https://www.python.org/dev/peps/pep-0498/) are more compact than other string composition methods but also perform better and are by far the most readable (usually the biggest perk). However, other times that coolness is only superficial and the boring way is better. Monkeypatching in Python is one of those things that feels very powerful and convenient, but when something is both powerful and convenient, you should definitely question its use (this is good advice in general).

As fun as it was to monkeypatch with a time machine (and more fun to say it), the existing code could have been structured much better. If monkeypatching is the only way to unit test something, it is very likely there are big improvements you can make that are easy to spot. Of course there are cases where you would need to build a time machine for testing, especially for higher-level testing, but in this case, I should have just taken the time to do some good old refactoring instead. 
