

## Architecture:

Unix Machine-->Docker-->Nginx-Rtmp_Html Container-->Configured Rtmp endpoint with S3 bucket to push video file to s3 bucket and nginx.conf with ffmpeg for transcoding.
                    |
                    |
                    \-->Node Container for authentication.

S3bucket-->CloudFront-->index.html

## How to Run Current Architecture:

1. Install Docker
2. `docker-compose build`
3. `docker-compose up`
4. Open OBS and in settings set the server to `rtmp://localhost:1935/live` and the stream key to `test?key=supersecret`
5. Open a browser and go to `http://localhost:3000` to view your live stream!
