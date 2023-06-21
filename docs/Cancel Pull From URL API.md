Cancel ongoing pull from URL tasks

The API can cancel downloads for both Direct Post and Video Upload endpoints on a best-effort basis.

While it is possible to cancel ongoing slow downloads, it is not feasible to cancel downloads that are nearing completion or already in the file processing state.
Request

POST /v2/post/transmission/cancel/ HTTP /1.1
Host: open-platform.tiktokapis.com
Authorization: Bearer {{AccessToken}}
Content-Type: application/json; charset=UTF-8

{
    "publish_id": {PUBLISH_ID}
}

Response

200 OK

{
    "error": {
         "code": "ok",
         "message": "",
         "log_id": "202210112248442CB9319E1FB30C1073F3"
     }
}

response.error.code specification

HTTP Status
	

error.code
	

Description

200
	

ok
	

400

	

invalid_publish_id
	

The publish_id does not exist.

token_not_authorized_for_specified_publish_id
	

The access_token does not have authorization to cancel the publish.

publish_not_cancellable

	

The task associated with this publish_id is already in a final state and can't be cancelled.

403
	

url_ownership_unverified
	

To use PULL_FROM_URL as the video transfer method, developer must verify the ownership of the URL prefix or domain.

401
	

access_token_invalid
	

The access_token is invalid or has expired.

scope_not_authorized
	

The access_token does not bear user's grant on video.upload or video.publish.

429
	

rate_limit_exceeded
	

Your request is blocked due to exceeding the API rate limit.

5xx
		

TikTok server or network error. Try again later.
Video restrictions

Supported media formats
	

    MP4 (recommended)
    WebM
    MOV

Supported codecs

	

    H.264 (recommended)
    H.265
    VP8
    VP9

Framerate restrictions
	

    Minimum of 23 FPS
    Maximum of 60 FPS

Picture size restrictions

	

    Minimum of 360 pixels for both height and width
    Maximum of 4096 pixels for both height and width

Duration restrictions

	

    All TikTok creators can post 3-minute videos, while some have access to post 5-minute or 10-minute videos.
    The longest video a developer can send via the initialize Upload Video endpoint is 10 minutes. TikTok users may trim developer-sent videos inside the TikTok app to fit their accounts' actual maximum publish durations.

Size restrictions
	

    Maximum of 4GB
