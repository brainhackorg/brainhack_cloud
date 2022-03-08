---
title: "Setting up policies"
linkTitle: "Setting up policies"
weight: 3
description: >
  This is about Oracle cloud policies.
---

## Overview

Access permissions are controlled via Policies. If something is not working, it's usually a missing policy. One important thing to note is that Policies can ONLY be applied to Groups, NOT Users!

## Policies for Cloud Shell access

A new user can by default not open a Cloud Shell. To provide every user with this permission, we created a Group called "cloudshell-access" and we add every user to this group. The group then as a Policy that allows opening a cloud shell: `allow group cloudshell-access to use cloud-shell in tenancy`