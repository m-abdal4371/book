---
id: chapter2
title: 'Chapter 2: Core ROS 2 Primitives'
---

# Chapter 2: Core ROS 2 Primitives

In the previous chapter, we introduced ROS 2 as the "nervous system" for modern robots. Now, we'll dissect that system to understand its fundamental components. A ROS 2 application is a distributed graph of independent programs. The three core primitives that enable this architecture are **Nodes**, **Topics**, and **Services**. A fourth concept, **Messages**, defines the structure of the data that flows between them.

Understanding these building blocks is the key to understanding how to design, build, and debug any ROS 2-powered robot.

## 1. Nodes: The Building Blocks of Behavior

A **Node** is the most basic computational unit in ROS 2. Think of a node as a small, self-contained program responsible for a single, specific task. A complex robotic system is built by combining many simple nodes, each with its own purpose. This is a classic example of a microservices architecture, applied to robotics.

For a humanoid robot, you might have nodes for:
-   `camera_driver`: Acquires images from the robot's head camera and nothing else.
-   `object_detector`: Receives images and identifies objects within them.
-   `path_planner`: Calculates a route from the robot's current location to a target.
-   `leg_controller`: Controls the motors in the robot's legs to execute walking motions.
-   `gripper_actuator`: Manages the opening and closing of the robot's hand.

This modular, single-responsibility design is a cornerstone of ROS 2. It offers several key advantages:
-   **Fault Tolerance**: If the `object_detector` node crashes, the `leg_controller` can continue to function, allowing the robot to remain stable. The system degrades gracefully rather than failing completely.
-   **Reusability**: The `camera_driver` node you write for one robot can be reused on another robot, even if the rest of the system is completely different.
-   **Ease of Development**: Small, focused programs are easier to write, test, and debug than a single, monolithic application. Different teams can work on different nodes in parallel.
-   **Resource Management**: You can run resource-intensive nodes (like a vision-processing AI model) on a powerful, dedicated computer, while lightweight nodes (like a battery monitor) can run on a low-power microcontroller, and ROS 2 will manage the communication between them.

A ROS 2 system is composed of many nodes working in concert. But how do they communicate? This is where Topics and Services come in.

## 2. Topics and Messages: The Public Address System

A **Topic** is a communication bus that allows for asynchronous, many-to-many message passing. It works on a **publish-subscribe** model, much like a YouTube channel or a newsletter.

-   **Publishers**: A node can **publish** messages to a topic. It doesn't know or care who is listening.
-   **Subscribers**: Other nodes can **subscribe** to a topic to receive those messages.

The data itself is sent in the form of **Messages**. A message is a simple data structure with typed fields. ROS 2 provides a library of standard, reusable messages for common data types, such as:
-   `std_msgs/String`: A simple text string.
-   `sensor_msgs/Image`: A raw image with metadata like height, width, and encoding.
-   `geometry_msgs/Twist`: A message for representing velocity, often used to command a robot's movement.
-   `geometry_msgs/PoseStamped`: A robot's position and orientation in space, with a timestamp.

You can also define your own custom messages. For example, if our `object_detector` node identifies an object, it might publish a custom message to a `/detected_objects` topic. The message definition could look like this:

```
# Object.msg
string class_name
float32 confidence
geometry_msgs/Point position
```

This decoupling of publishers and subscribers is incredibly powerful. You can have:
-   One node publishing data that is consumed by many subscribers (e.g., one camera feeding an object detector, a face recognizer, and a remote monitoring display).
-   Many nodes publishing to the same topic (e.g., multiple temperature sensors reporting to a `/robot/temperature` topic).
-   Nodes can be added or removed dynamically without reconfiguring the system. You can start a new logging node that subscribes to any topic to save data to disk without restarting any part of the system.

<!-- Diagram: Publish/Subscribe Model for Topics. Refer to static/img/chapter2_diagram.txt for content. -->

Topics are the ideal communication method for continuous data streams, such as sensor readings, robot state information, and control commands that are sent repeatedly.

## 3. Services: The Question and Answer

While Topics are great for one-way data flow, sometimes you need a direct, two-way interaction: a request followed by a response. This is what a **Service** provides.

A Service works on a **client-server** model, analogous to making a function call or querying a web API. Like messages, services have a defined data structure, typically split into a `Request` and a `Response` part.

-   **Service Server**: A node can provide a service, advertising that it is available to perform a specific task upon request.
-   **Service Client**: Another node can act as a client, sending a request to the service and **waiting for a response**.

The key characteristics of a service are:
-   **Synchronous**: The client sends a request and blocks (waits) until the server sends back a response.
-   **One-to-One**: A service call is a direct interaction between one client and one server.
-   **Guaranteed Execution**: The model ensures that the request is received and a response (either success or failure) is returned.

Services are perfect for tasks that need to be triggered on-demand and have a clear result, such as:
-   Requesting a robot to perform a specific action (`/gripper/close`).
-   Querying the robot's state (`/battery/get_status`).
-   Triggering a computation that has a distinct end (`/compute_ik` to calculate inverse kinematics).

## 4. Introspection with the `ros2` CLI

One of the most powerful aspects of ROS 2 is its suite of command-line tools for introspecting a running system. The `ros2` command is your window into the robot's nervous system, allowing you to observe, debug, and interact with it in real time.

Even without writing any code, you can use these tools to:
-   `ros2 node list`: See all the nodes that are currently running.
-   `ros2 topic list`: List all the topics that are being published to.
-   `ros2 topic echo <topic_name>`: Print the messages being published on a specific topic to the console.
-   `ros2 service list`: See all the available services.
-   `ros2 service call <service_name> <service_type> <arguments>`: Manually call a service from the command line.

These tools are invaluable for debugging. If a robot isn't behaving as expected, you can use the `ros2` CLI to check if the right nodes are running, if they are publishing data on the correct topics, and if the data itself makes sense. It provides a level of transparency that is essential for managing complex robotic systems.

## 5. Quality of Service (QoS): Fine-Tuning the Nervous System

A major advancement in ROS 2 is the concept of **Quality of Service (QoS)**. Not all data is created equal. The requirements for a video stream are very different from the requirements for a command to stop the robot in an emergency. QoS settings allow developers to fine-tune the behavior of topics and services to match the needs of the data being transmitted.

Key QoS policies include:
-   **Reliability**: Do you need a guarantee that every message is delivered (`Reliable`), or is it acceptable to drop messages in favor of receiving the most recent data (`Best Effort`)? For a video stream, `Best Effort` is usually sufficient. For a command to deploy an airbag, `Reliable` is essential.
-   **Durability**: Should a new subscriber receive messages that were published *before* it joined (`Transient Local`)? This is useful for topics that publish configuration or static state. Or should it only receive new messages (`Volatile`)?
-   **History**: How many past messages should be kept for late-joining subscribers?

These settings provide a powerful way to configure the performance and behavior of the robot's communication system on a per-topic or per-service basis, ensuring that critical data is handled appropriately while non-critical data does not bog down the network.

## Summary: Primitives in Action

| Primitive | Model | Communication | Use Case | Analogy |
|-----------|---------|---------------|------------|---------|
| **Node** | Program | N/A | A single-purpose process | An organ in the body (e.g., the eye) |
| **Topic** | Publish/Subscribe | Asynchronous | Continuous data streams | A public broadcast radio station |
| **Service** | Client/Server | Synchronous | On-demand tasks with a result | Making a phone call and getting an answer |
| **Message** | Data Structure | N/A | Defining the data on topics/services | The language spoken on the radio |

By combining these primitives, you can build robotic applications of immense complexity. A `camera_driver` node publishes `Image` messages on a topic. An `object_detector` node subscribes to that data and, when it finds something interesting, calls a service on a `task_manager` node to decide on an action. The `task_manager` then publishes a command on another topic, which is received by a `leg_controller` node to move the robot. This is the fundamental workflow of the robotic nervous system.
