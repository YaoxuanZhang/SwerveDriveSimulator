# Swerve Drive Simulator

A Unity physics-based game to simulate and learn swerve drive mechanics.
Developed without external libraries other than built-in Unity math functions.
The intended purpose of this game is to learn the vector mathematics behind the calculation and optimization of swerve drive mechanics and characteristics.
There are limitations to the realism of this simulation, as the unrealistic friction creates excessive drifting.
If the drifting is resolved, overshoot observed during pathing will be greatly reduced.

[**Click here to play**](https://yaoxuanzhang.github.io/SwerveDriveSimulator/)

## Summary of Topics and Learning Time

| Topic                                       | Time Spent   |
| ------------------------------------------- | ------------ |
| Learning Unity and Drive Physics            | 3 hours      |
| Basic Swerve Vectoring                      | 4 hours      |
| Swerve Normalization and Optimization       | 3 hours      |
| PID and Absolute Control                    | 6 hours      |
| Finite State Machine                        | 2 hours      |
| Basic On The Fly Pathfinding                | 4 hours      |
| Waypoint Setting and Macro Recording        | 2 hours      |
| Predictive Pathing                          | 2 hours      |
| UI, Input Managing, Graphics and Visuals    | 6 hours      |
| Building To WebGL and Writing Documentation | 3 hours      |
| **Total Time Spent (Across 4 days)**        | **35 hours** |

_Started: `5/1/2023` Completed: `5/13/2023`_

## Keybinds

| Movement Controls | Keyboard Keybind | Controller Keybind |
| ----------------- | ---------------- | ------------------ |
| Move up           | Keyboard W       | Left stick up      |
| Move down         | Keyboard S       | Left stick down    |
| Move left         | Keyboard A       | Left stick left    |
| Move right        | Keyboard D       | Left stick right   |
| Rotate left       | Keyboard Q       | Right stick left   |
| Rotate right      | Keyboard E       | Right stick right  |

| Absolute Position Controls | Keyboard Keybind | Controller Keybind |
| -------------------------- | ---------------- | ------------------ |
| Disable absolute position  | Spacebar         | Left stick press   |
| Toggle absolute position   | Keyboard Z       | X button           |
| Restrict absolute position | Keyboard X       | Y button           |
| Toggle path follow         | Keyboard C       | A button           |
| Place waypoint             | Keyboard V       | B button           |
| Predictive pathing         | Tab              | Left bumper        |
| Brake on waypointing       | Control          | Right bumper       |

| Pathing Macro Controls | Keyboard Keybind | Controller Keybind |
| ---------------------- | ---------------- | ------------------ |
| Preset 1               | Keyboard 1       | D-pad left         |
| Preset 2               | Keyboard 2       | D-pad right        |
| Preset 3               | Keyboard 3       | D-pad down         |
| Preset 4               | Keyboard 4       | D-pad up           |

## Swerve Controller Overview

The `SwerveController` script implements a swerve drive system for controlling the movement and rotation of a robot in a Unity environment. Here's a high-level overview of its main functions:

### Input Handling

-   The script handles input events for movement, rotation, and toggling different modes of operation.

### Path Following

-   The script supports path following functionality, where the robot can follow a sequence of waypoints.

-   It manages a queue of waypoints (`_pathingQueue`) and provides methods to enqueue new waypoints (`QueuePathingWaypoint`) and check if the queue is empty (`PathingQueueIsEmpty`).
-   The script also handles toggling the path following mode (`OnPathFollowToggle`), placing additional waypoints (`OnPathFollowPlaceWaypoint`), and recording and using pathing macros (`OnPathingMacro`).

### Absolute Orientation Control

-   The script provides the ability to enable or disable absolute orientation control, which ensures the robot maintains a specific orientation regardless of its movement (`OnAbsoluteOrientationToggle`).

### State Machine

-   The script maintains a state variable (`_state`) to represent the current mode or behavior of the swerve controller.

-   The state can be disabled (`DISABLED`), enabled (`ENABLED`), restricted (`RESTRICTED`), or in path following mode (`WAYPOINTING`, `PATHING`, `PATHING_RECORDING`).
-   The script includes methods to toggle and manage the state machine (`ToggleStateMachine`, `DisableStateMachine`, `CheckState`, etc.).

### PID Control

-   The script uses PID (Proportional-Integral-Derivative) controllers to calculate corrections for position and orientation control.

-   It initializes and updates PID controllers for position (`_xPID`, `_yPID`) and orientation (`_rPID`) based on the target position and orientation.

### Update and Fixed Update

-   The `Update` method is responsible for updating wheel visuals, position and orientation targets, and managing the different modes of operation based on the current state.
-   The `FixedUpdate` method calculates the desired movement and rotation based on the current state and applies it to the robot's drive system.

_The script includes several private methods that handle specific functionalities or checks, such as handling pathing movement vector, toggling pathing mode, resetting position PID, and calculating position and orientation corrections using PID controllers._

## Swerve Controller Parent Overview

The `SwerveControllerParent` script is responsible for controlling the movement and rotation of a robot in a Unity environment using a swerve drive system. Here are the high-level functions of the script:

### Initialize Parent

-   Initializes the parent script by setting the tangent angles for each wheel.

### Drive

-   Calculates the target steer angle and magnitude of the target wheel based on input parameters.
-   Applies rotation compensation to adjust the translational vector for field-centric movement.
-   Calculates wheel vectors based on the translational vector, rotation, and tangent angles.
-   Normalizes the magnitudes of wheel vectors and determines the maximum magnitude.
-   Converts the wheel vectors to a vector of magnitude and direction, adjusted by the motor torque value.
-   Applies steering to each wheel using the calculated steer vector.

### Brake

-   Applies braking to all the wheels by setting the brake torque and zeroing the motor torque.

_Please note that the code includes private methods that handle specific functionalities, such as setting tangent angles and applying steering._

## Swerve Controller Updater Overview

The `SwerveControllerUpdater` script is responsible for updating various targets and visual elements related to the swerve controller in a Unity environment. Here are the high-level functions of the script:

### Initialize Updater

-   Initializes the updater script by assigning target transforms and line renderers, setting their visibility, and getting the reference to the Rigidbody component.

### Update Wheel Visuals

-   Updates the rotation of visual wheels based on the corresponding wheel colliders.

### Update Position Target

-   Updates the position target based on player input and the current state.
-   Adjusts the target velocity based on the distance from the drive train.
-   If predictive pathing is enabled, updates the prediction target and sets the color of prediction tracers.
-   Moves the position target according to the adjusted velocity.

### Update Position Target Pathing

-   Updates the position target specifically for pathing by restricting the movement.

### Update Prediction Target

-   Updates the prediction target position based on the current robot position, velocity, position target, and target velocity.

### Update Position Target Tracer

-   Sets the color of position tracers based on the current positions of the robot and position target.

### Update Position Target Tracer Waypoint

-   Sets the color and positions of position tracers when following waypoints.

### Update Position Target Tracer Pathing

-   Sets the color and positions of position tracers when pathing.

### Update Orientation Target

-   Updates the orientation target rotation based on the given rotation value.

### Update Position Target Color

-   Sets the color of the position target and its associated visual elements.

### Update Orientation Target Color

-   Sets the color of the orientation target based on whether brake on waypointing is enabled.

_Please note that the script includes several private helper methods that handle specific functionalities, such as setting line renderer colors, drawing lines, and creating gradients. These private methods support the main functions described above but are not directly accessible._

## Swerve Controller Constants Overview

The `SwerveControllerConstants` class defines various constants and parameters used in the swerve controller system. Here is an overview of the high-level functions of the class along with the explanation of each variable and its intended function:

### Wheel Object References

-   `_wheelCount`: The number of wheels in the swerve controller system.
-   `_wheels`: An array of `Transform` components representing the wheel objects.
-   `_wheelColliders`: An array of `WheelCollider` components representing the wheel colliders.

### Motor Control Values

-   `_motorTorque`: The maximum speed of each driving motor. Range: 0-40.
-   `_motorTorqueInterpolator`: The lerp factor used for motor torque interpolation. Range: 0-1.
-   `_brakeTorque`: The amount of brake torque applied. Range: 0-40.
-   `_coaxialInterpolator`: The interpolator used to simulate turning of the coaxial motor. Range: 0-1.

### Rotation Gain Values

-   `_rotateBias`: The priority given to rotating over moving. Range: 0-5.
-   `_rotationCompensation`: Compensation for translational motion when rotating. Range: 0-90.
-   `_rotationCompensationFalloff`: Adjusts the shape of the falloff curve for rotation compensation. Range: 0.1-1.

### Absolute Controller References

-   `_positionTargetGameObject`: The game object used as the position target.
-   `_predictionTargetGameObject`: The game object used as the prediction target.
-   `_orientationTargetGameObject`: The game object used as the orientation target.

### Pathing Characteristics Values

-   `_predictivePathingMinimumSwerveSpeed`: The minimum swerve speed used for predictive pathing. Range: 0-10.
-   `_pathingTargetDeadZone`: The dead zone for the pathing target. Range: 0-1.
-   `_pathingDeadZone`: The dead zone for pathing. Range: 0-5.

### Absolute Controller Visuals

-   `_positionTargetAlpha`: The alpha value for the position target. Range: 0-1.
-   `_positionTargetVerticalOffset`: The vertical offset for the position target. Range: 0-1.

### Absolute Controller Values

-   `_absolutePositionSpeedMultiplier`: The speed multiplier for absolute position control. Range: 0-75.
-   `_absolutePositionDistanceStart`: The distance at which the multiplier starts affecting the absolute position control. Range: 1-50.
-   `_absolutePositionDistanceFalloff`: The falloff factor for the distance multiplier in absolute position control. Range: 0.1-2.5.
-   `_absoluteOrientationSpeedMultiplier`: The speed multiplier for absolute orientation control. Range: 0-800.

### PID Controller Values

-   `_mp`, `_mi`, `_md`: The proportional, integral, and derivative factors respectively for the PID controller. Range: 0-2.
-   `_ma` : The dead zone factor for the PID controller. Range: 0-2.
-   `_rp`, `_ri`, `_rd`: The proportional, integral, and derivative factors respectively for the rotation PID controller. Range: 0-0.1.
-   `_ra`: The dead zone factor for the rotation PID controller. Range: 0-2.

_These variables define the parameters and characteristics of the swerve controller system and are used to fine-tune its behavior and performance._

## Swervef Overview

The `Swervef` class contains various mathematical methods used in the swerve controller system. Here is an overview of the high-level functions of the class:

### Purely Math Methods

-   `Atan2`: Calculates the angle in degrees between the positive x-axis and a given vector.
-   `Vector2Components`: Converts a vector's magnitude and angle into its x and y components.
-   `Components2Vector`: Converts x and y components into a vector with magnitude and angle.
-   `RotateVector`: Rotates a vector by a given angle.
-   `LerpSignedDeltaAngle`: Interpolates between two angles, returning the resulting angle and a sign value indicating the direction of rotation.
-   `Vector2State`: Determines the state of a 2D input vector (e.g., left, right, up, down).
-   `PredictTarget`: Predicts the target position based on current position, velocity, target position, target velocity, and a dead zone value.

### Private Helper Methods

-   `WrapAngle`: Wraps an angle value between 0 and 360 degrees.
-   `LerpDelta`: Interpolates between a current value and a delta value based on an interpolator.

_These methods provide mathematical operations and transformations necessary for the swerve controller system to function properly._
