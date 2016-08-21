## Microbenchmarking Guidelines
For successful microbenchmarks you have to make sure that what you decided to measure adheres to the following guidelines:

- Your test code’s environment is representative of the real environment for which it is being developed. For example, you should not run a method on in-memory data collections if it is designed to operate on database tables.
- Your test code’s inputs are representative of the real input for which it is being developed. For example, you should not measure how a sorting method fares on three-element lists if it is designed to operate on collections with several million elements.
- The supporting code used to set up the environment should be negligible compared to the actual test code you are measuring. If this is impossible, then the setup should happen once and the test code should be repeated many times.
- The test code should run for sufficiently long so as to be considerable and reliable in the face of hardware and software fluctuations.
- The test code should not be optimized away by the language compiler or the JIT compiler. This happens often in Release mode when trying to measure simple operations.
	- 	Use the Release build without an attached debugger.
	-  Use `[MethodImpl(MethodImplOptions.NoInlining)]` to prevent the JIT compiler from inlining this method (optimizing it away).

When you have ascertained that your test code is sufficiently robust and measures the precise effect that you intended to measure, you should invest some time in setting up the benchmarking environment:

- When the benchmark is running, no other processes should be allowed to run on the target system. Networking, file I/O, and other types of external activity should be minimized (for example, by disabling the network card and shutting down unnecessary services).
- Benchmarks that allocate many objects should be wary of garbage collection effects. It’s advisable to force a garbage collection before and after significant benchmark iterations to minimize their effects on each other.
- The hardware on the test system should be similar to the hardware to be used in the production environment.

Finally, you should focus on the measurement itself. These are the things to keep in mind when designing benchmarking code:

- Discard the first measurement result—it is often influenced by the JIT compiler and other application startup costs. Moreover, during the first measurement data and instructions are unlikely to be in the processor’s cache. (There are some benchmarks that measure cache effects, and should not heed this advice.)
- Repeat measurements many times and use more than just the average—the standard deviation (which represents the variance of the results) and the fluctuations between consecutive measurements are interesting as well.
- Subtract the measurement loop’s overhead from the benchmarked code—this requires measuring the overhead of an empty loop, which isn’t trivial because the JIT compiler is likely to optimize away empty loops. (Writing the loop by hand in assembly language is one way of countering this risk.)
- Subtract the time measurement overhead from the timing results, and use the least expensive and most accurate time measurement approach available—this is usually System.Diagnostics.Stopwatch.
- Know the resolution, precision, and accuracy of your measurement mechanism—for example, Environment.TickCount’s precision is usually only 10-15ms, although its resolution appears to be 1ms.

## Resoures
- Chapter 2 - Pro .NET Performance
- Rules of benchmarking https://perfdotnet.github.io/BenchmarkDotNet/RulesOfBenchmarking.htm
- The Art of Benchmarking http://mattwarren.org/2014/09/19/the-art-of-benchmarking
- Performance with Everyday Things
- Powerful .NET library for benchmarking https://github.com/PerfDotNet
- Performance benchmarking and testing framework for .NET applications https://github.com/petabridge/NBench
- A tool for measuring micro benchmarks in .NET https://github.com/zhangz/measureitdotnet


