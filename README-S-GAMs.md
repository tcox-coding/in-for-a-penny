# Student Gaming, Affect and Mastery Service (S-GAMS)
### Acknowledgements: 
**Ashish Gurung**: Providing feedback, support and suggestions throughout project

**Taylor Cox**: Implementing Flask web servers, Docker setup, and [LCAD server](http://lcad.assistments.org) administration

**Morgan Lee**: Providing BKT model

**Russell Thompson**: Providing LCAD Keras Test Model

**Anthony Botelho**: Providing feedback and original ASSISTments Affect Detection Model and service.
## Description
Based on the [DataDumper](https://github.com/ASSISTments/DataDumper) project by [Anthony Botelho](https://www.wpi.edu/people/staff/abotelho), and the [pyBKT](https://github.com/CAHLR/pyBKT) project by [Badrinath, A., Wang, F., Pardos, Z.A.](https://educationaldatamining.org/EDM2021/virtual/static/pdf/EDM21_paper_237.pdf).

The Student Affect and Mastery Service is a server backend for predicting student affect and mastery. It's original goals were to be used in research at the [ASSISTments' lab at Worcester Polytecnic Institute](https://www.wpi.edu/news/assistments-wpi-created-math-learning-tool-helping-thousands-teachers-transition-distance).

## Project Structure
Under the main directory are various subfolders; each contains a different Python 3.10.0 RESTful API service using Flask. In addition, each service is containerized using Docker, with the intent to make a) development as simple as possible, b) extensible, c) able to support different versions of Python if necessary, and d) secure.

## Setup
You will need to install [Docker](https://docs.docker.com/get-docker/) and have [Docker Compose](https://docs.docker.com/get-started/08_using_compose/) installed as well. Additionally, you must have access to the ASSISTments' [DevOpsSDK](https://github.com/ASSISTments/DevOpsSDK/tree/main/src/assistments/sdk/aws/manager), and locally have [DevOpsTools](https://github.com/ASSISTments/DevOpsTools) with the permissions to connect to the ASSISTments' database.

1. Download the [DevOpsSDK](https://github.com/ASSISTments/DevOpsSDK/tree/main/src/assistments/sdk/aws/manager), and copy it to the folder: `./devops-sdk-docker`, such that the file directory looks like: `./devops-sdk-docker/DevOpsSDK`.
2. Build the container with the command `docker build -t devopssdk:latest .` in `./devops-sdk-docker`
3. Local development requires the password information of the ASSISTment's database locally in the LCAD and BKT folders, such that `./lcad/password.conf` and `./bkt/password.conf` exist and are populated with the correct information.
   * Note that the password.conf file should take the form: `<database url>:<port>:*:<connection username>:<password>`
4. Finally, build the service containers by running `docker-compose build` in the top directory.
   * Note that depending on the system, you may use `docker-compose` or `docker compose`.
5. Run `docker-compose up` to start all the services.

### Common Errors
- If any service is unqueryable on Linux, then you must make sure your Docker container can receive internet. It is solvable by fixing `/etc/resolv.conf` or `/etc/docker/daemon.json`.
- If any service gets a temporary failure in name resolution, you must restart docker with `systemctl restart docker`
- Sometimes, `localhost` doesn't work; in this case use the IP address `0.0.0.0` to send / receive requests.
- Docker and Virtual Box can sometimes interact in strange ways; the only way to fix this currently is uninstalling virtual box, restarting, and attempting to install Docker.

## Services
### LCAD (LiveChart Affect Detector)
The LiveChart / LIVE-CHART Affect Detector finds assignment actions and problem logs from the ASSISTment's database, and pulls them together to generate a large amount of preprocessed data. These are available to be used by machine learning models uploaded to the service. A user interface for testing convenience and future development is provided.

### BKT (Bayseian Knowledge Tracing)
The Bayseian Knowledge Tracing (BKT) service applies a Hidden Markov Model (HMM) to predict student mastery and next problem correctness based on automatically pulled data from the ASSISTment's database. This populates the cache database, and on request pulls the mastery information for a given student (see API documentation).

### Gaming Detector (WIP)
N/A

## Credits and Acknowledgements:
 - Badrinath, A., Wang, F., Pardos, Z.A. (2021) pyBKT: An Accessible Python Library of Bayesian Knowledge Tracing Models. In S. Hsiao, & S. Sahebi (Eds.) Proceedings of the 14th International Conference on Educational Data Mining (EDM). Pages 468-474.
