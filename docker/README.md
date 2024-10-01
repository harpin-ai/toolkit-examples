# Harpin AI Toolkit - Docker Image

Download our harpin AI Toolkit Docker image and try out our identity resolution algorithm on your data. Run the Docker image anywhere you have access to the data and a Docker runtime, in your cloud or on your own workstation. The harpin AI shell will walk you through the setup and configuration process and have the resolution process running in Docker in no time at all.

The image supports executing identity resolution on up to 5 million records as part of the trial. For handling more than 5 million records, please contact [Harpin AI](https://harpin.ai/demo/) for additional options.

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

3. **Start the Harpin AI Toolkit**

   In the directory where the `compose.yml` file is located, start the Harpin AI Toolkit by running the following command. Replace `<N>` with the desired number of workers.  A minimum of 2 CPU cores and 4 GB of memory is required to run identity resolution with 1 worker. Each additional worker requires 1 extra CPU core and 2 GB of memory. Increasing the number of workers can significantly reduce the time it takes to perform identity resolution.

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

5. **Enter the Harpin AI Shell**

   Once the containers are up and running, enter the Harpin AI shell to configure input data and run identity resolution.

   ```bash
   docker exec -it harpin-ai-toolkit-master-1 harpin-shell
   ```

6. **Manage Input Sources**

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
   	3. Export config to S3
   	4. Exit
   
   Enter selection [1-4]: 1
   
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
   
   Enter a path to the source data: /sample
   
   Successully loaded a sample of data from "/sample"...
   
   +--------------------+---------------+-----------+--------+----------+--------------------+--------------------+---------------+-----+-----+-------+----------+------+------+------+----------+----------+
   |                  id|     given_name|middle_name|sur_name|       dob|               email|      street_address|           city|  zip|state|country|     phone|gender|email2|phone2|ip_address|loyalty_id|
   +--------------------+---------------+-----------+--------+----------+--------------------+--------------------+---------------+-----+-----+-------+----------+------+------+------+----------+----------+
   ... Removed for brevity ...
   +--------------------+---------------+-----------+--------+----------+--------------------+--------------------+---------------+-----+-----+-------+----------+------+------+------+----------+----------+
   only showing top 20 rows
   
   
   Do you wish to add another source location? [default: no]: 
   
   Here are the recommended mappings based on keyword matches:
   
   	Source Attribute: "id",              Mapped Attribute: Source Record Id
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
   	Source Attribute: "loyalty_id",      Mapped Attribute: Source Record Id
   
   
   
   Would you like to start with and edit these mappings? [default: yes]: 
   
   Edit attribute mappings:
   
   	1.  Source Attribute: "id",              Mapped Attribute: Source Record Id
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
   	17. Source Attribute: "loyalty_id",      Mapped Attribute: Source Record Id
   	18. Done
   
   Enter selection [1-18] [default: Done]: 17
   
   Select canonical attribute for source attribute "loyalty_id":
   
   	1.  Source Record Id - An ID that uniquely identifies the record in the source system
   	2.  Source Identity Id - An ID that uniquely identifies the identity in the source system
   	3.  Source Record Timestamp - The create or update timestamp of the record in the source system
   	4.  Account Id - The customer-specific identifier for this identity in a particular loyalty program
   	5.  Linking Key1 - A customer-specific identifier used for hard linking source records
   	6.  Linking Key2 - A customer-specific identifier used for hard linking source records
   	7.  Linking Key3 - A customer-specific identifier used for hard linking source records
   	8.  Full Name - The full name associated with an identity
   	9.  First Name - The first name associated with an identity
   	10. Middle Name - The middle name associated with an identity
   	11. Last Name - The last name associated with an identity
   	12. Name Suffix - The name suffix associated with an identity
   	13. Name Primary Indicator - Flags this name as the primary name
   	14. Street Address - The street number, street, road type, apartment number, etc.
   	15. Local Municipality - If a hamlet or village appears in the address before the city
   	16. City - The city or town of the location
   	17. Governing District - The state in the United States, province in Canada, etc.
   	18. Postal Code - The zip or postal code of the location. If US zip code, first 5 digits of zip
   	19. Zip4 - If US postal code, the extended 4 digits of the zip code
   	20. Country - The country name or ISO-3166 alpha-3 code
   	21. Country Code - The two letter ISO-3166 code for the country
   	22. Address Primary Indicator - Flags this address as the primary address
   	23. Email Address - The primary email address for this identity
   	24. Email Address Primary Indicator - Flags this email address as the primary email address
   	25. Phone Number - The phone number of this identity
   	26. Phone Number Type - The type of phone number such as mobile, home, or work
   	27. Phone Number Primary Indicator - Flags if this phone number is the primary phone number
   	28. Home Phone - The home phone number for this identity
   	29. Mobile Phone - The mobile phone number for this identity
   	30. Work Ohone - The work phone number for this identity
   	31. Date Of Birth - The date of birth for this identity
   	32. Ip Address - The IP address associated with this identity
   	33. Cc Last4 - The last 4 digits of the credit card used to make the purchase
   	34. Payment Fingerprint - The payment fingerprint used in the transaction
   	35. Gender - The gender of the identity.
   	36. None
   	37. Cancel
   
   Enter selection [1-37]: 4
   
   Edit attribute mappings:
   
   	1.  Source Attribute: "id",              Mapped Attribute: Source Record Id
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
   
   Select an option:

        1. Manage sources
        2. Run identity resolution
        3. Export config to S3
        4. Exit

   ```

   The toolkit also supports accessing input files in s3 by specifying the s3 URI (e.g. s3://<bucket_name>/<key>).  If credentials are needed to access files in s3, the following environments are supported and should be set prior to starting the harpin AI toolkit containers.

      - **AWS_ACCESS_KEY_ID** 
      - **AWS_SECRET_ACCESS_KEY**
      - **AWS_SESSION_TOKEN**
      - **AWS_DEFAULT_REGION** 


8. **Run Identity Resolution**

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
   	3. Export config to S3
   	4. Exit
   
   Enter selection [1-4]: 2
   
   Saving configuration to idres_config.yml...
   
   Executing identity resolution, this may take a while...
   
   
   Loading records for source "Test" from "/sample"...
   Loaded 50000 records for source "Test" from "/sample".
   Total record count: 50000
   Finished reading input data:  2024-09-24 21:57:44+00:00
   Finished excluding bad cases:  2024-09-24 21:58:18+00:00
   Finished pre-processing input data:  2024-09-24 22:00:35+00:00
   Finished creating blocks/groups:  2024-09-24 22:10:13+00:00
   Finished distributed clustering:  2024-09-24 22:15:39+00:00
   Finished post-processing:  2024-09-24 22:16:45+00:00
   
   
   Identity Graph Summary
   ----------------------
   Total records: 50000
   Records pinned: 49102
   
   Total identities: 26496
   
   Found 36372 records with duplicates in source "Test".
   
   Identity graph written to "output/4b52fe79-67f6-49cb-81bf-bc09660c013f".
   Identity resolution completed successfully
   Duration: 1253 seconds
   ```

   The generated CSV output files stored at `output/<uuid>` (stored in the same directory as the Docker compose file or the HARPIN_DATA location if specified) contain the PIN (Profile Identification Number) to source record ID mapping. This mapping can be used to determine which source records belong to each profile.

