# spark-slurm

Super simple script to start a spark cluster within an existing slurm allocation.

All existing machines/tasks are used. The first task starts the spark master, all
other tasks connect to the master.

Based on [this stackoverflow post](http://serverfault.com/a/776688). It has been
simplified for robustness (no check for `srunning`, which didn't work for me),
some error checking and other minor changes.

# Installation

None. Just copy the script to some location you can access.

# Usage

Make sure that `$SPARK_HOME` is defined and points to your spark installation and
`$JAVA_HOME` is defined as well and points to a working java installation.
Then just execute `$ spark-slurm` in some slurm allocation. Some scenarios:

From within interactive slurm allocation:
```bash
$ salloc -N <number of nodes>
salloc: Granted job allocation 7093
$ spark-slurm
starting master:  spark://<your master>
starting worker
...
# blocking call, does not return by design
```

As non-interactive batch job:
```bash
$ sbatch -N <number of nodes> spark-slurm
Submitted batch job 7094
$ squeue
             JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
              7094      defq spark-sl   <user>  R      14:58      1 <nodelist>
# shell is available, job executes in backgroung
```

After spark-slurm successfully started a spark cluster, look for the line starting with `starting master: ....`
and use that url for your spark-shell/scripts etc.

# Issues

* Setting worker cores/memory has not been tested
* Always sets worker dir, local dir, and log dir. Maybe this should be optional

# Meta

Feel free to send suggestions, bugs, etc.

Alexander Matz - a.matz.1988@gmail.com

Distributed under MIT license. see `LICENSE` for more information.
