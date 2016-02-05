title: Experience AWS IoT with MediaTek LinkIt One
date: 2015-11-24 17:09:35
categories:
- Hardware
tags:
- AWS
- IoT
- MTK LinkIt One
---
On recent AWS [re-invent](https://reinvent.awsevents.com) conference, the most attracting new product to me is the [AWS IoT](https://aws.amazon.com/iot/) service. This time, it's not only a service launch, but also a serials of hardware partners like Intel, Qualcomm, Microchip,  MediaTek and also Arduino. For people not familiar with AWS IoT, I recommend you watch some videos:
* [Introducing AWS IoT](https://youtu.be/N3-Az0OH5WM)
* [AWS re:Invent 2015 | (MBL205) Everything You Want to Know About IoT](https://youtu.be/OvoIh9ENxdM)
* [AWS re:Invent 2015 | (MBL313) New! AWS IoT: Understanding Hardware Kits, SDKs, & Protocols](https://youtu.be/rMiplPiU2nI)
* [AWS re:Invent 2015 | (MBL311) NEW! AWS IoT: Securely Building, Provisioning, & Using Things](https://youtu.be/G-kJxzd_NA8)

## Why AWS IoT?
### Security
Many IoTs are small sensors sending temperature, light, humidity, etc. It might not be a big deal if your room temperature is leaked for someone else. But there are also internet enabled cameras, microphones which might leak sensitive data. Further more, some IoTs are controllers receiving command remotely. So it would be dangerous when attacker gains control, for example, on our house to turn up temperature, open the lock, or disable alarms.

AWS IoT service enforce secured communication between device and service. You could assign and deploy individual certificate for each device, and also revoke certificate on cloud without updating the firmware on the device.

### Platform Integration
Thanks for rich features provided by AWS service, you might not need your own web service to process data from your device. You could dump the data into [dynamoDB](https://aws.amazon.com/dynamodb/), [S3](https://aws.amazon.com/s3/) or [Kinesis](https://aws.amazon.com/kinesis/). And then you could quickly build a dashboard on top of the data, or you could visualize the data on [QuickSight](https://aws.amazon.com/quicksight/). Meanwhile, you data could trigger event through [SNS](https://aws.amazon.com/sns/), [SQS](https://aws.amazon.com/sqs/), [Lambda](https://aws.amazon.com/lambda/), or to another IoT device.

### Rule engine
AWS IoT service introduced a way to specify action based on the signal. A rule is a SQL like statement:
```
SELECT temperature FROM 'mydevice' WHERE temperature > 75
```
Then an action will be assigned to the rule. Currently there are several action you could choose from:
* Insert message into a database table (DynamoDB)
* Send message as a push notification (SNS)
* Send message to a real-time data stream (Kinesis)
* Republish this message to another topic (AWS IoT)
* Insert this message into a code function and execute it (Lambda)
* Store the message in a file and store in the cloud (S3)
* Send the message to a queue in the cloud (SQS)
* Send the message to a real-time data stream that stores to a bucket (Kinesis Firehose)

### Price and scale
The price is usage based, and there is no at-front charge. It's currently $5 per million messages. The detailed pricing example can be found on the [pricing page](https://aws.amazon.com/iot/pricing/). Another thing is scale, AWS IoT service is completed managed service, you don't need to worry about the servers, DNS, load balancers setting. When the device is registered, you will get an endpoint, and that's all you need.

_Please refer [AWS IoT FAQ](https://aws.amazon.com/iot/faqs/) for more questions._

## Get Started
On the official website [getting started page](https://aws.amazon.com/iot/getting-started/), there are several recommended development kit. Here I am using [MediaTek LinkIt™ ONE](http://labs.mediatek.com/site/global/developer_tools/mediatek_linkit/whatis_linkit/index.gsp). The Linkit One has almost everything onboard: GSM, GPRS, Bluetooth 2.1 and 4.0, SD Cards, and MP3/AAC Audio, as well as WiFi. The Linked One SDK provides Arduino compatible development interface. The development kit is available on [Amazon](http://www.amazon.com/dp/B0168LBYWC) and [Seeedstudio](http://www.seeedstudio.com/depot/LinkIt-ONE-p-2017.html).

### Setup Environment.
I am developing on Mac, so I will be introducing OSX environment experience. For Windows user, the offical guide is available on [Mtk Lab website](http://labs.mediatek.com/site/global/developer_tools/mediatek_linkit/get-started/index.gsp).

* If you don't have Arduino IDE installed, you will need to [download Arduino IDE](https://www.arduino.cc/en/Main/Software) and move it to your application folder.
* Download and install [Linkit ONE development board USB driver](http://download.labs.mediatek.com/mediatek_linkit_os-x-com-port-driver.zip).
* Install Linkit ONE SDK for Arduino.
  * Open Arduino IDE, open `Preferences`.
  * Add `http://download.labs.mediatek.com/package_mtk_linkit_index.json` to `Additional Boards Manager URLs` and click `OK`
  * Click on `Tools` then point to `Board: XXX`, click on `Boards Manager...`.
  * Search for `LinkIt One`, select the latest version and click `Install`.
* Connect the development board through USB cable.
* Select `Port` in `Tools`. For me, it is `/dev/cu.usbmodem14111 (LinkIt ONE)`. If you see multiple similar options, choose the one with smaller number.
* Select `LinkIt ONE` in `Board` dropdown.

### Project Alpha: Blink the LED
Blink LED is just like "Hello World" when you learn any programming language. The Blink program comes with Arduino IDE. You can open it from `File` -> `Examples` -> `01.Bacis` -> `Blink`. The program looks like this:
```c
// the setup function runs once when you press reset or power the board
void setup() {
  // initialize digital pin 13 as an output.
  pinMode(13, OUTPUT);
}

// the loop function runs over and over again forever
void loop() {
  digitalWrite(13, HIGH);   // turn the LED on (HIGH is the voltage level)
  delay(1000);              // wait for a second
  digitalWrite(13, LOW);    // turn the LED off by making the voltage LOW
  delay(1000);              // wait for a second
}
```
On most of Arduino compatible development board, Pin 13 is already connected to LED which is labed "L" and next to a big white dot on LinkIt ONE board. The program will turn on the LED, wait for 1 second, turn of the LED, wait for 1 second, and then loop back.

To make it running on the board, just click the `Upload` button which is the round button next to the round button with a check mark. The IDE will compile the code, and upload to the board, and show `Done uploading`. Now you see a blinking LED on the board, Congrates! You did it!

## Get Connected
### Install AWS MQTT library for LinkedIn ONE
MediaTek ported the [AWS IoT Embedded C SDK](https://github.com/aws/aws-iot-device-sdk-embedded-C) for LinkIt ONE.
* We need to check out the library and example from [MediaTek GitHub](https://github.com/MediaTek-Labs/aws_mbedtls_mqtt): `https://github.com/MediaTek-Labs/aws_mbedtls_mqtt.git`.
* Copy `aws_iot_library` folder to `Documents/Arduino/libraries`, and restart Arduino IDE to let the IDE detect the library.

### Prepare AWS account for IoT
* Create a thing
  * Open [AWS IoT console](https://us-west-2.console.aws.amazon.com/iot/home?region=us-west-2). Note: I am using Oregon data center.
  * Click on `Create a resource`, and then click on `Create a thing`.
  * Type `mtktest_1` in name, then click on `Create`.
* Generate keys and certificate
  * Click on newly created thing `mtktest_1`.
  * Click on `Connect a device` on the right side at bottom.
  * Select `Embedded C`, then click on `Generate certificate and policy`.
  * Click on each of download link, and save the files.
  * Click on `Confirm & start connecting`.
  * You will see the console generate a config example for you:
  ```c
// Get from console
// =================================================
#define AWS_IOT_MQTT_HOST              "AD5MED63WUUJI.iot.us-west-2.amazonaws.com"
#define AWS_IOT_MQTT_PORT              8883
#define AWS_IOT_MQTT_CLIENT_ID         "mtktest_1"
#define AWS_IOT_MY_THING_NAME          "mtktest_1"
#define AWS_IOT_ROOT_CA_FILENAME       "root-CA.crt"
#define AWS_IOT_CERTIFICATE_FILENAME   "4492599be5-certificate.pem.crt"
#define AWS_IOT_PRIVATE_KEY_FILENAME   "4492599be5-private.pem.key"
// =================================================
  ```
### Work on the device
* Download keys and certificate to LinkIt ONE board
  * Unplug the development board.
  * Find a tiny switch on board next to usb connector, and turn it to `MS`.
  * Plug the development board. This time the board is recognized as a mass storage device.
  * Copy and paste the private key and certificate (`4492599be5-certificate.pem.crt` and `4492599be5-private.pem.key`) to the board's root folder.
  * Copy the root certificate also. You can find the root certificate in `aws_mbedtls_mqtt/root_cert` folder, OR [here](https://github.com/MediaTek-Labs/aws_mbedtls_mqtt/tree/master/root_cert). Rename it to `root-CA.crt`.
  * Eject the device, unplug, and turn the switch back to `UART`.
* Programming the application
  * Open `aws_mbedtls_mqtt/examples/aws_paho_shadow/aws_paho_shadow.ino` in Arduino IDE.
  * Switch to `aws_mtk_iot_config.h` file tab.
  * Overwrite MQTT and credential settings. This is the part you copied from console.
  * Update the WIFI setting.
  ```c
/* change Wifi settings here */
#define WIFI_AP "MyRouter" // Your Wifi's SSID
#define WIFI_PASSWORD "XXXXXX" // Your Wifi's password
#define WIFI_AUTH LWIFI_WPA  // Choose from LWIFI_OPEN, LWIFI_WPA, or LWIFI_WEP. Normally it's LWIFI_WPA
  ```
  * Update the server IP address, since the SDK doesn't support DNS yet.
    * Get the IP address of `AWS_IOT_MQTT_HOST`. For example: `ping AD5MED63WUUJI.iot.us-west-2.amazonaws.com`.
* Download and run the application
  * Click on `Upload` button. This time, it will take a little longer to compile and upload.
  * When it shows `Done uploading`, open the `Serial Monitor` in `Tools`.
  * You should see messages like below and keep going.
> . Connecting to AP...ok  
> . Shadow Init... ok  
> . Loading the CA root certificate ...ok  
> . Loading the client cert. and key...ok  
> . Connecting to server 54.191.102.154/8883...ok  
> . Setting up the SSL/TLS structure... ok  
>
> . Performing the SSL/TLS handshake...ok  
> . Verifying peer X.509 certificate... ok  
>
> ================================================================  
> On Device: window state false  
> Update Shadow:   {"state":{"reported":{"temperature":10.500000,"windowOpen":false}}, "clientToken":"-0"}  
> \*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
> Update Accepted !!

### Work on the console
By now, you have successfully launched AWS IoT connected device. The sample program is simulating temperature going up and down. You can see the state in AWS IoT console by clicking on the device `mtktest_1`. Shadow state window should be showing:
```json
{
  "desired": {
    "windowOpen": true
  },
  "reported": {
    "temperature": 12,
    "windowOpen": true
  }
}
```
Theo console also shows the last update time. By clicking on the refresh button, you will see the value is being updated by the device.

You could also update the shadow value on console:
* Click on `Update shadow`
* Let's close the window by:
```json
{
  "desired": {
    "windowOpen": false
  }
}
```
* On the serial monitor of the device, you will notice a line `Delta - Window state changed to 0`, which means the device received the update.

## Conclusion
IoT has soon became a very hot topic in the past year. I can image more and more connected device showing up around us. I believe AWS IoT has many advantages as price, scale, support and etc. to stand out from many other competitor services. I have been experimenting with AWS IoT for few months, it has been reliable and really cheap (I have not been charged by the service since first 250K messages are free).

However, the LinkIt ONE SDK has not been ready for production yet. It still lacks explanation about magical multi-thread functions, and DNS support. From the latest update, Mtk is working a more sophisticated SDK to allow more fundamental control with a better IDE environment. The current Arduino support is basically a shell on top of the kernel. I am looking forward to see an update on the SDK, so I could actually build something useful.

## Reference
* [Get Started with AWS IoT Services on the LinkIt™ ONE Development Platform](http://labs.mediatek.com/site/global/developer_tools/mediatek_linkit/get-started/aws/get-started/index.gsp)
* [AWS IoT Embedded-C SDK](https://github.com/aws/aws-iot-device-sdk-embedded-C/blob/master/README.md)
* [Quick guide for LinkitOne with AWS IoT](https://github.com/MediaTek-Labs/aws_mbedtls_mqtt/blob/master/Quick%20guide%20for%20LinkitOne%20with%20AWS%20IoT.pdf)
