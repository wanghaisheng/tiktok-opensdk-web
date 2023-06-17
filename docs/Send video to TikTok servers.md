Send video to TikTok servers
Note: If you used the source=PULL_FROM_URL to initialize the video export, you can skip this part. The TikTok server will handle the video uploading process for you.

Once you have initialized the video export and received an upload_url, you must send the video file to TikTok for processing. We support many video formats and provide chunking for larger files. Learn more about media transmission.

HTTP URL

Returned in upload_url

HTTP Method

PUT

Note: Use the entire URL returned as the upload_url including the returned query parameters.

Request
Note: This document provides schemas for the API request and response. Learn more about media upload formats and advanced capabilities.

Header
Field Name

Description

Value

Required

Content-Type

The content format of the body of this HTTP request.

Select from:

video/mp4
video/quicktime
video/webm
true


Content-Length

Byte size of this chunk.

{BYTE_SIZE_OF_THIS_CHUNK}

true


Content-Range

The metadata describing the portion of the overall file contained in this chunk.

bytes {FIRST_BYTE}-{LAST_BYTE}/{TOTAL_BYTE_LENGTH}

true

Body
The binary file data.

Example
curl --location --request PUT 'https://open-upload.tiktokapis.com/video/?upload_id=67890&upload_token=Xza123' \
--header 'Content-Range: bytes 0-30567099/30567100' \
--header 'Content-Length: 30567100'\
--header 'Content-Type: video/mp4' \
--data '@/path/to/file/example.mp4'
