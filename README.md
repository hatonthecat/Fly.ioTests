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



New host is https://pler1.fly.dev

found an interesting setup: https://icyphox.sh/blog/honk-fly/

One of the nice things about Pleroma is that they set up a Gopher interface: https://blog.soykaf.com/post/gopher-support-in-pleroma/

But there are a lot of interesting things to learn about Activity Pub before I can even work on Gopher. 

Additional resources:
--
https://fly.io/blog/all-in-on-sqlite-litestream/

https://fly.io/blog/litefs-cloud/

https://github.com/tursodatabase/libsql

https://gist.github.com/mheffner/a367e4f8424d937c511949d2d42c7943 Mastodon on Fly.io (with Redis, which supports HyperLogLog) (may need more than 256MB with that setup)

from: https://marketplace.digitalocean.com/apps/mastodon :

_"Optional: Deploy and Connect to Object Storage (DigitalOcean Spaces Object Storage, Wasabi, Minio or AWS S3)
You’ll need access credentials for your preferred storage system. This guide will walk through setting up and connecting DigitalOcean Spaces Object Storage._

_Why do I need object storage?_

_These buckets are used to store images, video and audio or whatever users will want to upload. Without Object storage, you will need to use your local database, which can be slow and overloaded quickly."_

Sure, it can run slow and get overloaded quickly. But how quickly, and at what file size limit should the local database use so that the DB run well before getting sluggish?
(e.g. a local file folder storing 1000 images compressed to 300KB or less, or 10,000 images at 30KB or less?)

https://fly.io/docs/litefs/getting-started-fly/

