# Pneumonia Detection using EdgeML and balenaCloud

This guide is a part of the series of deploying of the project [Pneumonia Detection using EdgeML](https://www.hackster.io/arijit_das_student/pneumonia-classification-detection-using-edgeml-991e18) on your Raspberry Pi using [balenaCloud](https://balena.io).

## Overview

This project is based on the great [BalenaCam project](https://github.com/balenalabs/balena-cam) to live stream your camera's feed by running a webapp in a container. For our application we leverage the multi-containers feature of Balena by adding a second container running Edge Impulse webassembly inference engine inside a Node.js server. The 2 containers communicate with each other through a websocket. The `balena-cam` webapp has been modified to call the inference engine every second and display the results on the webpage.

## Requirements

* Raspberry Pi: [v3](https://www.raspberrypi.org/products/raspberry-pi-3-model-b-plus/) and [v4](https://www.raspberrypi.org/products/raspberry-pi-4-model-b/) tested, [balenaFin v1.0](https://balena.io/fin) tested
* [Raspberry Pi camera](https://www.raspberrypi.org/products/camera-module-v2/)
* Sign up for a free [Edge Impulse account](https://edgeimpulse.com/)
* Sign up for a free [BalenaCloud account](https://www.balena.io/)

## Deploying on a Raspberry Pi

Click on the *deploy-with-balena* button as given below, which will help you to deploy your application to balenaCloud and then to your Raspbery Pi in **one-click!**


[![deploy button](https://balena.io/deploy.svg)](https://dashboard.balena-cloud.com/deploy?repoUrl=https://github.com/Pneumonia-Detection-using-EdgeML/balena-cam-tinyml)
## Testing our classifier

Open your web browser and enter your device's local IP. You can also enable public URL if you wish to access the device remotely!

The camera feed should be displayed on the webpage. If you notice slow framerate, probably your web browser doesn't support WebRTC and your client has switched to MJPEG. You can check next section to debug WebRTC.

Try to move different objects in front of the camera and see how well the classifier works! Predictions are displayed for all labels with values between 0 and 1, 1 being perfect prediction.

![Captura de Pantalla 2021-03-04 a les 16 21 41](https://user-images.githubusercontent.com/64097541/110072921-8e461c00-7da4-11eb-8a24-63ca21f12b01.png)


## Additional Information

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

## BalenaCam supported browsers

- **Chrome** (but see note above)
- **Firefox** (but see note above)
- **Safari**
- **Edge** (only mjpeg stream)
