# Project Dungeon
A simple replicate of Sekiro combat mechanic.

# Mechanic Content
* [General](#general)
  + [Locomotion](#locomotion)
  + [Schedule input](#schedule-input)

* [Camera](#camera)
  + [Lock target](#lock-target)
  + [Cull obstructing camera view object](#cull-obstructing-camera-view-object)
 
* [NPC](#npc)
  + [Predictable Behavior Graph](#predictable-behavior-graph)
  + [NPC Local Message](#npc-local-message)
  + [Patrol](#patrol)
* [Combat](#combat)
  + [Abnormal Status](#abnormal-status)
  + [Configurable Attack](#configurable-attack)
  + [Posture](#posture)
  + [Deflect](#deflect)
  + [Perilous Attacks](#perilous-attacks)

# General
## Locomotion
All movements (except jump) are animation driven.

## Schedule input
Sometimes, character action is blocked by previous action conditions (such as an attack action prevents character from performing another attack while it is on process). Queue input mechanic will reduce player timing effort to perform series of actions.

Following image explains how Schedule input works.
![image](https://github.com/Aluminum18/Project-Dungeon-Public-Information/assets/14157400/88e6ed5d-be60-41f7-8cbc-14f307058092)

Retry is not executed every frame, it only does when Character [Abnormal Status](#abnormal-status) changed.

Only one input is scheduled. If a new input comes when the old one is not executed, it takes old input place and refresh retry period.

In game [clip](https://youtu.be/CZTMMlZEBQI)

# Camera
## Lock Target
Check this [clip](https://www.youtube.com/clip/UgkxH7jG2xIonQjabVtlfri0cBDbTRGxhPZD) for detail.

## Cull obstructing camera view object
Check this [clip](https://youtu.be/xuJ9510OPOM) for detail.

# NPC
## Predictable Behavior Graph
The NPC behavior is determined by a Predictable Behavior Graph, ensuring that enemy actions follow predictable patterns. This predictability allows players to build muscle memory and effectively counter formidable opponents.
![image](https://github.com/Aluminum18/Dungeon/assets/14157400/d920b40e-35a5-4b49-bb06-44c0351d0afb)

Each behavior could be configured by Behavior Node

![image](https://github.com/Aluminum18/Dungeon/assets/14157400/3c860007-bedd-4008-8803-4cdc14d22cad)

+ **Priority:** Only higher priority node can interrupt this behavior.
+ **Decide Interval:** This node will try transiting to one of "Next behaviors" every "Decide Interval" seconds.
+ **Active Transit Ratio:** Probability of transiting to "Next behaviors" when decide.
+ **Execution Message:** Message that will be broadcasted when transit to this node.
+ **Next Behaviors:** Behaviors this node can self transit to. Sum of their ratio always equals 1 (auto adjust by editor script)
+ **Next Condition Behaviors:** Behaviors this node will transit to if it received "Condition Message".

## NPC Local Message
NPC logical modules communicate by SOMessage.

![image](https://github.com/Aluminum18/Dungeon/assets/14157400/d451ae26-5e2d-440c-8ed4-d9d8710bdb87)

External Deciders decide actions that react situation. Internal Deciders decide self actions, such as an Enemy does Attack B after he finishes Attack A.

## Patrol
Configurable patrol points. Unity NavMesh is used.

![image](https://github.com/Aluminum18/Dungeon/assets/14157400/4f67b05d-ab3d-434b-b8d4-339421c867a6)

Patrol Settings

![image](https://github.com/Aluminum18/Dungeon/assets/14157400/c9e1fc4c-a06b-4897-b5d9-16625fe1f300)

+ **Aware Distance:** NPC slowly detects (increase Detect Ratio) player is within this distance and View Angle.

+ **Detect Distance:** NPC instantly detects player is within this distance and View Angle.

+ **View Angle:** NPC view angle.

+ **Detect Rate:** How fast Detect Ratio is increased.

+ **Lose Focus Rate:** How fast Detect Ratio is decreased when player is out of view or distance.

+ **Detect Ratio:** When this value >= 1, NPC will detect player.

# Combat
## Abnormal Status
This status could be positive or negative. Abnormal Status limits or expands actions characters can do. When characters perform an action, Abnormal Status could be applied to them. For instance, when perform an attack, characters will be applied "RotationBlock", "MovementBlock" in specific frames during animation.
Check this [clip](https://youtu.be/4DfgSQTKMJs) for detail.

## Configurable Attack
Attack properties is configured following frame time to make it match the animation.
![image](https://github.com/Aluminum18/Dungeon/assets/14157400/e7e8542d-26e6-49a8-9c1c-4c5a811c0460)

## Posture
In game [clip](https://youtube.com/clip/UgkxIh0gjdpra2cljqOgkvcYhNzpW7geWyCG?si=KDd2fz_jHzE7oUPT)
## Deflect
In game [clip](https://youtube.com/clip/UgkxyPnWCCGPHhsUSlm-_vjA9f1fFhiWb-dY?si=P6UP_AoAaVQglerK)
## Perilous Attacks
In game [clip](https://youtube.com/clip/UgkxH8ok6iorMr2kWxos0nf4W7mVAsE0srIy?si=3YLyFm95LF0o54n-)