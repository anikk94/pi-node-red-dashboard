## Dashboard Flow Descriptions
### Camera Test Flow
1. template node: this will be the window to display the video object (mjpg img tag)
```
<div>
<img src="http://<CAMERA STREAM IP:PORT>/stream.mjpg" width="620" height="465" alt="video" />
</div>
```
2. exec node: this is how the script that starts the video is run from node red
```
python3 <LOCATION OF EXECUTABLE>/web_stream.py
```
3. exec node: to stop the video from node red 2 steps are needed - get the process id of the python program that is streaming camera data - killing the process with another exec node. this first exec will get the pid.
```
ps aux | grep web_stream | grep -v grep
```
4. function node: this will extract the pid from what is returned above
```
if (msg.payload == ""){
    msg.payload = "echo camera_not_running";
    return msg;
}
msg.payload = msg.payload.substring(9, 14).trim();
msg.payload = "kill " + msg.payload + "; echo camera_shutdown_complete";
return msg;
```
5. exec node: this doesn't do much. the incoming msg.payload is the command to execute in the right format.
```
in the node settings, just append the msg.payload to the blank exec command
```
6. add lots of inject and debug nodes to test all this out.
7. the dashboard will trigger the stream with a button and will trigger shutoff with another button.
8. stream is not running perpectually because I don't know if leaving the stream on is wrecking the write endurance of the sd card or just keeping the buffer/mjpg thing in ram. so just to be safe the stream will not stay on for more than 100 seconds, i.e. it will turn off in 100 seconds regardless of someone leaving it on.



![flow image](https://github.com/anikk94/raspberry-pi-node-red-dashboard/blob/main/README/camera_test_flow.JPG)
![dashboard image](https://github.com/anikk94/raspberry-pi-node-red-dashboard/blob/main/README/dashboard.JPG)
