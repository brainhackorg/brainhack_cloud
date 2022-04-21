---
title: "Kubernetes"
linkTitle: "Kubernetes"
weight: 100
description: >-
     A description how to get started with Kubernetes on Oracle Cloud
---

## Overview
Kubernetes enables running and orchestrating multiple software containers.

## Setup Kubernetes Cluster on Oracle Cloud using OKE
Search for `OKE` in the menu and then go to `Kubernetes Clusters (OKE)` under `Containers & Artifacts`:
![image](https://user-images.githubusercontent.com/4021595/164392430-231652d9-abf3-4cc0-ac90-4b2dd5c000ea.png)

Select the region where you would like to create the Cluster:
![image](https://user-images.githubusercontent.com/4021595/164392545-765fc2bd-50c1-460f-9615-213b81d3ae54.png)

Select the compartment where you would like to create the Cluster:
![image](https://user-images.githubusercontent.com/4021595/164392623-6bfc0561-7d6c-4e32-aa70-fef8259961bc.png)

`Quick Create` is great and gives a good starting point that works for most applications:
![image](https://user-images.githubusercontent.com/4021595/164392731-1f23834c-9be3-40f8-b2c9-24ea65c13fc4.png)

The defaults are sensible, but Public Workers make it easier to troubleshoot things in the beginning:
![image](https://user-images.githubusercontent.com/4021595/164392860-eb050636-e820-46e2-a98a-cfdaf45a9281.png)

Select the shape you like and 1 worker is good to start (can be increased and changed later!)
![image](https://user-images.githubusercontent.com/4021595/164393030-561bf2d1-1af2-408b-9e38-51802efdd976.png)

Under advanced you can configure the boot-volume size:
![image](https://user-images.githubusercontent.com/4021595/164393130-65e48e2e-605f-4d6f-9fe3-78518fba0d14.png)

Add an SSH key for troubleshooting worker nodes:
![image](https://user-images.githubusercontent.com/4021595/164393220-77da1837-5407-476c-a971-1b3dc69b3d14.png)

Review everything and then hit `Create` and it should go and set-up everything :)
![image](https://user-images.githubusercontent.com/4021595/164393349-2b34f99a-5ea0-462e-95c4-4dd2f6d7c16e.png)

That's how easy it can be to setup a whole Kubernetes cluster :) Thanks OCI team for creating OKE!
![image](https://user-images.githubusercontent.com/4021595/164393450-1abc92f1-7dad-4e77-a5b5-0c72e7b0c08f.png)

It will take a few minutes until everything is up and running -> Coffee break?


## Customizing nodes using Init-scripts

