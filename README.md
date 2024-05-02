# DEI Workforce Diversity SpotApp

SpotApps are ThoughtSpot’s out-of-the-box solution templates built for specific use cases and data sources. They utilize ThoughtSpot Modeling Language (TML) Blocks, which are pre-built pieces of code that are easy to download and implement directly from the product.

The DEI Workforce Diversity SpotApp mimics the BambooHR or Workday data model. When deployed, it creates several Worksheets, Answers, and Liveboards based on your BambooHR or Workday data in your cloud data warehouse.

This is a sample Liveboard, created after you deploy the DEI Workforce Diversity SpotApp:
![DEI Workforce Diversity SpotApp Liveboard](https://github.com/thoughtspot/DEI-Workforce-Diversity-SpotApp/assets/102629468/f1964592-f00c-4005-88fb-c82627fa6bf0)

Use the DEI Workforce Diversity SpotApp to gain invaluable insight into your workforce diversity data from your HR application.

## Prerequisites

Before you can deploy the DEI Workforce Diversity SpotApp, you must complete the following prerequisites:

- **Review Required Data**: Examine the required tables and columns for the SpotApp.
- **Ensure Column Compatibility**: Make sure that your columns match the required column type listed in the schema for your SpotApp.
- **Sync Data**: Synchronize all tables and columns from BambooHR or Workday to your cloud data warehouse. ThoughtSpot recommends syncing all tables and columns to ensure comprehensive data availability.
- **Collaborate on Data Movement**: If using an ETL/ELT tool or working with another team, ensure all columns from the tables listed in the SpotApp are synchronized.
- **Obtain Credentials**: Acquire SYSADMIN privileges to connect to your cloud data warehouse, which must contain the data ThoughtSpot will use to create Answers, Liveboards, and Worksheets.
- **Unique Connection Name**: Each new SpotApp connection name must be unique.
- **Administrator Access**: Maintain administrator access to BambooHR or Workday.

Access to the following data is required, refer to the DEI Workforce Diversity SpotApp schema for more details:
- BambooHR or Workday data

## Run SQL Commands

Execute the following SQL commands in your cloud data warehouse to standardize data types and column names:

### SQL commands for BambooHR

```sql
CREATE OR REPLACE VIEW "<DATABASE NAME>"."<SCHEMA NAME>"."EMPLOYEE_FACT" AS
SELECT
    EMPLOYEENUMBER AS EMPLOYEE_NUMBER,
    FULLNAME1 AS FULL_NAME,
    EMPLOYMENTHISTORYSTATUS AS EMPLOYMENT_STATUS,
    LOCATION AS CURRENT_LOCATION,
    DEPARTMENT AS CURRENT_DEPARTMENT,
    JOBTITLE AS CURRENT_JOB_TITLE,
    CAST(AGE AS INT) AS AGE,
    CITY AS CITY,
    COUNTRY AS COUNTRY,
    DIVISION AS REGION,
    <DISABILITY_COLUMN_NAME> AS DISABILITY,
    ETHNICITY AS ETHNICITY,
    GENDER AS GENDER,
    <GRADE_COLUMN_NAME> AS GRADE,
    TO_DATE(HIREDATE) AS HIRE_DATE,
    <LGBTQI+_COLUMN_NAME> AS "LGBTQI+",
    STATE AS STATE,
    STATUS AS STATUS,
    SUPERVISORID AS SUPERVISOR_ID,
    TO_DATE(TERMINATIONDATE) AS TERMINATION_DATE,
    WORKEMAIL AS WORK_EMAIL
FROM "<DATABASE NAME>"."<SCHEMA NAME>"."CUSTOM_REPORTS_STREAM";
```

### SQL commands for Workday
```
create or replace view "<DATABASE NAME>"."<SCHEMA NAME>"."EMPLOYEE_FACT"
as select
W.ID as EMPLOYEE_NUMBER,
CONCAT(PN.FIRST_NAME,' ', PN.LAST_NAME) as FULL_NAME,
ER.END_EMPLOYMENT_REASON as EMPLOYMENT_STATUS,
ORG.LOCATION as CURRENT_LOCATION,
ORG.NAME as CURRENT_DEPARTMENT,
PS.POSITION_TITLE as CURRENT_JOB_TITLE,
<AGE_COLUMN_NAME> as AGE,
AD.CITY as CITY,
AD.COUNTRY as COUNTRY,
AD.COUNTRY_REGION as REGION,
PD.DISABILITY_GRADE as DISABILITY,
PI.ETHNICITY as ETHNICITY,
PI.GENDER as GENDER,
<GRADE_COLUMN_NAME> as GRADE,
W.HIRE_DATE as HIRE_DATE,
<LGBTQI+_COLUMN_NAME> as "LGBTQI+",
<STATE_COLUMN_NAME> as STATE,
W.ACTIVE as STATUS,
PS.SUPERVISORY_ORGANIZATION_ID as SUPERVISOR_ID,
W.TERMINATION_DATE as TERMINATION_DATE,
EMAIL.EMAIL_ADDRESS as WORK_EMAIL from "<DATABASE NAME>"."<SCHEMA NAME>"."WORKER" W
LEFT OUTER JOIN "<DATABASE NAME>"."<SCHEMA NAME>"."ORGANIZATION" ORG ON W.ID = ORG.MANAGER_ID
LEFT OUTER JOIN "<DATABASE NAME>"."<SCHEMA NAME>"."POSITION" PS ON W.ID = PS.WORKER_ID
LEFT OUTER JOIN "<DATABASE NAME>"."<SCHEMA NAME>"."POSITION_END_EMPLOYMENT_REASON" ER ON W.ID = ER.WORKER_ID
LEFT OUTER JOIN "<DATABASE NAME>"."<SCHEMA NAME>"."PERSONAL_INFORMATION" PI ON W.ID = PI.ID
LEFT OUTER JOIN "<DATABASE NAME>"."<SCHEMA NAME>"."PERSON_NAME" PN ON PI.ID = PN.PERSONAL_INFO_SYSTEM_ID
LEFT OUTER JOIN "<DATABASE NAME>"."<SCHEMA NAME>"."ADDRESS" AD ON PI.ID = AD.PERSONAL_INFO_SYSTEM_ID
LEFT OUTER JOIN "<DATABASE NAME>"."<SCHEMA NAME>"."PERSON_DISABILITY" PD ON PI.ID = PD.PERSONAL_INFO_SYSTEM_ID
LEFT OUTER JOIN "<DATABASE NAME>"."<SCHEMA NAME>"."PERSON_CONTACT_EMAIL_ADDRESS" EMAIL ON PI.ID = EMAIL.PERSONAL_INFO_SYSTEM_ID;
```

## Deploy the SpotApp

After you have downloaded the Zip file and verified its contents, follow these steps:

1. Log into your ThoughtSpot instance and create an Embrace connection to all of the relevant views.
2. Import the TML for the Worksheets and verify that it has all been imported without any errors.
3. Import the TML for the Liveboards and verify that it has all been imported without any errors.
4. You are ready to start searching your data!

For detailed instructions on how to import TML files, refer to the [ThoughtSpot documentation](https://docs.thoughtspot.com/software/latest/tml-import-export-multiple).


## After you deploy the DEI Workforce Diversity SpotApp
After you deploy the DEI Workforce Diversity SpotApp, you must update the formulas in the Worksheet the SpotApp created with your specific information. Your department names may not match the exact names used in the formula for Functional Groups, for example, so you must replace the names in the formula with your own department names.

You must update the following formulas in the Employee Data Worksheet in ThoughtSpot:
- Employee Count(Full Time Employee)
- Functional Groups
- Level Buckets
- Level Groups





