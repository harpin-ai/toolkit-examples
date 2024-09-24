# Harpin AI Toolkit - Docker Image

Download our harpin AI Toolkit Docker image and try out our identity resolution algorithm on your data. Run the Docker image anywhere you have access to the data and a Docker runtime, in your cloud or on your own workstation. The harpin AI shell will walk you through the setup and configuration process and have the resolution process running in Docker in no time at all.

## Instructions

Follow the instructions below to set up and run the Harpin AI Toolkit in a Docker environment.

### Prerequisites

1. Ensure that Docker and Docker Compose (included with Docker Desktop) are installed on the target machine.
   - [Install Docker](https://docs.docker.com/get-docker/)
   - [Install Docker Compose](https://docs.docker.com/compose/install/)

### Steps

1. **Download the Compose File**

   Download the [compose.yml](compose.yml) file to a directory where Harpin AI configuration will be stored, and where source files are available (if stored locally).

   ```bash
   mkdir harpin-ai-toolkit
   cd harpin-ai-toolkit
   # Download or place the compose.yml file in this directory
   ```

2. **Start the Harpin AI Toolkit**

   In the directory where the `compose.yml` file is located, start the Harpin AI Toolkit by running the following command. Replace `<N>` with the desired number of workers (each worker requires 1 CPU and 2 GB of memory).

   ```bash
   docker compose up -d --scale toolkit-worker=<N>
   ```

3. **Enter the Harpin AI Shell**

   Once the containers are up and running, enter the Harpin AI shell to configuring input data and run identity resolution.

   ```bash
   docker exec -it harpin-ai-toolkit-master-1 harpin-shell
   ```

4. **Manage Sources and Run Identity Resolution**

   - Use the `Manage sources` option in the Harpin AI shell to specify input data and data mappings.

5. **Run Identity Resolution**

   - Use the `Run identity resolution` option to execute the identity resolution process.

