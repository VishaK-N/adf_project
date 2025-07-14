# Healthcare AR management using Azure Data Factory

This Azure Data Factory (ADF) project focuses on processing and transforming data from the healthcare Accounts Receivable (AR) domain to support better business evaluation and decision-making.

## Project Description:

In the Healthcare, **Account Receivable** (amount owed by patients or insurance company) should be managed properly. If there is any delay or uncollectible amount will affect the cash flow of the management. 

Thus, this project helps in understanding the data and helps the business stakeholders to take actions for the better cash flow, such as 

- total bill
- adjustment amount
- amount paid
- Remaining amount in the AR.

These data helps us to analyse the business even more in wide view and take better action for the good cash flow and reduce the collection period.

action could be done:-
- **regular follow-ups**
- **automatedremainders**

## 🧰 Tech Stack

- **Azure Data Factory (ADF)** – for building pipelines and orchestration 
- **Azure Data Lake Storage** – Used for storage purpose 
- **Mapping Data Flows** – to transform the data as per business requirements  
- **GitHub** – Integrated with ADF for version control, collaboration, and as a source location

## 🚀 Getting Started
steps to get started with the project

### 📁 Create Required Containers in Azure Data Lake
create the storage account with namespace heirarchy checkbox ticked to create the Azure Data lake or Azure blob stoarge can also be used, In the ADLS create the containers such as 

- `staging` – for raw source files 
- `staging` – here files will be coming from raw container with some condition and also from the git source 
- `final` – files from the staging will be joined, trasmformed and loaded in the final container 

### 🔗 Create Linked Services in ADF
- Azure Data Lake Storage – to connect with your blob containers
- GitHub – to get data from the gitHub using http source

### 🛠 Create Datasets
create the dataset whenever needed while creating a pipeline
dataset - file which is used for processing 

## 🛠 Create Pipelines:

### Pipeline Git
creating a pipeline with copy activity to get the data from the github and loading into the staging container

### Pipeline required Files
* Create a pipeline with metadata activity from which getting the childitems of the each file
* Each file will be scan by using the ForEach Activity based on the childItems
  **@activity('GetMetadata').output.childItems**
* Using the If Condition Activity pass a condition on each file, if it satisfy copy and load into the staging layer along with the git source file
  **@startswith(item().name,'Fact')**

### 🔄 Build Mapping Data Flows
- extracting all the necessary files from the stage container
- aggregate operation on fact_adjustment, fact_payments
- joining the fact_claims with the fact_adjustment, fact_payments and dim_payers
- using the derived column operation to get the receivable amount from the patient or insurers
  **receivable_amount = total_charge + total_adjustment - total_paid**

### Delete Activity
After processing Delete the data from the staging layer, so everytime the data get processed as new.

## Master pipeline using Executive Pipeline activity 
- creating a pipeline combining both pipelines, dataflow and delete activity altogether as master pipeline.

## ▶️ Usage

Once everything has setup

1.**Triggering the pipeline**
   - Running the pipeline manually or run it with any of the trigger

2. **Monitor Pipeline Execution**
   - Use the **Monitor** tab in ADF to track runs and debug any failures.

3. **Verify Output**
   - Final transformed data will be available in the `final` container in Azure Data Lake.
   - Use this data for reporting, analysis, or dashboarding.

![final_output](ScreenShots/final_table_preview_ss.png)


