# Examples of PVS-Studio integration in Makefile

Several examples in this repository will help you learn to properly integrate [PVS-Studio](https://www.viva64.com/en/pvs-studio/) in build systems on the example of *Makefile*.

This way of using the analyzer has the following advantages:

* Full control over the analyzer settings;
* Incremental analysis of files;
*	Parallel analysis of files.

Analyzer settings that are the same for all the project files, can be stored in a separate file. *PVS-Studio.cfg* file is used in test projects for this purpose. 

### Analysis without integration (Linux only)

This utility requires the [strace](http://man7.org/linux/man-pages/man1/strace.1.html) utility.

This can be built with the help of the command:

```
pvs-studio-analyzer trace -- make
```

You can use any other build command with all the necessary parameters instead of make, for example:

``
pvs-studio-analyzer trace -- make debug
``

After you build your project, you should execute the commands:

```
pvs-studio-analyzer analyze -o /path/to/project.log -e /path/to/exclude-path -j<N>
plog-converter -a GA:1,2 -t tasklist -o /path/to/project.tasks /path/to/project.log
```

Analyzer warnings will be saved into the specified *project.tasks* file.

If your project isn't CMake or you have problems with the *strace* utility, you may try generating the file *compile_commands.json* with the help of the [Bear](https://github.com/rizsotto/Bear) utility. This file will help the analyzer to check a project successfully only in cases where the environment variables don't influence the file compilation.

#### If you use cross compilers

In this case, the compilers may have special names and the analyzer will not be able to find them. To analyze such a project, you must explicitly list the names of the compilers without the paths:

```
pvs-studio-analyzer analyze ... --compiler COMPILER_NAME --compiler COMPILER_NAME ...
plog-converter ...
```

Also, when you use cross compilers, the directory with the header files of the compiler will be changed.  It's necessary to exclude such directories from the analysis with the help of ```-e``` flag, so that the analyzer doesn't issue warnings for these files.

```
pvs-studio-analyzer ... -e /path/to/exclude-path ...
```

There shouldn't be any issues with the cross compilers during the integration of the analyzer into the build system.
