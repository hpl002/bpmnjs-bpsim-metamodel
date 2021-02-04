# readme

Creating a custom metamodel for use with the bpmn-js modeler.

## what is bpsim?

The bpSim specification is a simulation specific extension of BPMN 2.0 and addresses much needed simulation specific characteristics that are missing in the official specification.  
More info about the bpSim spec can be found here: https://www.bpsim.org/  
The bpSim has seen practical use for some time and is currently deployed in the Sparx enterprise suite, https://sparxsystems.com/  

## objective
Perform a manual translation of the bpSim specification into the bpmn-js input format. The bpmn-js modeler has wide adoption and it is thought that this metamodel would have wide appeal as it would make the modeler available as a viable option for future adopters that are seeking a graphical modeler for the configuration of simulation models.

Using BPMN to model simulation models has some issued due to its poor semantics. Howerver, these issues will not be addressed here.