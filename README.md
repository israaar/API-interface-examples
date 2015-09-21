# Chrome Postman

In order to easily test TeraDeep's API, we can use [Postman](https://www.getpostman.com/) as shown in the video tutorial below.
The link for importing TeraDeep's API Postman collection is: https://goo.gl/YWVwGD

Postman

As an addition, we provide cURL code below.


# cURL Examples

Put your API key after "Basic" instead of mRFJ...

## Get models

    curl -H "Authorization: Basic mRFJHFaMlUG40wS1BTdEVwL0tmLDbnlmVd3T09ZlYvLUzZ08VXUFJyZGNnPk1ZSHZhQXdiRDJDJhJDEw" http://api.teradeep.com:8088/getmodels

Output:

    [{"name":"food","description":"E. Culurciello\nJanuary 2015\n\nNeural network:\nhttps://github.com/teradeep/train-net-master/blob/master/models/td-net-large.lua\n\nTrained on:\nhttps://www.vision.ee.ethz.ch/datasets_extra/food-101/\n“a challenging data set of 101 food categories, with 101'000 images. For each class, 250 manually reviewed test images are provided as well as 750 training images.”"},{"name":"generic","description":"A. Canziani May 2015 Large 1-weird trick neural net trained on camFind \ndataset 1000 categories of most typical objects (top 1000 words in \ncamFind captions) 10,000 images per category\n"},{"name":"home-basic","description":""}]

## Get targets

    curl -H "Authorization: Basic mRFJHFaMlUG40wS1BTdEVwL0tmLDbnlmVd3T09ZlYvLUzZ08VXUFJyZGNnPk1ZSHZhQXdiRDJDJhJDEw" http://api.teradeep.com:8088/gettargets -F network=home-basic
	
Output:

```
{
  "targets": [
    "dog",
    "cat",
    "pet",
    "person"
  ]
}
```

## Process image

    curl -H "Authorization: Basic mRFJHFaMlUG40wS1BTdEVwL0tmLDbnlmVd3T09ZlYvLUzZ08VXUFJyZGNnPk1ZSHZhQXdiRDJDJhJDEw" http://api.teradeep.com:8088/image/process -F inputnetwork=home-basic -F inputimagefile=@image.jpg 
	
Output:

```
{
  "errorcode": 0,
  "result": [
    {
      "category": "person",
      "confidence": 0.755
    },
    {
      "category": "cat",
      "confidence": 0.223
    },
    {
      "category": "chair",
      "confidence": 0.006
    },
    {
      "category": "dog",
      "confidence": 0.006
    },
    {
      "category": "book",
      "confidence": 0.005
    }
  ],
  "processid": "55fc6c9592bfa83907818e21"
}
```
    curl -H "Authorization: Basic mRFJHFaMlUG40wS1BTdEVwL0tmLDbnlmVd3T09ZlYvLUzZ08VXUFJyZGNnPk1ZSHZhQXdiRDJDJhJDEw" http://api.teradeep.com:8088/image/process -F inputnetwork=home-basic -F inputimagefile=@image.jpg -F targets=person,dog

Output:

```
{
  "errorcode": 0,
  "result": [
    {
      "category": "person",
      "confidence": 0.755
    },
    {
      "category": "dog",
      "confidence": 0.006
    }
  ],
  "processid": "55fc6caa92bfa83907818e22"
}
```

## Process video

    curl -H "Authorization: Basic mRFJHFaMlUG40wS1BTdEVwL0tmLDbnlmVd3T09ZlYvLUzZ08VXUFJyZGNnPk1ZSHZhQXdiRDJDJhJDEw" http://api.teradeep.com:8088/video/submit -F inputnetwork=home-basic -F inputvideofile=@video.mp4 -F targets=person

Output:

```
{
  "processid": "55fc6d2892bfa83907818e23",
  "result": {
    "message": "Video submitted to Queue for processing"
  }
}
```

    curl -H "Authorization: Basic mRFJHFaMlUG40wS1BTdEVwL0tmLDbnlmVd3T09ZlYvLUzZ08VXUFJyZGNnPk1ZSHZhQXdiRDJDJhJDEw" http://api.teradeep.com:8088/video/submit -F inputnetwork=home-basic -F inputvideofile=@video.mp4 -F targets=person -F outputtype=srt

Output:

```
```
	
## Video status

    curl -H "Authorization: Basic mRFJHFaMlUG40wS1BTdEVwL0tmLDbnlmVd3T09ZlYvLUzZ08VXUFJyZGNnPk1ZSHZhQXdiRDJDJhJDEw" http://api.teradeep.com:8088/video/status -F processid=55fc6ab592bfa83907818e1f

Output:

```
{
  "status": "Done",
  "statuscode": "3",
  "processid": "55fc6d2892bfa83907818e23",
  "datetime": "2015-09-18T19:59:36.000Z"
}
```

## Video result

    curl -H "Authorization: Basic mRFJHFaMlUG40wS1BTdEVwL0tmLDbnlmVd3T09ZlYvLUzZ08VXUFJyZGNnPk1ZSHZhQXdiRDJDJhJDEw" http://api.teradeep.com:8088/video/result -F processid=55fc6ab592bfa83907818e1f

Output:

```
{
  "_id": "55fc6d2892bfa83907818e23",
  "status": "Done",
  "statuscode": "3",
  "result": {
    "version": "1.0",
    "model": "home-basic",
    "videofps": "62.651452282158",
    "videoframes": "21",
    "detections": [
      {
        "text": "person",
        "end_frame": 17,
        "end_time": "00:00:00,271",
        "chart": [
          {
            "confidence": 0.504,
            "category": "person"
          }
        ],
        "thumbnail": "http://api.teradeep.com/data/users/vitez/webvideos/55fc6d2892bfa83907818e23/thumbs/20150918_155938_966748_person.jpg",
        "start_frame": 16,
        "start_time": "00:00:00,255"
      }
    ]
  }
}
```
