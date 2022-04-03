---
title: "HPC"
linkTitle: "HPC"
weight: 4
description: >
  High Performance Computing
---

## Overview

Oracle cloud supports High Performance Computing and makes it very easy to setup
your own HPC cluster in the cloud

## Configure HPC cluster

Download the Terraform configuration from here as a zip file:
https://github.com/oracle-quickstart/oci-hpc/releases/tag/v2.8.0.5

Then go to `Stacks` under `Resource Manager`:
![image](https://user-images.githubusercontent.com/4021595/161415757-409d264d-39e0-41f0-8bb0-3b5adc53abde.png)

Choose `My Configuration` and upload the zip file.
![image](https://user-images.githubusercontent.com/4021595/161415784-56f78544-fa20-48de-ae7c-89f2154f5e58.png)


Accept the defaults and add your public SSH key, disable `Use cluster network` (this is for MPI and not required for most people):
![image](https://user-images.githubusercontent.com/4021595/161415850-906a7cbf-8243-4df4-94b9-1dac7fcb1225.png)

This will then create a custom HPC for your project (it will take a couple of minutes to complete).

Once everything is done you find the bastion IP (the "head node" or "login node") under Outputs: 
![image](https://user-images.githubusercontent.com/4021595/161416418-6fcf7712-646d-48ea-9861-743fd679ba28.png)

Once logged in, you can create users using the `cluster` command:
```
cluster user add test
```

These users can then login using a password.

More information can be found here:
https://github.com/oracle-quickstart/oci-hpc

## Advanced: Use MPI networking

Your first need to request access to those resources with this
[form](./../../docs/request).

Then follow the above instructions, but leave `Use cluser network` activated.
