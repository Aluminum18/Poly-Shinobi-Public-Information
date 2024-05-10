# Poly Shinobi
A simple replicate of Sekiro combat mechanic. The project source code is set private because assets used in project is not allowed to distributed. Please share your github account with me via email tranvietan08@gmail.com to get access to the repo.

See game tutorial [here](https://youtu.be/Zh1bFNzlsxk) and gameplay [here](https://youtu.be/1nJys1QikOA)
# Mechanic Content
* [General](#general)
  + [Locomotion](#locomotion)
  + [Restrict Movement Based On Action](#restrict-movement-based-on-action)
  + [Grabbling Hook](#grabbling-hook)
  + [Schedule Input](#schedule-input)

* [Camera](#camera)
  + [Lock Target](#lock-target)
  + [Cull Obstructing Camera View Object](#cull-obstructing-camera-view-object)
  + [Screen Alert Indicator](#screen-alert-indicator)
 
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
  + [Secondary Weapon](#secondary-weapon)
  + [Combat Motion Variation](#combat-motion-variation)
  + [Deathblow Variation](#deathblow-variation)

# General
## Locomotion
All movements (except jump) are animation driven.

## Restrict Movement Based On Action
When a character is near an edge, the game prevents them from accidentally falling off during specific actions, particularly combat maneuvers like attacking or deflecting. This feature ensures that players can focus on combat without the worry of unintentionally falling. However, players retain the ability to jump or run off the edge intentionally, providing them with the freedom to explore and execute these actions as desired.

https://github.com/Aluminum18/Project-Dungeon-Public-Information/assets/14157400/7f909763-a30b-452c-ac23-5423f0fd8bfa

Baked path of unity NavMesh is employed to implement this feature.

## Grabbling Hook

* Procedural rope hook: Modify rope segments during runtime to create a soft and elastic rope.
* Hook point indicator: Screen space indicator presents the availability of hook point, supports finding hook point and estimating distance to hook point.
  
https://github.com/Aluminum18/Poly-Shinobi-Public-Information/assets/14157400/f29572f0-20a0-4859-89d3-dc011d34bb85

* Editable hook point: Visualize hook point properties to easily set up hook point on the map
  
  ![image](https://github.com/Aluminum18/Poly-Shinobi-Public-Information/assets/14157400/74413fd8-c238-47e6-9782-3afd70d53e3e)

  Red and green solid arc indicate horizontal and vertical angle range. Wired red arc indicates the possible landing position after hooking.
  
  ![image](https://github.com/Aluminum18/Poly-Shinobi-Public-Information/assets/14157400/7b00fc61-4e14-49b3-af7f-d9ffb6aa2e15)
  
  Two wired spheres indicate the active distance (min and max) of hook point.
  Players can only hook the point if their distance to the hook point is within min and max distance, and their direction is within both horizontal and vertical angle range.

## Schedule Input
Sometimes, character action is blocked by previous action conditions (such as an attack action prevents character from performing another attack while it is on process). Queue input mechanic will reduce player timing effort to perform series of actions.

Following image explains how Schedule input works.
![image](https://github.com/Aluminum18/Project-Dungeon-Public-Information/assets/14157400/88e6ed5d-be60-41f7-8cbc-14f307058092)

Retry is not executed every frame, it only does when Character [Abnormal Status](#abnormal-status) changed.

Only one input is scheduled. If a new input comes when the old one is not executed, it takes old input place and refresh retry period.

In game [clip](https://youtu.be/CZTMMlZEBQI)

# Camera
## Lock Target
Focus and keep enemy in front of character.
Check this [clip](https://www.youtube.com/clip/UgkxH7jG2xIonQjabVtlfri0cBDbTRGxhPZD) for detail.

## Cull Obstructing Camera View Object
Render objects as wireframe when whey obstruct the camera view.
Check this [clip](https://youtu.be/xuJ9510OPOM) for detail.

## Screen Alert Indicator
A Screen Indicator points to enemy position who is offscreen and aware of player appearance. Check this [clip](https://youtu.be/e4gYmEDRpgQ) for detail.

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
## Secondary Weapon
Support weapons with various function. In game [clip](https://youtu.be/6F2OttIxsrE)
## Combat Motion Variation
The next combat motion is depended on previous motion. In game [clip](https://youtu.be/OsKiRr1BobU)

It is configurable by Node Graph.
![image](https://github.com/Aluminum18/Poly-Shinobi-Public-Information/assets/14157400/3b7cb899-8a3b-4acf-a425-bbb90f7bf7e7)


## Deathblow Variation
Various deathblows are executed based on the conditions of the enemy. In game [clip](https://youtu.be/zXua75ZcNbI)

The Deathblow settings such as Animation Target Matching, Sound/Visual effect or Event is configurable. All settings are able to be modified with frame correct.

![image](https://github.com/Aluminum18/Project-Dungeon-Public-Information/assets/14157400/e2b7f72a-4b4e-4052-a86f-b93615d859ea)
![image](https://github.com/Aluminum18/Project-Dungeon-Public-Information/assets/14157400/58c9ddae-64b1-448f-a300-520e8d4c5057)



