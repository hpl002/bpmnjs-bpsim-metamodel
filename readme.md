# readme

Translating the bpSim metamodel for use with the bpmn-js modeler.

 

## What is bpsim?

BPSim is a standard for exchanging simulation models between different modelling and simulation tools.

The bpSim specification is a simulation specific extension of BPMN 2.0 and addresses much needed simulation specific characteristics that are missing in the official specification.  
More info about the bpSim spec can be found here: https://www.bpsim.org/  
The bpSim has seen practical use for some time and is currently being used by Trisotech, Lanner, Sparx, jBPM, and more.  

**bpSim XML schema available here** https://www.bpsim.org/schemas/2.0/  


## Business process simulation - How?
Simulation is performed by creating a detailed simulation model. The model can be understood as being comprised of a series of perspectices that each focus on a specific angle.  

There are many perspectives that all describe different views of the same model. One should only use the required perspectives and not over complicate a model with excess information. Occams razor and all that 

Contorl flow: describes the model element and how they relate to each other. For example activities, archs, gateways.  
Resource perspective: describes the resource that are involved in the process amd how they are involved (what activities they are tied to).   
Performance perspective: describes the amount of time it takes to complete some activity and waht resources it requires.  

By creating specific simulation scenarios we can record process execution and perform statistical analysis to key performance indicators which then tell us about how the process performs under certain conditions. 
Testing process alterations by use of simulation can be a viable tool to test alternatives in a low risk environment. 

## The context of this spec
Creating simulation models require good insights into the real process. There is often a large gap between the modeled (ideal) process and the real process. Creating a simualtion of some process that is not representative of reality may give incorrect results or worse.
Process mininig is used as a tool to initiate a simulation model. Instead of creating one out of what we might perceive to be the real process, we can use statistical techniques to extract metrics about earlier process exetutions. This allows us to create an initial simulation model which forms the bedrock for further analysis.

## Objective
Perform a manual translation of the bpSim specification into the bpmn-js input format. The bpmn-js modeler has wide adoption and it is thought that this metamodel would have wide appeal as it would make the modeler available as a viable option for future adopters that are seeking a graphical modeler for the configuration of simulation models.

## Research
https://github.com/bpmn-io/bpmn-js-examples/tree/master/custom-meta-model  
https://www.bpsim.org/specifications/2.0/WFMC-BPSWG-2016-01.pdf  
http://simulation.su/uploads/files/default/2016-laue-muller.pdf  
https://www.slideshare.net/dgagne/bpsim-the-technical-support-use-case  
