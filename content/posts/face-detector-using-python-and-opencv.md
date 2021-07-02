---
title: "Face Detection using Python & OpenCV"
date: 2021-07-02T10:17:37+05:30
tags: ["Python","OpenCV","face-detection"]
author: "Akash Gajare"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
description: "Face Detection using Python & OpenCV"
canonicalURL: "https://agajareiitr.github.io/posts/face-detector-using-python-and-opencv/"
disableHLJS: true # to disable highlightjs
disableShare: false
disableHLJS: false
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
cover:
    image: "/images/face-detection-python.png" # image path/url
    alt: "face-detection-python.png" # alt text
    caption: "Source : Google Images" # display caption under cover
    relative: false # when using page bundles set this to true
    hidden: false # only hide on current single page
---

<p align="center">
  <a href="https://github.com/agajareiitr/simple-face-detector-python"> 
  <img  src="https://github-readme-stats.vercel.app/api/pin/?username=agajareiitr&repo=simple-face-detector-python&bg_color=141515&text_color=9f9f9f"/>
    </a>
</p>

## In this Tutorial we will make a very simple face detector app using Python and a Pre-trained data of haarcascade
---

## This Tutorial consists of 2 steps:


## 1. Pre-requisites :
---
- Make a python file called `face-detector.py`
- Download this file [haarcascade frontalface default.xml](https://github.com/opencv/opencv/blob/master/data/haarcascades/haarcascade_frontalface_default.xml) and save in the current directory
- your directory must look like this

        └── Face Detector Folder
            ├── face-detector.py
            └── haarcascade_frontalface_default.xml

## 2. Now Let's write some code
### Below is a step by step process to make you understand the meaning of each line
---
### 1. Importing the opencv library 
```python
# importing OpenCV library if not present then run
# pip install opencv-python
import cv2
```
### 2. Loading Pre-trained data of frontal-face recognition from the current directory
```python
Trained_data = cv2.CascadeClassifier("haarcascade_frontalface_default.xml")
```
    Explanation: 
    OpenCV provides a pre-trained classifier that has a the chain of haar features the best matches a frontal face.

    After it's classified. we just pass a frame of image into the classifier, and it's run through all of the haar cascades. if it found a match it's a face.

    Hence OpenCV does all the hardwork for us

### 3. for now we will focus on real-time video capture but you can also uses a single image, below are codes for everything

```python
# for an image file
# image = cv2.imread("image_file_name.jpg/png")

# for Real time Video
webcam_image = cv2.VideoCapture(0)

# for a video file
# webcam_image = cv2.VideoCapture("video_file_name.mp4")
```
### 4. After setting up camera feed we now have to read the real-time image
`NOTE: frame_confirm is just a boolean vaule and frame is the image fetched by camera`
```python
# Read the current frame
frame_confirm, frame = webcam_image.read()
```
### 5. Converting the frame image to grayscale image
```python
# converting to grayscale (important step)
grayscaled_image = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
```
    Explanation: This Step is important because OpenCV searches for Change in Brightness which can only be found out in a grayscaled image this is how OpenCV works
### 6. Now let OpenCV do it work, i.e finding the face coordinates
```python
# Detexting face coordinates
face_coordinates = Trained_data.detectMultiScale(grayscaled_image)
```
### 7. Now that we have face coordinates, let's use it and make arectangle around the face using those coordinates
```python
# forming a rectacngle around the face/faces
for (x, y, width, height) in face_coordinates:
    cv2.rectangle(frame, (x, y), (x+width, y+height), (0, 255, 0), 2)
```
### 8. Now to View the Video feed/image and rectangele around it and to sop the feed lets a key which when pressed quit the video feed
```python
# Viewing the Video Feed
cv2.imshow("Akash's Face Detector", frame)

# assigning key pressed so that we can quit using specific key
key = cv2.waitKey(1)

# press q or Q to quit from the video capture window
if key == 81 or key == 113:
    break

# releasing webcam as it's a good practice
webcam_image.release()
```
### 9. As we need to run this in real-time we need to add all the above code in a while loop which the key press break the loop, Below is the full Code
```python
# importing OpenCV library if not present then run
# pip install opencv-python
import cv2

# loading Pre-trained data on face front face from opencv it uses haar cascade algo
Trained_data = cv2.CascadeClassifier("haarcascade_frontalface_default.xml")

# for an image file
# image = cv2.imread("image_file_name.jpg/png")

# for Real time Video
webcam_image = cv2.VideoCapture(0)

# for a video file
# webcam_image = cv2.VideoCapture("video_file_name.mp4")

# while loop so that video capture runs continuously
while True:

    # Read the current frame
    frame_confirm, frame = webcam_image.read()

    # converting to grayscale (important step)
    grayscaled_image = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)

    # Detexting face coordinates
    face_coordinates = Trained_data.detectMultiScale(grayscaled_image)

    # forming a rectacngle around the face/faces
    for (x, y, width, height) in face_coordinates:
        cv2.rectangle(frame, (x, y), (x+width, y+height), (0, 255, 0), 2)

    # Viewing the Video Feed
    cv2.imshow("Akash's Face Detector", frame)

    # assigning key pressed so that we can quit using specific key
    key = cv2.waitKey(1)

    # press q or Q to quit from the video capture window
    if key == 81 or key == 113:
        break

# releasing webcam as it's a good practice
webcam_image.release()
```
### Wow, we made a Real-time Face-Detection with just Few Lines of code, check other tutorials [Click Me](https://agajareiitr.github.io/)