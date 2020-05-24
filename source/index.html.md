---
title: Hi there!

language_tabs: # must be one of https://git.io/vQNgJ
  - shell: Sample request
  - json: Sample response

toc_footers:
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction
*Welcome to my Slate page. This reference document is based on an exercise in <a href ="https://www.udemy.com/course/learn-api-technical-writing-2-rest-for-writers/" target ="_blank">Peter Gruenbaum's course on API docs</a>. <br>This API does not exist. :)*

Use the SoundDate API to upload a sound file to your profile page, and get information on other users' sound files.
 
# Authentication

```shell
curl GET '{{soundgate.com}}/profile/token?email={{username}}&password={{password}}'
```
```json
# Sample response for Bearer token:
{
  "code": "0",
  "data": {
    "token": "QyJUCOyQThm3xG2Hwk5X"
  }
}
```

SoundDate uses Bearer authentication to allow access to the API. Include the token in all API requests to the server in the API header.  
Generate a Bearer token with your user name and password either from the UI (see `link`) or from command line.

## Headers

```shell
curl GET https://api.sounddate.com/profile/sound \
--header 'Content-Type: audio/mpeg' \
--header 'Accept: application/json' \
--header "Authorization: {sounddatetoken}" \

Replace `sounddatetoken` with your Bearer token.
```

Refer to the following table for details.

| Name         | Description                            | Required | Values                                                           |
|--------------|----------------------------------------|----------|------------------------------------------------------------------|
| Bearer       | Access Token                           | Required | generate a Bearer token with your user name and password.  |
| Content-Type | The format of the sound file to upload | Optional | audio/mpeg                                       |
| Accept       | The format of the returned data        | Optional | application/xml or application/json <br>Default is application/json. |

# Resource summary


| Method         | HTTP request                            | Description |
|--------------|----------------------------------------|----------|
| create       | `POST https://api.sounddate.com/profile/sound/{sound file}` | Uploads a new sound file in the user's profile. | 
| get | `GET https://api.sounddate.com/profile/{user id}/profile/sound` |Retrieves a list of sound file URLs and their lengths for the specified user.  | 

# Upload a sound file

```shell
curl POST https://api.sounddate.com/profile/sound/{sound file} \
--header 'Content-Type: audio/mpeg' \
--header 'Accept: application/json' \
--header "Authorization: {sounddatetoken}" \
```

```json
200 - OK
[
  {
    "id": 3543,
    "length": 19.8
  }
]
```

Uploads a sound file with the following characteristics:  
- Maximum duration: 5 minutes or shorter  
- Maximum file size: 4 MB  
- Accepted file types: .mpeg or .wav

### HTTP Request
`POST https://api.sounddate.com/profile/sound/{sound file}`

### Response

Parameter | Description | Type | Notes
--------- | ------- | -----------| ------
`id` | The ID of the new sound file | integer | 
`length` | The length of the sound file | float | Length is in seconds.


# Retrieve a list of sound files

```shell
curl GET https://api.sounddate.com/profile/345354/profile/sound?sortOrder=shortest \
--header 'Content-Type: audio/mpeg' \
--header 'Accept: application/json' \
--header "Authorization: {sounddatetoken}" 
```

```json
{
 "soundFiles": [
 {
 "id": 23456,
 "url": "https://api.sounddate.com/profile/sound/23456.mp3",
 "length": 11.2
 },
 {
 "id": 24559,
 "url": "https://api.sounddate.com/profile/sound/24559.mp3",
 "length": 19.8
 }
 ]
}

```

Retrieves a list of sound file URLs and their lengths for the specified user.

### HTTP Request

`GET https://api.sounddate.com/profile/{user id}/profile/sound`  
where `{user id}` is the ID of the user whose sound files you're interested in.

### Query Parameters

Parameter | Description| Type| Required | Notes 
--------- | ----------- | --------| --------| ------------
sortOrder | The order to return the sound file information. | string | Optional | Valid values: `mostRecent`, `earliest`, `shortest`, `longest`. <br>Default is `mostRecent`.  

Note:  
- `mostRecent` returns the most recent sound files to the earliest.<br>
- `earliest` returns the earliest sound files to the most recent.<br>
- `shortest` returns the shortest sound files to longest.<br>
- `longest` returns the longest sound files to the shortest.

### Response

| Element    | Description                    | Type    | Notes                 |
|------------|--------------------------------|---------|-----------------------|
| **soundFiles** | List of sound file information | Array   |                       |
|         `id` | The ID of the sound file       | integer |                       |
|        `url` | The URL of the sound file      | string  |                       |
|     `length` | The length of the sound file   | float   | Length is in seconds. |