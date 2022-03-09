---
title: "Virtual machines"
linkTitle: "Virtual machines"
weight: 4
description: >
  Virtual Machines
---

## Overview

Running a Virtual machine on the Oracle cloud is the basis for many other things.

## Setup
Make sure you selected the geographic region where you would like to create the resource. Here I create it in the Home Region:
![image](https://user-images.githubusercontent.com/4021595/157349780-69fdf973-d4aa-4850-9f49-8ecca369f399.png)


Head to Compute -> Instances
![image](https://user-images.githubusercontent.com/4021595/157349177-12c719c9-0285-489f-9e42-9d6b82b520c0.png)

Check that you selected YOUR project compartment (testproject, is the example here - but you need to change this!) and click "Create Instance"
![image](https://user-images.githubusercontent.com/4021595/157349261-28eb8f1e-44ff-4939-9ec5-af8e1b37b90c.png)

You can name the instance and then select an Image (Oracle Linux is a good starting point as it has many tools installed that make it work very well on the Cloud) and select a Shape:
![image](https://user-images.githubusercontent.com/4021595/157349502-ac932a4e-82b1-410e-8785-56e6e9ae147b.png)

VM.Standard.E4.Flex is a good starting point:
![image](https://user-images.githubusercontent.com/4021595/157349484-eff7d3ec-3f5d-4a6d-b020-a93e63d30745.png)

The network setup has sensible defaults to start with:
![image](https://user-images.githubusercontent.com/4021595/157349579-b23e4815-2b65-44f9-9f46-53b496b60425.png)

Paste a public key to access this VM. If you don't have a public key yet - this is how you can create one (example in the cloudshell):

Open the Cloud Shell (this will take a few seconds the first time):
![image](https://user-images.githubusercontent.com/4021595/157349825-f77d4e77-aba9-4578-beb8-fde536332d5b.png)

run `ssh-keygen` to create private/public key pairs (the defaults are sensible, so just hit Enter a few times):
![image](https://user-images.githubusercontent.com/4021595/157350101-0b01d89a-f839-473d-8d6e-6c312e1cfb16.png)

Now print the public key with `cat ~/.ssh/id_rsa.pub` and copy it to the clipboard:
![image](https://user-images.githubusercontent.com/4021595/157350191-67f4bc21-5c62-4b28-b2e0-3076cec65c60.png)

Important: Never share the private key with anyone, which is in id_rsa!

paste it in the Add SSH keys section:
![image](https://user-images.githubusercontent.com/4021595/157350315-ee920db6-0bf2-45de-9dd3-3f96c9bbc8fc.png)


You can specify a custom boot volume size, but you can also increase this easily later (note: it's not possible to shrink a volume! Only increasing the size is possible, so start small and increase when needed). The rest of the defaults is sensible:
![image](https://user-images.githubusercontent.com/4021595/157350468-7eac6e01-bbc7-48e3-9fbd-fc0ac01b2476.png)

This will now create the machine:
![image](https://user-images.githubusercontent.com/4021595/157350580-aaf564d0-8619-4122-82be-12bce8b3c47d.png)

## Connect to Instance



## Expand disk

## Login via Cloud shell

