# Report for assignment 4
This is the report repository for Assignment 4 for group 6. For the forked repository of the project, see [github.com/Glace97/loguru](https://github.com/Glace97/loguru)

### Contribution and Goals

- Zakaria: Goal is to achieve P+. An individual additional report has been written. See [this repository](https://github.com/zsabbagh/dd2480-assignment-4-plus).
It includes description and motivation why certain P+ criterions are fulfilled.
    - Participated in the decided meetings and discussions concerning the project.
    - Came up with the idea and implemented the refactored `Limiter` class (second) approach.
    - In the class, alternate functionalities recsides, for example sliding-window and copy functionality for fulfilling the class.
    - Created thorough testing
    - Created UML diagram for the corresponding class.
    - Generated PRE-coverage tests.
- Glacier: Goal is to achieve P.
    - Searched for projects and, eventually, issues in `Loguru`
    - Set up forked repo, and created project plan by breaking project down to tasks, with Einar.
    - Participated in meetings and discussions with the group
    - Suggested first approach solution and discussed this thoroughly with Einar
    - Pair programmed the implementation and tests through Live Share on VSCode with Einar.
    - Written parts of documentation and report together with the rest of the team
    - Created UML Class diagrams with Einar
    - Generated PRE-regression-test report, and POST-regression-test report for the first approach.
- Einar: Goal is to achieve P.
    - Searched for projects and issues for the assignment.
    - Broke the assignment and chosen issue down into tasks on GitHub with Glacier.
    - Participated in meetings and discussions.
    - Discussed and planned the first approach using pseudocode with Glacier.
    - Pair programmed the implementation and tests through the extension Live Share on VSCode with Glacier.
    - General documentation work.
    - Created UML Class diagrams with Glacier using Diagrams.net
    - Generated post code coverage report for the first approach.

## Project

Name: Loguru

URL: https://github.com/Delgan/loguru

A logger library for Python3.

## Onboarding experience

We chose a new project, since our previous library was a loosly coupled
algorithms and data structure collection.

**If you changed the project, how did your experience differ from before?**
This project is more difficult and not as modular as our previous project.
It is fairly small in comparison to other open source projects, with 12k LOC.
One advantage of the project was the fact that it is not rapidly updated or has high engagement, which lessens the risk of someone else working on our issue.
The [documentation](https://loguru.readthedocs.io/en/stable/overview.html) is fairly clear, and also contains a section dedicated to contribution.

The main difference from assignment 3 is dealing with one big class, or codebase, rather than multiple algorithms in separated classes. Working on an issue is more complicated since it requires more detailed reading and understanding of the whole repository rather than just the one specific algorithm.

### Dependencies
According to the documentation, running `tox` should be sufficient to run all tests, including checking lint with restricted mode, run `pip install tox` in your virtual environment.

In the case of running tests using `pytest` directly instead of using `tox`, the following dependencies must be installed manually.
- mypy
- freezegun
- colorama

## Effort spent

An approximate measure of amount of hours spent, per group member and area:
1. plenary discussions/meetings;
    - Zakaria: 4 - Includes project searching, discussion, setup, and design choices
    - Glacier: 2.5 - Includes project discussion, setup, design choices. Looking for projects/issues
    - Einar: 2.5 - Includes project discussion, setup, design choices

2. discussions within parts of the group, incl. text-discussions;
    - Zakaria: 1 - Participation, but less extensive since I worked on 2nd approach
    - Glacier: 3 - Initial plan for implementation, requirements, and 1st approach
    - Einar: 3 - Initial plan for implementation, requirements, and 1st approach

3. reading documentation;
    - Zakaria: 1 - Both before and during implementation
    - Glacier: 1 - Both before and during implementation
    - Einar: 1 - Both before and during implementation

4. configuration and setup;
    - Zakaria: 0.25 - Setting up GitHub report repositories, configuring access, installing dependencies, altering repository visibility, creating issues, etc
    - Glacier: 0.25 - Setting up forked repo, setting up virtual env, installing dependencies etc
    - Einar: 0.25 - Clonings repositories, setting up virtual environment, installing dependencies, etc.

5. analyzing code/output;
    - Zakaria: 4 - Debugging the Limiter, understanding where changes should be placed, etc.
    - Glacier: 4 - Reading code base/brainstorming implementations, debugging first draft and during pair programming session with Einar
    - Einar: 3.5 - Understanding the foundation, debugging implementation of functionality and tests with Glacier, etc.

6. writing documentation;
    - Zakaria: 4 - Regression test, report, code documentation, UML diagrams
    - Glacier: 4 - Regression test reports, README, commenting code, UML diagrams
    - Einar: 4 - README, code coverage report, commenting code, UML diagrams

7. writing/designing code/tests;
    - Zakaria: 10 - Writing the Limiter and designing what functionality it needed, further alteration done when requirements were not fulfilled.
    - Glacier: 7 - Writing initial draft, pair programming with Einar
    - Einar: 7 - Lay out pseudocode, pair programming with Glacier

- **Total effort spent**:
    - Zakaria: 24.25 hours
    - Glacier: 22 hours
    - Einar: 21.25 hours


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
The changes are mainly affects the `loguru/loguru/_logger.py/Logger` class.
The scope of the implementation may vary based on chosen approach.
The new feature is stand alone and does not (seem to) affect other parts of the code in terms of functionaly unless explicity used.

## Requirements for the new feature
| ID | Title | Description |
|---|---|---|
| 1 | New feature | The new feature will be implemented as a new function named `limited`, which is callable from a `Logger` object. |
| 2 | Object returned | The implemented `limit` function returns a new `Logger` object, wrapping the existing object. |
| 3 | Functionality | Given a number of logs (a `frequency_limit` or N), and given a `time_limit` in minutes, no more than `frequency_limit` logs should be logged within a period of `time_limit`. |
| 4 | Time restriction | The specified `time_limit` shall be a "moving window", meaning it is not a sequential restriction but a rate of how often the logger can log per minute. |
| 5 | Logging levels | The returned limited object should be able to limit all logging functions such as `warning`, `debug`, etc |
| 6 | Overflow | When `frequency_limit` has been reached, an overflow message is logged once, all other logging calls will be surpressed until `time_limit` allows to log again. |
| 7 | Testing | Write unit tests for implemented functionality, attempting at 100% code coverage. |


## Code changes

### Patch
**[Approach #1](https://github.com/Glace97/loguru/tree/1-implement-limit-first-approach)**

**Approach #1** allows the user to give a `frequency_limit`, a `time_limit`, and an optional `overflow_message`.
The `limit` function returns a new `Logger` object which will keep track of the frequency and rate of logs and limit them as defined by the parameters. If the maximum number of logs have been logged within the time limit, an optional overflow message is printed and future logging is surpressed until time allows.

The returned object can be used for specific purposes and allows the user to define in their own source code what specific logs they would like to limit, as long as they wrap the logger with `limit`. Below is an example/use case inspired from the [issue description](https://github.com/Delgan/loguru/issues/383):
```py
# io_logger is wrapped with a limit of 100 logs per hour
io_logger = logger.limit(100, 60, overflow_msg='Suppressing future io errors')
def decrypt_chars(data):
    for offset, c in enumerate(data):
        if cannot_decrypt(c):
            # Continues to log this as long as there is no more than 100 logs within the time frame
            io_logger.warning('Could not decrypt byte {} at offset {}', hex(c), offset)
            continue
        d = decrypt(c)
        if is_invalid_char(d):
            # Continues to log this as long as there is no more than 100 logs within the time frame
            io_logger.warning('Invalid character {!r} at offset {}', d, offset)
        yield d
```
If there are 100 loop within 60 minutes, at the 100th log, the io_logger would also print "Suppressing future io errors" and stop logging until the time has surpassed the limit from the oldest log. As requirement 4 describes, there has to be a moving window. To implement that there is a list (of size `frequency_limit`) of timestamps when logging occurs. Each time logging is called the list of timestamps is checked as a queue, if the first index is older than the `time_limit` it gets discarded by popping and then we know there is room for logging. If the first index is not older than the `time_limit` then the `frequency` variable is checked for how often the `Logger` has logged, if it equals the limit the overflow message gets logged, if it exceeds the limit then the log method is surpressed.

This approach has the benefits of being simple, readable, and is based on Delgans and the issue-openers original thoughts on how to solve the issue (for example by extending the existing function `opt`).
We chose a new `limit` function as this seemed to be adviced by Delgan to be a better approach rather than extending opt, due to there being several parameters other than the `frequency_limit` which needed to be added. An inline `logger.limit(…)` as the example describes was also to prefer rather than using libraries as `ratelimiter`. For refrence please check the [issue](https://github.com/Delgan/loguru/issues/383) and the first response from Delgan.

**Note:** this patch passes all tests, which include older tests as well as newly added tests. It has not been fully linted or formatted to pass the `pylint` check done by running `tox`, the suggested method to check ones solution before submission [source](https://loguru.readthedocs.io/en/stable/project/contributing.html).

**[Approach #2](https://github.com/Glace97/loguru/tree/2-implement-limiter-class)**

**Approach #2** is similar to Approach 1, but refactors most of the work to a separate class,
[`Limiter`](https://github.com/Glace97/loguru/blob/2-implement-limiter-class/loguru/_limiter.py).
This has the benefit of being easier to modify and add functionality to.
With a separate class, it is also easier to integrate to the current project and
verify functionality, as the class itself has own unit tests.
Currently, there are 7+ tests verifying the behaviour of the `Limiter` class and
the corresponding changes in `_logger.py` file.


## Test and coverage results

[Before changes - Regression tests](https://github.com/zsabbagh/dd2480-assignment-4/blob/main/PRE_regression_tests)

[Before changes - Code coverage](https://github.com/zsabbagh/dd2480-assignment-4/blob/main/PRE_code_coverage)

[First approach - Regression tests](https://github.com/zsabbagh/dd2480-assignment-4/blob/main/1st_approach/POST_regression_tests_1st_approach)

[First approach - Code coverage](https://github.com/zsabbagh/dd2480-assignment-4/blob/main/1st_approach/POST_code_coverage_1st_approach)


[Second approach - Regression tests](https://github.com/zsabbagh/dd2480-assignment-4/blob/main/2nd_approach/POST_regression_tests_2nd_approach)

[Second approach - Code coverage](https://github.com/zsabbagh/dd2480-assignment-4/blob/main/2nd_approach/POST_coverage_report_2nd_approach)


## UML class diagram and its description

[Class diagram before changes](https://github.com/zsabbagh/dd2480-assignment-4/blob/main/1st_approach/diagrams/uml_class_diagram_before_changes.drawio.png)

[First approach - Class diagram after changes](https://github.com/zsabbagh/dd2480-assignment-4/blob/main/1st_approach/diagrams/uml_class_diagram_after_changes.drawio.png)

[Second approach - Class diagram after changes](https://github.com/zsabbagh/dd2480-assignment-4/blob/main/2nd_approach/uml_2nd_approach.png)

### Key changes/classes affected
**Approach #1**
- The Logger class has 2 new functions. The `limit` function is public and avaible for the feature. The `_log_limit_helper` is a protected function used by `_log` if the attribute `limit_info` of the `Logger` object is populated.
- The Logger initializer has 1 new  `dict` attribute, `limit_info`, which can either be set to `None` if the logger is not to be limited at all, or contain necessary information for limiting the logs per rate.
```py
limit_info = {
    "time_limit": time_limit * 60,
    "frequency_limit": frequency_limit,
    "overflow_msg": overflow_msg,
    "frequency": 0,
    "timestamps": [None] * frequency_limit
}
```
- All unit tests which calls any function, which eventually calls `_log` (e.g., `warning`, or `debug` etc.) will pass through the check wether the `Logger` object is limited or not. Only our new unit tests will actually run the core logic of the feature, executed by the new helper function.

**Approach #2** altered similar points in the main code of `_logger.py` file. However, there are less changes considering that the functionality of rate-limiting is moved to the class `Limiter`.
In many ways, one could view the class as a refactoring of Approach #1 with the benefit of making the limiting functionality loosly coupled with the logger.
In a similar way to Approach #1, only `_log` has been altered when concerning control flow.

<!-- Optional (point 1): Architectural overview. -->

<!-- Optional (point 2): relation to design pattern(s). -->

## Overall experience

**What are your main take-aways from this project? What did you learn?**

Contributing to open source in general is a closer experience to working profesionally at a company, rather than smaller labs/assignments during ones education.
Navigating through large codebases, and being able to discern what is irrelevant for the task, is difficult in the beginning. Especially, if one is not very familiar/experienced with the codebase or is junior (as we are).

One big difference however, is that when we do start working we will most likely have access to team members and mentors who will be able to help and guide us in the right direction faster and more precisly, rather than an open-source admin who might have hundreds of issues to keep track of.

**How did you grow as a team, using the Essence standard to evaluate yourself?**

So far, we are in "Working well" phase, where the tools are in place and we are using them without thinking about them. We have worked more closely for this assignment with for example, pair programming, as this assignment was not as modular as the previous ones. There has been more discussions regarding requirements, and interpretations of the actual end goal since we have been working on the same issue.
When we wrap up this project, after presentation, we will enter the "retired" phase.
