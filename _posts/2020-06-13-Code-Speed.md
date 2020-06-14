# Code speed considerations

Sometimes the code for models in the lab can be slow enough that it poses an impediment for efficient research.
This is particularly true when large population sizes, 

## How to measure the speed of your code

### Total time to run the program

The simplest way to measure the speed of your code is with a command-line program that tells you how long a given command took.
On Linux and linux-emulators for Windows, you can use the `time` program. On mac it is `gtime`, which can be installed with `brew install gnu-time`.

For example, you can time a python program like this:

```
time python my_program.py
```

(swap in `gtime` if you're on a mac).

The output should look something like this:

```
real	0m0.609s
user	0m0.387s
sys	0m0.055s
```

The first line shows the "real" time the program ran for, also known as the "wall clock time." 
In other words, it shows the total amount of time you waited between starting the program and it finishing.
This is simplest line to look at. For a thorough description of what the "user" and "sys" lines mean,
see [this stackoverflow post](https://stackoverflow.com/questions/556405/what-do-real-user-and-sys-mean-in-the-output-of-time1).

#### Considerations when timing a program

Of course, a lot of factors other than your code can affect exactly how long it takes your program to run:
- Processor (speed + caching stuff)
- RAM (although this will only matter in certain cases)
- For parallelized programs 
- Whether your operating system is busy with other processes at the same time.
- Parameter values in your program
- If your program is stochastic, the way some runs play out may cause them to be faster than others due to chance (e.g. if your population dies off, things will often be faster)

The takeaway is that you should run your program enough times to account for variation (due to stochasticity in your code or due to your operating system doing other stuff).
Additionally, if you're comparing the speed of your program with other people's programs, you should make sure to use the same parameter values (e.g. population size, number of generations to run for, etc.)
and ideally use comparable computers.

In choosing parameter values for your speed test, it's sometimes helpful to think about the way the speed of your program scales with different parameter choices.
Some parameters (e.g. the fitness ratio between two genotypes) might not affect the speed of your program at all.
Other parameters, like the number of generations you run for, might increase the amount of time a run takes by a constant amount for every increment (i.e. every generation takes a fixed amount of time so every additional generation you run for increases the time by a constant amount).
Finally, some parameters may scale in a worse way. For instance, if you have an agent-based model in which every agent must be compared with every other agent (this comes up in a lot of ecological models),
the run time will scale with the square of the population size. This is a big topic in computer science that we don't have time to go into all the details of,
but if you want to know more look up ["big-O notation"](https://rob-bell.net/2009/06/a-beginners-guide-to-big-o-notation/).

### Profiling specific parts of your program

If you want to actually speed up your code, it's useful to know which parts are taking the most time. These are the parts to
optimize, because they're where you can get the biggest gains. A 2x speed-up to code that runs for 5 minutes saves more time 
than a 100x speedup to code that only takes 1 second to begin with. Profiling allows you to identify these spots where
optimization could be beneficial

If you're using Python, I reccomend [SnakeViz](https://jiffyclub.github.io/snakeviz/) for visualizing the speed of various
components of your code. The SnakeViz documentation has a great guide both to profiling your code and visualizating the results.

Note that in Python profiling is not a substitute for measuring the overall run-time of your code (e.g. using `time`). Generating
the profiling information takes time, so your code will take longer to run while you're profiling it than it would normally.
Profiling tells you the relative amounts of time spent in different parts of your program, not the total amount of time you
can expect it to run for (measuring the total amount of time is usually called "benchmarking").

### Testing the speed of small chunks of Python code

As you're optimizing code, it's often useful to test out the speed of various python functions in a given context.
The [timeit](https://docs.python.org/3/library/timeit.html) module is a great way to do this!


