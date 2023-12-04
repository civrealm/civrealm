# CivRealm: A Learning and Reasoning Odyssey for Decision-Making Agents

[![Documentation Status](https://readthedocs.org/projects/openreview-py/badge/?version=latest)](https://civrealm.github.io/civrealm/)

CivRealm is a reinforcement learning environment for the open-source strategy game [Freeciv-web](https://github.com/freeciv/freeciv-web) based on [Freeciv](https://www.freeciv.org/). The goal is to provide a simple interface for AI researchers to train agents on Freeciv. More documentation about the environment can be found [here](https://civrealm.github.io/civrealm/).

## Prerequisites

In order to test the civrealm on <http://localhost>, please download the docker image from: https://drive.google.com/file/d/1tf32JpwqGN7AtUPe0Q4fIRkE4icSM-51/view?usp=sharing.

> :warning:
> Please make sure you have installed **the latest** docker engine and docker-compose.
> Using older versions of docker may result in unexpected errors.

## Installation

Installation for civrealm

```bash
cd civrealm
pip install -e .
```

### Testing the installation

Start the freeciv docker based on the docker-compose.yml file located in the civrealm folder:

```bash
cd civrealm
docker compose up -d
```

To test if the installation is successful, run

```bash
test_civrealm 
```

To test with multiple players, run

```bash
test_civrealm --minp=2 --username=myagent --client_port=6001
```

Then start another terminal and run:

```bash
test_civrealm --username=myagent1 --client_port=6001
```
> :warning:
> Note that to run multiple agents in the same game, you need to make them connect to the same port (specified by client_port). The available client_port range is 6001, 6300~6331.

> :warning:
> Note that when a game finishes on a port, the server on that port will take some time (around 10 seconds) to restart itself. If you start a new game on that port before the server is ready, the program will encounter expected errors and may stop/halt.

To observe the game play, you can access http://localhost:8080/ on a browser. Then, please click the "Online Games" button. You can find the running game under the "MULTIPLAYER" tab.

## Observations and actions

Observation is a tuple of the current state of the game and the action_options. Both state and action_opt are themselves dictionaries describing different aspects of the game, namely:

* city - Overview on state of each individual city (improvements, production, etc.) and actions each city can take (work, unwork tiles, produce units/improvements, etc.)
* client - Contains information on the current client communicating with the server
* dipl - Currently deprecated information on diplomacy - see player
* game - Contains current game information - mostly irrelevant for training
* gov - Overview on government state - allows to change government forms
* map - Overview on map, i.e., status (known, visible, etc.), terrain types and potential extras
* options - Overview on additional options for the game that are active
* rules - Overview on all rules and detailed/static descriptions (tech types, unit types, etc.) that are relevant for the game
* player - Overview on player status (scores, etc.) and ability to take diplomatic actions (experimental stage)
* tech - Overview on currently active technologies as well as ability to change research goals or researching specific technologies
* unit - Overview on current unit status (health, moves left, etc.) and ability for moving/activity of units


## Trouble shooting

The following are some common issues that you may encounter when running the code. If you encounter any other issues, please feel free to open an issue.

* Note that each game corresponds to one port on the server. If one game just stops and you immediately start a new game connecting to the same port as the stopped game, the new game may encounter errors and exit. Our training has used ''Ports.get()'' in ''civrealm/freeciv/utils/port_utils.py'' to avoid the situation. If you want to configure the client port and start a client manually, you need to find the available ports by checking the 'Online Games/MULTIPLAYER' tab in localhost:8080.

* If firefox keeps loading the page, please try to add the following line to `/etc/hosts`:

    ```bash
    127.0.0.1 maxcdn.bootstrapcdn.com
    127.0.0.1 cdn.webglstats.com
    ```

