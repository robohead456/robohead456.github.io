---
layout: default
title: Matlab Symbolic Robot Kinematics
description: By Michael Sidler
excerpt_separator: <!--more-->
---

While taking RBE 3001 it was often necessary to convert Denavit–Hartenberg parameters into transformation matrices or a Jacobian. To make this process easier I created a symbolic Matlab class that could complete all of the calculations with either symbolic variables or real values.
<!--more-->

DH parameters are a series of rotations and translations that define the relationship between the coordinate axes of each joint of a robot arm.

The `d` value specifies a translation along the prior z axis <br>
The `θ` value specifies a rotation about the prior Z <br>
The `a` value specifies a tanslation about the common normal (new x axis) <br>
The `α` value specifies a rotation about the common normal

To start using the Matlab class, symbolic variables must be defined to hold the arbitrary joint positions of the robot actuators. Constant distances may be symbolic or real. Here there is one joint variable for each degree of freedom in the arm, plus two that define constant offsets of the robot.

```matlab
% Joint variables
syms t1 t2 d3 t4 t5 t6;

% Constant variables
syms d2 d6;
```

A symbolic version of pi must also be created, otherwise there will be floating point errors and trig equations will not simplify properly.

```matlab
pi = sym(pi);
```

Next the DH parameters are put into a table. The first entry is the transform from joint 1 to 2, the second 2 to 3, and so forth.

```matlab
% DH table [theta, d, alpha, a]
dht = [t1,  0, -pi/2, 0;...
       t2, d2,  pi/2, 0;...
        0, d3,     0, 0;...
       t4,  0, -pi/2, 0;...
       t5,  0,  pi/2, 0;...
       t6, d6,     0, 0];
```

Finally the joint types must be identified as revolute or prismatic.

```matlab
% Joint types [0=revolute, 1=prismatic]
joints = [0 0 1 0 0 0];
```

After that the kinematics can be instantiated and validated

```matlab
% Instantiate the kinematics
K = kinematics(dht,joints);

% Check that all transforms are valid
K.check_transforms();
```

The class has the following features
- Display the HTM from one joint to the following joint <br>
`function disp_Tij(obj, index)`
- Display the HTM from the base to any joint <br>
`function disp_T0i(obj, index)`
- Display the Jacbian <br>
`function disp_J(obj)`
- Return the DH table <br>
`function DH = get_DH(obj)`
- Return the joint types <br>
`function jt = get_jt(obj)`
- Return the HTM from one joint to the following joint <br>
`function Tij = get_Tij(obj)`
- Return the HTM from the base to any joint <br>
`function Tij = get_T0j(obj, index)`
- Return the HTMs from each joint to the following joint <br>
`function T0i = get_Tij_all(obj, index)`
- Return the HTMs from the base to all joints <br>
`function T0i = get_T0i_all(obj)`
- Return the Jacbian <br>
`function J = get_J(obj)`
- Substitute symbolic values and return the HTM from the base to any joint <br>
`function T0i = get_T0i_sub(obj, index, sym_vars, real_vars)`
- Substitute symbolic values and display the HTM from the base to any joint <br>
`function disp_T0i_sub(obj, index, sym_vars, real_vars)`
- Calculate the forward kinematics (end effector position given joint angles) <br>
`function p = FK(obj, sym_vars, real_vars)`

Writing this class gave me a greater understanding of transformation matricies and jacobians, as well as helping me learn Matlab's symbolic tools. One feature that I would have liked to add is a graphical display of the arm and its coordinate frames given a set of joint angles.

The source code can be found at: <br>
[https://github.com/robohead456/symbolic-kinematics](https://github.com/robohead456/symbolic-kinematics)

<br>
<hr>