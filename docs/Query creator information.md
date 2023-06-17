https://developers.tiktok.com/doc/content-posting-api-reference-direct-post/


This API returns profile and permission information of the current user.

When rendering the Export to TikTok page, your app must invoke the API and use the latest creator information returned to display the account's available privacy level options and video interaction settings.

HTTP URL

/v2/post/publish/creator_info/query/

HTTP Method

POST

Scope

video.publish

Request
Note: Each user access_token is limited to 20 requests per minute.

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

Example
curl --location --request POST 'https://open.tiktokapis.com/v2/post/publish/creator_info/query/' \
--header 'Authorization: Bearer exampleUserAccessToken' \
--header 'Content-Type: application/json; charset=UTF-8'
Response
Field Name

Nested Field

Type

Description

data

creator_avatar_url

string

The URL of the TikTok creator's avatar with a TTL of 2 hours.

creator_username

string

The unique ID of the TikTok creator.

creator_nickname

string

The nickname of TikTok creator.

privacy_level_options


list<string>


If the TikTok creator account is public, the available options are:

PUBLIC_TO_EVERYONE
MUTUAL_FOLLOW_FRIENDS
SELF_ONLY

If the TikTok creator account is private, the available options are:

FOLLOWER_OF_CREATOR
MUTUAL_FOLLOW_FRIENDS
SELF_ONLY
comment_disabled


boolean


Returns true if the creator sets comment interaction to "No one" in their privacy setting.

duet_disabled


boolean

Return true if the creator account is private or they set the Duet interaction to "No one" in their privacy setting.

stitch_disabled


boolean


Return true if the creator account is private or they set the Stitch interaction to "No one" in their privacy setting.

max_video_post_duration_sec

int32

The longest video duration in seconds that the TikTok creator can post. Different users have different maximum video-duration privileges. Developers should use this field to stop video posts that are too long.

error

code

string

You can decide whether the request is successful based on the error code. Any code other than ok indicates the request did not succeed. Learn more about error codes.

message

string

A human readable description of the error.

logid

string

A unique identifier for the execution of this request. Different users have different maximum video-duration privileges. Developers should use this field to stop video posts that are too long.

Note: Every field in the response.data (excluding max_video_post_duration_sec) must be displayed on your app's Export screen to the user. This will indicate the TikTok account to which the post will be published and provide creators with the available privacy settings they can choose from. Learn more about the UX guidelines here.

  
  Example
200 OK

{
   "data":{
      "creator_avatar_url": "https://lf16-tt4d.tiktokcdn.com/obj/tiktok-open-platform/8d5740ac3844be417beeacd0df75aef1",
      "creator_username": "tiktok",
      "creator_nickname": "TikTok Official",
      "privacy_level_options": ["PUBLIC_TO_EVERYONE", "MUTUAL_FOLLOW_FRIENDS", "SELF_ONLY"] 
      "comment_disabled": false,
      "duet_disabled": false,
      "stitch_disabled": true,
      "max_video_post_duration_sec": 300
   },
    "error": {
         "code": "ok",
         "message": "",
         "log_id": "202210112248442CB9319E1FB30C1073F3"
     }
}
