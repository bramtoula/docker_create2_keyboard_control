# docker_create2_keyboard_control
Docker image to run on a computer connected to an iRobot Create 2 robot to control it with the keyboard.

Tested on an Nvidia Jetson TX1.

Build the image:
```
cd docker
docker build -t create2-keyboard .
```

Run the image, giving access to the USB devices (Not very secure with the `--privileged` option):
```
docker run -it --privileged create2-keyboard
```

You should then be able to control the movements of the robot with your keyboard.
