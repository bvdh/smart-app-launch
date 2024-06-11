# SMART launch extensions

A set of extensions on SMART on FHIR to allow applications to connect to multiple resource servers.

The intent of this project is to describe various options for achieving such a goal and inside discussion on them. In time the results of these discussion might be merged into SMART app launch.

## build

The project uses the HL7 IG builder. The easiest way to build the project is to run

> _startDockerPublisher.sh

call 

> _updatePublisher.sh

and then 

> _genonce.sh

This will build and validate the project. The results will be stored in a newly created `output` directory.