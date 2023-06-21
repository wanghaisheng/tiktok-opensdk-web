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





## File upload with  Chunk upload


File upload

Using this method, you can transfer your media to TikTok using HTTP. Upon initializing your video upload with source=FILE_UPLOAD, an upload_url will be returned. You must send the media binary to this URL.

To learn how to send videos to TikTok servers and for details on the HTTP request (endpoint, request and response schema, and headers), see the API reference for the Upload Video endpoint or the Direct Post endpoint.
Work with chunks
Chunk restrictions

    Each chunk must be at least 5 MB but no greater than 64 MB, except for the final chunk, which can be greater (up to 128 MB) to accommodate any trailing bytes.
    Videos with a total size less than 5 MB must be uploaded as a whole, with chunk_size equal to the entire video's byte size. Videos with a total size greater than 64 MB must be uploaded in multiple chunks.
    The value of chunk_size should be equal to video_size divided by total_chunk_count, rounded down to the nearest integer.
    There must be a minimum of 1 chunk and a maximum of 1000 chunks.
    File chunks must be uploaded sequentially.

Media transfer HTTP schema

PUT {UPLOAD_URL} HTTP /1.1
Content-Type: {MIME_TYPE}
Content-Length: {BYTE_SIZE_OF_THIS_CHUNK}
Content-Range: bytes {FIRST_BYTE}-{LAST_BYTE}/{TOTAL_BYTE_LENGTH}

BINARY_FILE_DATA

Examples
Chunk upload

In this example, there is a file with a size of 50,000,123 bytes. The chunk size is specified to be 10,000,000 bytes. The trailing 123 bytes is merged into the 10,000,000-byte chunk to meet the restriction that each chunk must be greater than 5 MB.

Example UPLOAD_URL=https://upload.us.tiktokapis.com/video/?upload_id=67890&upload_token=chunkexample will be shared across all chunks.

Variable
	

1st Request
	

2nd Request
	

3rd Request
	

4th Request
	

5th Request

MIME_TYPE
	

video/mp4
	

video/mp4
	

video/mp4
	

video/mp4
	

video/mp4

TOTAL_BYTE_LENGTH
	

50,000,123
	

50,000,123
	

50,000,123
	

50,000,123
	

50,000,123

BYTE_SIZE_OF_THIS_CHUNK
	

10,000,000
	

10,000,000
	

10,000,000
	

10,000,000
	

10,000,123

FIRST_BYTE
	

0
	

10,000,000
	

20,000,000
	

30,000,000
	

40,000,000

LAST_BYTE
	

9,999,999
	

19,999,999
	

29,999,999
	

39,999,999
	

50,000,122

BINARY_FILE_DATA
	

BINARY1
	

BINARY2
	

BINARY3
	

BINARY4
	

BINARY5

response HTTP status
	

206
	

206
	

206
	

206
	

201

The following is the corresponding source_info for initializing video upload.

"source_info": {
      "source": "FILE_UPLOAD",
      "video_size": 50000123
      "chunk_size":  10000000
      "total_chunk_count": 5
  }

Whole upload

In this example, the media file is 4 MB, which must be uploaded as a whole in one request.

Variable
	

Single Request

UPLOAD_URL
	

https://open-upload.tiktokapis.com/video/?upload_id=123&upload_token=wholeexample

MIME_TYPE
	

video/mp4

TOTAL_BYTE_LENGTH
	

4,194,304

BYTE_SIZE_OF_THIS_CHUNK
	

4,194,304

FIRST_BYTE
	

0

LAST_BYTE
	

4,194,303

BINARY_FILE_DATA
	

BINARY1

response status code
	

201

The following is the corresponding source_info for initializing video upload.

"source_info": {
      "source": "FILE_UPLOAD",
      "video_size": 4194304
      "chunk_size":  4194304
      "total_chunk_count": 1
  }

Response

HTTP Code
	

Status
	

Description

201

	

Created

	

All parts are uploaded.

TikTok will start the posting process.

206
	

PartialContent

	

The current chunk has been successfully processed. There are additional chunks yet to be uploaded.

400
	

BadRequest

	

Malformated request headers, or BYTE_SIZE_OF_THIS_CHUNKdoes not reflect the true byte size of the binary in the request body.

403
	

Forbidden
	

The upload_url has expired.

404
	

NotFound
	

TikTok cannot find a valid upload task given the upload_url.

416

	

RequestedRangeNotSatisfiable

	

Content-Range does not reflect the actual upload progress.

5xx

	

InternalServerError

	

Gateway connection error or TikTok Internal error. You should retry submitting this chunk.

The response header includes the following key-value pair indicating the number of bytes uploaded:

Content-Range:bytes 0-{UPLOADED_BYTES}/{TOTAL_BYTE_LENGTH}
