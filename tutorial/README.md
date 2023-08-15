---
title: Tutorial
layout: docs
sidebar: ci
permalink: /tutorial/
---

# Tutorial

This is provided an example documentation. This comes from [snakemake-executor-flux](github.com/snakemake/snakemake-executor-flux).
For this tutorial you will need Docker installer.

[Flux-framework](https://flux-framework.org/) is a flexible resource scheduler that can work on both high performance computing systems and cloud (e.g., Kubernetes).
Since it is more modern (e.g., has an official Python API) we define it under a cloud resource. For this example, we will show you how to set up a "single node" local
Flux container to interact with snakemake using the plugin here. You can use the [Dockerfile](examples/Dockerfile) that will provide a container with Flux and snakemake
Note that we install from source and bind to `/home/fluxuser/snakemake` with the intention of being able to develop (if desired).

First, build the container:

```bash
$ docker build -f example/Dockerfile -t flux-snake .
```

Note that this currently uses a [custom branch](https://github.com/vsoch/snakemake/tree/add/executor-plugins) to install Snakemake.
We will add the plugin to `/home/fluxuser/plugin`, install it, and shell in as the fluxuser to optimally interact with flux.
After the container builds, shell in:

```bash
$ docker run -it flux-snake bash
```

And start a flux instance:

```bash
$ flux start --test-size=4
```

Go into the examples directory (where the Snakefile is) and run snakemake, targeting your executor plugin.

```bash
$ cd ./example

# This says "use the custom executor module named snakemake_executor_flux"
$ snakemake --jobs 1 --executor flux
```
```console
Building DAG of jobs...
Using shell: /bin/bash
Job stats:
job                         count    min threads    max threads
------------------------  -------  -------------  -------------
all                             1              1              1
multilingual_hello_world        2              1              1
total                           3              1              1

Select jobs to execute...

[Fri Jun 16 19:24:22 2023]
rule multilingual_hello_world:
    output: hola/world.txt
    jobid: 2
    reason: Missing output files: hola/world.txt
    wildcards: greeting=hola
    resources: tmpdir=/tmp

Job 2 has been submitted with flux jobid ƒcjn4t3R (log: .snakemake/flux_logs/multilingual_hello_world/greeting_hola.log).
[Fri Jun 16 19:24:32 2023]
Finished job 2.
1 of 3 steps (33%) done
Select jobs to execute...

[Fri Jun 16 19:24:32 2023]
rule multilingual_hello_world:
    output: hello/world.txt
    jobid: 1
    reason: Missing output files: hello/world.txt
    wildcards: greeting=hello
    resources: tmpdir=/tmp

Job 1 has been submitted with flux jobid ƒhAPLa79 (log: .snakemake/flux_logs/multilingual_hello_world/greeting_hello.log).
[Fri Jun 16 19:24:42 2023]
Finished job 1.
2 of 3 steps (67%) done
Select jobs to execute...

[Fri Jun 16 19:24:42 2023]
localrule all:
    input: hello/world.txt, hola/world.txt
    jobid: 0
    reason: Input files updated by another job: hello/world.txt, hola/world.txt
    resources: tmpdir=/tmp

[Fri Jun 16 19:24:42 2023]
Finished job 0.
3 of 3 steps (100%) done
Complete log: .snakemake/log/2023-06-16T192422.186675.snakemake.log
```

And that's it! Continue reading to learn more about plugin design, and how you can also design your own executor
plugin for use or development (that doesn't need to be added to upstream snakemake).
