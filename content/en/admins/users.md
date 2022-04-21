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

Here is an example of such a request:

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

Add the User to the group `projects` (This group has policies for giving users access to the cloudshell and the data science notebooks): 

![image](https://user-images.githubusercontent.com/4021595/164168268-e0921fa8-8236-4816-890d-ab1dbaafe240.png)


Repeat this procedure for every user in the project.

## Create Project Group

Go back to Identity and click `Create Group`

![image](https://user-images.githubusercontent.com/4021595/157341864-5582074f-fa90-48f2-a2a5-deebb1a241dc.png)

Give the group a name that represents the project (no spaces!) - as a Description
put the link to the Github issue.

![image](https://user-images.githubusercontent.com/4021595/157342047-92280e16-9518-4d04-a3be-796073d16c01.png)

Then add the User(s) to the group.

![image](https://user-images.githubusercontent.com/4021595/157342170-192e1e82-c1f7-46ed-a0f9-ec59bbe8de8e.png)

## Create Project Compartment

Go back to `Identity` and head to `Compartments` and click `Create Compartment`.
Name it like the group just created and add the Github issue link as
the description. Parent compartment is `projects` a sub-compartment under the main compartment `brainhack (root)`.

![image](https://user-images.githubusercontent.com/4021595/164388946-72ae0980-6975-4b2b-bdb1-e1442a0da344.png)


## Create Policy for group and compartment

Go back to `Identity` and click on `Policies`. Click `Create Policy`. Name the
policy like the group and compartment just created. The description is the Github
issue link. You can either use the policy builder or switch to manual. The
resulting policy needs to be
`Allow group REPLACEWITHGROUPNAME to manage all-resources in compartment REPALCEWITHCOMPARTMENTNAME`. Make sure that this policies is at the `project` level and not in the `brainhack (root)` compartment:

![image](https://user-images.githubusercontent.com/4021595/164389533-8f18ee73-c474-476d-8ed5-a0428b7e5aac.png)

## Create a Budget for compartment

Budgets help us to control and monitor costs. For every compartment, we need a
budget with someone being alerted when things go crazy:

Go to `Budgets` under `Cost Management` and click `Create Budget`. Add the
project details and add your email to the alert list.

![image](https://user-images.githubusercontent.com/4021595/157346505-be192493-6937-4574-87a8-1ceb898bae81.png)
