# autonomous-rover
Space exploration rover prototype capable of autonomous object detection and sample collection.

https://devpost.com/software/rovert

- LeRobot SO-101 arm supports both teleoperated and fully autonomous modes.
- Trained via ACT imitation learning algorithm on 50+ dual-camera demonstration episodes and deployed locally on AMD compute hardware with no cloud dependency.
- ROS2 camera pipeline provides live video feedback on custom local network.
- Custom 3D-printed chassis houses onboard electronics, including ultrasonic sensor array for terrain mapping and automatic braking for collision avoidance.
- Raspberry Pi handles motor movement, power distribution, and display of telemetry data.
<br><br>
<img width="1280" height="907" alt="image" src="https://github.com/user-attachments/assets/15cb1a6a-6693-42bf-b31b-c83837554fa5" />
<br><br>
Collaborators: Ethan Chen, Kieu Vi O'Brien, Ayyazul Hassan

# Components
- [LeRobot SO-101 arm](le-robot-so101-arm)
- [Dual Camera Perception System](dual-camera-perception-system)
- [ROS2 Camera Pipeline](ros2-camera-pipeline)
- [ACT Model via Hugging Face Framework](act-model-via-hugging-face-framework)

# LeRobot SO-101 arm
https://huggingface.co/docs/lerobot/so101
<br><br>
Initial system environment: Ubuntu X86 22.04, CUDA 12, Python 3.10, Torch 2.6
The follower arm uses 6x STS3215 motors with 1/345 gearing. 
The leader arm uses three differently geared motors to ensure it can sustain its own weight and move without requiring much force. 
Leader arm calibrated and operated using 5V power supply, follower arm using 12V power supply.

**Setup**

Each motor is identified by a unique id on the bus. For communication to work properly between the motors and the controller, each motor needs to be set with a unique ID. 
Additionally, the speed at which data is transmitted on the bus is determined by the baudrate, so the controller and all the motors need to be configured with the same baudrate.

**Calibration**

Calibration ensures that the leader and follower arms have the same position values when they are in the same physical position. This allows a neural network trained on one robot to work on another.

After running a calibration command, move the arm that's being calibrated across its full range of motion. Do this for both arms.

**Teleoperation**

If calibrated correctly, the teleoperation command should allow the follower arm to be teleoperated by the leader arm. 
It will also identify any missing calibrations and, if identified, initiate the calibration procedure.

# Dual Camera Perception System
We used two cameras for training: a stationary one on the front of the rover, and a camera mounted onto the arm.
The idea was to allow the ACT model to use two reference frames to train the robotic arm, one being the perspective of the arm itself, and one being a ground observer.
This way, we could prevent the arm from moving erratically.
<insert image here>

# ROS2 Camera Pipeline
Since we were using a separate mini-PC for computation and didn't have a monitor, we weren't able to directly display the camera feed during teleoperation, which was a problem.
So we opted to use a ROS2 pipeline over a custom local network.
<insert image here>

# ACT Model via Hugging Face Framework
We recorded 50+ episodes and uploaded to the Hugging Face Repository. The ACT algorithm used 300K+ steps to train the model.

