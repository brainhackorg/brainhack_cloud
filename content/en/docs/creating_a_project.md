---
title: "Object Storage"
linkTitle: "Object Storage"
weight: 2
description: >
  Object Storage
---

## Overview

Object Storage is like Dropbox ... it allows you to simply
store files and they can be accessible via links on the web (or APIs) and you
don't have to monitor the size of the disk (as opposed to block storage) and it
comes with very nice features for mirroring your data to other regions or
tiering data (making it cheaper if files are not accessed very often).

## Setup a new Bucket

Select a Region from the list where you want your files to be located:

![image](https://user-images.githubusercontent.com/4021595/161411496-e142dfdc-b84d-4e67-9ea0-71bc74a3f157.png)

Then search for `Buckets` and you will find it under `Storage` -> `Object Storage & Archival Storage` -> Buckets:
![image](https://user-images.githubusercontent.com/4021595/161411526-7e47aa08-5284-4d21-b3bb-9501f34a29fa.png)

Select your project's compartment:
![image](https://user-images.githubusercontent.com/4021595/161411532-efc61a06-e927-4f09-bfcb-ca777a2eb259.png)

Create a new Bucket:
- give it a name
- In addition to the defaults we recommend `Enable Auto-Tiering` (this will make the storage cheaper by moving objects to lower tier storage if they are not used frequently) and `Uncommitted Multipart Uploads Cleanup` (this will clean up in case uploads failed halfway)

![image](https://user-images.githubusercontent.com/4021595/161411600-37cf0399-2376-41cf-8dd7-cb75f7ad8a58.png)


## Uploading files to a bucket

You could upload files via the GUI in the Oracle cloud by clicking the `Upload` button:
![image](https://user-images.githubusercontent.com/4021595/161412082-f95b1d4c-67f9-4b69-a360-fd7aab8bc508.png)

You could also use tools like rclone or curl or the OCI CLI to upload files (more about these tools later)

## Making a Bucket public

By default, the files in the bucket will not be visible to everyone. Let's find the URL to the file we just uploaded: Click on the 3 dots next to the file and click on `View Object details`:
![image](https://user-images.githubusercontent.com/4021595/161412133-d4437e2b-4886-4b16-ae05-8ec2e2215dd1.png)

When opening this URL, you will get this error:
![image](https://user-images.githubusercontent.com/4021595/161412164-ed46dd79-ee10-42ea-80dc-b1a8088e38f1.png)

You can either make the WHOLE bucket visible to the world or use "Pre-Authenticated Requests". Let's start with the easy (and less control/secure) way first:

Click on `Edit Visibility` and switch to public:
![image](https://user-images.githubusercontent.com/4021595/161412193-e3ec0171-9b0b-46f5-885e-17163b1cb8c2.png)

Now the file and EVERYTHING else in the bucket are visible to EVERYONE on the internet. 

## Pre-Authenticated Requests

Click on the three dots next to the file again and Click `Create Pre-Authenticated Request`:
![image](https://user-images.githubusercontent.com/4021595/161412278-26bbead8-7a58-43b3-8d31-e88a892f83fa.png)

This gives you more options to control access and you can also expire the access :)

And you then get a specific URL to access the file (or the bucket or the files you configured):
![image](https://user-images.githubusercontent.com/4021595/161412301-5fe1b423-e6ed-458a-9347-fdf8e3915fe0.png)

The URL will stop working when it expires or when you delete the Request. You find all requests under `Pre-Authenticated Requests` in the Resources menu:
![image](https://user-images.githubusercontent.com/4021595/161412353-319882d3-7016-4e1b-800f-f9a77b2af0a1.png)


## Tiering
Lifecycle Rules allow you to control what happens with files after a certain amount of time. You can delete them or move them to Archival storage for example:
![image](https://user-images.githubusercontent.com/4021595/161412377-63896ae5-8d02-4bfc-bf4f-e8b30010281d.png)


## Mirroring
Mirroring allows you to keep the bucket up-to-date with another bucket in another region (e.g. main bucket is in Europe and the replica is in Australia). This is controlled under the `Replication Policy`.

You first need to create the target bucket in the other region and then you can configure it as a target:
![image](https://user-images.githubusercontent.com/4021595/161412505-d64a0177-e520-46f5-86b8-f08c4e60b962.png)




## Uploading files using CURL
To enable this you need to create a Pre-Authenticated Request which allows access to the Bucket and it allows objects read and write and Object Listing:
![image](https://user-images.githubusercontent.com/4021595/161412582-5f0ff3c7-f207-4ca1-811f-163ef2355fd2.png)

Then copy the URL, as it will never be shown again:
![image](https://user-images.githubusercontent.com/4021595/161412595-a9d4710a-f65d-408e-a025-1208e35ba0fe.png)

Now you can use curl to upload files:
```
curl -v -X PUT --upload-file YOUR_FILE_HERE YOUR_PRE_AUTHENTICATED_REQUEST_URL_HERE
```

## Uploading files using RCLONE
Rclone is a great tool for managing the remote file storages. To link it up with Oracles object storage you need to configure a few things (the full version is here: https://blogs.oracle.com/linux/post/using-rclone-to-copy-data-in-and-out-of-oracle-cloud-object-storage#:~:text=%20Using%20rclone%20to%20copy%20data%20in%20and,which%20Rclone%20will%20be%20used%20to...%20More%20):


1) The Amazon S3 Compatibility API relies on a signing key called a Customer Secret Key. You need to create this in your User's settings:
![image](https://user-images.githubusercontent.com/4021595/161412892-0e9d6190-362c-41b5-9579-5f9a2371ebed.png)

Then Click on `Generate Secret Key` under `Customer Secret Keys`:
![image](https://user-images.githubusercontent.com/4021595/161412964-dd036c50-834e-436f-a8d1-a44da80f29d2.png)

Save the Secret Key for later:
![image](https://user-images.githubusercontent.com/4021595/161412970-d22661cd-b549-48ff-8c33-15ba538f02d6.png)

Then save the `Access Key` from the table as well.

2) Find out where your rclone config file is located:
```
rclone config file
```

3) Add this to your rclone config file:

```
[myobjectstorage]
type = s3
provider = Other
env_auth = false
access_key_id = <ACCESS KEY>
secret_access_key = <SECRET KEY>
endpoint = froi4niecnpv.compat.objectstorage.<REGION>.oraclecloud.com
```
Replace `ACCESS KEY` and `SECRET KEY` with the ones generated earlier. Replace `REGION` with the region where the storage bucket is located (e.g. eu-frankfurt-1).

Now you can use rclone to for example list files in the bucket:
```
rclone ls myobjectstorage:/test-bucket
```

or you can upload files or whole directories (or download by reversing the order of Target/Source):
```
rclone copy YOURFILE_or_YOURDIRECTORY myobjectstorage:/test-bucket
```

or you can sync whole directories or other remote storage locations (includes deletes!):
```
rclone sync YOURDIRECTORY_OR_YOUR_OTHER_RCLONE_STORAGE myobjectstorage:/test-bucket
```
