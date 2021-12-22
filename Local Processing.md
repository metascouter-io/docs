#### Prerequisites
- Your account must have Local processing enabled. This allows the "Local" video option to be displayed. Ask an admin to enable this permission on your Metascouter account. 
- You must have a processor with AVX2 instructions. Most modern processors have this instruction.
- You must have Docker installed
- You need credentials to download from the Metascouter ECR repository or otherwise load the docker image. 
#### Populating the .env file
- Need credentials to write to the message broker (`mqtt`). 
	- The message broker is how the server receives *events* (health, stock, and character updates) from the watcher.
	- An admin should generate credentials for the mqtt server via the `mosquitto_passwd` command, update the ACL for the new user, and restart the broker. 
- Need an API key for Elastic APM. This can be generated from the private Elastic server by an admin. 

#### Running
```
docker run --env-file <path-to-env-file> -e STREAM_URL=<url-of-stream-or-video> -e WATCHER_ID=<video-id-from-metascouter.gg> -e REQUEST_TYPE=<vod|stream> -e HQ_OVERRIDE=<1|0> -it 792787478124.dkr.ecr.us-west-1.amazonaws.com/watchers:latest <game>
```
- **STREAM_URL**: the URL of the stream or video
- **WATCHER_ID**: the ID of the video to process for. Obtained from the UI after launching a Local video.
- **REQUEST_TYPE**: the type of video input to process. Either `stream` or `vod`. `stream` will use `streamlink` and `vod` will use `youtube-dl` as a backend.
- **HQ_OVERRIDE**: Whether or not to enable higher quality video streams. If you have a fast computer, set this to 1; otherwise, 0.
- **game**: The game to process. Either `melee` or `ultimate`. 

Example run:
```
docker run --env-file ./.env -e STREAM_URL=https://www.twitch.tv/sillintor -e WATCHER_ID=30cf4b54-dd86-4592-aad5-31cb310c6326 -e REQUEST_TYPE=stream -e HQ_OVERRIDE=1 -it 792787478124.dkr.ecr.us-west-1.amazonaws.com/watchers:latest ultimate
```
