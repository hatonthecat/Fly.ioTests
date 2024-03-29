# Fly.ioTests
A test of various Docker images and instances (Pleroma, GoToSocial and other ActivityPubs) using bandwidth limited config settings


I set up a Fly.io machine attempting to run Pleroma using this [guide](https://sal.dev/fediverse/running-pleroma-on-fly-io/), but ran into an issue when it could not find the fly.TOML. I did find it in the C: User folder, but it would not link up to the volume, and I gave up. In searching for a solution, I found other [ActivityPub](https://docs.gotosocial.org/en/latest/getting_started/#server-vps) images that ran under 100MB of RAM, thus I thought I would try them out. 

![image](https://github.com/hatonthecat/Fly.ioTests/assets/76194453/01e761ec-29e1-4769-801f-33893f0ce74e)

Picture of my VM setup hosted on a U.S. server. So far I have a pre-configured "Hello World": https://plero.fly.dev/ (the site may be occasionally down) 

This test shows some interesting benchmarks, which can show the loading times for small jpgs and png files. https://benhoyt.com/writings/flyio/#load-testing

![image](https://github.com/hatonthecat/Fly.ioTests/assets/76194453/7133bb98-3eef-4fb6-8a61-399a2837a97a)

(Only one DB is needed)

A different DB that GoToSocial can also uses is SQLite. I am curious whether the default Redis might be possible to use with an ActivityPub app, or whether the docker image bundles SQLite and the together. Render also offers a 25MB DB free storage option with up to 50 connections at a time (may or may not be practical for a follower count above 25): https://render.com/pricing#compute

whereas Upstash Redis allows a free account with [up to](https://upstash.com/pricing): 

Max Commands Per Second: 1,000
Daily Command Limit: 10,000



