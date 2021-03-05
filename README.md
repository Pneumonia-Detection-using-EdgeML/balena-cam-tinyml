# Pneumonia Detection using EdgeML and balena

This guide is a part of the series of deploying of the project [Pneumonia Detection using EdgeML](https://www.hackster.io/arijit_das_student/pneumonia-classification-detection-using-edgeml-991e18) on your Raspberry Pi using [balena](https://balena.io).

## Overview

This project enables a Raspberry Pi with a camera to become a Pneumonia Classification expert system using Edge Machine Learning system. This system is 
AI-based pneumonia classifier using Edge Impulse model.

This project is also based on the great [BalenaCam project](https://github.com/balenalabs/balena-cam) to live stream your camera's feed by running a webapp in a container. For this application we leverage the multi-containers feature of Balena by adding a second container running Edge Impulse webAssembly inference engine inside a Node.js server. The 2 containers communicate with each other through websockets. The `balena-cam` webapp has been modified to call the inference engine every second and display the results of the Pneumonia Classification expert on the webpage.

## Getting started

### Hardware

* Raspberry Pi: [v3](https://www.raspberrypi.org/products/raspberry-pi-3-model-b-plus/) and [v4](https://www.raspberrypi.org/products/raspberry-pi-4-model-b/) tested, [balenaFin v1.0](https://balena.io/fin) tested
* [Raspberry Pi camera](https://www.raspberrypi.org/products/camera-module-v2/) or USB camera.

### Software

* Sign up for a free [Edge Impulse account](https://edgeimpulse.com/)
* Sign up for a free [BalenaCloud account](https://www.balena.io/)
* [balenaEtcher](https://www.balena.io/etcher/)

## Deploying the code

Click on the *deploy-with-balena* button as given below, which will help you to deploy your application to balenaCloud and then to your Raspbery Pi in **one-click!**

[![deploy button](https://balena.io/deploy.svg)](https://dashboard.balena-cloud.com/deploy?repoUrl=https://github.com/Pneumonia-Detection-using-EdgeML/pnuemonia-detection-balenaCAM)

### Sign up to balenaCloud

If you don't have a balenaCloud account, first thing you'll need to do is [sign up for a balenaCloud account](https://dashboard.balena-cloud.com/signup). If you've already got a Github or Google account you can use that to log in and bypass the signup process on the balenaCloud.

### Add a device and download the OS

Once your application has been created and the code is being releases on the app on balenaCloud, click the blue button ```Add Device```. When you add a device you specify your device type (Raspberry Pi 3, 4 or balenaFin). If you are connecting to a wireless network you will need to set your WiFi SSID and credentials here too.

This process will create a customized image configured for your application and device type and includes your network settings.

### Flash your SD card and boot the device

Once the OS image has been downloaded, it's time to flash your SD card. You can use [balenaEtcher](https://www.balena.io/etcher/) for this.

Once the flashing process has completed, insert your SD card into your device and connect the power supply.

When the device boots for the first time, it connects to the balenaCloud dashboard, after wchich you'll be able to see it listed as online. If the release went well, the new services will be downloaded in your device and it will start the containers running the services.


## Testing the classifier

Once the containers are deployed on your device using balena. Enable the ```Public device URL``` on your device. Open your browser using the public URL provided by balenaCloud or enter your device's local IP.

![Captura de Pantalla 2021-03-04 a les 16 21 41](https://user-images.githubusercontent.com/64097541/110072921-8e461c00-7da4-11eb-8a24-63ca21f12b01.png)


The camera feed should be displayed on the webpage. If you notice slow framerate, probably your web browser doesn't support WebRTC and your client has switched to MJPEG. You can check next section to debug WebRTC.

Try to move different objects in front of the camera and see how well the classifier works! Predictions are displayed for all labels with values between 0 and 1, 1 being perfect prediction.


## Troubleshooting

### Additional Information

- This project uses [WebRTC](https://webrtc.org/) (a Real-Time Communication protocol).
- A direct WebRTC connection fails in some cases.
- This current version uses mjpeg streaming when the webRTC connection fails.
- Chrome browsers will hide the local IP address from WebRTC, making the page appear but no camera view. To resolve this try the following
  - Navigate to chrome://flags/#enable-webrtc-hide-local-ips-with-mdns and set it to Disabled
  - You will need to relaunch Chrome after altering the setting
- Firefox may also hide local IP address from WebRTC, confirm following in 'config:about'
  - media.peerconnection.enabled: true
  - media.peerconnection.ice.obfuscate_host_addresses: false

If you wish to test the app in Balena local mode, you'll need to add your Edge Impulse Project ID and API Key in `edgeimpulse-inference/app/downloadWasm.sh`.

BalenaCam advanced options are available in [this guide](BALENA-OPTIONS.md).

### BalenaCam supported browsers

- **Chrome** (but see note above)
- **Firefox** (but see note above)
- **Safari**
- **Edge** (only mjpeg stream)
