ukko
====

Tools for Ukko cluster:
http://www.cs.helsinki.fi/en/compfac/high-performance-cluster-ukko

Usage:

    ./ukko-update

The script creates the following files:

  - `~/tmp/ukko/hostfile-X`: all hosts with load at most X
  - `~/tmp/ukko/loginfile-X`: the same information for Gnu Parallel


Using Gnu Parallel
------------------

Recommended setup:

  - Select one Ukko node as your "master" node. Below, we will use
    `ukko030.hpc.cs.helsinki.fi`.

  - SSH from your own computer to `melkinkari.cs.helsinki.fi`
  
  - Start `screen` on melkinkari
  
  - Inside screen, run `ssh ukko030.hpc.cs.helsinki.fi` to
    connect to the master node.

  - Then we will run Gnu Parallel on the master node.

Preliminaries:

  - Make sure that `hpcssh` is in your path.

  - Configure SSH public key authentication so that you can open
    SSH connections from the master node to any other Ukko node,
    without entering a password. For example, this should work as
    expected:

        username@ukko030:~$ hpcssh ukko031.hpc.cs.helsinki.fi hostname
        ukko031
        username@ukko030:~$
  
Run `ukko-update` to make sure the login file is up-to-date.

Now we are ready to do some computation; see below for an example.


Example
-------

In this example, we would like to run the command `example-compute`
with a large number of different command line parameters.

We will assume that you have cloned this Git repository to
`~/ukko`, and your current working directory is `~`.

First, run the following command to create a batch file:

    ukko/example-batch

Now you should have a batch file in `~/tmp/example/batch`. There
are 500 line in the file, one line per command:

    ukko/example-compute 0
    ukko/example-compute 1
    ukko/example-compute 2
    ukko/example-compute 3
    ...

Let us first make sure that the tool works without Gnu Parallel.
Run the following command:

    ukko/example-compute 1

It should finish soon, print one line of output, and also store the
result in the file ~/tmp/example/result-1.

Now let us run the batch file:

    parallel --sshloginfile tmp/ukko/loginfile-4 --joblog tmp/example/joblog < tmp/example/batch

This should finish fairly quickly. You will see 500 lines of
output. The results of the computation will be stored in
`~/tmp/example`, and you will also find the log file there.
If something goes wrong, study the log file.


Notes
-----

If you follow the above procedure, there will be only one process
running on each node simultaneously.

Use, for example, OpenMP to benefit from multiple cores.
