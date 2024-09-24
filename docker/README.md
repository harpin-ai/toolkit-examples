# Harpin AI Toolkit - Docker Image

Download our harpin AI Toolkit Docker image and try out our identity resolution algorithm on your data. Run the Docker image anywhere you have access to the data and a Docker runtime, in your cloud or on your own workstation. The harpin AI shell will walk you through the setup and configuration process and have the resolution process running in Docker in no time at all.

The image supports executing identity resolution on up to 5 million records as part of the trial. For handling more than 5 million records, please contact Harpin AI for additional options (https://harpin.ai/demo/)

## Instructions

Follow the instructions below to set up and run the Harpin AI Toolkit in a Docker environment.

### Prerequisites

1. Ensure that Docker and Docker Compose (included with Docker Desktop) are installed on the target machine.
   - [Install Docker](https://docs.docker.com/get-docker/)
   - [Install Docker Compose](https://docs.docker.com/compose/install/)
2. Ensure the machine has a minimum of 2 CPU cores and 4GB of memory available.

### Steps

1. **Download the Docker Compose File**

   Download the [Docker Compose file](compose.yml) (right-click the link and choose "Save Link As") to a directory where Harpin AI configuration will be stored, and where input files are available (if stored locally).

   ```bash
   mkdir harpin-ai-toolkit
   cd harpin-ai-toolkit
   # Download or place the compose.yml file in this directory
   ```

   This file provides Docker with the necessary instructions to download, configure, and launch the Harpin AI Toolkit, including the required services, network settings, and resource allocations. It automates the process of setting up and scaling the toolkit environment, ensuring that all components work together seamlessly.

3. **Start the Harpin AI Toolkit**

   In the directory where the `compose.yml` file is located, start the Harpin AI Toolkit by running the following command. Replace `<N>` with the desired number of workers.  A minimum of 2 CPU cores and 4 GB of memory is required to run identity resolution with 1 worker. Each additional worker requires 1 extra CPU core and 2 GB of memory. Increasing the number of workers can significantly reduce the time it takes to perform identity resolution.

   ```bash
   docker compose up -d --scale toolkit-worker=<N>
   ```

   The table below provides a breakdown of how long the identity resolution process takes, depending on the number of records and workers used:

   | Number of Records  | 1 Worker    | 2 Workers  | 3 Workers  | 4 Workers  |
   |-------------------|-------------|------------|------------|------------|
   | **100,000**   | 0.21 hours  | 0.17 hours | 0.16 hours | 0.17 hours |
   | **1,000,000 Records** | 0.87 hours  | 0.54 hours | 0.46 hours | 0.45 hours |
   | **2,500,000 Records** | 1.34 hours  | 0.84 hours | 0.75 hours | 0.62 hours |
   | **5,000,000 Records** | 2.94 hours  | 1.38 hours | 1.14 hours | 0.99 hours |

   - With **1 worker**, the process can take significantly longer, especially for larger datasets. For example, processing around 5 million records with 1 worker takes about 2.94 hours.
   - Adding more workers can drastically reduce the time. For instance, processing 5 million records with **2 workers** reduces the time to approximately 1.38 hours, while using **4 workers** drops the time to around 0.99 hours.

   By scaling the number of workers, you can achieve faster identity resolution, especially when handling larger datasets. If the dataset is not large, then adding additional workers may not have a significant impact on processing time.

5. **Enter the Harpin AI Shell**

   Once the containers are up and running, enter the Harpin AI shell to configuring input data and run identity resolution.

   ```bash
   docker exec -it harpin-ai-toolkit-master-1 harpin-shell
   ```

6. **Manage Sources and Run Identity Resolution**

   - Use the `Manage sources` option in the Harpin AI shell to specify input data and data mappings.

7. **Run Identity Resolution**

   - Use the `Run identity resolution` option to execute the identity resolution process.

