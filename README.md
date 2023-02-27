# Report for assignment 4

(This is a template for your report. You are free to modify it as needed.
It is not required to use markdown for your report either, but the report
has to be delivered in a standard, cross-platform format.)

## Project

Name: Loguru

URL: https://github.com/Delgan/loguru

A logger library for Python3.

## Onboarding experience

We chose a new project, since our previous library was a loosly coupled
algorithms and data structure collection.

**If you changed the project, how did your experience differ from before?**
This project is more difficult and not as modular as our previous project.
It is fairly small in comparison to other open source projects, with 12LOC.
One advantage of the project was the fact that it is not rapidly updated/have high engagement, which lessens the risk of someone else working on our issue.
The documentation is fairly clear.

TODO: Fill out when project is finished

## Effort spent

For each team member, how much time was spent in
TODO: Fill out according to Google Docs file
    https://docs.google.com/document/d/1Jh7mRReLor8AUTQBKFd_LSFhRkREkOgaCRUXBXBbviA/edit
1. plenary discussions/meetings;

2. discussions within parts of the group;

3. reading documentation;

4. configuration and setup;

5. analyzing code/output;

6. writing documentation;

7. writing code;

8. running code?

For setting up tools and libraries (step 4), enumerate all dependencies
you took care of and where you spent your time, if that time exceeds
30 minutes.

## Overview of issue(s) and work done.

Title: Log something only once (or only N times)

URL: https://github.com/Delgan/loguru/issues/383

This issue was requested from a user who wanted to log something precisely 1 time.
Their suggestion included adding a optional parameter to the existing `opt` function.
At the moment, there is no support for this in `loguru`, and the user must implement a helper function of their own.
To generalize this, there should be support for a given rate limit.
For example, the developer does not want to log more than 100 logs per minute. 
By implementing `limit(100, 1, overflow_msg)`, the logger will supress any output after the 100th log within that minute. 
There seems to be several ways and workarounds to solve this.

**Scope (functionality and code affected):**
The issue is labled with the `feature` tag.
The changes are mainly in the `loguru/loguru/_logger.py/Logger` class.

## Requirements for the new feature or requirements affected by functionality being refactored

1. Given a number of logs (a `frequency_limit`), and given a `time_limit`, no more than `frequency_limit` logs should be logged within a period of `time_limit`.

**TODO: add more requirements?** 

Optional (point 3): trace tests to requirements.

## Code changes

### Patch
**[Approach #1](https://github.com/Glace97/loguru/tree/1-implement-limit-first-approach)**

**Approach #1**: allows the user to give a frquency limit, a time limit, and an optional overflow message.
The `limit()` function returns a new Logger object which will reset its period given the time limit. If the maximum number of logs have been logged within the timelimit, an optional overflow message is printed and that specific log is surpressed. 

This new function, allows the user to define in their own source code, what specific logs they would like to limit. An example inspired from the issue:
```
io_logger = logger.limit(100, 60, overflow_msg='Suppressing future io errors')
def decrypt_chars(data):
    for offset, c in enumerate(data):
        if cannot_decrypt(c):
            io_logger.warning('Could not decrypt byte {} at offset {}', hex(c), offset)
            continue
        d = decrypt(c)
        if is_invalid_char(d):
            io_logger.warning('Invalid character {!r} at offset {}', d, offset)
        yield d
```
If there are 100 loop within 60 minutes, at the 100th log, the io_logger would also print "Suppressing future io errors" and stop logging until a new time period is set, and the object is still alive.




Optional (point 4): the patch is clean.

Optional (point 5): considered for acceptance (passes all automated checks).

## Test results

Overall results with link to a copy or excerpt of the logs (before/after
refactoring).

## UML class diagram and its description

### Key changes/classes affected

Optional (point 1): Architectural overview.

Optional (point 2): relation to design pattern(s).

## Overall experience

What are your main take-aways from this project? What did you learn?

How did you grow as a team, using the Essence standard to evaluate yourself?

Optional (point 6): How would you put your work in context with best software engineering practice?

Optional (point 7): Is there something special you want to mention here?
