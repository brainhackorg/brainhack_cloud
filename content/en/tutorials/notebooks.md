---
title: "Notebooks"
linkTitle: "Notebooks"
weight: 3
description: >
  Notebook service
---

## Overview

The notebook service is like Google Colab, but without the time or resource
limitations.

## Starting a notebook environment
Select the geographic region where you want to run this (e.g. closest to you). 

!!! Important: If you want to use GPUs you need to select a region that has GPUs availabe!!!!
1) Nvidia GPUs V100 are available in Tokyo, London, Seoul
2) Nvidia GPUs P100 are available in Frankfurt 
3) Nvidia GPUs V100 AND P100 are available in Ashburn
4) CPU only instances are availabe in Sydney, Zurich, Stockholm, Singapore, Hyderabat, Marseille, Santiago, Toronto, Sao Paulo

![image](https://user-images.githubusercontent.com/4021595/159638803-174b68ad-c545-4539-8d01-b0952a0e7de4.png)


Then search for `Data Science` under `Machine Learning`
![image](https://user-images.githubusercontent.com/4021595/159638307-552138c0-01c1-43d3-823c-3a783e03ef5d.png)


then select your project compartment (in this example `testproject`)
![image](https://user-images.githubusercontent.com/4021595/159638417-30355f09-c965-4848-859a-ed04e49bf94f.png)

then click on create project:
![image](https://user-images.githubusercontent.com/4021595/159638445-11f09df9-1000-4703-8219-143a382e1d20.png)

enter a `Name` and a `Description`
![image](https://user-images.githubusercontent.com/4021595/159638513-b46da7b4-a401-4df4-b0ca-c3df613750b8.png)

then click `Create notebook session`
![image](https://user-images.githubusercontent.com/4021595/159638582-355145bb-ee9e-45d0-979e-902eb39b4565.png)

Name the notebook session and select which resources you need:
![image](https://user-images.githubusercontent.com/4021595/159638677-41a3239d-0e86-4159-8f5c-3ffcede23bdf.png)

Set how much disk space you want under `Block storage size (in GB)`, leave the Default networking and hit `Create`

It will now create everything for you:
![image](https://user-images.githubusercontent.com/4021595/159638967-1d69f18c-211e-4981-9709-b62514998de4.png)

Once this is done (it will take 2-3minutes), you can open the notebook environment with a click on `Open`:
![image](https://user-images.githubusercontent.com/4021595/159639682-9ce78519-2e7d-4886-b075-d168876d3711.png)

Then you have to log in - leave Tenancy as `brainhack` and click `Continue`:
![image](https://user-images.githubusercontent.com/4021595/159639819-38bda64e-61a3-4fcd-b4e4-2c55a6c8bb85.png)

and you have a fully configured notebook environment :)
![image](https://user-images.githubusercontent.com/4021595/159640023-b03a4e34-968f-4a78-963d-067cb82eb3ec.png)

The notebook environment uses Oracle Linux as a base image, so if you want to install additional packages use:
```
sudo yum update
sudo yum install ...
```

Hint for collaborating with multiple people: Multiple users can login to the same notebook system and work on separate notebooks simultaneously, but avoid editing the same notebook file - otherwise you risk overwriting your changes:
![image](https://user-images.githubusercontent.com/4021595/159642888-84589148-ed12-42fc-9282-dac7d3b07d5d.png)


## Clean up for the day
When completed for the day, you can save costs (especially important when using GPUs!) by deactivating the environment:

Close the window and hit `Deactivate`
![image](https://user-images.githubusercontent.com/4021595/159640588-874f2d3f-1123-41eb-98d1-c5d5b4c222c3.png)

This will shut down the compute instances but keep your data - so if you want to continue later, a click on `Activate` will bring everything back :)
![image](https://user-images.githubusercontent.com/4021595/159640942-5c8c1599-9e25-45c2-89eb-7de75b4a8b1e.png)

When reactivating you could even change the resources provided for the environment (e.g. adding a GPU or changing to a CPU only environment to save costs) :)

## Clean up for good
If you don't need the notebook environment anymore you can delete everything (including the data) by `More Actions` -> `Delete`

A quick confirmation and a click on `Delete` will remove *everything*:
![image](https://user-images.githubusercontent.com/4021595/159643054-942a61de-91eb-4b4c-adf4-6e3ba4a0f791.png)





