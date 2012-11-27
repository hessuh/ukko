ukko
====

Tools for Ukko cluster:
http://www.cs.helsinki.fi/en/compfac/high-performance-cluster-ukko

Usage:

    ./ukko-update

The script creates the following files:

  - `~/tmp/ukko/hostfile-X`: all hosts with load at most X
  - `~/tmp/ukko/loginfile-X`: the same information for Gnu Parallel

If you have `hpcssh` in your path, you should be able to use the login
files as follows:

    parallel --sshloginfile LOGINFILE --joblog LOGFILE < BATCHFILE

where

  - LOGINFILE: for example, ~/tmp/ukko/loginfile-2
  - LOGFILE: where to store the log file
  - BATCHFILE: commands to run.

