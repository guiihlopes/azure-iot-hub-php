# A PHP library to send data to Azure IoT Hub

Currently the library is [hosted on my GitHub account](https://github.com/tomconte/azure-iot-hub-php), so in order to use it, your `composer.json` file should contain something like this:

~~~ json
{
    "repositories": [
        {
            "type": "vcs",
            "url": "https://github.com/guiihlopes/azure-iot-hub-php"
        }
    ],
    "require": {
        "guiihlopes/azure-iot-hub": "dev-master"
    }
}
~~~

This will load the Composer package directly from GitHub. Then you can use it that way:

~~~ php
<?php

require __DIR__ . '/vendor/autoload.php';

$host = 'hubbhub.azure-devices.net';
$deviceId = 'php_device';
$deviceKey = 'xxxxy5TAxxxx73VBxxxxq4WNxxxxaTIPxxxxEvkCxxxx';

$client = new AzureIoTHub\DeviceClient($host, $deviceId, $deviceKey);

$response = $client->sendEvent('Hello World!');

print($response->getStatusCode());
~~~

In order to find the values for `host`, `deviceId` and `deviceKey`, you can follow our [Get started with Azure IoT Hub](Get started with Azure IoT Hub) article. Basically, you will use the portal to create an IoT Hub, and then you will use our `iothub-explorer` command-line tool to generate a new device and its associated secret keys.

You can also use a connection string to create the client:

~~~ php
$connectionString = 'HostName=hubbhub.azure-devices.net;DeviceId=php_device;SharedAccessKey=xxxx';
$client = new DeviceClient($connectionString);
~~~

The `sendEvent()` method will open an HTTPS connection and send a single message. There is currently no provision to reuse the connection, since in PHP, objects are typically short-lived.

Please note that as with all IoT Hub libraries, your secret key is never sent on the wire. The library will compute a Shared Access Signature (SAS) token and use it to authenticate the communication; you can look at the `computeSAS()` function in the source code to see how this is done.

Please use the GitHub repo if you use this little library and have any feedback, questions or issues!
