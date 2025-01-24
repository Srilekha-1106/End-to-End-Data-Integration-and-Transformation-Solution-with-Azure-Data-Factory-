# End-to-End-Data-Integration-and-Transformation-Solution-with-Azure-Data-Factory-
Designed and implemented an end-to-end data integration solution using Azure Data Factory (ADF). Ingested data from HTTP and Azure Blob Storage into Azure Data Lake Gen2, transformed it using Data Flows, HDInsight, and Databricks, and stored transformed data in Azure Data Lake Gen2 and Azure SQL Database. Visualized insights with Power BI.
![image](https://github.com/user-attachments/assets/ff663fca-752a-465c-a2bb-a562e114a7b2)
                                             
## Resource Creation
1.	Create a Resource Group in Azure.
![image](https://github.com/user-attachments/assets/ec9df0c6-b164-489b-b959-56955f72e596)
3.	Deploy an Azure Data Factory service.
![image](https://github.com/user-attachments/assets/79671099-9f4d-45a1-b76f-0f6d13f83523)

4.	Provision two storage accounts:
Blob Storage Account.
Azure Data Lake Storage Gen2 (ADLS2).
![image](https://github.com/user-attachments/assets/aef13da1-ea2e-4971-ab89-3a20c0525e1a)
![image](https://github.com/user-attachments/assets/b838e1d2-b73f-4a60-b064-b70202a61fe9)

 
## Data Setup
4.	Create an Azure SQL Database
 ![image](https://github.com/user-attachments/assets/e3d338d2-6160-4f3d-9acd-30192c2ba7b0)

5.	Upload the population_by_age.tsv file into a container named population in the Blob Storage account.
![image](https://github.com/user-attachments/assets/09753c02-73b3-48cd-99b6-4ea67b16aa0c)

6.	Create a raw container in adls2 storage account for data ingestion.
![image](https://github.com/user-attachments/assets/164dd91f-da19-4112-887d-f735ed2b4c6e)
 
## Linked Services and Datasets
7.	Create Linked Services for Blob Storage and ADLS2.
![image](https://github.com/user-attachments/assets/4fd618e5-4860-432f-ae2c-8a36d98ad6e9)

8.	Define Datasets for:
        Blob Storage (source container).
        Data Lake (destination container)
![image](https://github.com/user-attachments/assets/3043f21e-c384-4d7f-96f8-2cf3fcf0dada)

## Pipeline Development
9.	Build a pipeline to copy data from Blob Storage to the Data Lake.
![image](https://github.com/user-attachments/assets/7c0ed36d-a92e-49ac-94eb-fd22300505f2)

### Data Validation: 
10.	Ensure data validation before initiating the copy process, verifying that the source file contains data to be transferred to the destination.
![image](https://github.com/user-attachments/assets/097dff62-7f47-4696-b8ac-7730123daaf4)

a)	Use Get Metadata to verify the source file.
<br>
b)	Validate column count, file existence, and size before copying.
 ![image](https://github.com/user-attachments/assets/d4a90ed3-c7eb-427b-abeb-a1a240c2db3d)
c)	Configure an If Condition to:
 ![image](https://github.com/user-attachments/assets/c264fa0d-4a2d-45da-a56b-fe33bcb27942)
Proceed with the copy if the column count matches.
<br>
Log or display an error if the condition fails.
<br>
d)	If the condition is true then the data is copied
<br>
e)	If it is false it pops up the message written in there.
 ![image](https://github.com/user-attachments/assets/f02f86cc-b3ae-4f6e-87e8-85ce0f994682)

f)	The pipeline ran successfully
![image](https://github.com/user-attachments/assets/3d8d21ae-3738-4cdf-81d5-20250823dad5)

11.	Delete the source file after successful copying.
![image](https://github.com/user-attachments/assets/83a82559-c909-4c32-b58e-30b0f051ac02)

## Event Driven Automation
12.	Instead of manually running the pipeline, configure an event trigger that activates only when data is available in the data source. Ensure that Event Grid is registered in your subscription before creating the trigger.
![image](https://github.com/user-attachments/assets/0a79d8e4-1ac9-4651-ac9e-e7d2f7090ae8)

13.	Create a trigger by specifying the file name and selecting the option to execute the action when a blob is created.
![image](https://github.com/user-attachments/assets/6b0a0957-b095-47b0-9bb2-895c623a9192)

14.	So add the trigger to the pipeline
![image](https://github.com/user-attachments/assets/7329c38e-815e-48db-89b5-7296b754ba9e)

15.	Now, try adding a new file in the source container and within few seconds you can observe the trigger has been succeeded running.
![image](https://github.com/user-attachments/assets/dc200d0a-365b-468c-a7ca-ce56fd23be1e)

16.	The trigger has run the pipeline and each step successfully.
![image](https://github.com/user-attachments/assets/2cbafec1-443b-4e0f-85a7-ee299166ab0e)

## Ingest Data from Web
17.	Next, ingest data from the web. Unlike the previous step, the data is not stored in any storage account and requires an HTTP link for ingestion. 
18.	Create a linked service by selecting HTTP as the data store and specifying the base URL. There is no need to create another linked service for the destination, as it was already configured earlier for the Data Lake.
![image](https://github.com/user-attachments/assets/2be628cb-7c18-4cf7-af45-dfa1d4d10d55)

19.	Create datasets for both the HTTP source data and the Data Lake destination data. When creating these datasets, specify the type of data source and data format, and ensure that the previously created linked service is associated with the dataset. Datasets store data and are used in pipelines to facilitate copying into storage accounts. Additionally, provide the storage account location in the dataset so that the pipeline knows where to store the data. For the HTTP dataset, include the relative URL to specify the data's access location. 
![image](https://github.com/user-attachments/assets/aaaf2ae2-6d74-4e7e-af79-08118be57d9c)

20.	In destination dataset we should also mention the container name, folder name and file name so that it gets stored accordingly.
![image](https://github.com/user-attachments/assets/89b279ff-941a-4a39-9f7d-ca89878de499)

21.	Create a pipeline and use the copy data widget and enter source as http dataset and destination as data lake dataset.
![image](https://github.com/user-attachments/assets/6838c1e9-b423-493c-a02a-bd7c95c47cc8)

22.	The pipeline runs successfully.
![image](https://github.com/user-attachments/assets/7657561a-bfd8-4e4a-b8c1-8ce088aad3db)
![image](https://github.com/user-attachments/assets/0c5c2ff9-b91b-4f9f-9d48-e1dd57e0710c)
 
Dynamic File Handling
23.	Following the procedure outlined above, copying data between storage accounts for a single file is straightforward. However, managing thousands of files manually by creating individual linked services, datasets, and pipelines is impractical. To handle this efficiently, parameters can be used in the linked service, dataset, and pipeline. These parameters allow dynamic value assignment when the pipeline is run, eliminating the need to specify each file name beforehand. Utilize the **Lookup** activity to read a JSON file containing all file paths. Then, add a **ForEach** activity to iterate through each file path and execute a **Copy Data** activity within its action field to transfer the data. This approach automates the process for multiple files.
24.	So this is how the paths look like
![image](https://github.com/user-attachments/assets/7278d380-0c70-455d-8d37-56f49407c923)

sourceBaseURL, sourceRelativeURL are the parts in http link and sinkFileName is the name of the sink file i.e., name of the file how we want to store it in the delta lake. 
25.	Create linked service for http data. Parameters must be created in both the linked service and datasets to enable their use in dynamic content. 
![image](https://github.com/user-attachments/assets/968f0057-6e88-4630-9949-99866ffad420)

In the linked service created, add baseURL as parameter.
![image](https://github.com/user-attachments/assets/cf9f8176-9729-40a0-b17b-14a159424d06)


Click add dynamic content and select the parameter created - baseURL
![image](https://github.com/user-attachments/assets/eb890bfe-2fa9-4ba9-9634-bc02b805e3ad)
![image](https://github.com/user-attachments/assets/d3891bd4-9540-4af2-b0a2-e287c5322ae7)

 
26.	Create a dataset similar to the previous one by selecting HTTP as the data store and choosing CSV as the data format. Attach the linked service created for http data.
![image](https://github.com/user-attachments/assets/46ce1444-c308-46d6-8b88-d22be4d06d0f)

27.	Notice that it prompts for a Relative URL during configuration. To proceed, create two parameters: one for the Base URL and another for the Relative URL. This setup is essential to access and use these parameters in the dynamic content editor.
![image](https://github.com/user-attachments/assets/0971c85c-e4ef-409b-ba2b-903c8f2a9cf5)

28.	Add the parameters
![image](https://github.com/user-attachments/assets/61ab9ca2-ad8f-4529-9e34-db20eaec523e)

29.	Add the parameters for Relative URL and Base URL using the dynamic content editor.
![image](https://github.com/user-attachments/assets/2d9f783d-3dd1-437c-a1af-efa2e225bfec)

30.	Similarly, create a linked service and a dataset for the destination, which is the Data Lake. In the dataset, select **Azure Data Lake Storage Gen2** as the data store and **CSV** as the data format. There is no need to define parameters for the linked service's storage account, as it is already specified for a single storage account.
![image](https://github.com/user-attachments/assets/86ebdf45-c63e-4e71-88a1-2c3becce8eb7)

Similarly, configure a linked service and dataset for the destination, which is the Data Lake. Select **Azure Data Lake Storage Gen2** as the data store and **CSV** as the data format for the dataset. Parameters for the linked service's storage account are unnecessary since the configuration already specifies a single storage account.
![image](https://github.com/user-attachments/assets/b50ebe64-c6f4-4f25-9225-cacbc40c1e73)

Now add this to filename using dynamic content
![image](https://github.com/user-attachments/assets/cd0398ca-0e91-4ba8-98a8-80a2087d3e54)

31.	Now, load the **ecdc JSON** file containing all the file paths. Use the same procedure to create a linked service for the other storage account where the file is stored, and then create a corresponding dataset for it.
![image](https://github.com/user-attachments/assets/33c15d1c-88f2-48e5-a4ad-e9bf344c91ac)

Select azure blob storage in dataset, csv in data format
32.	Create a dataset for the ecdc file.
![image](https://github.com/user-attachments/assets/b021349f-9a9b-416f-b2a2-ab214f11f053)

Attach the linked service associated with it, select the location of the file where it exists. With all this in hand we are set to create a pipeline. So now linked services and datasets are done, it is time to work with pipelines.
The lookup retrieves all the data In ecdc json file
![image](https://github.com/user-attachments/assets/29bb80a8-f234-4a85-a02f-88a44f85361e)

33.	Add a **ForEach** activity next to the **Lookup** activity so that it iterates through each item retrieved by the lookup.
![image](https://github.com/user-attachments/assets/d6c295dd-dbbe-4d4c-94e4-9350c965e9e4)
   
a)	In the Items section of the ForEach activity, provide the value by adding dynamic content. 
![image](https://github.com/user-attachments/assets/96808361-3721-4a4d-a82d-410aaaf86e23)
 
b)	This approach allows retrieval of all values from the paths. These values should then be utilized in a **Copy Data** activity, where the URLs are applied as the source, and the sinkFilePath` is used as the destination in the sink.
![image](https://github.com/user-attachments/assets/574b6961-b47d-4730-8bcc-d1aba1b3a3c6)

c)	Add a Copy Data activity inside the ForEach activity. When you select the source as http_dataset, the parameters created for the dataset will automatically appear for configuration.
![image](https://github.com/user-attachments/assets/f8112f1c-b506-4774-be61-fd2883c83cbd)

d)	Using the **Add Dynamic Content** option, insert the values retrieved from the **ForEach** activity to dynamically configure the parameters.
![image](https://github.com/user-attachments/assets/10d2c477-13ab-4db6-b55f-d704a7520856)
 
e)	Include `.sourceBaseURL` and `.sourceRelativeURL` in the dynamic content, as specified in the **ecdc** file. For example, `item.sourceBaseURL` retrieves the base URL for each specific item. 
f)	For the sink, select the `datalake_dataset`, which was configured with a parameter for the filename. This allows dynamic assignment of filenames during the pipeline execution. So as soon as we select the dataset it pops the file name parameter.
![image](https://github.com/user-attachments/assets/92840ffb-a921-445f-8317-fad23cc38536)
 
g)	In the **ForEach** activity, use the **Add Dynamic Content** option to select the current item and append `.sinkFileName` to dynamically assign the filename for the sink.
![image](https://github.com/user-attachments/assets/278fd122-8612-4108-ac90-2544b7521370)
![image](https://github.com/user-attachments/assets/5fa2c1c4-7152-4e1b-91e8-fd847f51bc56)

 
34.	Now, debug the pipeline to ensure that all the data from the specified HTTP paths is successfully copied into the Data Lake
![image](https://github.com/user-attachments/assets/52a48bb5-1f47-42a3-a775-3ea3420277ce)

35.	As a result, the **ecdc** folder is created in the Data Lake, and all the files are successfully copied into it.
![image](https://github.com/user-attachments/assets/cf97a336-d8b0-4ab3-ac09-4478c015b74c)

36.	Add a scheduled trigger to the pipeline to enable automatic execution, eliminating the need for manual debugging each time.
![image](https://github.com/user-attachments/assets/f41bb3a4-6708-4c00-bc40-007bd4e9149d)
 
Data factory submits these data flows to a data bricks cluster and they run a spark job.37.
37.	In short, sample data in debug settings is used for quickly testing and debugging individual activities in Azure Data Factory pipelines before running them on full datasets. So we use a sample data for our testing purposes instead of using the whole dataset. Which is cases_deaths_uk_india_only.csv where it contains only few rows easy for testing.
![image](https://github.com/user-attachments/assets/b7052c42-6d2c-4e61-8359-d4488e1ed6a0)

Data Transformation
38.	Now its time to transform our data. Data transformation can be achieved in three ways:
Using Data Flows
Step 1: Create a dataset for the cases_and_deaths file and configure it as the source. Enable schema drift to accommodate any additional columns beyond the existing ones.
![image](https://github.com/user-attachments/assets/3c4be1c4-b02b-4337-a8a5-eebdae4fb2af)

a)	Select detect data type so that the data types will be inferred automatically.
![image](https://github.com/user-attachments/assets/6be0573a-a02b-434e-ab3b-b9715a6826cf)

b)	When you click inspect you get the details about the data
![image](https://github.com/user-attachments/assets/497b22d5-363f-4744-82c0-fde4d6361ce1)

c)	When you click preview data then the data which is retrieved from the source will be displayed.
![image](https://github.com/user-attachments/assets/e21e718a-6bd0-4d8e-b2db-b254438a9ad4)

Step 2: In the Filter Transformation, filter the data to include only rows where the continent is "Europe" and country_code is not null. 
![image](https://github.com/user-attachments/assets/5730eee3-9cc3-451f-bcc2-e06d39a49f06)

Step 3: Select only the necessary columns and remove any unwanted columns by clicking the **delete** button next to each column.
![image](https://github.com/user-attachments/assets/0846900b-70ac-4083-a5bb-98f4c1ead19a)


Step 4: Use the Pivot Transformation on the indicator column, which contains two values: deaths and confirmed cases. These row values will be transformed into separate columns, with their values aggregated using the sum(daily_count) function.
a)	Include all the columns in group by exclude the ones used for pivot
![image](https://github.com/user-attachments/assets/ef8faa0c-efb4-4e73-896f-fb722ab48146)

b)	Add pivot settings
![image](https://github.com/user-attachments/assets/c9078bc4-ad8f-48fc-924d-e34b19500d3b)

c)	Specify the name and the aggregation function for the pivot transformation.
![image](https://github.com/user-attachments/assets/4ecc5826-f277-4e53-804e-2f3fefc0376b)

Step 5: Our table contains a 3-digit country code, but not the 2-digit country code. To address this, use a lookup file that includes the 2-digit code and join it with the existing data. Configure the lookup file as a source dataset.
![image](https://github.com/user-attachments/assets/8d3c13b1-dcee-4b0f-a624-c918f4fe792a)

In projection change the data type of population to integer
Step 6: Add a **Lookup** activity next to the pivot transformation and configure its stream to use the lookup file. Perform a left outer join during the process, and remove any redundant columns later using the **Select** transformation.
![image](https://github.com/user-attachments/assets/08b64afb-a33f-4827-8ba7-3d275202ff97)

a)	In the data flow, you have the option to enable data broadcasting. You can allow the data flow to decide automatically or choose to broadcast manually. Broadcasting involves processing the data on a Spark cluster, which operates on a distributed computing framework. If broadcasting is set to fixed or decided automatically by the data flow, the entire dataset is sent to every node in the cluster. For optimal performance, ensure that the nodes have sufficient memory—memory-optimized nodes are ideal for holding the lookup data entirely on each node. Generally, it is recommended to leave the setting on auto for efficient handling. Let's examine it further.
![image](https://github.com/user-attachments/assets/cef3b331-a3a8-4bc6-a441-01d151bb11dc)

Step 7: Add a **Select** transformation again to rename columns as needed and remove any unwanted columns from the join output.
![image](https://github.com/user-attachments/assets/49e6c55d-d76e-489b-a419-5fed0e0a906e)
 
a)	Modify the columns as required.
![image](https://github.com/user-attachments/assets/1b552ccd-1f25-40ec-9d8e-34c6bbcd14af)

Step 8: Add a **Sink** transformation to copy the transformed data to the destination file. Create a dataset pointing to the destination file and configure it in the sink.  
![image](https://github.com/user-attachments/assets/bb241b54-cca2-4649-a4c8-f71dd08a4023)

a)	You can enable the **Output to Single File** option to consolidate all the data into a single file instead of distributing it across nodes. However, this approach may take more time to process.
![image](https://github.com/user-attachments/assets/0aa3d5b4-5d8c-499f-be39-b8e80bcb94f4)

Step 9: At last, you can add a pipeline and run it 
![image](https://github.com/user-attachments/assets/e79c08f4-6821-43d4-b7e2-ff865a3fdfae)

When manually triggering the pipeline, you may see a status message indicating **"acquiring compute."** This refers to the initialization of a Spark cluster to process the data flow activities. The process typically takes around 5 to 6 minutes. Once the cluster is ready, it begins executing the pipeline. Spark uses a distributed computing architecture, distributing workloads across multiple nodes within the cluster to efficiently process large datasets.
         You can add trigger now option without creating it explicitly, this way it runs a trigger 
![image](https://github.com/user-attachments/assets/056e8076-f94c-40ef-a3eb-e8caa520eca4)
![image](https://github.com/user-attachments/assets/770f45b9-15b4-4c0d-ac34-9a0171e3e483)

We can see the data copied into the destination folder in the storage account
![image](https://github.com/user-attachments/assets/71bcc392-e6d3-49bf-a0cf-9eddab20d24f)

Using HDInsight:
        Step 1: Create Managed Identity 
a)	Go to the Azure Portal and select "Create a Resource".
b)	Search for and create a “User-Assigned Managed Identity”.
c)	Assign it to the relevant resource group and region US east.
d)	Name it appropriately and complete the creation process.
![image](https://github.com/user-attachments/assets/46cfed01-6aef-4b91-a4a8-cace318bfb9e)

e)	Granting Access to the Managed Identity:
          - Go to the Data Lake Storage account (datafactorydl2406)
          - Navigate to Access Control (IAM) and add a role assignment.
          - Assign the Storage Blob Data Owner role to the managed identity.
![image](https://github.com/user-attachments/assets/734fcc41-7279-4d58-a4a9-f7f4bab5d259)
 
Step 2: Registering the HDInsight Resource Provider: If the subscription is not registered for HDInsight, register it via Subscriptions > Resource Providers.
![image](https://github.com/user-attachments/assets/381dd494-203a-4033-9d3e-b7fe19ad1e6a)
 
Step 3: Creating the HDInsight Cluster:
a)	Go to “Create a Resource” and search for “Azure HDInsight”.
b)	Select the subscription, resource group, and a unique cluster name.
c)	Ensure the cluster region matches the Data Lake region.
d)	Select the cluster type.
e)	Configure the admin and SSH user credentials.
![image](https://github.com/user-attachments/assets/8e386ebb-233b-4ed7-b62b-1d068a3dfadd)

Step 4: Attaching Storage Accounts: Attach the Data Lake Gen2 account and specify a file system.<br> Attach additional storage accounts as needed.
![image](https://github.com/user-attachments/assets/31f6cd2b-b84a-4fa9-8393-f744203f530c)

Step 5: Cluster Configuration. Note that HDInsight clusters are charged continuously until deleted.
Step 6: Prepare the hive script for processing the data.
![image](https://github.com/user-attachments/assets/848ffff5-320c-4bc9-842d-f1eabd2a2ac1)
 
Step 7: Creating Azure Data Factory Pipeline:
a)	Created a new pipeline named "PL Process Testing Data."
b)	Selected the Hive activity from HDInsight activities in Data Factory.
![image](https://github.com/user-attachments/assets/57c89d02-3bfd-479c-b933-9e9f64633d98)
 
Step 8: Linking HDInsight Cluster:
a)	Set up a link service to the HDInsight cluster.
b)	Used the "bring your own HDInsight cluster" option instead of on-demand.
![image](https://github.com/user-attachments/assets/a8b997ea-0132-490d-acff-bb044070bf30)

Step 9: Hive Script Specification: Specified the Hive script file path stored in the standard storage account under the scripts folder.
![image](https://github.com/user-attachments/assets/8722210b-0686-4680-9f7b-43d000bc6d09)

 
Step 10: Pipeline Publishing and Execution:
a)	Published the pipeline and triggered it manually.
b)	Monitored the pipeline execution in the Monitor tab.
![image](https://github.com/user-attachments/assets/fa290385-485a-431d-a433-fbed2fec985d)
 
Step 11: Used Ambari's Hive View to query the Hive table and verified the data. Confirmed expected columns like country, country codes, test data, and confirmed cases. In the context of HDInsight Ambari is a tool used for managing and monitoring Hadoop clusters and the various services running on them.
![image](https://github.com/user-attachments/assets/caf3b130-d8f8-40b0-abdf-f924f764d0a2)

Using Databricks 
![image](https://github.com/user-attachments/assets/6db487b9-a0f2-41b7-810d-781c7a72ccf2)

Step 1: Create an **App Registration** in Azure to establish a secure connection between the storage account and Azure Databricks. This registration will provide the necessary credentials for authentication and authorization.
![image](https://github.com/user-attachments/assets/ed7f28d3-a474-451c-91b6-904527e5ec59)

Step 2: Navigate to the **Storage Account**, go to **IAM (Access Control)**, and select **Add Role Assignment**. Assign the **Storage Blob Data Contributor** role to the previously registered app. This grants the app the necessary permissions to access and manage data in the storage account.  
![image](https://github.com/user-attachments/assets/da97f14d-8d45-46ea-a202-724b6da12b76)

Step 3: Launch the **Databricks Workspace** and import the two required files. Update the necessary values such as **Application ID**, **Directory ID**, **Secret ID**, and **Storage Account Names** in the files. Execute the `mount_storage` file to establish the connection between Databricks and the storage account. Leave the `transform_population_file` unchanged, as it will be executed later by the pipeline.
![image](https://github.com/user-attachments/assets/3248d86c-51a9-447a-ae6b-b75e0ade3268)
![image](https://github.com/user-attachments/assets/e2f14870-fb19-4c6e-afd1-34dec1e87090)

 
Step 4: Create a **Linked Service** for Azure Databricks by providing the access token generated from the Databricks workspace. This token will authenticate the connection between Azure Data Factory and the Databricks environment.
![image](https://github.com/user-attachments/assets/a0b8ce1c-3029-4db9-8997-3c04d54baac8)
 
Also set the cluster configuration
![image](https://github.com/user-attachments/assets/c10609f3-8718-4ffe-b12c-e1c482c0d3a0)
 
Step 5: Create a pipeline and attach the **Linked Service** configured for Azure Databricks. Browse and select the file that needs to be executed within the pipeline. Configure any additional parameters required for the execution. 
![image](https://github.com/user-attachments/assets/5e44c174-91de-4978-b790-e357e74b09f7)

Step 6: Publish the pipeline and trigger.
![image](https://github.com/user-attachments/assets/0420165b-e0bc-47a0-8fae-7879ca7b6549)
 
Upon checking the processed container, a new folder named population will be created, containing the processed files
![image](https://github.com/user-attachments/assets/b56db24f-9a8c-40d6-b447-754b1cf0130f)
 
Step 7: Use the **SQL Query Editor** to create tables in the database, allowing the processed data to be stored in the SQL database.
![image](https://github.com/user-attachments/assets/d4605260-4e03-40b6-9fc0-f70ac39bb2ca)

Step 8: Create pipelines to transfer the three processed datasets into the corresponding SQL tables. For each pipeline, configure a Sink activity to write the data to the Azure SQL Database.
a)	Configure a pipeline to copy the cases and deaths file into the corresponding SQL table.
![image](https://github.com/user-attachments/assets/bbcfe642-153b-49d7-84e7-4089afe274d1)

b)	Configure a pipeline to copy the hospital admissions file into the corresponding sql table.
![image](https://github.com/user-attachments/assets/24dcc27e-65f7-4f70-8645-10b44fa5f5c1)

c)	Configure a pipeline to copy the testing file into the corresponding sql table.
![image](https://github.com/user-attachments/assets/7f85fe83-a469-4c4a-8544-3aebbe51091c)

Advanced Orchestration
39.	Focus on orchestrating the pipelines by combining the **ingest** and **transform** pipelines into a single **parent pipeline**. Attach a trigger to the parent pipeline to enable automated and streamlined execution of the workflows.
![image](https://github.com/user-attachments/assets/080ee830-ce42-4fed-a082-7f96c7d7965f)

40.	Create a trigger and link it to the parent pipeline to automate its execution based on specified conditions or schedules.
![image](https://github.com/user-attachments/assets/dbaf9c3a-e7e7-4963-a6c7-ef8f702e9d89)

41.	The trigger was successfully run.
![image](https://github.com/user-attachments/assets/deb44ed2-b2ff-45fe-97ca-f5249d23aef3)

42.	The trigger executed successfully, initiating the parent pipeline and completing all tasks as configured.
![image](https://github.com/user-attachments/assets/798df6f9-3d81-49b8-a29b-ae841e027dc0)
![image](https://github.com/user-attachments/assets/596b191f-b9dc-4094-94c6-2eb8a17f86d0)

 
43.	Configure an alert rule to notify the appropriate team or individual in case a trigger fails. This can be set up in Azure Monitor to track the trigger's execution status and send alerts via email, SMS, or other preferred channels.
## Data Visualization
![image](https://github.com/user-attachments/assets/e27400a1-5929-4a19-8020-78bfd922606c)

44.	Visualize the data using Power BI reports by connecting Power BI to the Azure SQL Database. Provide the server name, database name, and the admin username and password to establish the connection.
![image](https://github.com/user-attachments/assets/0ab9b996-aab3-4c01-91dc-a7c1b06b3450)
![image](https://github.com/user-attachments/assets/8f885c43-3907-4e6b-861e-14836d4459cc)
![image](https://github.com/user-attachments/assets/9d1d02d7-a540-4c9b-bd51-cd5108c6255d)










Data Visualization
 
   

