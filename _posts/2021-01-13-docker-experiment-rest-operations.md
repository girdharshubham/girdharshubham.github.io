---  
title: "Docker Experiments: REST Operations"
comments: false
category: ["docker"]
---  

The Docker Daemon(**dockerd**) listens on `/var/run/docker.sock` by default and provides a REST API to control or communicate with the Docker daemon through scripting.  

cURL can be used to send requests to sockets and this is what we are going to do today.

## Get version

{% highlight shell %}
$ curl -s -XGET --unix-socket /var/run/docker.sock http://localhost/version | jq 
{
  "Platform": {
    "Name": "Docker Engine - Community"
  },
  "Components": [
    {
      "Name": "Engine",
      "Version": "20.10.2",
      "Details": {
        "ApiVersion": "1.41",
        "Arch": "amd64",
        "BuildTime": "2020-12-28T16:15:09.000000000+00:00",
        "Experimental": "false",
        "GitCommit": "8891c58",
        "GoVersion": "go1.13.15",
        "KernelVersion": "5.4.0-60-generic",
        "MinAPIVersion": "1.12",
        "Os": "linux"
      }
    },
    {
      "Name": "containerd",
      "Version": "1.4.3",
      "Details": {
        "GitCommit": "269548fa27e0089a8b8278fc4fc781d7f65a939b"
      }
    },
    {
      "Name": "runc",
      "Version": "1.0.0-rc92",
      "Details": {
        "GitCommit": "ff819c7e9184c13b7c2607fe6c30ae19403a7aff"
      }
    },
    {
      "Name": "docker-init",
      "Version": "0.19.0",
      "Details": {
        "GitCommit": "de40ad0"
      }
    }
  ],
  "Version": "20.10.2",
  "ApiVersion": "1.41",
  "MinAPIVersion": "1.12",
  "GitCommit": "8891c58",
  "GoVersion": "go1.13.15",
  "Os": "linux",
  "Arch": "amd64",
  "KernelVersion": "5.4.0-60-generic",
  "BuildTime": "2020-12-28T16:15:09.000000000+00:00"
}
{% endhighlight %}

## Ping the Docker Server

{% highlight shell %}
curl -s -XGET --unix-socket /var/run/docker.sock http://localhost/_ping
OK
{% endhighlight %}

## List Volumes

{% highlight shell %}
$ curl -s -XGET --unix-socket /var/run/docker.sock -H 'CONTENT-TYPE: application/json' http://localhost/volumes | jq
{
  "Volumes": [],
  "Warnings": null
}
{% endhighlight %}

## Create Volume

{% highlight shell %}
$ curl -s -XPOST --unix-socket /var/run/docker.sock -H 'CONTENT-TYPE: application/json' -d '{
  "Name": "cassandra",
  "Driver": "local"
}'  http://localhost/volumes/create | jq
{
  "CreatedAt": "2021-01-13T00:57:20+05:30",
  "Driver": "local",
  "Labels": null,
  "Mountpoint": "/var/lib/docker/volumes/cassandra/_data",
  "Name": "cassandra",
  "Options": null,
  "Scope": "local"
}

$ docker volume ls 
DRIVER    VOLUME NAME
local     cassandra

{% endhighlight %}  
