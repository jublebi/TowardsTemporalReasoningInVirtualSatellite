# Towards Temporal Reasoning in Virtual Satellite
This repository contains data and instructions to try the temporal extension of virtual satellite. We provide two docker images and a docker-compose file to start the tool.

First, clone the repository and navigate to the it with the following commands:
```shell
git clone git@github.com:jublebi/TowardsTemporalReasoningInVirtualSatellite.git temporal_virsat
cd temporal_virsat
```

Next, the provided docker images can be loaded
```shell
docker load < virsat_temporal.tar.gz 
```

To start the tool, execute the following command
```shell
docker compose up -d
```

The tool is now available at http://localhost:8000 

The following two users are added by default to experiment collaboration between users. Both have the same access to the Demo repository
| Username | Password |
| :--- | ---: |
| admin | admin |
| user | user |

A video demonstrating our temporal extension of Virtual Satellite is available at: https://youtu.be/FOI76H2N828


