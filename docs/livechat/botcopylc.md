# Botcopy Live Chat

Seamlessly hand-off between agents and departments without disrupting your customer service flow. 

## Departments
Each department has an associated parameter value associated with it. Use this value to route between departments.

### Dialogflow CX 

1. To invoke live chat -> Set a session parameter of `bcHumanHandover` with a value of `true` on your route
2. To route live chat to a specific department ->  Set a session parameter of `bcDepartment` with a value equal to `yourDepartmentValue` set in Departments.


### Dialogflow ES

1. To invoke live chat -> Set an output context of `bc-livechat-handover` with a value of true on your route
2. To route live chat to a specific department ->  Set a parameter of `bc-department` with a value equal to `yourDepartmentValue` set in Departments.
