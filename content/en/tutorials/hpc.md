---
title: "HPC"
linkTitle: "HPC"
weight: 4
description: >
  High Performance Computing
---

## Overview

Oracle cloud supports High Performance Computing and makes it very easy to setup
your own HPC cluster in the cloud. This tutorial here is a basic introduction to get your started. You can find an alternative setup (tailored at deep learning and GPUs here: https://docs.oracle.com/en/solutions/hpc-bare-metal-gpu-cluster/?source=:so:ch:or:awr::::Cloud&SC=:so:ch:or:awr::::Cloud&pcode=#GUID-F00DA828-106C-40CB-9279-B90D10807358)

## Before you get started
Consider if you actually need a High performance cluster (HPC) for your work. An HPC is a cluster consisting of multiple Machines and it uses a head-node  from where jobs are submitted to this cluster using for example slurm. If you have many jobs that need to be run independently than the setup described here will work well. A "real" HPC does more on top: There is a high-performance network between machines and it enables to run jobs that combine multiple machines (MPI). This would be needed if you have a problem that's so large that a single machine wouldn't be big enough. In this example here we build a cluster without this advanced networking. Most people will not need an HPC for their work and they should use a single virtual machine, because it requires considerably less setup work.

## Configure HPC cluster

Download the Terraform configuration from here as a zip file:
[https://github.com/oracle-quickstart/oci-hpc/releases/tag/v2.8.0.5](https://github.com/oracle-quickstart/oci-hpc/releases/tag/v2.9.2)

Make sure you selected the geographic region where you would like to create the resource.
![image](https://user-images.githubusercontent.com/4021595/157349780-69fdf973-d4aa-4850-9f49-8ecca369f399.png)

Then go to `Stacks` under `Resource Manager`:
![image](https://user-images.githubusercontent.com/4021595/161415757-409d264d-39e0-41f0-8bb0-3b5adc53abde.png)

In the `List Scope` drop down menu, select your project title.  Choose `Create Stack` and upload the zip file.
![image](https://user-images.githubusercontent.com/4021595/161415784-56f78544-fa20-48de-ae7c-89f2154f5e58.png)

Accept the defaults and add your public SSH key, disable `Use cluster network` (this is for MPI and not required for most people):
![image](https://user-images.githubusercontent.com/4021595/161415850-906a7cbf-8243-4df4-94b9-1dac7fcb1225.png)

Specify the number of compute nodes you would like the HPC to have using `Initial cluster size`.

This will then create a custom HPC for your project (it will take a couple of minutes to complete).

Once everything is done you find the bastion IP (the "head node" or "login node") under Outputs: 
![image](https://user-images.githubusercontent.com/4021595/161416418-6fcf7712-646d-48ea-9861-743fd679ba28.png)

If you selected the default options (make sure to setup your SSH keys: http://brainhack.org/brainhack_cloud/tutorials/vm/#create-a-public-key), you can now ssh into the HPC as follows:
```
ssh opc@ipbastion
```

Be aware that this "opc" account is the admin account of the cluster and should not be used to perform analyses. It is better to create a user account to perform the work in:

Once logged in with the opc account, you can create users using the `cluster` command:
```
cluster user add test
```

These users can then login using a password only.

There is a shared file storage (which can also be configured in size in the stack settings) in /nfs/cluster

More information can be found here:
https://github.com/oracle-quickstart/oci-hpc

## Configuring node memory

When you first submit jobs using `sbatch`, if you followed the above setup you may find you recieve the following error:
```
error: Memory specification can not be satisfied
```

This is happening as the `RealMemory` for each node (e.g. the amount of memory each compute node may use) has not yet been specified and defaults to a very low value. To rectify this, first work out how much memory to allocate to each node by running `scontrol show nodes` and looking at `FreeMem`.
![image](https://user-images.githubusercontent.com/4021595/176095292-c17e658b-0372-4c04-9f51-32d00c2e0f1c.png)

To change the `RealMemory`, you must edit the slurm configuration file (which may be found in `/etc/slurm/slurm.conf`). Inside the slurm configuration file you will find several lines which begin `NodeName=`. These specify the settings for each node. To fix the error, on each of these lines, add `RealMemory=AMOUNT` where `AMOUNT` is the amount of memory you wish to allow the node to use.
![image](https://user-images.githubusercontent.com/4021595/176095320-b9f55fb1-7365-4e8a-9a30-eee5117c12dc.png)

Once you have done this, you must reconfigure slurm by running the following command:
```
sudo scontrol reconfigure
```

## Advanced: Use MPI networking

Your first need to request access to those resources with this
[form](./../../docs/request).

Then follow the above instructions, but leave `Use cluser network` activated.
