# Model Proposal for _[Essential Tension Model]_

_Anne Lin_

* Course ID: CMPLXSYS 530,
* Course Title: Computer Modeling of Complex Systems
* Term: Winter, 2019



&nbsp; 

### Goal 
*****
 
_To model the emergent effect of what Thomas Kuhn calls the "essential tension" of science, which is the tension amongst scientists to pursue traditional versus innnovative work._

&nbsp;  
### Justification
****
_The essential tension resembles models of exploration exploitation, which is a dynamic that has been successfully represented using agent-based modeling in the past. Futhermore, agent-based modeling allows us to represent the process as a network evolution. This is a suitable way of representing the process of scientific discovery, since scientific works link together and form a network._

*LS COMMENTS: Solid justification and motivation. For the final write-up, consider identifying a few existing models of exploration/exploitation that you can refer back to.*

&nbsp; 
### Main Micro-level Processes and Macro-level Dynamics of Interest
****
* The fluctuation of innovativeness within a scientific community over time
* The relationship between the overall level of innovativeness of a scientific community and the distribution of success amongst its members (as measured through citation couonts)
 
*__LS COMMENTS:__ Good. You can also think about how structural properties of the networks that are formed are also an macro-level dynamic (i.e. this is a "dynamics **of** networks" situation).*


&nbsp; 


## Model Outline
****
&nbsp; 
### 1) Environment

This model does not have an environment. It only has agents, who create nodes and form a network.

*__LS COMMENTS:__ you can actually consider the network that agents construct to be an environment, especially since it is something that factors into their decision making as time goes on.*

* discipline.popularity
   * this variable dictates the rate at which new members join the scientific community. The rate fluctuates based on the average success of the community-- the higher the average citation counts of the community members, the higher the discipline popularity.
   
*__LS COMMENTS:__ I wasn't sure how you were defining "disciplines" here? Will there be different disciplines in the model simultaneously? If so, will being part of a discipline be indicated by having agents only join a certain part/certain set of nodes in the graph?*

&nbsp; 

### 2) Agents
 
* agent.basic_personality = random number on a right-skewed distribution with a variance of discipline_innovation_skew
* agent.innovativeness = (citation count *percentile*) x (basic_personality / 100)
   * this parameter takes into account both the agent's basic personality, and also their citation count
* agent.color = random *unique* color
* agent.citation_count = 0
* agent.project_duration = 0
* agent.career_length = 0
&nbsp; 

*__LS COMMENTS:__ I like the way you're thinking about this! Are higher values of innovativeness indicative of being more innovative?*

* agent.connect()  <br />
<pre><code>
   if agent has not previously created a node in the network:
      create a node that links to another pre-existing random node
   else:
      randomly pick a pre-existing node created by the agent in previous rounds
      survey the nodes that are within a certain degree of connection to that node
      based on agent's innovativeness, choose a node to connect to (the less innovative, the more likely to connect to a node with high connections)
</code></pre>
      
* agent.create() <br />
   agent creates a node that is not connected to any pre-existing nodes

*__LS COMMENTS:__ Neat way of representing the knowledge creation process! By surveying the nodes within a certain degree of connection, do you mean looking for nodes that are a certain pathlength away?*

&nbsp; 

### 3) Action and Interaction 
 
**_Interaction Topology_**

_Agents interact with each other through building a network._
 
**_Action Sequence_**

1. Agents create nodes
   * a set of agents create nodes in the environment. The nodes represent a research paper that they publish
   * there are three ways an agent can create a node
      1. The agent can create a node that is connected to a pre-existing node
      2. The agent can create an independent node that is not connected to any pre-exisisting node

*__LS COMMENTS:__ 3 ways?*

   * The less innovative an agent is, the more likely the agent is to choose #1; the more innovative an agent is, the more likely the agent is to choose action #2
   * If the agent chooses #1, then the agent's new node must be under a certain degree away from one of the previous node(s) of the same agent, given that it is not the agent's first turn. The number of degrees will be randomly assigned based on a positively-skewed distribution. This is to capture the fact that scientists usually only explore a small number of topics.
   * If the agent chooses #1, the agent's level of innovativeness then further determines which pre-existing node the agent's new node will connect to. The less innovative the agent is, the more likely that the agent will choose to connect to a node with high centrality.
   
*__LS COMMENTS:__ To clarify, are you planning that nodes created by process 2 may ultimately be connected to by other agents as well? If so, may need to think of how to operationalize this in terms of "distance" as the default in networks is to assume isolated, unconnected nodes are "infinitely" far away.*

2. Agents adjust their total citation count
   * Each agent of the round will be rewarded for their research paper through receiving citations. The number of citations received for a paper is determined probablistically depending on the number of connections the node has. If the node is not connected to any other nodes (aka the agent chooses action #2), then the number of citations received for the node will be determined by a bimodal distribution. Otherwise, the number of citations will be determined by a binomial distribution, where the more connections the node has, the smaller the variance of the curve. (CORRECTION: how can #1 produce node with 1+ connections?)
   * The citations an agent receives for the round is added to the agent's total number of citations (agent.citation_count)

*__LS COMMENTS:__ Not sure what you mean by the question regardin #1, but feel free to talk to me about it in class*

3. Agents adjust their innovativeness
   * The new agent.innovativeness for each agent depends on both on their agent.basic_personality and their updated agent.citation_count
4. Discipline adjusts their popularity
   * (discipline.popularity())
   * Based on the average number of citations of a given agent in the last few rounds
4. Agents reset their project durations
   * The more connections an agent's node has, the shorter the project duration will be. This will determine how many turns it will take before the same agent creates another node. This is to account for the idea that the less explored a topic is, the longer it will take to conduct research surrounding the topic. 
   * The agent is assigned a new project_duration (agent.project_duration).
5. Other agents decrease their project_duration by 1
6. All agents increase agent.career_duation by 1
7. Some old agents retire
   * For each agent, if agent.career_duration = discipline.career_duration, agent retires and will no longer be creating nodes.
8. Some new agents join in
   * The number of new agents that join in a round is determined probablistically by a normal distribution. The mean of the normal distribution depends on the discipline's popularity (discipline.popularity).

*__LS COMMENTS:__ May have some further details to hammer out as you implement this into a computational model, but again, cool approach!*      

&nbsp; 
### 4) Model Parameters and Initialization

_Describe and list any global parameters you will be applying in your model._

* career_length()
   * the number of ticks an agent will be active until they retire
* ticks()
   * how long the model runs before it stops
* initial_population()
   * the number of agents that create nodes in the intial round
* discipline_innovation_skew() 
   * the liklihood that an agent has an innovative basic personality

This model does not have patches in the envrionment. So instead, the model will start off with a group of agents creating nodes in the environment. The number of intial agents is specified in parameter intial_population. 

*__LS COMMENTS:__ Good.*

&nbsp; 

### 5) Assessment and Outcome Measures

* The distribution of node connections measured by gini-coefficient
* The distribution of citation counts measured by gini-coefficient
* The variance between citation counts and node connections 
* The variance between citation counts and number of nodes
* The variance between basic personality and citation counts
* The variance betweeen discipline innovation skew and average node connection count
* The variance betweeen discipline innovation skew and average citation count
* Average citation count amongst all agents over time

&nbsp; 

*__LS COMMENTS:__ Might also consider some additional network metrics such as avg. clustering coefficient and path length. These would likely have some interesting substantive interpretations.*


### 6) Parameter Sweep

* ticks()
   * Not sure yet how long it will take for emergent dynamics to occur
   * Tentative range: 1000-10000, in increments of 1000
* discipline_innovation_skew()
   * 1-10

*__LS COMMENTS__: Likely will want to consider population size and career length parameters here as well.* 

*__LS COMMENTS__: This project has definitely come along, and I think you have a very feasible and interesting first pass model here! Look forward to seeing where it goes. Priority now is to start implementing these into some code if you are not already doing so. That will be very clarifying into some outstanding design question. 18.5/20*

