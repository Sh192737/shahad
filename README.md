 Manipulating the `turtlesim` Package in ROS Noetic and ROS2 Foxy on a MacBook with an i3 Processor

Tools and Requirements
1. **Operating System**: macOS
2. **Homebrew**: For managing installations
3. **Docker**: To run ROS Noetic
4. **ROS Noetic** (via Docker)
5. **ROS2 Foxy**
6. **`turtlesim` Package**

 Part 1: Manipulating `turtlesim` with ROS Noetic

1. **Install Homebrew**:
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```

2. **Install Dependencies**:
   ```bash
   brew install python3 cmake boost tinyxml eigen pcre poco
   ```

3. **Install Docker**:
   ```bash
   brew install --cask docker
   ```

4. **Run ROS Noetic in Docker**:
   ```bash
   docker pull osrf/ros:noetic-desktop-full
   docker run -it osrf/ros:noetic-desktop-full
   ```

5. **Install `turtlesim` Package**:
   ```bash
   sudo apt-get update
   sudo apt-get install ros-noetic-turtlesim
   ```

6. **Run `turtlesim` Node**:
   - Start ROS core:
     ```bash
     roscore
     ```
   - Run `turtlesim` node:
     ```bash
     rosrun turtlesim turtlesim_node
     ```

7. **Control the Turtle**:
   ```bash
   rosrun turtlesim turtle_teleop_key
   ```

8. **Create a Simple Publisher**:
   ```python
   # publisher.py
   import rospy
   from geometry_msgs.msg import Twist

   def move_turtle():
       rospy.init_node('move_turtle', anonymous=True)
       pub = rospy.Publisher('/turtle1/cmd_vel', Twist, queue_size=10)
       rate = rospy.Rate(10)
       while not rospy.is_shutdown():
           vel_msg = Twist()
           vel_msg.linear.x = 2.0
           vel_msg.angular.z = 1.8
           pub.publish(vel_msg)
           rate.sleep()

   if __name__ == '__main__':
       try:
           move_turtle()
       except rospy.ROSInterruptException:
           pass
   ```

 Part 2: Manipulating `turtlesim` with ROS2 Foxy

1. **Install ROS2 Foxy**:
   Follow the official installation guide: [Installation Guide](https://docs.ros.org/en/foxy/Installation/macOS-Development-Setup.html).

2. **Install Dependencies**:
   ```bash
   brew install python3 cmake tinyxml2 poco
   ```

3. **Install ROS2 Foxy**:
   ```bash
   brew install ros2/ros2/ros-foxy-desktop
   ```

4. **Source ROS2 Setup Script**:
   ```bash
   source /opt/ros/foxy/setup.bash
   ```

5. **Install `turtlesim` Package**:
   ```bash
   sudo apt update
   sudo apt install ros-foxy-turtlesim
   ```

6. **Run `turtlesim` Node**:
   ```bash
   source /opt/ros/foxy/setup.bash
   ros2 run turtlesim turtlesim_node
   ```

7. **Control the Turtle**:
   ```bash
   source /opt/ros/foxy/setup.bash
   ros2 run turtlesim turtle_teleop_key
   ```

8. **Create a Simple Publisher**:
   ```python
   # publisher.py
   import rclpy
   from rclpy.node import Node
   from geometry_msgs.msg import Twist

   class TurtleController(Node):
       def __init__(self):
           super().__init__('turtle_controller')
           self.publisher_ = self.create_publisher(Twist, 'turtle1/cmd_vel', 10)
           self.timer = self.create_timer(0.5, self.timer_callback)

       def timer_callback(self):
           msg = Twist()
           msg.linear.x = 2.0
           msg.angular.z = 1.8
           self.publisher_.publish(msg)

   def main(args=None):
       rclpy.init(args=args)
       turtle_controller = TurtleController()
       rclpy.spin(turtle_controller)
       turtle_controller.destroy_node()
       rclpy.shutdown()

   if __name__ == '__main__':
       main()
   ```

By following these steps, I successfully used the `turtlesim` package in both ROS Noetic and ROS2 Foxy on my MacBook with an i3 processor.
