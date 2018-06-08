#!/usr/bin/env python

# print or rospy.loginfo will make a serious influence for the control of ur robot, so please do not use them!
# -- inksci@outlook.com

import rospy
from std_msgs.msg import String

import numpy as np

from trajectory_msgs.msg import JointTrajectory
from trajectory_msgs.msg import JointTrajectoryPoint











import threading
import time

import numpy as np



def talker_1():
    try:

        pub = rospy.Publisher('/joint_speed_1', JointTrajectory, queue_size=10)

        rate = rospy.Rate(125)

        trajectory = JointTrajectory()

        i_seq = 0

        qvel = [0, 0, 0, 0, 0, 0]

        while not rospy.is_shutdown():

            for i in range(6):
                qvel[i] += (np.random.rand()-0.5)*2* 0.01

            current_time = rospy.Time.now()
            trajectory.header.seq = i_seq
            i_seq += 1

            trajectory.header.stamp = current_time
            trajectory.joint_names = ['elbow_joint', 'shoulder_lift_joint', 'shoulder_pan_joint', 'wrist_1_joint', 'wrist_2_joint', 'wrist_3_joint']
            trajectory.points = [
                JointTrajectoryPoint( velocities=qvel )]
            pub.publish(trajectory)

            # rospy.loginfo(trajectory)

            rate.sleep()


    except rospy.ROSInterruptException:
        pass








def talker_2():
    try:

        pub = rospy.Publisher('/joint_speed_2', JointTrajectory, queue_size=10)

        rate = rospy.Rate(2)

        trajectory = JointTrajectory()

        i_seq = 0

        qvel = [0, 0, 0, 0, 0, 0]

        while not rospy.is_shutdown():

            for i in range(6):
                qvel[i] += (np.random.rand()-0.5)*2* 0.01

            current_time = rospy.Time.now()
            trajectory.header.seq = i_seq
            i_seq += 1

            trajectory.header.stamp = current_time
            trajectory.joint_names = ['elbow_joint', 'shoulder_lift_joint', 'shoulder_pan_joint', 'wrist_1_joint', 'wrist_2_joint', 'wrist_3_joint']
            trajectory.points = [
                JointTrajectoryPoint( velocities=qvel )]
            pub.publish(trajectory)

            # rospy.loginfo(trajectory)

            rate.sleep()


    except rospy.ROSInterruptException:
        pass




def talker_3():
    pub = rospy.Publisher('chatter', String, queue_size=10)
    rate = rospy.Rate(10) # 10hz
    while not rospy.is_shutdown():
        hello_str = "hello world %s" % rospy.get_time()
        # rospy.loginfo(hello_str)
        pub.publish(hello_str)
        rate.sleep()



def callback(data):
    pass
    # rospy.loginfo(rospy.get_caller_id() + 'I heard %s', data)


def listener():

    # In ROS, nodes are uniquely named. If two nodes with the same
    # name are launched, the previous one is kicked off. The
    # anonymous=True flag means that rospy will choose a unique
    # name for our 'listener' node so that multiple listeners can
    # run simultaneously.

    rospy.Subscriber('chatter', String, callback)

    # spin() simply keeps python from exiting until this node is stopped
    rospy.spin()



qvel_global = [0, 0, 0, 0, 0, 0]
mutex = threading.Lock()


def talker_4():


    try:

        pub = rospy.Publisher('/ur_driver/joint_speed', JointTrajectory, queue_size=10)

        rate = rospy.Rate(125)

        qvel = [0, 0, 0, 0, 0, 0]

        i_seq = 0

        global qvel_global, mutex


        while not rospy.is_shutdown():


            if mutex.acquire():
                qvel = qvel_global
                mutex.release()


            trajectory = JointTrajectory()
            trajectory.joint_names = ['elbow_joint', 'shoulder_lift_joint', 'shoulder_pan_joint', 'wrist_1_joint', 'wrist_2_joint', 'wrist_3_joint']




            current_time = rospy.Time.now()
            trajectory.header.seq = i_seq
            i_seq += 1

            trajectory.header.stamp = current_time
            trajectory.points = [JointTrajectoryPoint(velocities=qvel)]
            pub.publish(trajectory)








            rate.sleep()

    except rospy.ROSInterruptException:
        pass









from sensor_msgs.msg import JointState

ur_position = []
ur_velocity = []

def callback_2(data):
    pass
    # rospy.loginfo(rospy.get_caller_id() + 'I heard %s', data)


    global ur_position, ur_velocity
    ur_position = data.position
    ur_velocity = data.velocity


def listener_2():

    # In ROS, nodes are uniquely named. If two nodes with the same
    # name are launched, the previous one is kicked off. The
    # anonymous=True flag means that rospy will choose a unique
    # name for our 'listener' node so that multiple listeners can
    # run simultaneously.


    rospy.Subscriber('/joint_states', JointState, callback_2)

    # spin() simply keeps python from exiting until this node is stopped
    rospy.spin()






def ink_control():

    global qvel_global, mutex


    rate = rospy.Rate(10)

    while 1:

        ink = 0
        for i in range(1000):
            ink += 1


        if mutex.acquire():
            for i in range(6):
                qvel_global[i] += (np.random.rand()-0.5)*2* 0.01
            mutex.release()


        rate.sleep()



rospy.init_node('talker', anonymous=True)

t = threading.Thread( target = talker_1 )
t.start()

threading.Thread( target = talker_2 ).start()

threading.Thread( target = talker_3 ).start()




t = threading.Thread( target = listener )
t.start()

threading.Thread( target = talker_4 ).start()


threading.Thread( target = listener_2 ).start()


threading.Thread( target = ink_control ).start()




