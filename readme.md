# readme

Translating the bpSim metamodel for use with the bpmn-js modeler.
## What is bpsim?

BPSim is a standard for exchanging simulation models between different modelling and simulation tools.

The bpSim specification is a simulation specific extension of BPMN 2.0 and addresses much needed simulation specific characteristics that are missing in the official specification.  
More info about the bpSim spec can be found here: https://www.bpsim.org/  
The bpSim has seen practical use for some time and is currently being used by Trisotech, Lanner, Sparx, jBPM, and more.  


The specification is formally described in two ways. 
A metamodel that that acts as a description of its containing elements, concepts, and their relations. This model is available in the *.EAP* format but can be exported for use with other modelling toos via *sparx enterprise architecture*
A XML based schema thas serves as the interchange format. The primary focus of this effort being the readability of the XML schema. The metamodel does therefore not adhere to all known best practices.  


**repository with metamodel and schema available here** https://github.com/BusinessProcessSimulation/BPSim   
**bpSim XML schema available here** https://www.bpsim.org/schemas/2.0/  
**implementers guide available here** https://www.bpsim.org/specifications/2.0/WFMC-BPSWG-2016-01.pdf  


## Business process simulation - How?
Simulation is performed by creating a detailed simulation model. The model can be understood as being comprised of a series of perspectices that each focus on a specific angle.  

There are many perspectives that all describe different views of the same model. One should only use the required perspectives and not over complicate a model with excess information. Occams razor and all that 

Contorl flow: describes the model element and how they relate to each other. For example activities, archs, gateways.  
Resource perspective: describes the resource that are involved in the process amd how they are involved (what activities they are tied to).   
Performance perspective: describes the amount of time it takes to complete some activity and waht resources it requires.  

By creating specific simulation scenarios we can record process execution and perform statistical analysis to key performance indicators which then tell us about how the process performs under certain conditions. 
Testing process alterations by use of simulation can be a viable tool to test alternatives in a low risk environment. 

## The context of this specification
Creating simulation models require good insights into the real process. There is often a large gap between the modeled (ideal) process and the real process. Creating a simualtion of some process that is not representative of reality may give incorrect results or worse.
Process mininig is used as a tool to initiate a simulation model. Instead of creating one out of what we might perceive to be the real process, we can use statistical techniques to extract metrics about earlier process exetutions. This allows us to create an initial simulation model which forms the bedrock for further analysis.

## Objective
Perform a manual translation of the bpSim specification into the bpmn-js input format. The bpmn-js modeler has wide adoption and it is thought that this metamodel would have wide appeal as it would make the modeler available as a viable option for future adopters that are seeking a graphical modeler for the configuration of simulation models.

## Research
[1] - https://github.com/bpmn-io/bpmn-js-examples/tree/master/custom-meta-model  
[2] - https://www.bpsim.org/specifications/2.0/WFMC-BPSWG-2016-01.pdf  
[3] - http://simulation.su/uploads/files/default/2016-laue-muller.pdf  
[4] - https://www.slideshare.net/dgagne/bpsim-the-technical-support-use-case  
[5] - https://www.eg.bucknell.edu/~xmeng/Course/CS6337/Note/master/node8.html
[6] - https://sparxsystems.com/enterprise_architect_user_guide/15.2/model_simulation/bpsim_examples.html

 
 

## Understanding the basic requirements of business process simulation
Before taking a deepdive into the specification itself, we should remind ourselves of some fundamentals.   

Simulation in this context is described as *play-out analysis*.

First we have a process model which describes a series of activities, gateways and possible sequence flows.  
A token is said to traverse through this process model and its trajectory is determined in gateways by use of branching conditions that evaluates some token attribute.  
The token flow through the process model is recorded as events. Events can for example describe when a token has entered some activity and when it exited. 
After the token has terminated by use of the end event, we have a series of events. These events can be combined into a single log and be order after execution.
This produces what is described as a process *trace*. By looking at the events we can see where the token was flowing throughout the process. 

Over time we can accumulate these traces into large logs. These traces can be used as a source of truth for investigating what real-world process executions look like, as opposed to the original modeled process.
Real world traces are of course only subsets of the allowed traces that the process model permits.

Business process simulation can be described in terms of *static* and *dynamic* simulation.

Provided a model M, a token T, and a state S then we can perform a static simulaton that produces events and thereby traces. However such a simulation model can only address static conditons and therefore provide little value to business processes, which are inherently dynamic.
However, they can be used as a useful tool provided that you have metrics for all required conditions. In dynamic environments we do not have these answers as they are dynamic and dependent on other variables. A typical example being resource availability.

The resource availability problem can be described as such;  

A resource is tied to one or more activities.   
If an activity is active then the resource is occupied, either fully or partially.   
The availability of some resource has a direct correlaction to the completion time of some activity.
If the resource is not available or only partially available then the activity will of course take longer to complete and we can cause blockage or queues.
These increased waiting times natually have a direct impact on the performance metrics of the proess as a whole, but they can also impact the performance of other seemingly unrelated activities.

If we suddenly get a surge of tokens that all get routed to some activity due one of their token attributes then this can lead to other neighbouring activities standing without work and therefore being underutilizied as opposed to the now overutilized activity. 
If the overutilizied activity has a resource discrepancy then it acts as a large funnel that all tokens have to flow through and slows down the overall process flow, i.e., a bottleneck. 
This can cause activities further down the process to have less work than expected.

Having these blockages lead to poor overall performance and poor efficiencies. The primary goal being to get the most amount of output for the leas amount of input. Having some bottleneck reduce the input of some activity, thus leading to it being underworked will result in poor efficiencies.

Dynamic simulation is intended to explore these exact problems. Here we can tie resources to activities. Resources themselves can of course have describing attributes such as cost. We can also describe input scenarios or *simulation profiles* that serve as input to the simulaton model.
This allows us to test specific scenarios in a dynamic model. The simulation results can then be analyzed to determine how the process performed with the given input. 


**summarizing**  
We need concepts that describe:
1. the underlying process model itself
   1. branching conditions
      1. can be dependent on some token attribute or determined via a stochastic distribution
2. resources
   1. availability
   2. cost
   3. descriptors
3. simulation profiles/scenarios
   1. distribution
      1. at what interval are they arriving and according to what distribution


## Understanding the specification
The specification is divided according to its containing elements. Here i will try to recap some the essence of these elements. For full details i reference to the official document.

### Scenario
A simulation activity can be understood as being comprised of the creation of the simulation model itself, its analysis, and the results of this analysis.
The scenario element acts as a wrapper for describing all of this. This provides a single mechanism for collecting all these techniques which are relevant to the model directly in the model itself instead of splitting this into other documents.

The specification describes this as:
```
Scenarios can be used to capture:
 a)    input parameter specification for analysis, simulation and optimization; 
 b)    results from analysis, simulation and optimization; 
 c)    historical data from past real world execution of the business process model.
```

A scenario is composed of:
1. a collection of element parameters
2. each element parameter references a specific element of a process within the process model