---
title: Troubleshoot Azure IoT Edge | Microsoft Docs 
description: Resolve common issues and learn troubleshooting skills for Azure IoT Edge 
services: iot-edge
keywords: 
author: kgremban
manager: timlt

ms.author: kgremban
ms.date: 12/15/2017
ms.topic: tutorial
ms.service: iot-edge

ms.custom: mvc
---

# Common issues and resolutions for Azure IoT Edge

If you experience issues running Azure IoT Edge in your environment, use this article as a guide for troubleshooting and resolution. 

## Standard diagnostic steps 

When you encounter an issue, learn more about the state of your IoT Edge device by reviewing the container logs and messages that pass to and from the device. Use the commands and tools in this section to gather information. 

* Look at the logs of the docker containers to detect issues. Start with your deployed containers, then look at the containers that make up the IoT Edge runtime: Edge Agent and Edge Hub. The Edge Agent logs typically provide info on the lifecycle of each container. The Edge Hub logs provide info on messaging and routing. 

   ```cmd
   docker logs <container name>
   ```

* View the messages going through the Edge Hub, and gather insights on device properties updates with verbose logs from the runtime containers.

   ```cmd
   iotedgectl setup --runtime-log-level DEBUG
   ```

* If you experience connectivity issues, inspect your edge device environment variables like your device connection string:

   ```cmd
   docker exec edgeAgent printenv
   ```

You can also check the messages being sent between IoT Hub and the IoT Edge devices. View these messages by using the [Azure IoT Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) extension for Visual Studio Code. For more guidance, see [Handy tool when you develop with Azure IoT](https://blogs.msdn.microsoft.com/iotdev/2017/09/01/handy-tool-when-you-develop-with-azure-iot/).

After investigating the logs and messages for information, you can also try restarting the Azure IoT Edge runtime:

   ```cmd
   iotedgectl restart
   ```

## Edge Agent stops after about a minute

The Edge Agent starts and runs successfully for about a minute, then stops. The logs indicate that the Edge Agent is attempting to connect to IoT Hub over AMQP, and then approximately 30 seconds later attempt to connect using AMQP over websocket. When that fails, the Edge Agent exits. 

Example Edge Agent logs:

```
2017-11-28 18:46:19 [INF] - Starting module management agent. 
2017-11-28 18:46:19 [INF] - Version - 1.0.7516610 (03c94f85d0833a861a43c669842f0817924911d5) 
2017-11-28 18:46:19 [INF] - Edge agent attempting to connect to IoT Hub via AMQP... 
2017-11-28 18:46:49 [INF] - Edge agent attempting to connect to IoT Hub via AMQP over WebSocket... 
```

### Root cause
A networking configuration on the host network is preventing the Edge Agent from reaching the network. The agent attempts to connect over AMQP (port 5671) first. If this fails, it tries websockets (port 443).

The IoT Edge runtime sets up a network for each of the modules to communicate on. On Linux, this network is a bridge network. On Windows, it uses NAT. This issue is more common on Windows devices using Windows containers that use the NAT network. 

### Resolution
Ensure that there is a route to the internet for the IP addresses assigned to this bridge/NAT network. Sometimes a VPN configuration on the host overrides the IoT Edge network. 

## Edge Hub fails to start

The Edge Hub fails to start, and prints the following message to the logs: 

```
One or more errors occurred. 
(Docker API responded with status code=InternalServerError, response=
{\"message\":\"driver failed programming external connectivity on endpoint edgeHub (6a82e5e994bab5187939049684fb64efe07606d2bb8a4cc5655b2a9bad5f8c80): 
Error starting userland proxy: Bind for 0.0.0.0:443 failed: port is already allocated\"}\n) 
```

### Root cause
Some other process on the host machine has bound port 443. The Edge Hub maps ports 5671 and 443 for use in gateway scenarios. This port mapping fails if another process has already bound this port. 

### Resolution
Find and stop the process that is using port 443. This process is usually a web server.

## Edge Agent can't access a module's image (403)
A container fails to run, and the Edge Agent logs show a 403 error. 

### Root cause
The Edge Agent doesn't have permissions to access a module's image. 

### Resolution
Try running the `iotedgectl login` command again.

## Next steps
Do you think that you found a bug in the IoT Edge platform? Please, [submit an issue](https://github.com/Azure/iot-edge/issues) so that we can continue to improve. 
