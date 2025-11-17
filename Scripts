# 🛠 Step-by-Step Implementation

##  Create File Format
CREATE OR REPLACE FILE FORMAT json_ff
  TYPE = 'json'
  STRIP_OUTER_ARRAY = TRUE
  ALLOW_DUPLICATE = TRUE;

##  Create Storage Integration
CREATE OR REPLACE STORAGE INTEGRATION aws_integrations
  TYPE = EXTERNAL_STAGE
  STORAGE_PROVIDER = S3
  ENABLED = TRUE
  STORAGE_AWS_ROLE_ARN = 'arn:aws:iam::827859361686:role/anand_snowflake_developer'
  STORAGE_ALLOWED_LOCATIONS = ('s3://realtimeusecases2/Schema_Evolution/');

## Check integration: 

DESC STORAGE INTEGRATION aws_integrations;

STORAGE_AWS_IAM_USER_ARN -- Updates in IAM (Trust Relationships)
STORAGE_AWS_EXTERNAL_ID  -- Updates in IAM (Trust Relationships)

## Create External Stage

CREATE OR REPLACE STAGE schema_stages
  STORAGE_INTEGRATION = aws_integrations
  URL = 's3://realtimeusecases2/Schema_Evolution/'
  FILE_FORMAT = 'json_ff';

## Auto-Create Table Using INFER_SCHEMA

CREATE OR REPLACE TABLE transactions_raw_v1 
      USING TEMPLATE (
      SELECT ARRAY_AGG(OBJECT_CONSTRUCT(*))
      FROM  TABLE (
            INFER_SCHEMA(
                LOCATION => '@schema_stages/transactions_v1.json',
                FILE_FORMAT => 'json_ff',
                IGNORE_CASE => TRUE
            )
      )
);

Validate :
SELECT * FROM transactions_raw_v1;   -- empty tables 

## Load V1 File

COPY INTO transactions_raw_v1
FROM @schema_stages/transactions_v1.json
MATCH_BY_COLUMN_NAME = CASE_INSENSITIVE;

SELECT * FROM transactions_raw_v1; -- load the data into tables

##Schema Evolution Starts (V2 File)

Enable Schema Evolution on the Table :
ALTER TABLE transactions_raw_v1 
SET ENABLE_SCHEMA_EVOLUTION = TRUE;

Load V2 File (Schema Evolution Happens Automatically) :

COPY INTO transactions_raw_v1
FROM @schema_stages/transactions_v2.json
MATCH_BY_COLUMN_NAME = CASE_INSENSITIVE;

Check results:
SELECT * FROM transactions_raw_v1;








