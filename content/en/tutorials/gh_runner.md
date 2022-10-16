---
title: "Github action runner"
linkTitle: "Github action runner"
weight: 4
description: >
  Github action runner
---

## Overview

This section describes how to setup a Github action runner on the cloud that you
can then use to run huge Github workflows that wouldn't run in the hosted
runners :)

## Create a new VM or HPC

See our [VM](./../vm) or [HPC](./../hpc) Tutorials.

## Configure Github

Go to the repository settings and under `Actions` you will find `Runners` where you can add a self-hosted runner: 
![image](https://user-images.githubusercontent.com/4021595/161414358-b5487e21-daec-4f04-b008-26664a19f8f8.png)

After clicking on `New self-hosted runner` and selecting Linux you can copy and paste the first section to the VM created in Step 1:
![image](https://user-images.githubusercontent.com/4021595/161414412-a7ec45c5-1283-430c-aab3-3fe99717da9a.png)

run the commands listed on that page in your VM and accept the defaults.

To keep the session running you can use either tmux:
```
sudo install tmux
tmux new -s runner
./run.sh
CTRL-B d
```

Or as an alternative start it as a service, because this will then survive restarts of the VM:
```
sudo ./svc.sh install
sudo ./svc.sh start
```

## Use custom runner in Action
```
# Use this YAML in your workflow file for each job
runs-on: self-hosted
```

Here is an example: https://github.com/QSMxT/QSMxT/blob/master/.github/workflows/test_segmentation_pipeline.yml


# Using cirun.io
The next level to the above workflow is to create the runners on-demand instead of letting them run all the time. This will be more cost effective and it lets you run multiple CI workflows in parallel :). Cirun.io is a wonderful free service that lets us do this!

First, sign up to cirun.io: https://cirun.io/auth/login

Second, make sure your brainhack cloud user account is in the group "cirun" (open github issue or indicate this in your request)

Then you need to link your cirun.io account to your github account and to the repository that should trigger the ciruns:
<img width="1228" alt="image" src="https://user-images.githubusercontent.com/4021595/196020466-fef75f01-03cd-4d96-a2ea-f046ed1a7786.png">

Then you need to create an API key for cirun inside oracle cloud. Go to your User Settings (little user icon in the top right corner) and then "API Keys". Then add an API Key, download the private key and copy the Configuration File Preview to cirun and add the compartment id:

```
[DEFAULT]
user=ocid1.user.oc1..aaaaaaaaj2poftoscirunfororaclecmpcbrmvvescirunfororacle4mtq <<<<<< FILL IN YOUR USER ID HERE!
fingerprint=78:4c:99:1t:3d:1b:a8:ea:f2:dd:cr:01:5r:86:a2:84 <<<< FILL IN YOUR FINGERPRINT HERE!
tenancy=ocid1.tenancy.oc1..aaaaaaaaydlj6wd4ldaamhmhcirunfororaclemocirunfororacle
compartment_id=ocid1.compartment.oc1..aaaaaaaawc7zqarq7xddumdzuatu3bu3ir6ytlkgauyokgxtixj2y6szrd4q
```

Then copy the private key inside the textbox on cirun and hit save.

Now you need to create a .cirun.yml file at the top level of your github repository:
```
runners:
  - name: oracle-runner
    cloud: oracle
    instance_type: VM.Standard2.4
    machine_image: ocid1.image.oc1.uk-london-1.aaaaaaaavy5v3inu2ho2h57vwvvsclukdh4jvhg45um2nrejyxa7s46zcwoq
    region: uk-london-1
    labels:
      - oracle
```

and you need to change the "runs-on" inside your github action from "self-hosted" to "[self-hosted, oracle]"
