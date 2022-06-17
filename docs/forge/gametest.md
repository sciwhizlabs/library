---
hide:
  - footer
---

!!! danger "Work in Progress"
    This article is a work in progress. The information presented here may be incomplete or inaccurate.

# Game Test Framework

!!! warning inline end
    This article is written before the release of Forge 1.17.1, and uses Mojang names.

With the release of 1.17 comes the availability of Mojang's own **game test framework**, which
allows running in-game unit-like tests. The root package of the game test framework is
`net.minecraft.gametest.framework`.

??? note "Present but Unusable before 1.17.x"
    While the game test framework is included in versions prior to 1.17.x, the obfuscator that
    Mojang uses for the Minecraft code removes important chunks of the framework, notably the
    test completion mechanics.

    While libraries have been made for 1.16.x which mimic the game test framework (through
    observation of the remaining code and through a video released by Mojang which showcases
    the framework in action), only in 1.17.x was the game test framework included in Minecraft
    truly accessible due to Mojang disabling the portion of the obfuscator responsible for
    removing unused portions of code, which now leaves the game test framework whole.

??? abstract "List of game test framework classes and their uses"
    **Annotations**:

    - `AfterBatch` - annotation to mark a method to be run after a game test batch is ran.
    - `BeforeBatch` - annotation to mark a method to be run before a game test batch is ran.
    - `GameTest` - annotation to mark a method as a game test
    - `GameTestGenerator` - annotation to mark a method as a generator of game tests

    **Exceptions**:

    - `ExhaustedAttemptsException` - exception thrown when a flaky game test has failed due to the 
      amount of attempts reaching the limit while not reaching the success threshold
    - `GameTestAssertException` - exception thrown when an assertion in the game test has failed
    - `GameTestAssertPosException` - exception thrown when an assertion linked with a position in 
      the world has failed
    - `GameTestTimeoutException` - exception thrown when a game test has exceeded its timeout duration

    **Test Reporter Classes**:

    - `GlobalTestReporter` - global static test reporter which delegates to another test reporter, 
      by default the log test reporter
    - `JUnitLikeTestReporter` - test reporter which writes out a JUnit-like XML output file
    - `LogTestReporter` - test reporter which reports failures to a logger
    - `TeamcityTestReporter` - test reporter which prints out TeamCity service messages
    - `TestReporter` - interface for classes which report test success or failure, with an optional 
      finish method

    **Classes**:

    - `GameTestBatch` - a named collection of game tests, executed together
    - `GameTestBatchRunner` - runner for a collection of game test batches
    - `GameTestEvent` - a single possibly-delayed event in a game test sequence
    - `GameTestHelper` - helper class for game tests, including utilities to manipulate the world 
      and perform assertions
    - `GameTestInfo` - information about a (running or to-be-run) game test
    - `GameTestListener` - interface for listeners for certain events, such as test success, 
      failure, or structure load
    - `GameTestRegistry` - central registry of game tests
    - `GameTestRunner` - helper class for running game tests
    - `GameTestSequence` - a sequence of game test events, chained together to allow executing 
      game test action in order with optional delays
    - `GameTestServer` - a server implementation for the purpose of running game tests on headless
      machines without user interaction (such as CI servers)
    - `GameTestTicker` - shared ticker for running game tests
    - `MultipleTestTracker` - tracker for multiple running game tests
    - `ReportGameListener` - test listener which broadcasts game test in-game through chat and beacons
    - `StructureUtils` - utility class for loading and managing structures in-game
    - `TestClassNameArgument` - command argument for test class names as defined in the game test 
      registry
    - `TestCommand` - the `/test` command, used for creating and manipulating game tests in-game
    - `TestFunction` - represents a single game test
    - `TestFunctionArgument` - command argument for game test functions as defined in the game test 
      registry