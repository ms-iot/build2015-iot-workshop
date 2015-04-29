# Welcome: Say hello to your little friend!
------------------------------------
This project will soon be available in full on GitHub and we wanted to give you a sneakpick at what you will be able to create on your own: A robot on wheels powered by Windows running on a Raspberry Pi 2.

In this workshop you will get the opportunity to navigate the projects source code, deploy the app to the Rsaspberry Pi device, and experience remote debugging.
After that you will get a chance to play with the code to surprise us!

# About the robot
------------------------------------

## Directional movement
The robot can move in 8 directions:

1. Forward
2. Reverse
3. Hard left turn
4. Hard right turn
5. Soft forward left turn
6. Soft forward right turn
7. Soft backward left turn
8. Soft backward right turn

A **hard** turn rotates the wheels in opposite directions. A **soft** turn rotates only one wheel in the specified direction.

## Joystick Input
The robot can be controlled via the left analog stick or digital direction pad.
They joystick can be plugged into the robot directly via the USB port on the Raspberry Pi 2 or the Windows 10 Desktop PC via USB.

## Keyboard Input
From the Windows 10 Desktop PC UWP app, the keyboard controls are:

* Forward - Up arrow / W
* Backward - Down arrow / X
* Left - Left arrow / A
* Right - Right arrow / D
* Back Left - Z
* Back Right - C
* Forward Left - Q
* Forward Right - E
* Stop - Enter / Space

## Mouse/Touch
When run on the Windows 10 Desktop PC, there is an input screen with the 8 directional movements of the robot.
Click or touch any of these to move the robot.

# About the software
------------------------------------
The robot kit software is a UWP project with 6 major files:

1. **MainPage.xaml.cs** - The main application code and entry point
2. **XboxHidController.cs** - The initialization and handling logic for the Xbox controller
3. **MotorControl.cs** - The main logic for controlling the continuous rotation servos
4. **Controller.cs** - The logic for converting joystick, key press, mouse, and touch events to robot movements
5. **NetworkCommands.cs** - the logic to setup client and server networking threads and create/process network messages
6. **package.appxmanifest** - the manifest file that defines properties of this UWP application

## MainPage.xaml.cs
The MainPage class is where the supporting robot classes get started.  The RobotApp reads the previously saved mode, and initializes itself to run as a Robot or Controller.  The MainPage class is the only UI page for the RobotApp.  The onscreen buttons, and key input properties, are setup in MainPage.xaml.

## XboxHidController.cs
The XboxHidController class contains the interface logic for getting input from the Xbox game controller.  Once the initialization method in Controllers.cs finds an attached gaming HID device, it sets up the delegates in XboxHidController.cs to process the input events.  Ultimately, these methods return a direction and a magnitude value, which are used to determine how to drive the servo motors. 

There are two types of directional inputs from the game controller, DPad and joystick.  The DPad type simply returns one of eight directions, making it easy to work with.  The joystick type requires translating an X and Y value into appropriate directions, as well as filtering out minute movements while the stick is closer to its center position.

## MotorControl.cs
This MotorCtrl class handles all of the I/O to control the continuous rotation servo motors, and block sensor.  The GPIO library is used to control selected I/O pins for this project.  The main timing loop generates appropriate pulse signals to drive the motors, for any of the eight selected directions.  This loop, is also where other critical system checks are done (i.e. block sensor, device communication breaks, etc.).  

The block sensor, is also defined here, and was setup to demonstrate a basic motion-safety feature.  With it connected, the robot will stop, and turn-around, if an obstacle triggers the switch.

## Controllers.cs
This Controllers class coordinates the several types of input which can be used to drive the robot.  Directional inputs are handled from key presses, mouse or touch input, and Xbox game controller input, which are either connected directly, or sent from a remote RobotApp.  The Controllers class provides methods to translate each of these into the basic directional values used by the MotorControl class.

## NetworkCommands.cs
This NetworkCmd class sets up either a client or server stream socket object.  If the RobotApp is a basic robot, a client object is used.  A basic robot reads in commands from a networked App, to optionally use along with any locally attached input devices.   If the App is setup as a remote controller, a server object which listens for connections is used.  When the App is in a server mode, it writes directional commands out to connected client robots.  Both modes of the RobotApp utilize the same Controllers classes.

## package.appxmanifest 
The Capabilities defined in the manifest file, enable networking, and human interface device privileges.  Be sure to include these within your modified projects.

    <Capabilities>
      <Capability Name="privateNetworkClientServer" />
      <DeviceCapability Name="humaninterfacedevice">
        <Device Id="any">
          <Function Type="usage:0001 0005"/>
        </Device>
      </DeviceCapability>
    </Capabilities>

# Get Started!
------------------------------------
In Visual Studio 2015 Preview, open the solution C:\Users\BUILD_Attendee\Desktop\RobotKit\RobotApp\RobotApp.sln.

## Connecting to the Pi2 from Visual Studio
Windows 10 IoT Core is running on the Raspberry Pi and you will be deploying the application to the device from Visual Studio.
In order to ensure you are talking to the right robot kit in the room, right click on the project in the project explorer and click on properties.
in the project properties page, choose the Debug tab and ensure that the Target Device is set to Remote Machine and that the Remote Machine fields has the IP address written on the post it on the table at your station.
It is as simple as that. A remote debugger client is running on the Windows image on the Pi that allows the deployement of the app and the remote debugging.

## Deploy and start the app
Ensure that in the Visual Studio Ribbon you have ARM as the configuration (NOT x64 or x86 as the Pi is an ARM device) and that the target device is set to Remote Machine.
Press F5 to deploy and start the app.

## Debug the app
From there you can put brakpoints in the code (for example in the MotorControl.cs file, PulsMotor function on the following line:

'''leftPwmPin.Write(GpioPinValue.High);

Once the breakpoint is set in the code, use the joystick to move the robot. You should hit the breakpoint...

From here on, its all yours!!

## Inspect the Windows Image
There are several ways to inspect the windows image. We will explain all of them soon :-)
One of them consist in a webserver running on the device.
Start a web browser and type the IP address of the Pi2 (the one written on the postit on your station's desk). And naviaget the pages!

More details to come soon!


Thanks from the Microsoft IoT Team!

