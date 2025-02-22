# Harpin AI Toolkit - Docker Image

Download our harpin AI Toolkit Docker image and try out our identity resolution algorithm and data quality analysis on your data. Run the Docker image anywhere you have access to the data and a Docker runtime, in your cloud or on your own workstation. The harpin AI shell will walk you through the setup and configuration process and have the resolution process running in Docker in no time at all.

The image supports executing identity resolution and data quality reports on up to 5 million records as part of the trial. For handling more than 5 million records, please contact [Harpin AI](https://harpin.ai/demo/) for additional options.

## Instructions

Follow the instructions below to set up and run the Harpin AI Toolkit in a Docker environment.

### Prerequisites

1. Ensure that Docker and Docker Compose (included with Docker Desktop) are installed on the target machine.
   - [Install Docker](https://docs.docker.com/get-docker/)
   - [Install Docker Compose](https://docs.docker.com/compose/install/)
2. Ensure the machine has a minimum of 2 CPU cores and 4GB of memory available.

### Steps

1. **Download the Docker Compose File**

   Download the [Docker Compose file](compose.yml?raw=true) (right-click the link and choose "Save Link As..." or "Download Linked File As...") to a directory where Harpin AI configuration will be stored, and where input files are available (if stored locally).

   ```bash
   mkdir harpin-ai-toolkit
   cd harpin-ai-toolkit
   # Download or place the compose.yml file in this directory
   ```

   This file provides Docker with the necessary instructions to download, configure, and launch the Harpin AI Toolkit, including the required services, network settings, and resource allocations. It automates the process of setting up and scaling the toolkit environment, ensuring that all components work together seamlessly.

   The directory containing the compose.yml is the directory where inputs and outputs to the toolkit will be stored. To use a different directory, a HARPIN_DATA environment variable can be set to the absolute path of your data's parent directory prior to starting the containers.  If running the toolkit on Linux, make sure the directory has permission to be read by the harpin container user (UID=1001).  Run `chown 1001 <directory>` to enable the containers to access the directory. 


2. **Start the Harpin AI Toolkit**

   In the directory where the `compose.yml` file is located, start the Harpin AI Toolkit by running the following command. Replace `<N>` with the desired number of workers.  A minimum of 2 CPU cores and 4 GB of memory is required to run identity resolution with 1 worker. Each additional worker requires 1 extra CPU core and 2 GB of memory. Increasing the number of workers can significantly reduce the time it takes to perform identity resolution.  Ensure the Resources defined in your Docker Settings exceed the resources required to run at the desired scale. 

   * 1 worker - 4GB memory and 2 CPU cores
   * 2 workers - 6GB memory and 3 CPU cores
   * 3 workers - 8GB memory and 4 CPU cores
   * 4 workers - 10GB memory and 5 CPU cores

   ```bash
   docker compose up -d --scale toolkit-worker=<N>
   ```

   The table below provides a breakdown of how long approximately the identity resolution process can take, depending on the number of records and workers used:

   | Number of Records  | 1 Worker    | 2 Workers  | 3 Workers  | 4 Workers  |
   |-------------------|-------------|------------|------------|------------|
   | 100,000 Records   | 0.21 hours  | 0.17 hours | 0.16 hours | 0.17 hours |
   | 1,000,000 Records | 0.87 hours  | 0.54 hours | 0.46 hours | 0.45 hours |
   | 2,500,000 Records | 1.34 hours  | 0.84 hours | 0.75 hours | 0.62 hours |
   | 5,000,000 Records | 2.94 hours  | 1.38 hours | 1.14 hours | 0.99 hours |

   - With **1 worker**, the process can take significantly longer, especially for larger datasets. For example, processing around 5 million records with 1 worker takes about 2.94 hours.
   - Adding more workers can drastically reduce the time. For instance, processing 5 million records with **2 workers** reduces the time to approximately 1.38 hours, while using **4 workers** drops the time to around 0.99 hours.

   By scaling the number of workers, you can achieve faster identity resolution, especially when handling larger datasets. If the dataset is not large, then adding additional workers may not have a significant impact on processing time.

3. **Enter the Harpin AI Shell**

   Once the containers are up and running, enter the Harpin AI shell to configure input data and run identity resolution.

   ```bash
   docker exec -it harpin-ai-toolkit-master-1 harpin-shell
   ```

4. **Manage Input Sources**

   Use the `Manage sources` option in the Harpin AI shell to specify input data locations and data mappings.

   The location of input data is relative to the directory where the Docker compose file is located.  To use a different directory, a HARPIN_DATA environment variable can be set to the absolute path of your data's parent directory prior to starting the containers.

   A sample data file with 50,000 fake identity records is available at `/sample` in the container.  The example below demostrates using this file.

   ```
    _                      _            _    ___ 
   | |__   __ _ _ __ _ __ (_)_ __      / \  |_ _|
   | '_ \ / _\ | '__| '_ \| | '_ \    / _ \  | | 
   | | | | (_| | |  | |_) | | | | |  / ___ \ | | 
   |_| |_|\__,_|_|  | .__/|_|_| |_| /_/   \_\___|
                    |_|                          
                                                

   Select an option:

      1. Manage sources
      2. Run identity resolution
      3. Get data quality report
      4. Export config to S3
      5. Exit

   Enter selection [1-5]: 1

   Select an option:

      1. Add source
      2. Update source
      3. Delete source
      4. Show sources
      5. Back to main menu

   Enter selection [1-5]: 1

   Enter a name for the source: Test

   Enter an ID for the source [default: test]: 

   Select the source data format:

      1. csv
      2. avro
      3. parquet

   Enter selection [1-3]: 1

   Enter csv delimiter character [default: ,]: 

   Enter csv quote character [default: "]: 

   Enter csv escape character [default: \]: 

   Enter a path to the source data: /sample

   Loading sample of data from "/sample"...


   Successully loaded a sample of 100 records from "/sample".

   +--------------------+----------+-----------+---------+----------+--------------------+------------------+----------------+-----+-----+-------+----------+------+------+------+----------+----------+
   |           record_id|given_name|middle_name| sur_name|       dob|               email|    street_address|            city|  zip|state|country|     phone|gender|email2|phone2|ip_address|loyalty_id|
   +--------------------+----------+-----------+---------+----------+--------------------+------------------+----------------+-----+-----+-------+----------+------+------+------+----------+----------+
   <Excluded for brevity>                                                                                                                                                         
   +--------------------+----------+-----------+---------+----------+--------------------+------------------+----------------+-----+-----+-------+----------+------+------+------+----------+----------+
   only showing top 20 rows


   Do you wish to add another source location? [default: no]: 

   Extracting source attribute names and example values...

   Here are the recommended mappings based on keyword matches:

      Source Attribute: "record_id",       Mapped Attribute: Source Record Id
      Source Attribute: "given_name",      Mapped Attribute: First Name
      Source Attribute: "middle_name",     Mapped Attribute: Middle Name
      Source Attribute: "sur_name",        Mapped Attribute: Last Name
      Source Attribute: "dob",             Mapped Attribute: Date Of Birth
      Source Attribute: "email",           Mapped Attribute: Email Address
      Source Attribute: "street_address",  Mapped Attribute: Street Address
      Source Attribute: "city",            Mapped Attribute: City
      Source Attribute: "zip",             Mapped Attribute: Postal Code
      Source Attribute: "state",           Mapped Attribute: Governing District
      Source Attribute: "country",         Mapped Attribute: Country
      Source Attribute: "phone",           Mapped Attribute: Phone Number
      Source Attribute: "gender",          Mapped Attribute: Gender
      Source Attribute: "email2",          Mapped Attribute: Email Address
      Source Attribute: "phone2",          Mapped Attribute: Phone Number
      Source Attribute: "ip_address",      Mapped Attribute: Ip Address
      Source Attribute: "loyalty_id",      Mapped Attribute: Account Id



   Would you like to start with and edit these mappings? [default: yes]: 

   Edit attribute mappings:

      1.  Source Attribute: "record_id",       Mapped Attribute: Source Record Id
      2.  Source Attribute: "given_name",      Mapped Attribute: First Name
      3.  Source Attribute: "middle_name",     Mapped Attribute: Middle Name
      4.  Source Attribute: "sur_name",        Mapped Attribute: Last Name
      5.  Source Attribute: "dob",             Mapped Attribute: Date Of Birth
      6.  Source Attribute: "email",           Mapped Attribute: Email Address
      7.  Source Attribute: "street_address",  Mapped Attribute: Street Address
      8.  Source Attribute: "city",            Mapped Attribute: City
      9.  Source Attribute: "zip",             Mapped Attribute: Postal Code
      10. Source Attribute: "state",           Mapped Attribute: Governing District
      11. Source Attribute: "country",         Mapped Attribute: Country
      12. Source Attribute: "phone",           Mapped Attribute: Phone Number
      13. Source Attribute: "gender",          Mapped Attribute: Gender
      14. Source Attribute: "email2",          Mapped Attribute: Email Address
      15. Source Attribute: "phone2",          Mapped Attribute: Phone Number
      16. Source Attribute: "ip_address",      Mapped Attribute: Ip Address
      17. Source Attribute: "loyalty_id",      Mapped Attribute: Account Id
      18. Done

   Enter selection [1-18] [default: Done]: 

   Edit attribute order:

      1. Mapped Attribute: "emailAddress", Source Attribute Order: ["email", "email2"]
      2. Mapped Attribute: "phoneNumber", Source Attribute Order: ["phone", "phone2"]
      3. Done

   Enter selection [1-3] [default: Done]: 

   How many records are expected per individual?

      1. Single
      2. Multiple

   Enter selection [1-2]: 1

   Saving source configuration to .harpin/sources/test.yml...


   Select an option:

      1. Add source
      2. Update source
      3. Delete source
      4. Show sources
      5. Back to main menu

   Enter selection [1-5]: 5
   ```

   Configured sources are persisted across executions in a directory named ".harpin" located in the same directory as the Docker compose file or the HARPIN_DATA location if specified.

   The toolkit also supports accessing input files in s3 by specifying the s3 URI (e.g. s3://<bucket_name>/<key>).  If credentials are needed to access files in s3, the following environment variables are supported and should be set prior to starting the harpin AI toolkit containers.

      - **AWS_ACCESS_KEY_ID** 
      - **AWS_SECRET_ACCESS_KEY**
      - **AWS_SESSION_TOKEN**
      - **AWS_DEFAULT_REGION** 


5. **Run Identity Resolution**

   Use the `Run identity resolution` option to execute the identity resolution process.

   Here is an example:

   ```
    _                      _            _    ___ 
   | |__   __ _ _ __ _ __ (_)_ __      / \  |_ _|
   | '_ \ / _\ | '__| '_ \| | '_ \    / _ \  | | 
   | | | | (_| | |  | |_) | | | | |  / ___ \ | | 
   |_| |_|\__,_|_|  | .__/|_|_| |_| /_/   \_\___|
                    |_|                          
                                                

   Select an option:

      1. Manage sources
      2. Run identity resolution
      3. Get data quality report
      4. Export config to S3
      5. Exit

   Enter selection [1-5]: 2

   Saving configuration to idres_config.yml...

   Executing identity resolution, this may take a while...


   Loading records for source "Test" from "/sample"...
   Loaded 50000 records for source "Test" from "/sample".
   Total record count: 50000
   Finished reading input data.
   Finished excluding bad cases.
   Finished pre-processing input data.
   Finished creating blocks/groups.
   Finished distributed clustering.
   Finished post-processing.


   Identity Graph Summary
   ----------------------
   Total records: 50000
   Records pinned: 49162

   Total identities: 25939

   Found 37247 records with duplicates in source "Test".

   Identity graph written to "output/9899a4ea-eafe-4362-9fa7-0e91e439bf9e".
   Identity resolution completed successfully
   Duration: 1491 seconds
   ```

   The generated CSV output files stored at `output/<uuid>` (stored in the same directory as the Docker compose file or the HARPIN_DATA location if specified) contain the PIN (Profile Identification Number) to source record ID mapping. This mapping can be used to determine which source records belong to each profile.

   If Identity Resolution fails to complete, check the Resources section in Docker Settings to ensure enough resources are available for your desired scale.


6. **Run Data Quality Report**

   Use the `Run data quality report` option to analyze the quality of your identity data.

   Here is an example:

   ```
    _                      _            _    ___ 
   | |__   __ _ _ __ _ __ (_)_ __      / \  |_ _|
   | '_ \ / _\ | '__| '_ \| | '_ \    / _ \  | | 
   | | | | (_| | |  | |_) | | | | |  / ___ \ | | 
   |_| |_|\__,_|_|  | .__/|_|_| |_| /_/   \_\___|
                    |_|                          
                                                

   Select an option:

      1. Manage sources
      2. Run identity resolution
      3. Get data quality report
      4. Export config to S3
      5. Exit

   Enter selection [1-5]: 3

   Select a source to get data quality report:

      1. Test

   Enter selection [1] [default: cancel]: 1


   Running data quality report for source "Test"...
   Loading records for source "Test" from "/sample"...
   Loaded 50000 records for source "Test" from "/sample".
   Running data quality analysis...


   Data Quality Report
   -------------------
   24910  Records with an invalid phone number
    6357  Records with an invalid email address
    4679  Records with unrecognized names
    2917  Records with some portion of the first or last name repeated in both fields
    1970  Records with placeholder consumer data
    1424  Records with suffix in a name field
    1269  Records with date of birth detected in the remote past or future
    1099  Records with common misspelling in an email address domain
     490  Records with same first and last names values
     287  Records with both first and last name detected in a single field
      31  Records containing only an email address



   Here is a record sample for data quality issue


   Records with an invalid phone number
   +--------+---------------------+-----------+---------------+
   |sourceId|sourceRecordId       |phoneNumber|phoneNumberType|
   +--------+---------------------+-----------+---------------+
   |  <Excluded for brevity>                                  |
   +--------+---------------------+-----------+---------------+
   only showing top 20 rows

   Full results written to output/dataQuality/test/phoneInvalid


   Records with an invalid email address
   +--------+---------------------+--------------------------------------+
   |sourceId|sourceRecordId       |emailAddress                          |
   +--------+---------------------+--------------------------------------+
   |  <Excluded for brevity>                                             |
   +--------+---------------------+--------------------------------------+
   only showing top 20 rows

   Full results written to output/dataQuality/test/emailInvalid


   Records with unrecognized names
   +--------+---------------------+-----------------------+--------------+----------+----------+-------------+----------+
   |sourceId|sourceRecordId       |firstName              |firstNameScore|middleName|lastName  |lastNameScore|nameSuffix|
   +--------+---------------------+-----------------------+--------------+----------+----------+-------------+----------+
   |  <Excluded for brevity>                                                                                            |
   +--------+---------------------+-----------------------+--------------+----------+----------+-------------+----------+
   only showing top 20 rows

   Full results written to output/dataQuality/test/nameUnrecognized


   Records with some portion of the first or last name repeated in both fields
   +--------+---------------------+----------------+--------------+----------+----------------+-------------+----------+
   |sourceId|sourceRecordId       |firstName       |firstNameScore|middleName|lastName        |lastNameScore|nameSuffix|
   +--------+---------------------+----------------+--------------+----------+----------------+-------------+----------+
   |  <Excluded for brevity>                                                                                           |
   +--------+---------------------+----------------+--------------+----------+----------------+-------------+----------+
   only showing top 20 rows

   Full results written to output/dataQuality/test/nameIsRepeated


   Records with placeholder consumer data
   +--------+---------------------+---------+--------------+----------+----------+-------------+----------+
   |sourceId|sourceRecordId       |firstName|firstNameScore|middleName|lastName  |lastNameScore|nameSuffix|
   +--------+---------------------+---------+--------------+----------+----------+-------------+----------+
   |  <Excluded for brevity>                                                                              |
   +--------+---------------------+---------+--------------+----------+----------+-------------+----------+
   only showing top 20 rows

   Full results written to output/dataQuality/test/placeholderData


   Records with suffix in a name field
   +--------+---------------------+---------+--------------+----------+------------+-------------+----------+
   |sourceId|sourceRecordId       |firstName|firstNameScore|middleName|lastName    |lastNameScore|nameSuffix|
   +--------+---------------------+---------+--------------+----------+------------+-------------+----------+
   |  <Excluded for brevity>                                                                                |
   +--------+---------------------+---------+--------------+----------+------------+-------------+----------+
   only showing top 20 rows

   Full results written to output/dataQuality/test/suffixInName


   Records with date of birth detected in the remote past or future
   +--------+---------------------+-----------+
   |sourceId|sourceRecordId       |dateOfBirth|
   +--------+---------------------+-----------+
   |  <Excluded for brevity>                  |
   +--------+---------------------+-----------+
   only showing top 20 rows

   Full results written to output/dataQuality/test/dateOfBirthInvalid


   Records with common misspelling in an email address domain
   +--------+---------------------+------------------------------+
   |sourceId|sourceRecordId       |emailAddress                  |
   +--------+---------------------+------------------------------+
   |  <Excluded for brevity>                                     |
   +--------+---------------------+------------------------------+
   only showing top 20 rows

   Full results written to output/dataQuality/test/emailDomainMisspelled


   Records with same first and last names values
   +--------+---------------------+-------------------+--------------+----------+-------------------+-------------+----------+
   |sourceId|sourceRecordId       |firstName          |firstNameScore|middleName|lastName           |lastNameScore|nameSuffix|
   +--------+---------------------+-------------------+--------------+----------+-------------------+-------------+----------+
   |  <Excluded for brevity>                                                                                                 |
   +--------+---------------------+-------------------+--------------+----------+-------------------+-------------+----------+
   only showing top 20 rows

   Full results written to output/dataQuality/test/firstNameLastNameEqual


   Records with both first and last name detected in a single field
   +--------+---------------------+----------------+--------------+-----------+---------------+-------------+----------+
   |sourceId|sourceRecordId       |firstName       |firstNameScore|middleName |lastName       |lastNameScore|nameSuffix|
   +--------+---------------------+----------------+--------------+-----------+---------------+-------------+----------+
   |  <Excluded for brevity>                                                                                           |
   +--------+---------------------+----------------+--------------+-----------+---------------+-------------+----------+
   only showing top 20 rows

   Full results written to output/dataQuality/test/multipleNames


   Records containing only an email address
   +--------+---------------------+---------------------------------+
   |sourceId|sourceRecordId       |emailAddress                     |
   +--------+---------------------+---------------------------------+
   |  <Excluded for brevity>                                        |
   +--------+---------------------+---------------------------------+
   only showing top 20 rows

   Full results written to output/dataQuality/test/emailOnly
   ```
