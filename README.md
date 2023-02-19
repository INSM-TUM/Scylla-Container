# Scylla-container
This repository contains the components of the Scylla container.
It consists of:
- Scylla Simulator (https://github.com/bptlab/scylla)
- A data converter from PetriSim parameters to Scylla global and simulation configuration
- The API which provides an endpoint to the functionality of this container

The parameter and bpmn file from PetriSim is sent to the endpoint of the server of this container.
The server then returns the simulation output of Scylla
## Prerequisites
- clone repository
- python
- Flask==2.2.2
- Flask-RESTful==0.3.9
- node 
- xml-js (to install the xml-js package run `npm install --save xml-js` in command line)

## Run Server

  `python apiTool.py`

## Sending a http POST request

### Sample input files:
In folder _requestData_ there are sample files to simulate a request from PetriSim: 
  - BPMN-File: _pizza_1.bpmn_
  - Parameter-File: _pizza1.json_


### with Postman:
  - choose endpoint: http://127.0.0.1:8080/scyllaapi
  - define header _projectid: your_project_ID_
  - send output from PetriSim in body as _form-data_ each:
    - `bpmn=@"path-to_BPMN_file"`
    - `param=@"path-to_.json_Parameter_file"`

### the same request with curl:

curl --location 'http://127.0.0.1:8080/scyllaapi' \
--header 'projectid: your_projectID' \
--form 'bpmn=@"path-to_BPMN_file"' \
--form 'param=@"path-to_.json_Parameter_file"'  

### for example:
  
curl --location 'http://127.0.0.1:8080/scyllaapi' \
--header 'projectid: 123' \
--form 'bpmn=@"/C:/Users/user1/github/Scylla-container/requestData/pizza_1.bpmn"' \
--form 'param=@"/C:/Users/user1/github/Scylla-container/requestData/pizza1.json"'


## Returned files to Client
zipped Scylla output files

### 📦️ DockerFile
Following instructers briefly explains the steps required to start the Flask listener in a Scylla Docker container (to run separately from PetriSim):

First, build the Docker image using the Dockerfile. From this directory, in the terminal, call 'sudo docker build -t apiTool .'

This will get Linux, Java, python and all the dependencies and set up the Docker image for later use)

After the Docker image is created, use 'sudo docker run -p 8000:8000 -d apiTool'

This runs the created image and exposes port 8000 for the Flask listener.
You can use 'sudo docker ps' to see it's tag and 'sudo docker logs <container-tag>' to check what it prints. 

! The above instructions assume you compile scylla successfully, using Apache Maven.