---  
title: "Google Cloud Platform Commands"
comments: false
category: ["gcp","cloud"]
---  
This document contains a list of gcloud commands that I end up using almost daily.  
{% highlight shell %}
$ gcloud init --console-only --skip-diagnostics # initialize or reinitialize gcloud
$ gcloud compute zones list | grep -i "us-east1\|REGION" # list zones with header
$ gcloud config set compute/zone us-central1-c # update zone property in the global config
$ gcloud compute images list | grep -i "name\|ubuntu" # list os images with header
$ gcloud compute instances create "mothership" \
--machine-type "n1-standard-1" \
--image-project "ubuntu-os-cloud" \
--image "ubuntu-2004-focal-v20201211" \
--subnet "default" # create an ubuntu VM in the default subnet with n1-standard-1 machine type
$ gcloud compute instances delete "mothership" # delete a compute engine by its name
{% endhighlight %}