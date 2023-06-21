Get Video Status

For content uploaded with the Content Posting API, two mechanisms are provided for developers to check the status of the post by the TikTok user:

    Fetch Status endpoint: An API endpoint for polling the status of the post.
    Content Posting webhooks: Events that notify your registered endpoint of the final outcome of the post.

Video status overview

A video uploaded to TikTok undergoes several stages before it is published. This process can be visualized with the following diagram:

The time taken in any given stage can vary by use cases and a time limit is not guaranteed for the video posting process. The following are some helpful details:

    The average processing times for video processing vary by video size:
        512MB: Less than half a minute
        1GB: About one minute
        4GB: More than two minutes
    If the post was created for public viewership, it must undergo TikTok's moderation process. Based on TikTok's policies, developers are not provided with the post_id until this process is complete.
        Moderation usually finishes within one minute.
        In some cases, moderation may take a few hours.

Fetch Status endpoint

HTTP URL
	

/v2/post/publish/status/fetch/

HTTP Method
	

POST

Scope
	

video.upload/video.publish
Request

Note: Each user access_token is limited to 30 requests per minute.

POST /v2/post/publish/status/fetch/ HTTP /1.1
Host: open.tiktokapis.com
Authorization: Bearer {{AccessToken}}
Content-Type: application/json; charset=UTF-8

{
    "publish_id": {PUBLISH_ID}
}

Response

200 OK

{
    "data": {
        "status": "FAILED",
        "fail_reason": "picture_size_check_failed",
        "publicaly_available_post_id": [],
        "uploaded_bytes": 10000
    },
    
    "error": {
         "code": "ok",
         "message": "",
         "log_id": "202210112248442CB9319E1FB30C1073F3"
     }
}

Nested data struct

Field
	

Type
	

Description

status
	

string

	

The following are the available statuses:

    PROCESSING_UPLOAD: Only available for FILE_UPLOAD. Indicate that the upload is in process.
    PROCESSING_DOWNLOAD: Only available for PULL_FROM_URL. Indicates that the download from the URL is in process.
    SEND_TO_USER_INBOX: Only available for the Upload Video endpoint. Indicates that a notification has been sent to creator's inbox to complete the draft post using Tiktok's editing flow.
    PUBLISH_COMPLETE: For the Direct Post endpoint, it indicates that a video has been posted. For the Upload Video endpoint, it indicates that the uploaded video has been posted.
    FAILED: Indicates that an error has occurred and the entire process has failed.

fail_reason
	

string
	

Refer to the fail_reason table to see whether the issue is with the developer, the TikTok creator, or TikTok APIs.

publicaly_available_post_id

	

list<int64>
	

post_id is returned only if the post is published for public viewership and has been approved by the TikTok moderation process.

Creators may use the uploaded video draft to create multiple videos.

uploaded_bytes
	

int64
	

The number of bytes uploaded (1-indexed) for FILE_UPLOAD.

downloaded_bytes
	

int64
	

The number of bytes downloaded (1-indexed) for PULL_FROM_URL.
Nested error struct

HTTP Status
	

error.code
	

Description

200
	

ok
	

400
	

invalid_publish_id
	

The publish_id does not exist.

400
	

token_not_authorized_for_specified_publish_id
	

The access_token does not have authorization to cancel the publish.

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
Content Posting webhooks

These events will be sent to your registered server when you have a webhook URL configured for your app in the TikTok for Developers website.

Event Name
	

Event Values
	

Description

post.publish.failed
	

publish_id

reason

publish_type
	

The publishing action is not successful. The failure reason is sent as an enum string.

publish_type should be INBOX_SHARE for uploading videos for TikTok users to review.

post.publish.complete
	

publish_id

publish_type
	

When uploading videos, the event indicates that the TikTok user has created a post from the video you sent.

Note: It's possible for the user to make multiple posts from the video associated with one publish_id.

post.publish.inbox_delivered
	

publish_id

publish_type
	

The video has been delivered to the user's inbox of the TikTok mobile app.

publish_type can only be INBOX_SHARE when uploading videos.

post.publish.publicaly_available
	

publish_id

post_id

publish_type
	

This event is sent when a post associated with the publish_id has become publicly viewable on TikTok. Non-public posts will not trigger this event unless the user makes them public later.

publish_type can only be INBOX_SHARE for now.

post.publish.no_longer_publicaly_available
	

publish_id

post_id

publish_type
	

The event is sent when a post associated with the publish_id has ceased to be publicly viewable.

publish_type can only be INBOX_SHARE for now.
Fail reasons

The following is a list of fail_reason that may be returned by the HTTP endpoint or webhook events.

fail_reason
	

Guidance

file_format_check_failed
	

Unsupported media format. See Video Restrictions.

duration_check_failed
	

Video does not meet our duration restrictions. See Video Restrictions.

frame_rate_check_failed
	

Unsupported frame rate. See Video Restrictions.

picture_size_check_failed
	

Upsupported picture size. See Video Restrictions.

internal
	

Some parts of the TikTok server may currently be unavailable. This is a retryable error.

video_pull_failed
	

The TikTok server encountered a connection error while downloading the specified video resource, or the download is terminated since it can not be completed within the one-hour timeout.

Check if the supplied URL is publicly accessible or bandwidth-limited. If developers are certain that the URL is valid, a retry is recommended.

publish_cancelled

	

Developers can cancel an ongoing download. After a successful cancellation, developers will receive a webhook containing this error reason.
