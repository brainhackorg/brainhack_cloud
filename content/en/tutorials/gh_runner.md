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

## Create new VM

See our [VM Tutorial](./../vm)

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
