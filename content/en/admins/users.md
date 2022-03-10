---
title: "Creating new users"
linkTitle: "Creating new users"
weight: 1
description: >
  Creating new users
---

## Overview

Once a new project is requested via the
[Issue Template](https://github.com/brainhackorg/brainhack_cloud/issues/new?assignees=&labels=resource_request&template=request-resource-access.yml)
one of the admins has to provision the project on the cloud.

Here is an example for such a request:

![image](https://user-images.githubusercontent.com/4021595/157339176-d222994f-7d25-484c-99bb-67b354ab1e5a.png)

## Creating a new user

Login to Oracle Cloud:
https://cloud.oracle.com/?region=eu-frankfurt-1&tenant=brainhack

Open the menu and search for users, then open Users in Identity

![image](https://user-images.githubusercontent.com/4021595/157339416-1c6f3dcd-3d78-4613-81f1-a955badc3d28.png)

Then hit `Create User`.

Change the selection to `IAM User`, add the user name as `firstname_lastname`
and the same for description, and add an email.

![image](https://user-images.githubusercontent.com/4021595/157339931-96380614-7e14-434d-a9d6-9821c7aa2463.png)

Hit `create`.

Now, generate a password for the user by Clicking `Create/Reset Password`

![image](https://user-images.githubusercontent.com/4021595/157340057-14baf64d-4737-4dae-ad6a-c31694eac9ab.png)

Copy this password and send it to the user you just created.

The new user has to follow this procedure: [User request](./../../docs/request).

Add the User to the group `cloudshell-access`

![image](https://user-images.githubusercontent.com/4021595/157342248-9a63cdf0-c630-42b9-9222-c45e54916a38.png)

Repeat this procedure for every user in the project.

## Create Project Group

Go back to Identity and click `Create Group`

![image](https://user-images.githubusercontent.com/4021595/157341864-5582074f-fa90-48f2-a2a5-deebb1a241dc.png)

Give the group a name that represents the project (no spaces!) - as Description
put the link to the github issue.

![image](https://user-images.githubusercontent.com/4021595/157342047-92280e16-9518-4d04-a3be-796073d16c01.png)

Then add the User(s) to the group.

![image](https://user-images.githubusercontent.com/4021595/157342170-192e1e82-c1f7-46ed-a0f9-ec59bbe8de8e.png)

## Create Project Compartment

Go back to `Identity` and head to `Compartments` and click `Create Compartment`.
Name it like the group just created and add the Github issue link as
the description. Parent compartment is `projects` under `brainhack (root)`.

![image](https://user-images.githubusercontent.com/4021595/157600826-7056e58c-f4a6-4d12-b66f-13b6007b7095.png)


## Create Policy for group and compartment

Go back to `Identity` and click on `Policies`. Click `Create Policy`. Name the
policy like the group and compartment just created. The description is the Github
issue link. You can either use the policy builder or switch to manual. The
resulting policy needs to be
`Allow group REPLACEWITHGROUPNAME to manage all-resources in compartment REPALCEWITHCOMPARTMENTNAME`.

![image](https://user-images.githubusercontent.com/4021595/157343055-f726641a-ae85-4eab-9cff-5b1f08a70db3.png)

## Create Budget for compartment

Budgets help us to control and monitor costs. For every compartment, we need a
budget with someone being alerted when things go wrong:

Go to `Budgets` under `Cost Management` and click `Create Budget`. Add the
project details, set the Budget to `1000` AUD, switch to `Forecast Spend` and sset a threshold of `100`% and add your email to the alert list + a second admin as backup.

![image](https://user-images.githubusercontent.com/4021595/157601668-ee7281f3-8f49-4db9-b20b-deca29e03c19.png)

