# nginx rtmp multistream

## TL;DR
Edit conf/nginx.conf
Then run the following commands:
```sh
docker-compose build
docker-compose up -d
```
RTMP server now available push to rtmp://server-hostname/live

## About
This Docker image is for having a central point to stream to multiple streaming providers at once.
Providers included in this config are: Twitch, Youtube, Facebook
Instagram is not properly supported due to multiple reasons, one is they don't official support this..

This Dockerfile will let you run nginx with the compiled rtmp module from https://github.com/sergey-dryabzhinsky/nginx-rtmp-module

This rtmp module doesn't properly support pushing to rtmps however, so we so a little magic:
with docker-compose we also spin up a [stunnel](https://www.stunnel.org/)
this stunnel makes it possible to translate traffic to rtmps for the module.

## Prerequisites 
You will need to have both [docker](https://www.docker.com/) and [docker-compose](https://docs.docker.com/compose/install/) installed on your machine.
Have at least 4 cores so transcoding doesn't take your entire machine, so a Raspberry pi is __not__ recommended.

## Config
You will need to edit the nginx.conf file in the conf directory.

update the following things:
* ALLOWED_BROADCAST_IP : The IP address from where the stream will be pushed to this instance.
* TWITCH_KEY : The twitch stream key
* FACEBOOK_KEY : The Facebook stream key
* INSTAGRAM_KEY : The instagram stream key (optional, this is disabled by default for now in the 'live' app)
* YOUTUBE_KEY : The yourube stream key
 
You can disable any of these by commenting out either their 'push' or 'exec' in the live application.

## Usage
First we build the docker image
```sh
docker-compose build
```
Then make sure you have updated the conf/nginx.conf
```sh
docker-compose up -d
```

## Notes
Check to make sure the streaming push links are still up to date with what the platform gives you..
it usually doesn't change, but do verify
for facebook/instagram also check the conf/stunnel.conf

If you want to experiment with instagram, be free to figure out the right encoder settings and a way to get the rtmp key
if you find a clean solution and want to share back, I'll accept it.

License
----

MIT
