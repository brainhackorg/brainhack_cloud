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


Accept the defaults, but disable `Use cluster network` (this is for MPI and not required for most people):
![image](https://user-images.githubusercontent.com/4021595/161415850-906a7cbf-8243-4df4-94b9-1dac7fcb1225.png)

This will then create a custom HPC for your project.

## Advanced: Use MPI networking

Your first need to request access to those resources with this
[form](./../../docs/request).

Then follow the above instructions, but leave `Use cluser network` activated.
