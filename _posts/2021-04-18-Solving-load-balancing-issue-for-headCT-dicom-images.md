---
layout: post
title:  "Implementing load balancing on gateways for head CT Dicom images"
date:   2021-04-18 12:23:27 +0530
---

### Problem Statement

A Head CT is quite complicated when compared to an X-Ray part because CT consists of a series of images coming from the scanning modality. To correctly process the CT we have to be sure that all the images part of the series are being sent to the same modality(in our case, it's our gateway). Say if half of the images are sent to modality A and the remaining half to modality B we cannot accurately process the CT and we duplicate a single study of the patient. The idea is to process studies using multiple gateways and place a load balancer ahead of gateways which takes care of load-balancing on the gateways.

### Logic behind the Load Balancer

The approach we took here is by using `Orthanc Server Side` scripting in `lua`. Orthanc provides various call-back functions in lua to implement custom features. We here are leveraging Orthanc to build a load balancer.

Refer [this](https://book.orthanc-server.com/users/lua.html#auto-routing-of-dicom-images) before going ahead in this blog.

We will be using this callback provided in lua to implement a load-balancer

{% highlight ruby %}
function OnStoredInstance(instanceId, tags, metadata)
  Delete(SendToModality(instanceId, 'sample'))
end
{% endhighlight %}

The above callback `OnStoredInstance` sends the `instanceID` to the modality named `sample` and deletes the instance successfully forwarded to the mentioned modality.

This is what we exactly needed to build a load balancer. The ability to send to the modality of our choice(load-balancers choice) is the need of the hour and we have it. The next thing that bothers is how do you decide where to send a certain study. I guess you might have heard of terms like hashing and buckets. The basic idea is to hash a unique identifier for a study and allot the study the bucket_number basing on the hash and the number of buckets we chose to sort them into.

Below here is the lua script(function A, B, C combined in hash.lua) which does all the work for us.

Here, the unique identifier for the study is `StudyInstanceUID` and it comes from the tags parameter.
We must be able to make a hash out of this `StudyInstanceUID`. It was pretty hard to find an in-built hash function in lua. Hence there was a need for creating a custom hash that is uniformly random. 

{% highlight ruby %}
-- function A
function OnStoredInstance(instanceId, tags, metadata)
    local uid = tags['StudyInstanceUID']
    local hash = random_bucket(uid,3) -- number of buckets is 3 at the moment
    local modality = GetModalitybyBucket(hash)
    Delete(SendToModality(instanceId, modality))
end
{% endhighlight %}
say `uid = "1.2.2.12.12667.35356677546776445.44447887346356547"` is our string which has to be hashed and note the string always consists of digits and `.` . Ponder over this, you now are building your own hash function. Be innovative and make sure your hash has to be uniformly random. 

Here is my approach, since we only have digits why can't we `just sum them all and create a hash out of it` and `sort them into buckets by taking dividing it with a number of buckets` and alloting the study into the bucket numbered `remainder`.

This is a very basic hash function but works unless the `StudyInstanceUID` generated at the source is biased.

{% highlight ruby %}
-- function B
function random_bucket(s, num_buckets)
    local sum = 0
    for i = 1, string.len(s) do
        local c = s:sub(i,i)
        if c ~= "." then
            sum = sum + tonumber(c)
        end
    end
    return sum % num_buckets
end

-- fucntion C
function GetModalitybyBucket(hash)
    return 'dcmio' .. tostring(hash+1)
end
{% endhighlight %}

Now we have achieved load-balancer, it's time to integrate it into our solution.

### Integrating load balancer with the gateways

We use docker-compose to do it seamlessly by running them as different services.

{% highlight ruby %}
version: "3"
services:
  loadbalancer:
    image: jodogne/orthanc-plugins
    restart: always
    ports:
      - your ports to be exposed
    volumes:
      - ./path/from/somewhere/orthanc.json:/path/to/somewhere/orthanc.json:ro
      - ./path/from/somewhere/hash.lua:/path/to/somewhere/hash.lua
  gateway1:
    image: image-name
    command: command to start the gateway
    restart: always
  gateway2:
    image: image-name
    command: command to start the gateway
    restart: always
  gateway3:
    image: image-name
    command: command to start the gateway
    restart: always

{% endhighlight %}


In the function `GetModalitybyBucket` we are actually returning the modality name which must be present in the `DicomModalities config` of orthanc.json

{% highlight ruby %}
{% endhighlight %}

{% highlight ruby %}
DicomModalities:{
    "dcmio1": ["aetname","gateway1",port],
    "dcmio2": ["aetname","gateway2",port],
    "dcmio3": ["aetname","gateway3",port],
}
{% endhighlight %}

The generalised form of an entry in DicomModalities is: 
`"modalityname": ["aetname","IPaddress","portnumber"]`

Instead of IP address we provide the service name as we are inside the docker, containers can be recognized with their service names.

Explanation : Say ,the function `SendToModality()` picks modality `dcmio2` to send the study. From the above, we know the `dcmio2` modality is our docker service `gateway2`. Thus, `gateway2` receives the study based on our hash function and the number of buckets.




