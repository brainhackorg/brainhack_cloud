---
title: "HPC"
linkTitle: "HPC"
weight: 4
description: >
  High Performance Computing
---

## Overview

Oracle cloud supports High Performance Computing and makes it very easy to setup
your own HPC cluster in the cloud. This tutorial here is a basic introduction to get your started. You can find an alternative setup (tailored at deep learning and GPUs here: [GPU cluster](https://docs.oracle.com/en/solutions/hpc-bare-metal-gpu-cluster/?source=:so:ch:or:awr::::Cloud&SC=:so:ch:or:awr::::Cloud&pcode=#GUID-F00DA828-106C-40CB-9279-B90D10807358))

## Before you get started
Consider if you actually need High Performance Computing (HPC) for your work. An HPC is a cluster consisting of multiple machines and it uses a head-node (here bastion host) from where jobs are submitted to this cluster using a job engine (for example slurm). If you have many jobs that need to be run independently than the setup described here will work well. A "real" HPC does more on top: There is a high-performance network between machines and it enables to run jobs that combine multiple machines (e.g. MPI). This would be needed if you have a problem that's so large that a single machine wouldn't be big enough. In this example here we build a cluster without this advanced networking. Most people will not need an HPC for their work and they should use a single [virtual machine](http://brainhack.org/brainhack_cloud/tutorials/vm/), because it requires considerably less setup work and easier to maintain.

## Configure HPC cluster

Download the Terraform configuration from here as a zip file:
[https://github.com/oracle-quickstart/oci-hpc/releases/tag/v2.9.2](https://github.com/oracle-quickstart/oci-hpc/releases/tag/v2.9.2)

Make sure you selected the geographic region where you would like to create the resource (it should be close to you for best latencies).
![image](https://user-images.githubusercontent.com/4021595/157349780-69fdf973-d4aa-4850-9f49-8ecca369f399.png)

Then go to `Stacks` under `Resource Manager`:
![image](https://user-images.githubusercontent.com/4021595/161415757-409d264d-39e0-41f0-8bb0-3b5adc53abde.png)

In the `List Scope` drop down menu, select your project compartment.  Click `Create Stack` and upload the zip file as a Terraform configuration source.

<img width="1043" alt="create stack" src="https://user-images.githubusercontent.com/4021595/184516975-a8188aa9-8337-4523-9111-fb7a1c7868ba.png">

give your cluster a name, but leave the default options for the rest:

<img width="846" alt="naming the cluster" src="https://user-images.githubusercontent.com/4021595/184517026-76cc8d32-b19d-48a4-b4c3-8b88d0131ba1.png">

Check that the cluster is being created in your compartment again and then hit `Next`


In `cluster configuration` you need to add your public SSH key for the opc admin account. Make sure to setup your SSH keys first [create a public key](http://brainhack.org/brainhack_cloud/tutorials/vm/#create-a-public-key)

<img width="852" alt="image" src="https://user-images.githubusercontent.com/4021595/184517096-e62de0bf-e535-4aaa-84fc-70001e142051.png">


In `Headnode options` you need to select an Availability Domain. It doesn't matter what you select there and the options will depend on the geographic region where you launch your HPC. You can keep the headnode default size, or you can select a different flavour:

<img width="849" alt="image" src="https://user-images.githubusercontent.com/4021595/184517208-247ffd57-56a6-44b2-a9a5-259ee728e00a.png">



In `Compute node options` you need to disable `Use cluster network` (this is for MPI and not required for most people. It requires special network hardware that's not available in every region. If you need MPI please get in touch and we can help you setting this up). Select a compute node size that fits your problem size. Drop the initial compute size node to 1, because we will scale the cluster using autoscaling.

<img width="829" alt="image" src="https://user-images.githubusercontent.com/4021595/184517285-b6805297-1a16-4f29-aa4c-816334b28e86.png">


In `Autoscaling` you should enable `scheduler based autoscaling`, `monitor the autoscaling` and disable `RDMA latency check` if you are not using MPI.

<img width="847" alt="image" src="https://user-images.githubusercontent.com/4021595/184517301-b08cf59e-f07f-42c4-865b-64a88e466274.png">



For `API authentication` and `Monitoring` leave the defaults:

<img width="851" alt="image" src="https://user-images.githubusercontent.com/4021595/184517316-b8a93508-17a7-44a6-8024-0a01f3e01a06.png">


For `Additional file system` accept the defaults:

<img width="848" alt="image" src="https://user-images.githubusercontent.com/4021595/184517726-d07ad10f-dc95-48ec-97fd-85e5ee19a6b9.png">


For `Advanced bastion options`, `Avanced storage options` and `Network options` you can accept the defaults:

<img width="836" alt="image" src="https://user-images.githubusercontent.com/4021595/184517368-589f1876-d703-4560-86c9-60cb23a0c711.png">



For `Software` enable `Install Spack package manager` in addition to the defaults:

<img width="843" alt="image" src="https://user-images.githubusercontent.com/4021595/184517390-d36d605d-e51b-46aa-84bf-140b2b711065.png">


Then hit `next` and on the next page scroll to the end and tick `Run apply`:

<img width="848" alt="image" src="https://user-images.githubusercontent.com/4021595/184517402-a02367f8-d3df-4436-9cf6-356994172b1e.png">

Then hit `Create`

This will then create a custom HPC for your project (it will take a couple of minutes to complete).

Once everything is done you find the bastion IP (the "head node" or "login node") under Outputs: 
![image](https://user-images.githubusercontent.com/4021595/161416418-6fcf7712-646d-48ea-9861-743fd679ba28.png)

You can now ssh into the HPC as follows:
```
ssh opc@ipbastion
```

Be aware that this "opc" account is the admin account with sudo access of the cluster and should not be used to perform analyses. It is better to create a user account to perform the work in:

Once logged in with the opc account, you can create normal cluster users using the `cluster` command:
```
cluster user add test
```

These users can then login using a password only and do not require an SSH key.

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

## Configuring X11 forwarding
If you want to use graphical aplications you need to install:
```
sudo yum install install mesa-dri-drivers xorg-x11-server-Xorg xorg-x11-xauth xorg-x11-apps mesa-libGL xorg-x11-drv-nouveau.x86_64 -y
```

```
sudo vi /etc/ssh/sshd_config
```

change to:
```
X11Forwarding yes 
X11UseLocalhost no
```

then
```
sudo systemctl restart sshd
```


## Troublehsooting: Editing a deployd stack fails
This can have many reasons, but the first one to check is:
```
Error: 409-Conflict, The Instance Configuration ocid1.instanceconfiguration.oc1.phx.aaaaaaaabycbnzxq4uskt4f7mklp4g4fcqk4m42aabj2r2fkchjygppdudua is associated to one or more Instance Pools.
```
This means that the Instance Pool blocks the terraform script. To get it back working you need to destroy the stack first and then rebuild it.

Another option is that the resource type you used is not supported:
```
Error: 400-InvalidParameter, Shape VM.Standard1.4 is incompatible with image ocid1.image.oc1..aaaaaaaamy4z6turov5otuvb3wlej2ipv3534agxcd7loajk2f54bfmlyhnq
Suggestion: Please update the parameter(s) in the Terraform config as per error message Shape VM.Standard1.4 is incompatible with image ocid1.image.oc1..aaaaaaaamy4z6turov5otuvb3wlej2ipv3534agxcd7loajk2f54bfmlyhnq
```
Here, I selected a shape that is too "small" and it fails. It needs at least VM.Standard2.4

## Installing Custom Software

If you don't want to use spack (or cannot) then a good strategy is to install under `/nfs/cluster`, add any relevant "bin" directories it to your path, and install there.
As an example we will install [go](https://go.dev/doc/install):

```bash
$ cd /nfs/cluster
$ wget https://go.dev/dl/go1.19.linux-amd64.tar.gz
$ sudo tar -C /nfs/cluster -xzf go1.19.linux-amd64.tar.gz
$ rm go1.19.linux-amd64.tar.gz
```

And then add the go bin to your bash profile (`vim ~/.bash_profile`) as follows:

```bash
export PATH=/nfs/cluster/go/bin:$PATH
```
and when you open a new shell or `source ~/.bash_profile` you should be able to see go on your path:

```bash
$ which go
/nfs/cluster/go/bin/go
```
```bash
$ go version
go version go1.19 linux/amd64
```

Further, since it's located in the /nfs/cluster directory, it will be available on other nodes! Here is how to see the other nodes you have:

```bash
$ sinfo
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
compute*     up   infinite      1   idle compute-permanent-node-941
```

And then shell into one, and also find the go binary.

```bash
$ ssh compute-permanent-node-941
Last login: Sun Aug 14 02:30:01 2022 from relative-flamingo-bastion.public.cluster.oraclevcn.com
$ which go
/nfs/cluster/go/bin/go
```

### Install Singularity

First, system dependencies. Follow the example above in [install custom software](#installing-custom-software) to install Go.
Next, install Singularity dependencies. These will need to be installed to each node.

```bash
sudo yum groupinstall -y 'Development Tools'
sudo yum install libseccomp-devel squashfs-tools cryptsetup -y
sudo yum install glib2-devel -y
```
Ensure Go is on your path (as shown above). Then install Singularity. We will install from source.

**Important** ensure you don't have anything (e.g., pkg-config) loaded from spack, as this can interfere with installing Singularity using system libs. Also note that installing with system libs is a workaround for spack singularity not working perfectly (due to setuid). This means you'll need to do these steps on each of your head login and worker nodes.

You can do the same with an official [release](https://github.com/sylabs/singularity/releases/).
Note that you don't need to compile this on the nfs node - you can compile it anywhere and make install
to /nfs/cluster.

```bash
$ git clone https://github.com/sylabs/singularity
$ cd singularity
$ git submodule update --init

# You can also set prefix to be it's own directory, e.g., /nfs/cluster/singularity-<version>
$ ./mconfig --prefix=/nfs/cluster
$ cd ./builddir
$ make
$ make install
```
Once you install, make sure you add the newly created bin to your path (wherever that happens to be). E.g.,
that might look like:

```bash
export PATH=/nfs/cluster/go/bin:/nfs/cluster/bin:$PATH
```
And then when you source your `~/.bash_profile` you can test:

```bash
$ which singularity
/nfs/cluster/bin/singularity
```

## Advanced: Use MPI networking

Your first need to request access to those resources with this
[form](./../../docs/request).

Then follow the above instructions, but leave `Use cluser network` activated and `RDMA options enabled`.
