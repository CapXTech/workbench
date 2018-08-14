# Workbench

Workbench is an easy way to start Peatio development environment.

## Prerequisites

- Docker [installed](https://docs.docker.com/engine/installation/)
- Docker Compose [installed](https://docs.docker.com/compose/install/)

## Usage

### Fix Docker performance

When Docker is consistently using all its assigned CPUs during build at all times, even when seemingly nothing is happening, it's best to restart Docker. 
I even reset it to factory defaults at some point. This seems to be a bug in docker. It can be mitigated by giving it a little more memory, and removing some CPUs from it to keep the system responsive.
Symptoms: Using all CPU cores (e.g. 400% when 4 CPUs are assigned); extremely slow build times.

 - Set Memory to 4GB (from 2)
 - Set number of processors to 2 if you have 4 processors or 1 if you have 2 (half)
 - Remove shared directories other than /Users /tmp and /private


### Prepare the workbench

Note: We were unable to get the latest RK release 1.9 to build reliably, wasted lots of time on that, using 1.8 turned out to work just fine. 

1. Recursive clone : `git clone --recursive https://github.com/rubykube/workbench.git`

2. Checkout 1-8-stable release : `git checkout 1-8-stable`

3.  Make sure everything is on 1.8 release : `git submodule status | cut -d' ' -f3-4`

4. Move to workbench `cd workbench`

5. Build the images: `make build`

6. Run the application: `make run`

You should add those hosts to your `/etc/hosts` file:

```
0.0.0.0 api.wb.local
0.0.0.0 auth.wb.local

0.0.0.0 api.slanger.wb.local
0.0.0.0 ws.slanger.wb.local

0.0.0.0 pma.wb.local
0.0.0.0 monitor.wb.local

0.0.0.0 btc.wb.local
0.0.0.0 eth.wb.local
```

Now you have peatio up and running.

#### Barong

Start barong server

```sh
$> docker-compose run --rm barong bash -c "./bin/link_config && ./bin/setup"
$> docker-compose up -d barong
```

This will output password for **admin@barong.io**. Default password is `Qwerty123`

#### Peatio

Start peatio server

```sh
$> docker-compose run --rm peatio bash -c "./bin/link_config && rake db:create db:migrate db:seed"
$> docker-compose up -d peatio
```

#### Frontend

Simply start your local server. Now you're able to log in with your local Barong and Peatio.

## Running Tests

Run toolbox stress tests

```sh
$> make stress
```
