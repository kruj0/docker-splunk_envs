# docker-splunk_envs

some saved docker compose environments for testing.


## Latest used docker-splunk image
![Docker Image Version (tag latest semver)](https://img.shields.io/docker/v/kruj0/splunk/latest?color=green&label=Splunk-UID:1000&style=for-the-badge)


---

## Table of Contents
1. [Purpose](#purpose)
1. [Structure](#structure)
1. [Quickstart](#quickstart)

# purpose
have a backup for windows wsl docker  testing enviroment and share it to everyone who can need it.

With VSC and my modded docker image (uid=1000) its easy to make live changes on a splunk app and push it directory from VSC to a repo you need.

# structure
my strucute is following in short:
```
mkdir splunk
mkdir splunk/apps
git clone https://github.com/kruj0/docker-splunk_envs splunk/docker-splunk_envs
```
Inside the apps folder are the splunk apps which i either clone/download from github/splunkbase or use customer defined apps where i need to work with.

it should looks like:
```
/splunk$ tree -L 3
.
├── apps
│   ├── TA-jira-service-desk-simple-addon
│   ├── Splunk_SA_CIM
│   ├── Splunk_ML_Toolkit
│   ├── customer_a
│   ├── customer_b
... ...
│   └── trackme
└── docker-splunk_envs
    ├── README.md
    ├── default.yml
    ├── .env
    ├── .secrets.env
    ├── docker-compose.yml
    ├── scenarios
    |   ├── .env
    │   ├── docker-compose.yml-2sites_2idx
    │   ├── docker-compose.yml-3sites_2idx
    │   ├── docker-compose.yml-single
    │   └── docker-compose.yml_idxc_shc
    ├── scenarios_live
    │   ├── customer_a_single.yml
    │   └── customer_b_idxc_shc.yml
    ├── scenarios_splunk_NOTTESTED
    │   ├── 1dep3sh2idx.yaml
    │   ├── 1dep3sh2idx1dmc.yaml
    ... ...
        ├── 1deployment1so.yaml
        ├── massive_absolute_unit.yaml
        └── multisite_2site2idx2sh1cm.yaml
└── splunk.lic
```
the scenarios inside the scenarios_live folder are my customer test enviroments 

# quickstart

## fast way2go
```
docker pull kruj0/splunk:latest
docker run --rm -it kruj0/splunk:latest create-defaults > default.yml
echo -e "SPLUNK_IMAGE=kruj0/splunk\nSPLUNK_START_ARGS=--accept-license\nSPLUNK_LICENSE_URI=/tmp/splunk.lic\nTZ=Europe/Berlin\nDEBUG=true" > .env
echo -e "SPLUNK_PASSWORD=SplunkP4ssw0rd!" > .secrets.env

```

## default.yml
First you need a default.yml from docker-splunk.

```
docker pull kruj0/splunk:latest
docker run --rm -it kruj0/splunk:latest create-defaults > default.yml
```

## splunk license
copy your splunk license to the root folder  `outside the repo!`

## .env files
I use a the following env strucuture: 

```
└── docker-splunk_envs
    ├── ...
    ├── .env
    ├── .secrets.env
```

### .env
file for global definitions for every container which are not defined in default.yml from splunk. 
Use the following content as an example for a .env file:
```
SPLUNK_IMAGE=kruj0/splunk
SPLUNK_START_ARGS=--accept-license
SPLUNK_LICENSE_URI=/tmp/splunk.lic
TZ=Europe/Berlin
DEBUG=true
```

### .secrets.env file
Create a .secrets.env file.

I splited the passwords into a seperate file.
Use the following content as an example for a .secrets.env file:
```
SPLUNK_PASSWORD=SplunkP4ssw0rd!
SPLUNK_IDXC_DISCOVERYPASS4SYMMKEY=F00bar1234!
SPLUNK_IDXC_PASS4SYMMKEY=F00bar1234!
SPLUNK_SHC_PASS4SYMMKEY=F00bar1234!
```


# go
copy the `docker-compose.yml` from the scenarios folder into the current folder. 

This file is for a **single search head**, which covers 90% of all test cases. 

Then:
```
docker-compose up
```

# scenarios

Use the compose files from the scenarios folder.
```
docker-compose -f scenarios/idxc_shc.yml up
docker-compose -f scenarios/2sites_i2dx.yml up
```

## docker-copose env hack
if you use a compose file from the scenarios folder, please be aware of the .env file inside the sencarios folder. Otherise the containers would load the envs from the default.yml file from splunk. which means, you will use their splunk docker image.

Beware about the .env files! 

If you need more scenarios use the the docker-splunk files. They should work, but they dont adapt to the .env files, which i use : [docker-splunk scenarios](https://github.com/kruj0/docker-splunk/tree/develop/test_scenarios)


# Links

 [Dockerhub Image](https://hub.docker.com/r/kruj0/splunk)

 [Github kruj0/docker-splunk](https://github.com/kruj0/docker-splunk)
