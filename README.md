# fly.io tests
A test of various Docker images and instances (Pleroma, GoToSocial and other ActivityPubs) using bandwidth limited config settings


I set up a Fly.io machine attempting to run Pleroma using this [guide](https://sal.dev/fediverse/running-pleroma-on-fly-io/), but ran into an issue when it could not find the fly.TOML. I did find it in the C: User folder, but it would not link up to the volume, and I gave up. In searching for a solution, I found other [ActivityPub](https://docs.gotosocial.org/en/latest/getting_started/#server-vps) images that ran under 100MB of RAM, thus I thought I would try them out. 

![image](https://github.com/hatonthecat/Fly.ioTests/assets/76194453/01e761ec-29e1-4769-801f-33893f0ce74e)

Picture of my VM setup hosted on a U.S. server. So far I have a pre-configured "Hello World": ~~https://plero.fly.dev/~~ (the site was replaced with one below) 

This test shows some interesting benchmarks, which can show the loading times for small jpgs and png files: https://benhoyt.com/writings/flyio/#load-testing This thread details additional benchmark stats: https://community.fly.io/t/this-is-only-possible-on-fly/12075/2


![image](https://github.com/hatonthecat/Fly.ioTests/assets/76194453/7133bb98-3eef-4fb6-8a61-399a2837a97a)

(Only one DB is needed)

A different DB that GoToSocial can also uses is SQLite. I am curious whether the default Redis might be possible to use with an ActivityPub app, or whether the docker image bundles SQLite and the together. Render also offers a 25MB DB free storage option with up to 50 connections at a time (may or may not be practical for a follower count above 25): https://render.com/pricing#compute

whereas Upstash Redis allows a free account with [up to](https://upstash.com/pricing): 

Max Commands Per Second: 1,000

Daily Command Limit: 10,000

One of the issues with deleting and recreating a fly.io machine has is that it won't allow me to reuse the name for a machine that was deleted. Whether this is because there is a config file with the same name, or because it is stored on some directory on my PC, or on fly.io, I am not sure. In any case, if there is a name that you want to use, you might need to choose wisely as it might be taken!

![image](https://github.com/hatonthecat/Fly.ioTests/assets/76194453/218eb3e5-141a-46c8-b60a-7f569be9f4ab)



New host is ~~https://pler1.fly.dev~~ (See 4/1 log below)

found an interesting setup: https://icyphox.sh/blog/honk-fly/

One of the nice things about Pleroma is that they set up a Gopher interface: https://blog.soykaf.com/post/gopher-support-in-pleroma/

But there are a lot of interesting things to learn about Activity Pub before I can even work on Gopher. 

Additional resources:
--
https://fly.io/blog/all-in-on-sqlite-litestream/

https://fly.io/blog/litefs-cloud/

https://github.com/tursodatabase/libsql

https://fnordig.de/2022/11/21/gotosocial-on-fly-io/

https://github.com/badboy/gotosocial-fly

https://github.com/kwaa/flytosocial

https://gist.github.com/mheffner/a367e4f8424d937c511949d2d42c7943 Mastodon on Fly.io (with Redis, which supports HyperLogLog) (may need more than 256MB with that setup)

from: https://marketplace.digitalocean.com/apps/mastodon :

_"Optional: Deploy and Connect to Object Storage (DigitalOcean Spaces Object Storage, Wasabi, Minio or AWS S3)
You’ll need access credentials for your preferred storage system. This guide will walk through setting up and connecting DigitalOcean Spaces Object Storage._

_Why do I need object storage?_

_These buckets are used to store images, video and audio or whatever users will want to upload. Without Object storage, you will need to use your local database, which can be slow and overloaded quickly."_

Sure, it can run slow and get overloaded quickly. But how quickly, and at what file size limit should the local database use so that the DB run well before getting sluggish?
(e.g. a local file folder storing 1000 images compressed to 300KB or less, or 10,000 images at 30KB or less?) Or 100 images maxing out at 10KB? That would only be 1MB of storage. Small enough to not matter. The files could also be routinely backed up, but I am skeptical of this solution in all cases. The S3 backup is practical for testing, but relying on too many servers might not be ideal. See: https://solar.lowtechmagazine.com/2023/06/rebuilding-a-solar-powered-website/ Is what I have in mind

https://fly.io/docs/litefs/getting-started-fly/

----

April 1, Log 
---

Using a [tutorial](https://fnordig.de/2022/11/21/gotosocial-on-fly-io/) above, I was able to host a GoToSocial instance:

![image](https://github.com/hatonthecat/fly.iotests/assets/76194453/4cd18f85-e0a6-45e2-9ab5-80db63b67b04)

Some errors I made in the process (may or may not be common to others but I am a newb):

I assumed Dockerfile meant Docker _image,_ which caused delays (searching for image, where it downloads it via CLI, when done correctly)

I had not understood the config.yaml and fly.toml differences (was somewhat confused by so many config files). 

While I haven't setup my account nor SMTP yet, this is a magnitude further than my "Hello World" Pleroma "server" without a front-end (and back end too, probably- an accessible one, at least, that I had the other day). This is also the first SQLite instance I have running (the other was PostgreSQL).

fly.io dashboard:

![image](https://github.com/hatonthecat/fly.iotests/assets/76194453/93ea41b4-1267-4d8c-a7e4-e4ef9b6eb5e5)

![image](https://github.com/hatonthecat/fly.iotests/assets/76194453/ae719d9c-5993-4714-b445-7256bf57a2fe)

April 2nd Log
--

https://mac.fly.dev/ Set up a new instance. Signup hasn't been set up yet. I had a mc.fly.dev before (but since it was taken I could not reuse it, not knowing how to).

Definitely shorter than the other one. While it deploys two machines as the default (which are still free on fly.io, which allows up to 3), I am searching the option to set only one, to free up machines in case I want to test something else. Having 100% uptime is not a priority, as long as the machine can restart itself, which fly.io is programmed to do. 

I also edited the [config.yaml](https://github.com/badboy/gotosocial-fly/blob/main/config.yaml) file to limit the maximum image, video, emoji, status and poll character/file size count:

![image](https://github.com/hatonthecat/fly.iotests/assets/76194453/78301cd3-16db-466c-98bf-0c9ece33cfa0)

"accounts-registration-open: true
accounts-approval-required: true
accounts-reason-required: false
accounts-allow-custom-css: false

media-image-max-size: 15760
media-video-max-size: 43040
media-description-min-chars: 0
media-description-max-chars: 100
media-remote-cache-days: 3
media-emoji-local-max-size: 20200
media-emoji-remote-max-size: 20400

storage-backend: "local"
storage-local-base-path: "/gotosocial/storage"

statuses-max-chars: 100
statuses-cw-max-chars: 50
statuses-poll-max-options: 3
statuses-poll-option-max-chars: 20
statuses-media-max-files: 1

letsencrypt-enabled: false"

While this might sound parsimonious, it is designed to function more as an extremely light status exchange board that could potentially accomodate several or hundreds of accounts, even on 256MB of RAM. With a concurrency rivalling that of LMDB. (Well, maybe not that lightning fast, but Tigris is working on some interesting things https://fly.io/blog/tigris-public-beta/). In other words, how to load test a single instance, similar to this, but measuring concurrent fediverse postings and access times: https://benhoyt.com/writings/flyio/#load-testing The HTTP tool [Vegeta](https://github.com/tsenart/vegeta) is used to benchmark the load times of the pages. This would be handy in determining the maximum number of user accounts and connections/followers that could run on a single GtS instance.

The theory is, by limiting the images to 20KB or less, one could have a mini-instance that can support 10-100x the typical GoToSocial user accounts. 
One thing that would benefit a service would be an image resize tool that automatically calculates the maximum size of the desired image and compresses+resizes the image to fit under the instance's designated file size limit, so that one doesn't need to use countless image resizing programs (See [Paint Shop Pro](https://github.com/EI2030/Low-power-E-Paper-OS/blob/master/Hyperlinks%20and%20Scratchpad.md), for example).

4/3/2024 Log
--
Using Fly.io's 100GB/month limit for a low cost single VM (shared CPU), I will rely on a 31 day month to estimate the daily average bandwidth alotted for a single instance (including all of the user accounts). 100GB/31= 3.22GB (I round down to avoid overage). 

A 128Kbps internet connection (similar to DSL in the early 00s) allowed approximately 16KB/s. Multiplied by 60 seconds, 60 minutes, and 24 hours, this 1,382,400 bytes, or approx 1.3GB. Thus a 256Kbps internet speed would reach 2.6GB per month. Assuming the GotoSocial instance were very popular and busy, with users accessing it 24 hours a day, 7 days a week, for 4 weeks, this implies the server would need to limit connections and/or requests to under 960KB per minute (on average) and 57.6MB per hour. 10s of users _might_ use that amount, if they were uploading 20KB images every minute. However, it is unlikely that this limit would be reached at that frequency. Hence, it is safe to say that the bottlneck is not the bandwidth, but the volume's ability to read and write data while maintaining other HTTP requests (boosts/follows, etc).

In some regards, HTTP requests typically use more overhead than JSON or other lightweight protocols, thus the instance can be optimized further. However, without using Gemini/Gopher protocols (although bridges and inter-operable clients would be an added benefit), it is worthwhile to examine the different combinations of bandwidth allocation and rate-limited that can maintain a steady uptime for pre-defined usage estimates.

