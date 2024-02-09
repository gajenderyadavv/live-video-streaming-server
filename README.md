## Architecture:

Note:We can use the camera and a Physical/Sowftware Encoder to encode the video from the camera and then send it to the UNix Machine.

OBS-->Stream Config.(refer to Point 2 in suggestions)---Will send the stream from your obs-->Unix Machine

Unix Machine-->Docker-->Nginx-Rtmp_Html Container-->Configured Rtmp endpoint with S3 bucket to push video file to s3 bucket and nginx.conf with ffmpeg for transcoding.
                    |                                                                                              |
                    |                                                                                              |
                    |                                                                                              \-->S3bucket-->CloudFront-->index.html
                    \-->Node Container for authentication.


First of all, Create the s3 Bucket and then congiure it with Cloudfront.

## Modifications to be done in Current Architecture:

K8s Manifestst files be developed for this infrastructure's orchestration.

Documentation for Deployment of this on EKS/AKS/GKE is to be done.

Terraform Code for infrastructure to be developed.

## How to Run Current Architecture:

0. Clone this repo using following command
    `git clone https://github.com/gajenderyadavv/video-streamer.git`
1. Install Docker using following command
    `curl https://gist.githubusercontent.com/gajendra121102/6498d66cd446f5818ad9226c155be852/raw/34c07f7ca6d7162f73011aa25c60571fb9692f45/install-docker-unix | bash`

2. `docker-compose build`
3. `docker-compose up`
4. Open OBS and in settings set the server to `rtmp://server-ip:1935/live` to send video from local machine to server and the stream key to `test?key=supersecret`
5. Open a browser and go to `http://server-ip:3000` to view your live stream!

## Suggestions to Noteb before Starting: (Made following suggestions from Chat GPT)

Update Stream URLs and Paths:
Make sure to update the stream URLs and paths in the HTML and Nginx configuration with your actual server and S3 bucket details.

RTMP Stream from OBS to EC2:
Set up OBS on your local system to stream to the RTMP endpoint on your EC2 instance. Ensure that OBS is configured with the correct RTMP URL, which in this case is rtmp://your-server-ip/live/stream-key.

Configure HLS Transcoding:
The Nginx configuration is set up to transcode the RTMP stream to HLS. Make sure to have FFmpeg installed on your EC2 instance and adjust the path in the Nginx configuration accordingly (/path/to/ffmpeg).

Update CloudFront URL:
If you're using CloudFront to serve the transcoded HLS stream, update the transcodedVideoSrc in your HTML with the correct CloudFront URL.

Set Up Authentication:
If you're using authentication for publishing the stream, make sure your authentication server is configured correctly. Update the on_publish directive with the appropriate URL (http://auth_server:8000/auth).

Adjust Server IP:
Update the server IP in the HTML file with the public IP or DNS of your EC2 instance.

Security Groups and Ports:
Ensure that your EC2 instance has the necessary security group rules to allow traffic on ports 1935 (RTMP) and 8080 (HTTP).

Access from Anywhere:
Once everything is set up, users should be able to access the stream by opening the HTML file in their browser and using the EC2 instance's public IP address.

Cross-Domain Requests:
If you encounter issues with cross-domain HTTP requests during development, the Nginx configuration includes headers for handling CORS. Make sure that these headers suit your deployment needs.

SSL/TLS (Optional):
For a more secure connection, consider setting up SSL/TLS for your Nginx server. This is especially important if you plan to serve the stream over HTTPS.

