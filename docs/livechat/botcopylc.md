# Botcopy Live Chat

Seamlessly hand-off between agents and departments without disrupting your customer service flow.

## Departments

Each department has an associated parameter value associated with it. Use this value to route between departments.

### Dialogflow CX

1. To invoke live chat -> Set a session parameter of `bcHumanHandover` with a value of `true` on your route
2. To route live chat to a specific department -> Set a session parameter of `bcDepartment` with a value equal to `yourDepartmentValue` set in Departments.

### Dialogflow ES

1. To invoke live chat -> Set an output context of `bc-livechat-handover` with a value of true on your route
2. To route live chat to a specific department -> Set a parameter of `bc-department` with a value equal to `yourDepartmentValue` set in Departments.

### What happens if a users loses connection during a livechat conversation?

If a user loses connection temporarily and comes back, the messages that were sent during the time the user was disconnected will be displayed to the user.  
Here is a demo of this behavior in action:

<div style="position: relative; padding-bottom: 104.77611940298507%; height: 0;"><iframe src="https://www.loom.com/embed/2f0424cafd3745989a1f2a03109145fc?sid=9b581236-4b28-4d09-b291-60bf8b1cc376" frameborder="0" webkitallowfullscreen mozallowfullscreen allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe></div>
