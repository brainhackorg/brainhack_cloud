---
title: "Virtual machines"
linkTitle: "Virtual machines"
weight: 1
description: >
  Virtual Machines
---

## Overview

Running a Virtual machine on the Oracle cloud is the basis for many other
things.

## Setup

Make sure you selected the geographic region where you would like to create the
resource.

Here I create it in the Home Region.

![image](https://user-images.githubusercontent.com/4021595/157349780-69fdf973-d4aa-4850-9f49-8ecca369f399.png)

Head to `Compute -> Instances`

![image](https://user-images.githubusercontent.com/4021595/157349177-12c719c9-0285-489f-9e42-9d6b82b520c0.png)

Check that you selected YOUR project compartment (testproject, is the example
here - but you need to change this!) and click `Create Instance`

![image](https://user-images.githubusercontent.com/4021595/157349261-28eb8f1e-44ff-4939-9ec5-af8e1b37b90c.png)

### Selecting an image and a shape

You can name the instance and then select an Image (Oracle Linux is a good
starting point as it has many tools installed that make it work very well on the
Cloud) and select a Shape.

![image](https://user-images.githubusercontent.com/4021595/157349502-ac932a4e-82b1-410e-8785-56e6e9ae147b.png)

VM.Standard.E4.Flex is a good starting point

![image](https://user-images.githubusercontent.com/4021595/157349484-eff7d3ec-3f5d-4a6d-b020-a93e63d30745.png)

The network setup has sensible defaults to start with

![image](https://user-images.githubusercontent.com/4021595/157349579-b23e4815-2b65-44f9-9f46-53b496b60425.png)

Paste a public key to access this VM.

### Create a public key

If you don't have a public key yet - this is how you can create one (for example in
the cloudshell)

Open the Cloud Shell (this will take a few seconds the first time)

![image](https://user-images.githubusercontent.com/4021595/157349825-f77d4e77-aba9-4578-beb8-fde536332d5b.png)

Run `ssh-keygen` to create private/public key pairs (the defaults are sensible,
so just hit `Enter` a few times)

![image](https://user-images.githubusercontent.com/4021595/157350101-0b01d89a-f839-473d-8d6e-6c312e1cfb16.png)

Now print the public key with `cat ~/.ssh/id_rsa.pub` and copy it to the
clipboard.

![image](https://user-images.githubusercontent.com/4021595/157350191-67f4bc21-5c62-4b28-b2e0-3076cec65c60.png)

{{% alert title="Warning" color="warning" %}}
Never share the private key with anyone, which is in `id_rsa`!
{{% /alert %}}

Paste it in the Add SSH keys section

![image](https://user-images.githubusercontent.com/4021595/157350315-ee920db6-0bf2-45de-9dd3-3f96c9bbc8fc.png)

{{% alert title="Warning" color="warning" %}}
Make sure to generate the key pair via `ssh-keygen` in the cloud shell (or in your local shell). The option `generate a key pair for me` will not work and result in generating a .key file, which is used for  connecting to APIs and does not work for `ssh` connections.
{{% /alert %}} 

### Disk size

You can specify a custom boot volume size, but you can also increase this easily
later.

**Note:** it's not possible to shrink a volume! Only increasing the size is
possible, so start small and increase when needed. Increasing the size is even possible while the instance is running and will not interrupt your work :)

The rest of the defaults are
sensible.

![image](https://user-images.githubusercontent.com/4021595/157350468-7eac6e01-bbc7-48e3-9fbd-fc0ac01b2476.png)

### Create the VM

This will now create the machine.

![image](https://user-images.githubusercontent.com/4021595/157350580-aaf564d0-8619-4122-82be-12bce8b3c47d.png)

## Connect to Instance

You can now use an SSH client on your computer to connect to the Instance, or
the cloudshell.

You find the connection details in.

![image](https://user-images.githubusercontent.com/4021595/157351454-d888fd88-130b-40fe-986f-46e451d569ae.png)

So in this case you would connect to your instance by typing.

```bash
ssh opc@130.61.212.59
```

Accept the fingerprint and you should be connected.

![image](https://user-images.githubusercontent.com/4021595/157351631-ea6d6e0e-bf8c-4816-99bd-b92b89b033cd.png)

## Expand disk

By default, the instance will not utilize the whole disk size!

You can check with `df -h`.

![image](https://user-images.githubusercontent.com/4021595/157351914-38855be5-9b2a-4883-bfc4-768890fd1f8e.png)

But it can expand the disk with the following commands.

```bash
sudo dd iflag=direct if=/dev/oracleoci/oraclevda of=/dev/null count=1
echo "1" | sudo tee /sys/class/block/`readlink /dev/oracleoci/oraclevda | cut -d'/' -f 2`/device/rescan
sudo /usr/libexec/oci-growfs -y
```

![image](https://user-images.githubusercontent.com/4021595/157352261-3d30774a-4052-4e12-846a-133f4f8ffa98.png)

Now it's using the full volume.

![image](https://user-images.githubusercontent.com/4021595/157352396-a3a4a3a9-38a7-49d5-a18b-880c058bbc2d.png)

## Terminate the instance

If you don't need the machine anymore, you can `Stop` it (you don't pay for the compute anymore, but the disk stays and you can start it back up later) or `Terminate` it (everything gets removed, including the boot volume if you want to):

![image](https://user-images.githubusercontent.com/4021595/157352624-f3b2b358-1f3d-4388-bf2f-6fc417d4a439.png)

To cleanup the storage as well, you can select
`Permanently delete the attached boot volume` and click `Terminate Instance`.

![image](https://user-images.githubusercontent.com/4021595/157352698-9788c610-b5f1-43bf-95e7-ca444e8813fb.png)
