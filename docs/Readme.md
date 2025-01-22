# NeuroLensAI Video Generation API Documentation

## Base URL
```
https://nova.neuro-lens.com/api
```

---

## Authentication

The API requires an authentication key to be passed in the request header. This key is tied to the email address provided during the waitlist application. **Please use the md5 hash of your email address in lower case.** Only whitelisted emails are allowed to use the service.

### Request Header Format
```http
Authorization: Bearer <md5-of-you-email-address>
```

---

## Endpoint 1: Submit AI Video Generation Request

### Description
Submits a request to generate an AI video based on the provided prompt and input image URL. This operation is asynchronous, and the generated video can be queried later.

### Method
`POST`

### URL
```
/v1/generate-video
```

### Request Headers
| Header             | Type   | Required | Description                                    |
|--------------------|--------|----------|------------------------------------------------|
| Authorization      | String | Yes      | Bearer token linked to a whitelisted email.    |
| Content-Type       | String | Yes      | Must be `application/json`.                   |

### Request Body
| Field              | Type   | Required | Description                                    |
|--------------------|--------|----------|------------------------------------------------|
| prompt             | String | Yes      | A textual description of the desired video.    |
| image_url          | String | Yes      | URL of the image to be used as input.          |

#### Example Request Body
```json
{
  "prompt": "A futuristic city with flying cars",
  "image_url": "https://example.com/images/sample.jpg"
}
```

### Response
The API responds with a JSON object containing the status and a unique request ID.

#### Success Response
**Status Code:** `202 Accepted`

```json
{
  "status": "processing",
  "request_id": "12345abcdef",
  "message": "Your video generation request has been received. Please query the status using the request ID."
}
```

| Field              | Type   | Description                                     |
|--------------------|--------|-------------------------------------------------|
| status             | String | Indicates the request status (`processing`).   |
| request_id         | String | Unique identifier for the video generation request. |
| message            | String | Additional information about the request.      |

#### Error Response
**Status Code:** `400 Bad Request`

```json
{
  "status": "error",
  "message": "Invalid request. Please check the prompt and image_url fields."
}
```

**Status Code:** `401 Unauthorized`

```json
{
  "status": "error",
  "message": "Invalid or missing authentication token."
}
```

**Status Code:** `500 Internal Server Error`

```json
{
  "status": "error",
  "message": "An unexpected error occurred. Please try again later."
}
```

---

## Endpoint 2: Query Video Generation Status

### Description
Queries the status of a previously submitted video generation request using the unique request ID.

### Method
`GET`

### URL
```
/v1/generate-video/{request_id}
```

### Request Headers
| Header             | Type   | Required | Description                                    |
|--------------------|--------|----------|------------------------------------------------|
| Authorization      | String | Yes      | Bearer token linked to a whitelisted email.    |

### Path Parameters
| Parameter          | Type   | Required | Description                                    |
|--------------------|--------|----------|------------------------------------------------|
| request_id         | String | Yes      | The unique request ID returned from the submit endpoint. |

### Response
The API responds with a JSON object containing the status, video URL (if ready), and metadata.

#### Success Response (Processing)
**Status Code:** `200 OK`

```json
{
  "status": "processing",
  "message": "Your video is still being generated. Please try again later."
}
```

#### Success Response (Completed)
**Status Code:** `200 OK`

```json
{
  "status": "completed",
  "video_url": "https://example.com/videos/generated_video.mp4",
  "metadata": {
    "prompt": "A futuristic city with flying cars",
    "image_url": "https://example.com/images/sample.jpg",
    "video_duration": 10,
    "creation_time": "2025-01-22T12:34:56Z"
  }
}
```

| Field              | Type   | Description                                     |
|--------------------|--------|-------------------------------------------------|
| status             | String | Indicates the request status (`completed`, `processing`, or `failed`). |
| video_url          | String | URL where the generated video can be downloaded (if completed). |
| metadata           | Object | Additional information about the generated video (if completed). |
| metadata.prompt    | String | The original prompt provided in the request.   |
| metadata.image_url | String | The input image URL provided in the request.    |
| metadata.video_duration | Integer | Duration of the generated video (in seconds). |
| metadata.creation_time | String | Timestamp of when the video was created (ISO 8601 format). |

#### Error Response
**Status Code:** `404 Not Found`

```json
{
  "status": "error",
  "message": "Request ID not found. Please check the ID and try again."
}
```

**Status Code:** `401 Unauthorized`

```json
{
  "status": "error",
  "message": "Invalid or missing authentication token."
}
```

**Status Code:** `500 Internal Server Error`

```json
{
  "status": "error",
  "message": "An unexpected error occurred. Please try again later."
}
```

---

## Waitlist Application

If you do not have access, please apply via the waitlist:

**Waitlist URL:** [https://forms.gle/ToiKi9cPTqgbgGFJ6](https://forms.gle/ToiKi9cPTqgbgGFJ6)

