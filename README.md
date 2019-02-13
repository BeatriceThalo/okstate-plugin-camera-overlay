<!---
# license: Licensed to the Apache Software Foundation (ASF) under one
#         or more contributor license agreements.  See the NOTICE file
#         distributed with this work for additional information
#         regarding copyright ownership.  The ASF licenses this file
#         to you under the Apache License, Version 2.0 (the
#         "License"); you may not use this file except in compliance
#         with the License.  You may obtain a copy of the License at
#
#           http://www.apache.org/licenses/LICENSE-2.0
#
#         Unless required by applicable law or agreed to in writing,
#         software distributed under the License is distributed on an
#         "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
#         KIND, either express or implied.  See the License for the
#         specific language governing permissions and limitations
#         under the License.
-->

# okstate-plugin-camera-overlay

You are likely here if you are using currently using `cordova-plugin-camera` in your project to take pictures, and wish to display an overlay during the camera feed preview. The dilemma is this: your web app html page (within a webview activity) is hidden while the native camera activity is launched, limiting your customization during the camera feed display. (If you don't know what an activity is, think of a PowerPoint slide as an activity, which can have elements in it like textboxes or shapes.)

Some good news is that HTML5 now allows a camera feed to be captured within the html page (webview activity) itself via `window.navigator.mediaDevices.getUserMedia()` (full walkthrough here https://www.html5rocks.com/en/tutorials/getusermedia/intro/) without using a native camera activity at all. Bad news is that while the iOS Safari app has the required `.mediaDevices`, an iOS UIWebView or WKWebView does not have that property, due to their security policies. So the fastest way to add an overlay is to launch a native activity that has the camera feed element and an extra imageview element in it, using either Java or Objective-C. And since you love JavaScript best, this has the native code you need.

At the moment for Android, you can use HTML5 to add an overlay and any other customizations you like (or modify the Java in your own fork). Therefore this plugin's purpose is to add an overlay to the iOS take picture. Because it modifies `cordova-plugin-camera`, it includes `cordova-plugin-camera`, taken at version 1.1.0 or so, so refer to cordova's documentation for full specs, aka the readme in wkevina's fork of this project.

## Installation

    cordova plugin add okstate-plugin-camera-overlay # TODO try

### Description

Because the overlay also covers the camera feed's shutterButton and cancelButton, they are rebuilt above the overlay. There is also a slider above the overlay to control the overlay's transparency from 0 to 1. There is rudimentary 'sizeToFit' scaling to resize the provided overlay image to cover the whole feed element.
TODO specify synchronous img paths, either data: or file: in local iOS directory and how to get it stored there.

### Supported Platforms

- iOS

### Example

The global scoped (different than window.navigator??) `navigator`'s `camera` property is not available until after the `deviceready` event.

    document.addEventListener("deviceready", () => {
        navigator.camera.getPicture(onSuccess, onError, {
            sourceType: Camera.PictureSourceType.CAMERA,
            overlayImageURL: 'data:image/png;base64,<the-image-data>', // the modification
        });
    }, false);