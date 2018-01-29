Manage Job with HTCondor™
***************************

Quick Start Guide
==================

To users, HTCondor is a job scheduler. You give HTCondor a file containing commands that tell it how to run jobs. HTCondor locates a machine that can run each job within the pool of machines, packages up the job and ships it off to this execute machine. The jobs run, and output is returned to the machine that submitted the jobs.

At the beginning, we are going to run the traditional 'hello world' program. In order to demonstrate the distributed resource nature, we will produce a 'Hello DGX' message 3 times, where each time is its own job. Since you are not directly invoking the execution of each job, you need to tell HTCondor how to run the jobs for you. The information needed is placed into a submit file, which defines variables that describe the set of jobs.

1. Copy the text below, and paste it into file called hello-DGX.sub, the submit file, in your home directory on the submit machine::
    
    # hello-DGX.sub
    # My very first HTCondor submit file
    #
    # Specify the HTCondor Universe (vanilla is the default and is used
    #  for almost all jobs), the desired name of the HTCondor log file,
    #  and the desired name of the standard error file.  
    #  Wherever you see $(Cluster), HTCondor will insert the queue number
    #  assigned to this set of jobs at the time of submission.
    universe = vanilla
    log = hello-DGX_$(Cluster).log
    error = hello-DGX_$(Cluster)_$(Process).err
    #
    # Specify your executable (single binary or a script that runs several
    #  commands), arguments, and a files for HTCondor to store standard
    #  output (or "screen output").
    #  $(Process) will be a integer number for each job, starting with "0"
    #  and increasing for the relevant number of jobs.
    executable = hello-DGX.sh
    arguments = $(Process)
    output = hello-DGX_$(Cluster)_$(Process).out
    #
    # Specify that HTCondor should transfer files to and from the
    #  computer where each job runs. The last of these lines *would* be
    #  used if there were any other files needed for the executable to run.
    should_transfer_files = YES
    when_to_transfer_output = ON_EXIT
    # transfer_input_files = file1,/absolute/pathto/file2,etc
    #
    # Tell HTCondor what amount of compute resources
    #  each job will need on the computer where it runs.
    request_cpus = 1
    request_gpus = 1
    request_memory = 1GB
    request_disk = 1MB
    #
    # Tell HTCondor to run 3 instances of our job:
    queue 3

2. Now, create the executable that we specified above: copy the text below and paste it into a file called hello-DGX.sh::

    #!/bin/bash
    #
    # hello-chtc.sh
    # My very first DGX job
    #
    echo "Hello DGX from Job $1 running on `whoami`@`hostname`"

  When HTCondor runs this executable, it will pass the $(Process) value for each job and hello-DGX.sh will insert that value for "$1", above.

3. Now, submit your job to the queue using condor_submit::
    
    condor_submit hello-DGX.sub

  The condor_submit command actually submits your jobs to HTCondor. If all goes well, you will see output from the condor_submit command that appears as::

    Submitting job(s)...
    3 job(s) submitted to cluster 9.

4. To check on the status of your jobs, run the following command::

    condor_q

  The output of condor_q should look like this::

    -- Schedd: DGX-Lei : <146.95.214.135:9618?... @ 01/29/18 02:15:29
    OWNER     BATCH_NAME          SUBMITTED   DONE   RUN    IDLE  TOTAL JOB_IDS
    shuozhang CMD: hello-DGX.s   1/29 02:14      _      _      3      3 9.0-2

    3 jobs; 0 completed, 0 removed, 3 idle, 0 running, 0 held, 0 suspended

  You can run the condor_q command periodically to see the progress of your jobs. By default, condor_q shows jobs grouped into batches by batch name (if provided), or executable name. To show all of your jobs on individual lines, add the -nobatch option.

5. When your jobs complete after a few minutes, they'll leave the queue. If you do a listing of your home directory with the command ls -l, you should see something like::

    -rw-r--r-- 1 shuozhang shuozhang    0 Jan 29 02:14 hello-DGX_9_0.err
    -rw-r--r-- 1 shuozhang shuozhang   47 Jan 29 02:47 hello-DGX_9_0.out
    -rw-r--r-- 1 shuozhang shuozhang    0 Jan 29 02:14 hello-DGX_9_1.err
    -rw-r--r-- 1 shuozhang shuozhang   47 Jan 29 02:47 hello-DGX_9_1.out
    -rw-r--r-- 1 shuozhang shuozhang    0 Jan 29 02:14 hello-DGX_9_2.err
    -rw-r--r-- 1 shuozhang shuozhang   47 Jan 29 02:47 hello-DGX_9_2.out
    -rw-rw-r-- 1 shuozhang shuozhang 3425 Jan 29 02:47 hello-DGX_9.log
    -rw-rw-r-- 1 shuozhang shuozhang  115 Jan 29 02:13 hello-DGX.sh
    -rw-rw-r-- 1 shuozhang shuozhang 1416 Jan 29 02:14 hello-DGX.sub

  Useful information is provided in the user log and the output files.

  HTCondor creates a transaction log of everything that happens to your jobs. Looking at the log file is very useful for debugging problems that may arise.

  Congratulations. You've run your first jobs in the CHTC! The parts below are detailed information about how to use HTCondor to deal with your jobs.

HTCondor Commands
==================

HTCondor commands cheat-sheet: https://raggleton.github.io/condor-cheatsheet/

Submitting a Job: http://research.cs.wisc.edu/htcondor/manual/current/2_5Submitting_Job.html

Managing a Job: http://research.cs.wisc.edu/htcondor/manual/current/2_6Managing_Job.html

Run a docker Job: http://research.cs.wisc.edu/htcondor/manual/current/2_12Docker_Universe.html#sec:dockeruniverse
http://chtc.cs.wisc.edu/docker-jobs.shtml#image

Other important commands/procedures maybe useful:

1. To ensure that HTCondor is running, you can run::

    ps -ef | egrep condor_

  On a central manager machine that can submit jobs as well as execute them, there will be processes for::

    condor_master
    condor_collector
    condor_negotiator
    condor_startd
    condor_schedd

2. Change HTCondor configurations(do it only if you know the effects)::

    vi /etc/condor_config

3. Looking at the SchedLog if you met some problems in using of HPCondor::

    less /var/log/condor/SchedLog

4. Another way to debug::

    condor_q -analyze