# carla-autoware-demo

This repository provides a vehicle remote control using Carla
simulator and Autoware. In this demo, two machines are prepared, one
running the Carla and the other running the manual controller. Both
machines are reachable to each other through Zenoh network.

## Host 1: CARLA Simulator

Run CARLA simulator.

```sh
sudo docker run \
    --privileged \
    --gpus all \
    --network=host \
    -e DISPLAY=$DISPLAY \
    carlasim/carla:0.9.13
```

Run CARLA-Autoware-Zenoh bridge.

```sh
docker run \
    --network=host \
    jerry73204/carla-autoware-bridge
```

Run Carla simulation setup script.

```sh
git clone -b 3a-demo https://github.com/jerry73204/carla_autoware_zenoh_bridge.git
cd carla_autoware_zenoh_bridge/carla_agent

poetry install
poetry run main --rolename 'v1'
```

## Host 2: Manual Controller

Run a manual vehicle controller.

```sh
docker run \
    -it \
    --network=host \
    jerry73204/autoware_manual_control
```

Run Zenoh-DDS bridge.

```sh
docker run \
    -it \
    --network=host \
    jerry73204/zenoh-bridge-dds \
     -s 'v1'
```
