# Big Robot!

This is it! Use your skills to program a full size robot!

## Safety talk

## Coding Step 1 - Convert an Example
1. Create a new project based on "GyroDriveCommands".
2. Open a browser window and open [docs.wpilib.org](https://docs.wpilib.org/en/stable/docs/software/vscode-overview/3rd-party-libraries.html#libraries)
3. Find the instructions to add "2023 CTRE Phoenix Framework (v5)".
4. Copy the Phoenix (v5) json line.
5. In VSCode, right click on "build.gradle" and select "Manage Vendor Libraries"
6. Select "Install New Libraries (online)" and paste the json file location.
7. In "DriveSubsystem", change all motor controller references from PWMSparkMax to WPI_TalonSRX.
8. Remove all "encoder" code.
9. Switch to RobotContainer.
10. Remove all imports that reference "PS4Controller".
11. Change the joystick from "PS4Controller" to "Joystick".
12. Change "getLeftY()" to "getRawAxis(1)"
13. Change "getRightX()" to "getRawAxis(2)"
14. Change button bindings to:
```
    // Drive at half speed when the right bumper is held
    Trigger ButtonRtBump = new JoystickButton(m_driverController, 5);
    ButtonRtBump
        .onTrue(new InstantCommand(() -> m_robotDrive.setMaxOutput(0.5)))
        .onFalse(new InstantCommand(() -> m_robotDrive.setMaxOutput(1)));

    // Stabilize robot to drive straight with gyro when left bumper is held
    Trigger ButtonLtBump = new JoystickButton(m_driverController, 6);
    ButtonLtBump
        .whileTrue(
            new PIDCommand(
                new PIDController(
                    DriveConstants.kStabilizationP,
                    DriveConstants.kStabilizationI,
                    DriveConstants.kStabilizationD),
                // Close the loop on the turn rate
                m_robotDrive::getTurnRate,
                // Setpoint is 0
                0,
                // Pipe the output to the turning controls
                output -> m_robotDrive.arcadeDrive(-m_driverController.getRawAxis(1), output),
                // Require the robot drive
                m_robotDrive));

    // Turn to 90 degrees when the 'X' button is pressed, with a 5 second timeout
    Trigger ButtonX = new JoystickButton(m_driverController, 3);
    ButtonX
        .onTrue(new TurnToAngle(90, m_robotDrive).withTimeout(5));

    // Turn to -90 degrees with a profile when the Circle button is pressed, with a 5 second timeout
    Trigger ButtonY = new JoystickButton(m_driverController, 4);
    ButtonY
        .onTrue(new TurnToAngleProfiled(-90, m_robotDrive).withTimeout(5));
```
15. Update all constants!
16. VERY CAREFULLY try it out.

## Coding Step 2 - Add in the KauaiLabs Gyro
1. Go back to the browser and open [docs.wpilib.org](https://docs.wpilib.org/en/stable/docs/software/vscode-overview/3rd-party-libraries.html#libraries)
3. Find the instructions to add "Kauai Labs".
4. Copy the Kauai Labs json line.
5. In VSCode, right click on "build.gradle" and select "Manage Vendor Libraries"
6. Select "Install New Libraries (online)" and paste the json file location.
7. In "DriveSubsystem", change "private final Gyro m_gyro = new ADXRS450_Gyro();" to
```
private final AHRS m_gyro = new AHRS(SerialPort.Port.kUSB);
```

Show off your work!

### Possible helpful extras
```
  // Slew rate limiters to make joystick inputs more gentle; 1/3 sec from 0
  // to 1.
  private final SlewRateLimiter m_speedLimiter = new SlewRateLimiter(3);
  private final SlewRateLimiter m_rotLimiter = new SlewRateLimiter(3);

    // Get the x speed. We are inverting this because Xbox controllers return
    // negative values when we push forward.
    double xSpeed = -m_speedLimiter.calculate(m_controller.getLeftY()) * Drivetrain.kMaxSpeed;

    // Get the rate of angular rotation. We are inverting this because we want a
    // positive value when we pull to the left (remember, CCW is positive in
    // mathematics). Xbox controllers return positive values when you pull to
    // the right by default.
    double rot = -m_rotLimiter.calculate(m_controller.getRightX()) * Drivetrain.kMaxAngularSpeed;
    m_drive.drive(xSpeed, rot);
```
