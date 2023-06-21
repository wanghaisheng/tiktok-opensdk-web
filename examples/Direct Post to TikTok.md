1.
Request:

curl --location --request POST 'https://open.tiktokapis.com/v2/post/publish/creator_info/query/' \
--header 'Authorization: Bearer exampleUserAccessToken' \
--header 'Content-Type: application/json; charset=UTF-8'

2.
    If you have the video file locally, set the source parameter to FILE_UPLOAD in your request.
    If the video is hosted on a URL, set the source parameter to PULL_FROM_URL.
    
    
2.1 FILE_UPLOAD

```
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

Response:
```

    If you are using source=FILE_UPLOAD:
        Extract the upload_url and publish_id from the response data.
        Send the video from your local filesystem to the extracted upload_url using a PUT request. The video processing will occur asynchronously once the upload is complete.

curl --location --request PUT 'https://open-upload.tiktokapis.com/video/?upload_id=67890&upload_token=Xza123' \
--header 'Content-Range: bytes 0-30567099/30567100' \
--header 'Content-Type: video/mp4' \
--data '@/path/to/file/example.mp4'




2.2 PULL_FROM_URL

```
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
      "source": "PULL_FROM_URL",
      "video_url": "https://example.verified.domain.com/example_video.mp4",
  }
}'

Response:
```



    With the publish_id returned earlier, check for status updates using the Get Video Status endpoint.

curl --location 'https://open.tiktokapis.com/v2/post/publish/status/fetch/' \
--header 'Authorization: Bearer exampleUserAccessToken' \
--header 'Content-Type: application/json; charset=UTF-8' \
--data '{
    "publish_id": "v_pub_url~v2.123456789"
}'

