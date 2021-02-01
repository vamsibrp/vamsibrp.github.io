---
layout: post
title:  "Setting Orthanc DICOM server and using DCMTK"
date:   2021-01-29 12:23:27 +0530
---

### Introduction

In this blog, we are going to setup a `DICOM` server and use this server to upload, query and download DICOM files. This is going to be interesting as we are going to deal with many new terms like DICOM, ORTHANC , DCMTK and many more utilities. This author of this blog will help you setup the workinhg environment on MAC.

Let's start by setting up a server.

### Installing ORTHANC and Setting DICOM server

The easiest way is to use Docker to set this up. If you are new to Docker please refer [my-work](/).

`Docker-hub` link: click [here](https://hub.docker.com/r/jodogne/orthanc)

The other way to set up your server is by downloading it from Official [website](https://www.orthanc-server.com/download.php). 

Once you are ready with your downloaded and un-zipped folder. You are almost there. 

Double-click on `startOrthanc.command` inside the folder. Check the permissions and related security stuff if you face any issues. 

A terminal gets opened and you can see the messege that your `Orthanc server and viewer are up on 4242 and 8042 ports` of your machine. It is cool. Isn't it?

If you want to leverage the third-party package and build some custom products, you better download Orthanc instead of running on docker. It should make sense.

### Downloading DCMTK

DCMTK is a DICOM toolkit that helps you to build your custom features. It should be fun working with these tools because documentation is proper but there are few people round the net to help you if you get stuck. Before going deep into the tools we use in this tutorial let's Download the tools.

Download from [here](https://dicom.offis.de/dcmtk.php.en). After you download, go through [support-document](https://support.dcmtk.org/docs/file_install.html) to build and better understand things about DCMTK. 

Once you build, You have all the tools in your /bin folder . Open your terminal in the bin folder and you are good to explore all the tools. There are many useful tools for different use-cases but we go forward with a few in our tutorial.

We use `dcmsend` to upload files to our DICOM server running on 4242 port. We use `findscu` to query on the Server.


### Using dcmsend to upload DICOM files

{% highlight ruby %}
{% endhighlight %}
Template of the command:
{% highlight ruby %}
dcmsend options peer port dcmfile-in
{% endhighlight %}
`peer` : Ip of the host-server
`port` : The port on which the server is listening to
`dcmfile-in` : The DICOM files/folders we wish to upload. 
`options` : These are the flags that does many things for us

Please go through the Official [Documentation](https://support.dcmtk.org/docs/dcmsend.html) for extensive understanding about options

sample command to upload will look like :
{% highlight ruby %}
path/to/bin % dcmsend -v +sd +r localhost 4242 <FOLDER_1> <image-name>.dcm
{% endhighlight %}
On successful upload you can will recieve an acknowledgement on the terminal. If you face any issue, please check if your DICOM server is up on the port you have mentioned. The other reason you might face issue is `Firewall`. Please allow Firewall to access connections to the port our DICOM server is listening to.


### Using findscu to query on the server

{% highlight ruby %}
findscu options peer port dcmfile-in...
{% endhighlight %}
`peer` : Ip of the host-server
`port` : The port on which the server is listening to
`dcmfile-in` : The DICOM query files.
`options` : These are the flags that does many things for us

The query file could, for instance, be created with the `dump2dcm` utility from a script like the following example:
{% highlight ruby %}
query patient names and IDs
(0008,0052) CS [PATIENT]     # QueryRetrieveLevel
(0010,0010) PN []            # PatientName
(0010,0020) LO []            # PatientID
{% endhighlight %}

Please go through the Official [Documentation](https://support.dcmtk.org/docs/findscu.html) for more insights on `findscu` .

sample command to upload will look like :
{% highlight ruby %}

findscu -P -k PatientName="Hartgard*"  localhost 4242 /path/to/query-file/query_dicom_file.dcm

{% endhighlight %}

`query_dicom_file` is created using dump2dcm utility provided in dcmtk. refer [this](https://support.dcmtk.org/docs/dump2dcm.html)

### Downloading the DICOM files from the server

