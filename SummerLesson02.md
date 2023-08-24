# Maze Runner Training

Try, fix, retry!  Make your robot run the line, turn around, and return!

## Coding Exercise 1 - Maze Runner Autonomous
1. Re-locate your laptop to the floor, so you can restart your own robot.
2. Adjust your code so that it follows the line as closely as possible.
3. When you can stop the robot at the end of the line - turn around and drive back to the start.
4. The person who can end up closest to the start point wins!

* What problems did you run into?
* Why did the robot not do the same thing each time?

[Comparison operators to If statements](https://www.youtube.com/watch?v=eIrMbAQSU34&t=5970s) <!-- 11 min -->

## Coding Exercise 2 - Turn to Gyro Angle
1. In VSCode, copy the file TurnDegrees.java and paste into the "commands" directory.
2. Rename the file "TurnGyro.java".
3. Open the file, and rename the class and constructor from "TurnDegrees" to "TurnGyro".
4. In RobotContainer add this code to configureButtonBindings():
```
    // Turn the robot when these buttons are pressed
    Trigger TurnLeft = new JoystickButton(m_controller, 3);
    TurnLeft.onTrue(new TurnGyro(0.5, 90, m_drivetrain));

    Trigger TurnRight = new JoystickButton(m_controller, 4);
    TurnRight.onTrue(new TurnGyro(-0.5, 90, m_drivetrain));
```
5. Verify that your robot will turn each direction when the buttons are pressed.

* What happens if a wheel slips on the carpet?

## Coding Exercise 3 - Use Gyro for turning
1. In the TurnGyro initialize() method, change "m_drive.resetEncoders();" to "m_drive.resetGyro();"
2. Remove the method getAverageTurningDistance()
3. Change isFinished() to:
```
  public boolean isFinished() {
    return m_drive.getGyroAngleZ() >= m_degrees;
  }
```
4. Verify that it works

## Extra credit - Debug
How do you send variables to the screen so you can read them?

Hint: [WPILib documentation](https://docs.wpilib.org/en/stable/docs/software/dashboards/shuffleboard/getting-started/shuffleboard-displaying-data.html)
