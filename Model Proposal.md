# Model Proposal for _[Project Name]_

_Your Name_

* Course ID: CMPLXSYS 530,
* Course Title: Computer Modeling of Complex Systems
* Term: Winter, 2019



&nbsp; 

### Goal 
*****
 
_Provide a short, 1-3 sentence description of the goal of your model_

&nbsp;  
### Justification
****
_Short explanation on why you are using ABM_

&nbsp; 
### Main Micro-level Processes and Macro-level Dynamics of Interest
****

_Short overview of the key processes and/or relationships you are interested in using your model to explore. Will likely be something regarding emergent behavior that arises from individual interactions_

&nbsp; 


## Model Outline
****
&nbsp; 
### 1) Environment
_Description of the environment in your model. Things to specify *if they apply*:_

* _Boundary conditions (e.g. wrapping, infinite, etc.)_
* _Dimensionality (e.g. 1D, 2D, etc.)_
* _List of environment-owned variables (e.g. resources, states, roughness)_
* _List of environment-owned methods/procedures (e.g. resource production, state change, etc.)_


```python
# Include first pass of the code you are thinking of using to construct your environment
# This may be a set of "patches-own" variables and a command in the "setup" procedure, a list, an array, or Class constructor
# Feel free to include any patch methods/procedures you have. Filling in with pseudocode is ok! 
# NOTE: If using Netlogo, remove "python" from the markdown at the top of this section to get a generic code block
```

&nbsp; 

### 2) Agents
 
 _Description of the "agents" in the system. Things to specify *if they apply*:_
 
* _List of agent-owned variables (e.g. age, heading, ID, etc.)_

* agent.basic_personality()
* agent.innovativeness()
   * this parameter takes into account both the agent's basic personality, and also the discipline's innovativeness
* agent.success()
* agent.color()

* _List of agent-owned methods/procedures (e.g. move, consume, reproduce, die, etc.)_
* agent.bridge()
* agent.connect()
* agent.create()

```python
# Include first pass of the code you are thinking of using to construct your agents
# This may be a set of "turtle-own" variables and a command in the "setup" procedure, a list, an array, or Class constructor
# Feel free to include any agent methods/procedures you have so far. Filling in with pseudocode is ok! 
# NOTE: If using Netlogo, remove "python" from the markdown at the top of this section to get a generic code block
```

&nbsp; 

### 3) Action and Interaction 
 
**_Interaction Topology_**

_Description of the topology of who interacts with whom in the system. Perfectly mixed? Spatial proximity? Along a network? CA neighborhood?_
 
**_Action Sequence_**

_What does an agent, cell, etc. do on a given turn? Provide a step-by-step description of what happens on a given turn for each part of your model_

1. Agents create nodes
   * a set of agents create nodes in the environment
   * there are three ways an agent can create a node
      1. The agent can create a node that is connected to a pre-existing node
      2. The agent can create a node that bridges two clusters of nodes
      3. The agent can create an independent node that is not connected to any pre-exisisting node
   * The less innovative an agent is, the more likely the agent is to choose #1; the more innovative an agent is, the more likely the agent is to choose action #3
   * radius
   * If the agent chooses #1 or #2, then the agent's node 
   * direction
      

&nbsp; 
### 4) Model Parameters and Initialization

_Describe and list any global parameters you will be applying in your model._

* discipline.innovativeness()

_Describe how your model will be initialized_

_Provide a high level, step-by-step description of your schedule during each "tick" of the model_

&nbsp; 

### 5) Assessment and Outcome Measures

_What quantitative metrics and/or qualitative features will you use to assess your model outcomes?_

&nbsp; 

### 6) Parameter Sweep

_What parameters are you most interested in sweeping through? What value ranges do you expect to look at for your analysis?_
