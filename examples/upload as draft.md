Upload as a Draft

This section demonstrates how to successfully send a video to TikTok, ready for the user to review and post.

User notified of video upload
	

User reviews and posts video

    To initiate video upload on TikTok's servers, you must invoke the Content Posting API - Video Upload endpoint. You have the following two options:
        If you have the video file locally, set the source parameter to FILE_UPLOAD in your request.
        If the video is hosted on a URL, set the source parameter to PULL_FROM_URL.

Examples

Example using source=FILE_UPLOAD:

Request:

curl --location 'https://open.tiktokapis.com/v2/post/publish/inbox/video/init/' \
--header 'Authorization: Bearer exampleUserAccessToken' \
--header 'Content-Type: application/json' \
--data '{
    "source_info": {
        "source": "FILE_UPLOAD",
        "video_size": exampleVideoSize,
        "chunk_size" : exampleVideoSize,
        "total_chunk_count": 1
    }
}'

Response:

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


Example using source=PULL_FROM_URL:

Request:

curl --location 'https://open.tiktokapis.com/v2/post/publish/inbox/video/init/' \
--header 'Authorization: Bearer exampleUserAccessToken' \
--header 'Content-Type: application/json' \
--data '{
    "source_info": {
        "source": "PULL_FROM_URL",
        "video_url": "https://example.verified.domain.com/example_video.mp4",
    }
}'

Response:

200 OK

{
    "data": {
        "publish_id": "v_inbox_url~v2.123456789"
    },
    "error": {
         "code": "ok",
         "message": "",
         "log_id": "202210112248442CB9319E1FB30C1073F4"
     }
}


    If you are using source=FILE_UPLOAD:
        Extract the upload_url and publish_id from the response data.
        Send the video from your local filesystem to the extracted upload_url using a PUT request. The video processing will occur asynchronously once the upload is complete.

curl --location --request PUT 'https://open-upload.tiktokapis.com/video/?upload_id=67890&upload_token=Xza123' \
--header 'Content-Range: bytes 0-30567099/30567100' \
--header 'Content-Type: video/mp4' \
--data '@/path/to/file/example.mp4'

    With the publish_id returned earlier, check for status updates using the Get Video Status endpoint.

curl --location 'https://open.tiktokapis.com/v2/post/publish/status/fetch/' \
--header 'Authorization: Bearer exampleUserAccessToken' \
--header 'Content-Type: application/json; charset=UTF-8' \
--data '{
    "publish_id": "v_inbox_file~v2.123456789"
}'
