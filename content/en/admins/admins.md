---
title: "Creating new admins"
linkTitle: "Creating new admins"
weight: 5
description: >
  Creating new admins
---

## Overview

To increase security of our cloud tenancy an admin cannot create or remove other admins. This is the instruction for the admin-administrator to create new admin accounts: 


## Create new admin

Admins need to use a "federated account" to login to the cloud instead of "local accounts".


Go to `Federation` in `Identity`
![image](https://user-images.githubusercontent.com/4021595/159428315-c2335864-23d9-467b-bc93-b618f1e3d840.png)

Click on `OracleIdentityCloudService` and then `Create User`

append `_admin` to the Username (firstname_lastname) of the new user and add the user to the Group `OCI_Administrators`

![image](https://user-images.githubusercontent.com/4021595/159451717-af0c4b33-f10a-40dc-a5e3-d9453ae8f931.png)

Warning: DO NOT ADD THE USER TO THE GROUP IDCS_Administrators - otherwise this admin will be able to manage other admin accounts.

You don't have to assign any Roles to the user, so on the next screen click on `Close`.


The new admin will receive an email like this:
![image](https://user-images.githubusercontent.com/4021595/159452490-1e93e40b-a5f4-4ba9-89e7-6d6bb3f0ce2f.png)

and after activating the account everything is ready to go and the new admin needs to sign on via federated accounts Single Sign On:
![image](https://user-images.githubusercontent.com/4021595/159452817-bce541c0-8996-431e-b670-b8db3818959e.png)
