---
title: "Setting up policies"
linkTitle: "Setting up policies"
weight: 2
description: >
  This is about Oracle cloud policies.
---

## Overview

Access permissions are controlled via Policies. If something is not working,
it's usually a missing policy. One important thing to note is that Policies can
ONLY be applied to Groups, NOT to the Users!

## Policy for Cloud Shell access

By default, a new user cannot open a Cloud Shell. To provide every user with
this permission, we created a group called "cloudshell-access" and we add every
user to this group. The group then has a Policy that allows opening a cloud
shell: `allow group cloudshell-access to use cloud-shell in tenancy`

## Policies for Object Storage Lifecycle management
To enable lifecycle management the local services need a policy to do this. I configured it for every possible region a user would choose:
Allow service objectstorage-eu-frankfurt-1 to manage object-family in compartment projects

Allow service objectstorage-ap-sydney-1 to manage object-family in compartment projects

Allow service objectstorage-ap-melbourne-1 to manage object-family in compartment projects

Allow service objectstorage-sa-saopaulo-1 to manage object-family in compartment projects

Allow service objectstorage-sa-vinhedo-1 to manage object-family in compartment projects

Allow service objectstorage-ca-montreal-1 to manage object-family in compartment projects

Allow service objectstorage-ca-toronto-1 to manage object-family in compartment projects

Allow service objectstorage-sa-santiago-1 to manage object-family in compartment projects

Allow service objectstorage-eu-marseille-1 to manage object-family in compartment projects

Allow service objectstorage-ap-hyderabad-1 to manage object-family in compartment projects

Allow service objectstorage-ap-mumbai-1 to manage object-family in compartment projects

Allow service objectstorage-il-jerusalem-1 to manage object-family in compartment projects

Allow service objectstorage-eu-milan-1 to manage object-family in compartment projects

Allow service objectstorage-ap-osaka-1 to manage object-family in compartment projects

Allow service objectstorage-ap-tokyo-1 to manage object-family in compartment projects

Allow service objectstorage-eu-amsterdam-1 to manage object-family in compartment projects

Allow service objectstorage-me-jeddah-1 to manage object-family in compartment projects

Allow service objectstorage-ap-singapore-1 to manage object-family in compartment projects

Allow service objectstorage-af-johannesburg-1 to manage object-family in compartment projects

Allow service objectstorage-ap-seoul-1 to manage object-family in compartment projects

Allow service objectstorage-ap-chuncheon-1 to manage object-family in compartment projects

Allow service objectstorage-eu-stockholm-1 to manage object-family in compartment projects

Allow service objectstorage-eu-zurich-1 to manage object-family in compartment projects

Allow service objectstorage-me-abudhabi-1 to manage object-family in compartment projects

Allow service objectstorage-me-dubai-1 to manage object-family in compartment projects

Allow service objectstorage-uk-london-1 to manage object-family in compartment projects

Allow service objectstorage-uk-cardiff-1 to manage object-family in compartment projects

Allow service objectstorage-us-ashburn-1 to manage object-family in compartment projects

Allow service objectstorage-us-phoenix-1 to manage object-family in compartment projects

Allow service objectstorage-us-sanjose-1 to manage object-family in compartment projects


## Policies for HPC
allow service compute_management to use tag-namespace in tenancy

allow service compute_management to manage compute-management-family in tenancy

allow service compute_management to read app-catalog-listing in tenancy

Allow dynamic-group instance_principal to read app-catalog-listing in tenancy

Allow dynamic-group instance_principal to use tag-namespace in tenancy

Allow dynamic-group instance_principal to manage all-resources in compartment projects
