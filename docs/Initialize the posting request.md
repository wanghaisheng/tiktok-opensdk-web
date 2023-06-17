Initialize the posting request
Once users have provided the necessary metadata for their posts and given explicit consent to send their video to TikTok, the next step is to initialize the posting request.

HTTP URL

/v2/post/publish/video/init/

HTTP Method

POST

Scope

video.publish

Request
Note: Each user access_token is limited to 6 requests per minute.

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

Nested Field Name

Type

Description

Required

post_info

privacy_level


string

Enum of:

PUBLIC_TO_EVERYONE

MUTUAL_FOLLOW_FRIENDS

FOLLOWER_OF_CREATOR

SELF_ONLY


The provided value must match one of the privacy_level_options returned in the /creator_info/query/ API.

true


title


string

The video caption. Hashtags (#) and mentions (@) will be matched, or deliminated by spaces or new lines.


The maximum length is 150 in UTF-16 runes.

If not specified, the ticket post will not have any captions.










false


disable_duet

bool

If set to true, other TikTok users will not be allowed to make Duets using this post.


TikTok server disables Duets for private accounts and those who set the Duet permission to "No one" in their privacy setting.

disable_stitch


bool

If set to true, other TikTok users will not be allowed to make Stitches using this post.


TikTok server disables Stitches for private accounts and those who set the Stitch permission to "No one" in their privacy setting.

disable_comment


bool


If set to true, other TikTok users will not be allowed to make comments on this post.


TikTok server disables comments for users who set the Stitch permission to "No one" in their privacy setting.

video_cover_timestamp_ms

int32

Specifies which frame (measured in milli-seconds) will be used as the video cover.


If not set, or the specified value is invalid, the cover is set to the first frame of the uploaded video.

source_info

source


string

Choose from:

PULL_FROM_URL

FILE_UPLOAD

Learn about the limitations for these file transmission methods.

true


video_url

string

A public-accessible URL from which the TikTok server will pull to retrieve the video resource.

true for PULL_FROM_URL

video_size

int64

The size of the to-be-uploaded video file in bytes.


true forFILE_UPLOAD


chunk_size

int64

The size of the chunk in bytes.

total_chunk_count

int64

The total number of chunks.


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


The URL provided by TikTok where the video file can be uploaded. The maximum length of this field is 256.

This field is only for source=FILE_UPLOAD.

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

Error codes
HTTP Status

error.code

Description

400

invalid_param

Check error message for details.

403


spam_risk_too_many_posts

The daily post cap from API is reached for the current user.

spam_risk_user_banned_from_posting

The user is banned from making new posts.


reached_active_user_cap

The daily quota for active publishing users from your client is reached.

unaudited_client_can_only_post_to_private_accounts

Unaudited clients can only post to private account. The publish attempt will be blocked when calling /publish/video/init/.

url_ownership_unverified


To use PULL_FROM_URL as the video transfer method, developer must verify the ownership of the URL prefix or domain. Refer to this doc for more details.

privacy_level_option_mismatch

privacy_level is not specified or not among the options from the privacy_level_options returned in /publish/creator_info/query/ API.


All clients are required to correctly display the creator account's privacy level options and honor the users' choice. Occurances of this error for product-use applications suggest violations to TikTok's product-use guidance.

401

access_token_invalid

The access_token is invalid or has expired.

scope_not_authorized

The access_token does not bear user's grant on video.publish scope

429

rate_limit_exceeded

Your request is blocked due to exceeding the API rate limit.

5xx

TikTok server or network error. Try again later.



Example
curl --location 'https://open.tiktokapis.com/v2/post/publish/video/init/' \
--header 'Authorization: Bearer act.6ee8f794b64300804c5767f3c8cbf2c1ppnjwo0fyzE4NI9gXt0qxxvqD3Ft' \
--header 'Content-Type: application/json; charset=UTF-8' \
--data-raw '{
  "post_info": {
    "title": "this will be a funny #cat video on your @tiktok #fyp",
    "privacy_level": "MUTUAL_FOLLOW_FRIENDS",
    "disable_duet": false,
    "disable_comment": true,
    "disable_stitch": false,
    "video_cover_timestamp_ms": 1000
  },
  "source_info": {
      "source": "FILE_UPLOAD",
      "video_size": 50000123,
      "chunk_size":  10000000,
      "total_chunk_count": 5
  }
}'

Example
200 OK
{
    "data": {
        "publish_id": "v_pub_file~v2-1.123456789",
        "upload_url": "https://open-upload.tiktokapis.com/video/?upload_id=67890&upload_token=Xza123"    
    },
    "error": {
         "code": "ok",
         "message": "",
         "log_id": "202210112248442CB9319E1FB30C1073F3"
     }
}
