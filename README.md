<div id="top"></div>
<!--
*** Thanks for checking out the Best-README-Template. If you have a suggestion
*** that would make this better, please fork the repo and create a pull request
*** or simply open an issue with the tag "enhancement".
*** Don't forget to give the project a star!
*** Thanks again! Now go create something AMAZING! :D
***
*** Keeping this for attribution -- ayushka
-->

<!-- TABLE OF CONTENTS -->
<details>
  <summary>Table of Contents</summary>
  <ol>
    <li>
      <a href="#Overview">Overview</a>
      <ul>
        <li><a href="#built-with">Built With</a></li>
      </ul>
    </li>
    <li>
      <a href="#getting-started">Getting Started</a>
      <ul>
        <li><a href="#prerequisites">Prerequisites</a></li>
        <li><a href="#setup">Setup</a></li>
      </ul>
    </li>
    <li>
      <a href="#usage">Usage</a>
    </li>
    <li><a href="#resources">Resources</a></li>
    <li><a href="#contributing">Contributing</a></li>
  </ol>
</details>

<!-- ABOUT THE PROJECT -->

## Overview

This repo hosts the pusbub emulator docker file. It is forked from [this archieved repo](https://github.com/SingularitiesCR/pubsub-emulator-docker). There are some changes made to the original dockerfile including to use the latest alpine version of google cloud sdk image as a base.

The main use for this docker image for our system is to emulate a pubsub service when running [local-debug](https://github.com/Gaji-Gesa/local-debug) on our local machine. It is not deployed to the cloud on any environment.

<p align="right">(<a href="#top">back to top</a>)</p>

### Built With

- [Docker](https://www.docker.com)
- [Google Cloud SDK Docker](https://hub.docker.com/r/google/cloud-sdk/)
- [Google PubSub](https://cloud.google.com/pubsub)

<p align="right">(<a href="#top">back to top</a>)</p>

<!-- GETTING STARTED -->

## Getting Started

To get a local copy up and running follow these simple example steps.

### Prerequisites

- Docker

  You must have docker installed on your machine. Please follow the instructions in this [Getting Started](https://docs.docker.com/get-started/) guide from docker. For mac users, pay attention to the type of processor your machine has.

### Setup

1. Clone the repo
   ```sh
   git clone git@github.com:Gaji-Gesa/pubsub-emulator-docker
   ```
2. CD into the cloned repo
   ```sh
   cd pubsub-emulator-docker
   ```
3. Build the docker image:
    - On Intel machines:
      ```shell
       docker build -t pubsub:latest ./
      ```
    - On Apple Silicon machines:
      ```shell
       docker build --platform linux/arm64 -t pubsub:latest ./
      ```
4. Running the image
   ```sh
   docker run -d -e PUBSUB_PROJECT_ID=<project_id> -e PUBSUB_LISTEN_ADDRESS=<HOST:PORT> -p <SOURCE_PORT:TARGET_PORT> pubsub:latest
   ```
   or use the provided `run_script` for default configuration
   ```sh
   ./run_script
   ```

### Using Docker Compose

The easiest way to create an emulator with this image is by using [Docker Compose](https://docs.docker.com/compose). The following snippet can be used as a `docker-compose.yml` for a Pub/Sub emulator:

```YAML
version: "2"

services:
  pubsub:
    image: pubsub:latest
    environment:
      - PUBSUB_PROJECT_ID=project-test
      - PUBSUB_LISTEN_ADDRESS=0.0.0.0:8432
    ports:
      - "8432:8432"
```
### Persistence

The image has a volume mounted at `/opt/data`. To maintain states between restarts, mount a volume at this location.

<p align="right">(<a href="#top">back to top</a>)</p>

## Usage
### Connect application with the emulator

The following environment variables need to be set on the environment of the connecting application so it connects to the emulator instead of the production Cloud Pub/Sub environment:

- `PUBSUB_EMULATOR_HOST`: The listen address used by the emulator.
- `PUBSUB_PROJECT_ID`: The ID of the Google Cloud project used by the emulator.

<p align="right">(<a href="#top">back to top</a>)</p>

## Resources

- [PubSub emulator docs](https://cloud.google.com/pubsub/docs/emulator)

<p align="right">(<a href="#top">back to top</a>)</p>

<!-- CONTRIBUTING -->

## Contributing

This repo is not deployable hence does not use our standard branching strategy. Contributions can be made by making pull requests directly to the master branch.

<p align="right">(<a href="#top">back to top</a>)</p>
