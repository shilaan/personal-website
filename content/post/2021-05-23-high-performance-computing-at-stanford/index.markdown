---
title: High Performance Computing at Stanford
author: Shilaan Alzahawi
date: '2021-05-23'
slug: high-performance-computing-at-stanford
categories: 
  - computing
tags: []
subtitle: 'A step-by-step guide to using ClusterJob and Sherlock to run massive computational experiments at Stanford'
summary: 'A step-by-step guide to running massive computational experiments using ClusterJob and Sherlock, aimed at the Stanford community.'
authors: 
- admin
lastmod: '2021-05-23T12:20:23-07:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
---

{{< toc >}} 

In this tutorial, I'll walk through the process of running a large computational experiment using two tools: [ClusterJob](https://clusterjob.org) and [Sherlock](http://sherlock.stanford.edu). ClusterJob is an automation system for high-throughput reproducible computations, created by [Hatef Monajemi](https://datascience.stanford.edu/people/hatef-monajemi) and [David L. Donoho](https://statistics.stanford.edu/people/david-donoho). Sherlock is Stanford's High Performance Computing (HPC) cluster.  

You can also find [an exact replica of this tutorial](https://stats285shilaan.netlify.app/post/high-performance-computing-with-clusterjob-and-sherlock/) on a [separate website I created](https://stats285shilaan.netlify.app) for *Statistics 285: Massive Computational Experiments, Painlessly*, a course taught at Stanford University in Spring 2021. Note that Sherlock is, unfortunately, only accessible to the Stanford community. 

My goal is to provide a relatively painless introduction to High Performance Computing at Stanford. 

## Step 1: Setting up Sherlock
  
### Create an account   
{{< cta cta_text="Request a Sherlock account" cta_link="https://www.sherlock.stanford.edu" cta_new_tab="true" >}}

If you don't already have a Sherlock account, now's the time to request one. After creating an account, you'll want to set up your access credentials using SSH keys, which allows for remote communication between your local machine and the cluster.  

### Set up SSH connection

To set up your connection,  check if you already have SSH keys on your machine. You can do this in two ways:  
☞︎ Go to your home directory and navigate to the hidden[^1] `.ssh` folder   
☞︎ Go to your terminal and enter `ls -al ~/.ssh` 

If you see a `.pub` file, you already have SSH keys set up. If you don't, run the following in your terminal:  
```
ssh-keygen -t rsa -C "your_email@example.com"
```  

Now, all you need to do is copy your keys over to the remote cluster.

[^1]: On Mac, you can view your hidden folders using `Command + Shift + .`  
 
```
ssh-copy-id your-username@sherlock.stanford.edu #run this in your terminal
```   

Next, add the following to your `~.ssh/config` file to avoid  Two-Factor Authentication every time you access the cluster, either from your local machine or through ClusterJob. 

```
Host sherlock sherlock?? sherlock.stanford.edu sherlock??.stanford.edu
 	ControlMaster auto
 	ControlPath ~/.ssh/%r@%h:%p
 	ControlPersist yes #this enables direct access to sherlock using your terminal

Host login.sherlock.stanford.edu
	ControlMaster auto
	ControlPath ~/.ssh/%l%r@%h:%p
 	ControlPersist yes #this enables access to sherlock using ClusterJob
```  

{{< spoiler text="Show me how to edit my `~.ssh/config` file" >}}
Editing your `~.ssh/config` file can be done in two ways:  
☞︎ Directly, by navigating to `.ssh > config` from your home directory  
☞︎ Through your terminal, by running: 

```
cd ~/.ssh  #change directory to the .ssh folder
open config  
```  
{{< /spoiler >}}



You can now check whether your connection works by trying to connect directly to the server. In your terminal, run:
```
ssh your-username@sherlock.stanford.edu 
```  
The first time you do this, you'll likely get a warning like this:
```
The authenticity of host 'login.sherlock.stanford.edu' can't be established.
ECDSA key fingerprint is SHA256:eB0bODKdaCWtPgv0pYozsdC5ckfcBFVOxeMwrNKdkmg.
Are you sure you want to continue connecting (yes/no)?
```
Simply type 'yes' and proceed. After completing these steps, your Sherlock account should be ready to go! 🎉 

## Step 2: Setting up ClusterJob

### Create an account   
{{< cta cta_text="Create a ClusterJob account" cta_link="https://clusterjob.org/register.php" cta_new_tab="true" >}}

Set up an account with ClusterJob using your `@edu` email account. Take note of your chosen ClusterJob ID and the ClusterJob Key assigned to your account.   

### Install ClusterJob

In your terminal, run:  
```
git clone https://github.com/monajemi/clusterjob.git ~/CJ_install #clones CJ from GitHub
sudo cpan -i Data::Dumper Data::UUID FindBin File::chdir File::Basename File::Spec IO::Socket::INET IO::Socket::SSL Getopt::Declare  Term::ReadLine JSON::PP JSON::XS Digest::SHA Time::Local Time::Piece Moo HTTP::Thin HTTP::Request::Common JSON URI #installs perl dependencies
alias cj='perl ~/CJ_install/src/CJ.pl'; #builds an alias for CJ
``` 

### Set up SSH configuration

You just installed a `CJ_install` folder to your home directory. In it, you will find two important files that you'll need to edit: `cj_config` and `ssh_config`.

In `cj_config`, you'll provide your ClusterJob ID (the username you chose) and the ClusterJob Key you received when creating your account.

Copy the following into the `cj_config` file:

```
CJID	your-id #edit this	 

CJKEY	
eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiJ9.eyJhZG1pbiI6MCwiZCI6eyJ1aWQiOiJzaGlsYWFuIiwiY2pwYXNzY29kZSI6IjJmYWQ2OWU1YjZlNDQxNDE2NjhhOTIxZThiMmNlNTYwIn19.P60piuQFOmzny9dFmwWoWDGeNrtsi6UHl_16OIdoICa-C6Y8KeGadT6pcMJvyKLlBs163rR_p1CXkm33l6L8fhH9tsJGG3UN4cMocWsVeWH_ORfZsdNvuWa24IO2Yh7MPMTj067e9UodDcOYe7N2swu9eWfvC82YBk7Ubna3ZDnHi4icK06exK1_mIj8jv0fDzHS4m0eWd5u0Sg1YecMp9YXU3DEc_l3Hxroyc_qnfVmK9WhiDTfAx6ZYoHxFF2VecWVsOB6-Pq6cjYKKw7BQIiLbQ0VLIZmwjX3QiQRTvi6vX4vsfwxHTvsNKGE_L2ru9NAfcuRigX1mgOCLBwU9g #edit this	
	
	
SYNC_TYPE	manual	
SYNC_INTERVAL	300
```  

{{% callout warning %}}
The `cj_config` file is somehow very sensitive to spacing and line breaks. Make sure you add linebreaks, like in the example above, or you'll get obscure error messages when running ClusterJob.
{{% /callout %}}

In `ssh_config`, you'll provide information about the Sherlock cluster. Copy the following into the `ssh_config` file:

```
[sherlock2]
Host	login.sherlock.stanford.edu
User	your-username	#edit this to your own username
Bqs	SLURM
Repo	/scratch/users/your-username/CJRepo_Remote #edit this
Python		python/3.8.8
Pythonlib	IPython:pandas:numpy:libgcc:scipy:matplotlib:cvxpy:-c conda-forge
Alloc		--time 48:00:00 --mem 32G
R	R/3.4.0
Rlib	ggplot2
[sherlock2]
```  

{{< spoiler text="Show me how to edit my `cj_config` and `ssh_config` files" >}}
Again, you can edit these two files through your terminal or manually:  
☞︎ Directly navigate to `CJ_install > cj_config/ssh_config`  from your home directory   
☞︎ Through your terminal, run:  
```
cd ~/CJ_install
open cj_config 
open ssh_config
```  
{{< /spoiler >}}

We're ready to check if everything is working correctly. In your terminal, run:  
```
cj init #initialize your CJ agent
cj who #check if the agent is installed
cj update #update to newest version
```  

## Step 3: Submit your first job ⚒︎

Here's where the real benefit of ClusterJob comes in. To run a job on Sherlock, you normally have to write a job submission script describing your resource request and submission options. ClusterJob automates this process and does it for you! No need to learn anything about Sherlock's job scheduler, [Slurm](https://slurm.schedmd.com).   

### Run your first serial computation

The `CJ_install` folders comes with some example scripts to run, so let's give those a try. 

{{< spoiler text="Show me the script I'm about to run" >}}
```
# This is a test Python script for CJ
# Author: Hatef Monajemi June 11 2017
import numpy as np;
import csv;

SUID = 'monajemi'
file = SUID+'_results.csv';

Var0  = np.array([1,2,3]);
Var1  = [1,2];
with open('file.txt','w') as myfile:
    for i in range(len(Var0)):
        for j in range(len(Var1)):    # This is a comment
        # write to a text file for testing reduce
            with open(file,'a') as csvfile:
                resultswriter = csv.writer(csvfile,delimiter=',');
                resultswriter.writerow([i,j,Var0[i]+Var1[j] ]);
```
{{< /spoiler >}}

In your terminal, run:
```
cd ~/CJ_install/example/Python/ #change directory to the folder with Python example
cj run simpleExample.py sherlock2 -m “A message.” #run your first serial job!
```  

When you run the second command, starting with `cj run`, you'll get a message like this: 
```
CJmessage::initiating package 0df1b4e7
```  
In this case, `0df1b4e7` is your job ID (referred to by ClusterJob as `pid`, for process identifier); take note of it.

{{< spoiler text="Show me what my CJ messages should look like" >}}
```
CJmessage::initiating package 0df1b4e7
CJmessage::runing [simpleExample.py] on [sherlock2] with:
                alloc: --time 48:00:00 --mem 32G 
CJmessage::sending from: /Users/shilaan/CJ_install/example/Python
CJmessage::Creating/checking conda venv. This may take a while the first time...
CJmessage::Creating reproducible script(s) reproduce_simpleExample.py
CJmessage::compressing files to propagate...
CJmessage::sending 1.92 KB to: sherlock2:/scratch/users/shilaan/CJRepo_Remote/simpleExample
CJmessage::extracting package...
CJmessage::Submitting job...
CJmessage::1 job(s) submitted (24992894)
```
{{< /spoiler >}}

After you've successfully submitted your job, you can check its status by running:
```
cj state
```  

When your job is done, you'll see a message like this: 

```
pid 0df1b4e7aed70b1a367212f861729d0bc8fcfc29
remote_account: shilaan@login.sherlock.stanford.edu
job_id: 24992894 
state: COMPLETED 
```  

Now, another one of ClusterJob's benefits: it's really easy to get the results of your job back onto your local computer. Run: 
```
cj get 0df1b4e7 #replace with your own job id
```  

Now, you'll get the following message: 

```
CJmessage::Getting results from 'sherlock2'
CJmessage::Please see your last results in /Users/shilaan/CJ_get_tmp/0df1b4e7aed70b1a367212f861729d0bc8fcfc29
``` 

Your results are ready for viewing! 🥳 The mentioned folder will include your results, your original script, and a script that fully reproduces the results. 

### Run your first parallel computation

We can rerun the same job we just submitted, but this time do it in parallel. The script we ran included a for loop over 6 elements (or index combinations). Instead of running this script serially, we can run it in parallel: we can submit a separate job for each index combination. In other words, we'll submit 6 separate jobs to Sherlock. Again, ClusterJob will fully automate this process for you. 

In your terminal, simply run:  
```
cd ~/CJ_install/example/Python/ #change directory to the folder with Python example
cj parrun simpleExample.py sherlock2 -m “A message.” #run your first parallel job!
```  

Now, you should receive the following message: 
```
CJmessage::6/6 job(s) submitted
```  

{{< spoiler text="Show me what my CJ messages should look like" >}}
```
CJmessage::initiating package 30236535
CJmessage::parruning [simpleExample.py] on [sherlock2] with:
                alloc: --time 48:00:00 --mem 32G 
CJmessage::sending from: /Users/shilaan/CJ_install/example/Python
CJmessage::Creating/checking conda venv. This may take a while the first time...
CJmessage::Invoking Python to find range of indices. Please be patient...
                Checking command 'python' is available...
                python available.
                finding range of indices...
                Closing Python session!
CJmessage::no SLURM partition specified. CJ is using default partition: long,normal
CJmessage::Creating reproducible script(s) reproduce_simpleExample.py
CJmessage::compressing files to propagate...
CJmessage::sending 2.96 KB to: sherlock2:/scratch/users/shilaan/CJRepo_Remote/simpleExample
CJmessage::extracting package...
CJmessage::Submitting job(s)
CJmessage::6/6 job(s) submitted (24994033-24994039)
```
{{< /spoiler >}}

To get the results, we have to first reduce the results of our parallel run into a single file. To do this, we have to identify the file that contains our results -- in this case, `monajemi_results.csv` -- and include our job ID.
In your terminal, run:
```
cj reduce monajemi_results.csv 30236535 #change to your own job ID
```  
You'll be asked if you want to submit the reduce script to the queue via srun. This is recommended for big jobs, but in this case you can simple answer `n`. Afterwards, you'll get the following message: 
```
CJmessage::Reducing results done! Use "CJ get 30236535 " to get your results.
```  
{{< spoiler text="Show me what my CJ messages should look like" >}}
```
CJmessage::30236535
CJmessage::Checking progress of runs...
CJmessage::Reducing monajemi_results.csv
CJmessage::Do you want to submit the reduce script to the queue via srun? (recommneded for big jobs) Y/N?
n
system:ssh shilaan@login.sherlock.stanford.edu 'cd /scratch/users/shilaan/CJRepo_Remote/simpleExample/3023653586353cab1f4601074519eec3c34f9346; bash -l cj_collect.sh'
 SubPackage 1 Collected (16.67%)
 SubPackage 2 Collected (33.33%)
 SubPackage 3 Collected (50.00%)
 SubPackage 4 Collected (66.67%)
 SubPackage 5 Collected (83.33%)
 SubPackage 6 Collected (100.00%)
CJmessage::Reducing results done! Use "CJ get 30236535 " to get your results.
```
{{< /spoiler >}}
Note that the terminal is case-sensitive so we actually have to remove the capitalization from the suggestion and run `cj get 30236535`. Afterwards, you should get a message like this:    
```
CJmessage::Please see your last results in /Users/shilaan/CJ_get_tmp/3023653586353cab1f4601074519eec3c34f9346
```  

Navigate to the folder to see the results of your parallel computation! 

![](parallel.jpg)

## Acknowledgements
❤︎ Statistics 285 Teaching Team  
❤︎ Mahsa Lotfi and Andrew Donoho, who went above and beyond in helping me get set up  
❤︎ The [ElastiCluster and Clusterjob Tutorial](https://stats285.github.io/elasticlusterjob-tutorial.html), written by Mahsa Lotfi  
❤︎ The [ClusterJob documentation](https://clusterjob.org/documentation/)   
❤︎ The [Sherlock documentation](https://www.sherlock.stanford.edu/docs/overview/introduction/)

