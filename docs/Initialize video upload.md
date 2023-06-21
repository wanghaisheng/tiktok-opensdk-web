Initialize video upload

To upload a video to a TikTok user's account, the first step is to initialize the upload.

HTTP URL
	

/v2/post/publish/inbox/video/init/

HTTP Method
	

POST

Scope
	

video.upload
Request

Restriction: Each user access_token is limited to 6 requests per minute.
Header

Field Name
	

Description
	

Value
	

Required

Authorization
	

The token that bears the authorization of the TikTok user, which is obtained through /oauth/access_token/.
	

Bearer {$UserAccessToken}
	

true

Content-Type
	

The content format of the body of this HTTP request.
	

application/json; charset=UTF-8
	

true
Body

Field Name
	

Nested Field
	

Type
	

Description
	

Required

source_info
	

source

	

string
	

The mechanism by which you will provide the video. You can choose from FILE_UPLOAD and PULL_FROM_URL.
	

true

video_size
	

int64
	

The size of the video to be uploaded in bytes.
	

true for FILE_UPLOAD

chunk_size
	

int64
	

The size of the chunk in bytes.
	

true for FILE_UPLOAD

total_chunk_count
	

int64
	

The total number of chunks.
	

true for FILE_UPLOAD

video_url

	

string
	

The URL of the video to be uploaded. The domain or URL prefix of the video_url should already be verified. Learn more about verifying the URL prefix.
	

true for PULL_FROM_URL

Examples

Example with source=FILE_UPLOAD:

curl --location 'https://open.tiktokapis.com/v2/post/publish/inbox/video/init/' \
--header 'Authorization: Bearer exampleUserAccessToken' \
--header 'Content-Type: application/json; charset=UTF-8'
--data '{
    "source_info": {
        "source": "FILE_UPLOAD",
        "video_size": exampleVideoSize,
        "chunk_size" : exampleChunkSize,
        "total_chunk_count": exampleTotalChunkCount
    }
}'

Example withsource=PULL_FROM_URL:

curl --location 'https://open.tiktokapis.com/v2/post/publish/inbox/video/init/' \
--header 'Authorization: Bearer exampleUserAccessToken' \
--header 'Content-Type: application/json' \
--data '{
    "source_info": {
        "source": "PULL_FROM_URL",
        "video_url": "https://example.verified.domain.com/example_video.mp4",
    }
}'

Response

Field Name
	

Nested Field
	

Type
	

Description

data
	

publish_id

	

string

	

An identifier to track the posting action, which you can use to check status.

The maximum length of this field is 64.

upload_url

	

string

	

The URL provided by TikTok where the video file can be uploaded.

The maximum length of this field is 256. This field is only for source=FILE_UPLOAD.

error
	

code
	

string
	

You can decide whether the request is successful based on the error code. Any code other than ok indicates the request did not succeed. Learn more about error codes.

message
	

string
	

A human readable description of the error.

logid
	

string
	

A unique identifier for the execution of this request.

Note: The upload_url is valid for one hour after issuance. The upload must be completed in this time range.
Example

200 OK
{
    "data": {
        "publish_id": "v_inbox_file~v2.123456789",
        "upload_url": "https://open-upload.tiktokapis.com/video/?upload_id=67890&upload_token=Xza123"    
    },
    "error": {
         "code": "ok",
         "message": "",
         "log_id": "202210112248442CB9319E1FB30C1073F3"
     }
}

Error codes

HTTP status code
	

Error code
	

Descrption

400
	

invalid_param
	

Check error message for details.

403

	

spam_risk_too_many_pending_share

	

The daily upload cap from API is reached for the current user.

To reduce spamming, TikTok limits the number of videos that can be uploaded via API that are not pending approval and posting by the creator. There may be at most 5 pending shares within any 24-hour period.

spam_risk_user_banned_from_posting
	

The user is banned from making new posts.

url_ownership_unverified

	

To use PULL_FROM_URL as the video transfer method, developer must verify the ownership of the URL prefix or domain. Refer to this doc for more details.

401
	

access_token_invalid
	

The access_token for the TikTok user is invalid or has expired.

scope_not_authorized
	

The access_token does not bear user's grant on video.upload scope.

429
	

rate_limit_exceeded
	

Your request is blocked due to exceeding the API rate limit.

5xx
		

TikTok server or network error. Try again later.
