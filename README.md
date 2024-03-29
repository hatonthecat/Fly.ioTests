# Fly.ioTests
A test of various Docker images and instances (Pleroma, GoToSocial and other ActivityPubs) using bandwidth limited config settings


I set up a Fly.io machine attempting to run Pleroma using this [guide](https://sal.dev/fediverse/running-pleroma-on-fly-io/), but ran into an issue when it could not find the fly.TOML. I did find it in the C: User folder, but it would not link up to the volume, and I gave up. In searching for a solution, I found other [ActivityPub](https://docs.gotosocial.org/en/latest/getting_started/#server-vps) images that ran under 100MB of RAM, thus I thought I would try them out. 

