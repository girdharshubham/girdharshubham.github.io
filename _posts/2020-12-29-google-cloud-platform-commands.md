---  
title: "Google Cloud Platform Commands"
comments: false
category: ["gcp","cloud"]
---  
This document contains a list of gcloud commands that I end up using almost daily.  
\
{% highlight shell %}
$ gcloud init --console-only --skip-diagnostics # initialize or reinitialize gcloud
$ gcloud compute zones list | grep -i "us-east1\|REGION" # list zones with headers
$ gcloud config set compute/zone us-central1-c # update zone property in the global config
{% endhighlight %}

