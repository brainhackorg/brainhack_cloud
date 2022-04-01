---
title: "Datascience setup"
linkTitle: "Datascience setup"
weight: 100
description: >-
     How to set up the datascience environments (only needs to be done once).
---

## Step 1) Create VCN and Subnets
Create a VCN and subnets using Virtual Cloud Networks > Start VCN Wizard > VCN with Internet Connectivity option.
The Networking Quickstart option automatically creates the necessary private subnet with a NAT gateway.
![image](https://user-images.githubusercontent.com/4021595/159605361-d8f4bf9b-e7c7-445d-89ce-8daee43801f1.png)

## Step 2) Create a Dynamic Group
Create a dynamic group with the following matching rule:
ALL { resource.type = 'datasciencenotebooksession' }
![image](https://user-images.githubusercontent.com/4021595/159605506-d0579916-2cce-461b-9d54-f703a53b7ad1.png)

created as "datascience-notebooks-dynamic-group"

## Step 3) Create Policies
Create a policy in the root compartment with the following statements:

### 3.1 Service Policies
allow service datascience to use virtual-network-family in tenancy

-> created in root compartment as "datascience"

### 3.2 Non-Administrator User Policies
allow group `data-scientists` to use virtual-network-family in tenancy
allow group `data-scientists` to manage data-science-family in tenancy
where `data-scientists` represents the name of your user group -> setup as "projects" group

  
### 3.3 Dynamic Group Policies
allow dynamic-group `dynamic-group` to manage data-science-family in tenancy
where `dynamic-group` represents the name of your dynamic group

-> created as 	datascience-dynamic-group-policy in the root compartment
