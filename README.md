2014-Off-Season-Drivetrain
==========================

2014 Offseason Mecanum Drive with Kinect
This code originally came from the FIRST sample KinectStick project in NetBeans
Team 4215 added the mecanum specific code

 /*----------------------------------------------------------------------------*/
/* Copyright (c) FIRST 2011. All Rights Reserved.                             */
/* Open Source Software - may be modified and shared by FRC teams. The code   */
/* must be accompanied by the FIRST BSD license file in the root directory of */
/* the project.                                                               */
/*----------------------------------------------------------------------------*/

package edu.wpi.first.wpilibj.templates;


import edu.wpi.first.wpilibj.Jaguar;
import edu.wpi.first.wpilibj.Joystick;
import edu.wpi.first.wpilibj.SimpleRobot;
import edu.wpi.first.wpilibj.RobotDrive;
import edu.wpi.first.wpilibj.KinectStick;
import edu.wpi.first.wpilibj.Timer;
import edu.wpi.first.wpilibj.Watchdog;

/**
 *
 * @author koconnor
 * This code demonstrates the use of the KinectStick
 * class to drive your robot during the autonomous mode
 * of the match, making it a hybrid machine. The gestures
 * used to control the KinectStick class are described in the
 * "Getting Started with the Microsoft Kinect for FRC" document
 *
 */
public class KinectStickExample extends SimpleRobot {

    
    Joystick leftStick = new Joystick(1);
    Joystick rightStick = new Joystick(2);
    
    // no RobotDrive object because we program each wheel individually
    Jaguar frontLeft = new Jaguar(1);
    Jaguar frontRight = new Jaguar(2);
    Jaguar backLeft = new Jaguar(3);
    Jaguar backRight = new Jaguar(4);
    
    
    // don't change the value of these. They are Cim motor values
    // they can only exist from -1 to 1
    double tank = 0;
    double tank2 = 0;
    double turn = 0;
    double strafe = 0;
    
    
    RobotDrive drivetrain;
    KinectStick leftArm;
    KinectStick rightArm;
    boolean exampleButton;

    public KinectStickExample() {
  
        leftArm = new KinectStick(1);
        rightArm = new KinectStick(2);
    }

    /**
     * This function is called once each time the robot enters autonomous mode.
     */
     
    public void autonomous() {
        while(isAutonomous() && isEnabled()){
            /**
             * KinectStick axis values are accessed identically to those of a joystick
             * In this example the axis values have been scaled by ~1/3 for safer
             * operation when learning to use the Kinect.
             */
            
            frontRight.set(rightArm.getY()*.33);
            backRight.set(rightArm.getY()*.33);
            frontLeft.set(leftArm.getY()*.33);
            backLeft.set(leftArm.getY()*.33);

            /* An alternative illustrating that the KinectStick can be used just like a Joystick */
            //drivetrain.tankDrive(leftArm, rightArm);

            /*Example illustrating that accessing buttons is identical to a Joystick */
            exampleButton = leftArm.getRawButton(1);

            Timer.delay(.01); /* Delay 10ms to reduce processing load */
        }
    }

    /**
     * This function is called once each time the robot enters operator control.
     */
     
    public void operatorControl() {
       
        while(isOperatorControl() && isEnabled()){
           Watchdog.getInstance().feed();
           
           drivingMethod();
        }
    }
        
    public void drivingMethod(){
        // attempt at making tank + strafeing
   
        Watchdog.getInstance().feed();
        
        // If motors are backwards, set tank to negative
        backLeft.set(tank - strafe);
        frontRight.set(tank2 - strafe); 
        backRight.set(tank2 + strafe);
        frontLeft.set(tank + strafe);
        
        // Makes 3 doubles from value of joysticks, 
        tank = leftStick.getY();
        tank2 = rightStick.getY();
        strafe = rightStick.getX();
    
    }
        
    }

