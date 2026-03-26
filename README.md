# COCO_SNOW

**###Prompts###**

Fetch the data from table SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.INCIDENTS and pass the raw text to 
 Snowflake Cortex-hosted Large Language Model (e.g., Llama 3) and give me back what should be assignment group and assigned to parameters along with the  confidence score
 
 -----------------------------------------------------------------------------------------------------------------------------------
 Column names from incident table - NUMBER, OPENED_AT, SHORT_DESCRIPTION, CALLER_ID, PRIORITY, STATE, CATEGORY, ASSIGNMENT_GROUP, ASSIGNED_TO, SYS_UPDATED_ON, SYS_UPDATED_BY
filtering requirement - Status : In Progress and New
LLM model preference -llama3-70b (more accurate, slower)
Show the results in - Parsed JSON format (separate columns for assignment_group, assigned_to, confidence_score)?
All the above data in incidents table is dummy data created  for a hackathon use case , so prepare the list of assignment recommendation accordingly
I need a python code

------------------------------------------------------------------------------------------------------------------------------------
this command is giving syntax error -pip install snowflake-connector-python pandas openpyxl

------------------------------------------------------------------------------------------------------------------------------------
I was running the abve command in snowflake python sheet

------------------------------------------------------------------------------------------------------------------------------------
will the sql query take lot of time in the first run ?

------------------------------------------------------------------------------------------------------------------------------------
got the results .. how do I train this model ? Can I provide multiple older incident details and train the model to increase the accuracy and confidence score ?

------------------------------------------------------------------------------------------------------------------------------------
here's the result of some closed incidents :
Category : Inquiry / Help , Priority : 3 - Moderate , Description : Unable to connect to email , Group : Network , Person : David Loo
Category : Software , Priority : 4 - Low , Description : my PDF docs are all locked from editing , Group : Service Desk , Person : Luke Wilson
Category : Inquiry / Help , Priority : 5 - Planning , Description : Issue with networking , Group : Network , Person : Luke Wilson
Category : Inquiry / Help , Priority : 5 - Planning , Description : Reset my password , Group : Service Desk , Person : Luke Wilson
Category : Inquiry / Help , Priority : 1 - Critical , Description : Does not look like a backup occurred last night , Group : Software , Person : David Loo
Category : Software , Priority : 1 - Critical , Description : File Server is 80% full - Needs upgrade , Group : Hardware , Person : Don Goodliffe
Category : Inquiry / Help , Priority : 5 - Planning , Description : EMAIL Server Down Again , Group : Hardware , Person : David Loo
Category : Inquiry / Help , Priority : 5 - Planning , Description : Lost connection to the wireless network , Group : Network , Person : David Loo
Category : Inquiry / Help , Priority : 5 - Planning , Description : My disk is still having issues. Cant delete a file  , Group : Service Desk , Person : Don Goodliffe
Category : Hardware , Priority : 5 - Planning , Description : Seem to have an issue with my hard drive... , Group : Hardware , Person : Don Goodliffe
Category : Inquiry / Help , Priority : 5 - Planning , Description : Issue with a web page on wiki , Group : Service Desk , Person : ITIL User
Category : Inquiry / Help , Priority : 5 - Planning , Description : New employee hire , Group :  , Person : Beth Anglin
Category : Inquiry / Help , Priority : 1 - Critical , Description : Missing my home directory , Group :  , Person : Bud Richman
Category : Inquiry / Help , Priority : 1 - Critical , Description : EMAIL is slow when an attachment is involved , Group : Software , Person : David Loo
Category : Software , Priority : 5 - Planning , Description : Customer didnt receive eFax , Group : Database , Person : David Loo
Category : Inquiry / Help  , Priority : 3 - Moderate , Description : Need new Blackberry set up , Group : Hardware , Person : ITIL User

------------------------------------------------------------------------------------------------------------------------------------
if I give details of more historical tickets  , can we store them at some place and only refer it in the final query as this query would become too large

------------------------------------------------------------------------------------------------------------------------------------
In the earlier thread , I got a code the below code but its not showing any data for recommended_assignment_group , recommended_assigned_to and confidence_score:

------------------------------------------------------------------------------------------------------------------------------------
this is what I see in llm_response column : {"assignment_group": "IT Support", "assigned_to": "John Doe", "confidence_score": 0.8, "reasoning": "Shared folder access issues are typically handled by the IT Support team, and John Doe is the team lead."}

------------------------------------------------------------------------------------------------------------------------------------
in the fixed query getting an error - unexpected 'ELSE'.

------------------------------------------------------------------------------------------------------------------------------------
I would need to load this data with recommended values and confidence score in another snowflake table :
SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.LLM_DATA
Modify the query accordingly

------------------------------------------------------------------------------------------------------------------------------------
For our hackathon use case , we have decided the following steps , can you create an automatic running process or an app or an interface which can do the below tasks:
As soon as data enters into SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.INCIDENTS for records with SENT_TO_SERVICE_NOW = FALSE , all the valid records should be sent to the the below sql code . you should use the vector search algorithm on the SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.INCIDENT_TRAINING_EXAMPLES data in order to provide recommended assignement group and recommended assigne dto values. do not just look at category and sub catergory. Modify the code accordingly.
what ever data gets loaded into SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.LLM_DATA , immediately insert the same data into SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.INCIDENT_TRAINING_EXAMPLES also

------------------------------------------------------------------------------------------------------------------------------------
While running below code :
UPDATE SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.INCIDENT_TRAINING_EXAMPLES
SET DESCRIPTION_EMBEDDING = SNOWFLAKE.CORTEX.EMBED_TEXT_1024(
    'e5-base-v2',
    COALESCE(SHORT_DESCRIPTION, '') || ' ' || 
    COALESCE(CATEGORY, '') 
    --|| ' ' || 
    --COALESCE(SUBCATEGORY, '')
)
WHERE DESCRIPTION_EMBEDDING IS NULL;
getting an error - Array-like value being cast to a vector has incorrect dimension

------------------------------------------------------------------------------------------------------------------------------------
EMBED_TEXT_768 worked

------------------------------------------------------------------------------------------------------------------------------------
getting an error - unexpected 'WITH'. syntax error line 3 at position 8 unexpected 'SELECT'. syntax error line 7 at position 8 unexpected ')'.

------------------------------------------------------------------------------------------------------------------------------------
getting error in function generation - unexpected 'SELECT'. syntax error line 4 at position 28 unexpected 'AS'. syntax error line 9 at position 12 unexpected 't'. syntax error line 13 at position 12 unexpected ')'.

------------------------------------------------------------------------------------------------------------------------------------
still getting an error - unexpected 'SELECT'. syntax error line 2 at position 16 unexpected 'top_k'. syntax error line 13 at position 12 unexpected ')'.

------------------------------------------------------------------------------------------------------------------------------------
In the stored procedure code , getting an error -  unexpected 'DIAGNOSTICS'.

------------------------------------------------------------------------------------------------------------------------------------
while creating a streamlit app for monitoring , getting an error - Installing dependencies failed because the pyproject.toml file does not exist. Please create it.

------------------------------------------------------------------------------------------------------------------------------------
Getting an error while running the above code in streamlit in snowflake :
An error occurred while loading the app. Error: Installing dependencies failed because the pyproject.toml file does not exist. Please create it.

------------------------------------------------------------------------------------------------------------------------------------
why do I keep getting an error - Installing dependencies failed because the pyproject.toml file does not exist. Please create it.
Do I need to create pyproject.toml file and if yes , what should I put in it

------------------------------------------------------------------------------------------------------------------------------------
getting an error related to toml file :
Failed to parse: `pyproject.toml`
  Caused by: TOML parse error at line 1, column 1
  |
1 | [project]
  | ^^^^^^^^^
`pyproject.toml` is using the `[project]` table, but the required `project.name` field is not set

------------------------------------------------------------------------------------------------------------------------------------
I am runing the streamlit app on python enviroment - run on warehouse and getting an error :
An error occurred while loading the app. Error: SQL compilation error: Cannot create a Python function with the specified packages. Please check your packages specification and try again. 'Packages not found: python==3.11'.

------------------------------------------------------------------------------------------------------------------------------------

**###Skills###**
Service Now - Ticketing tool
Zapier - Automation tool to load the incident data from service now to snowflake
Snowflake - Data platform and custom logic
Snowflake Cortex - AI/LLM engine
e5-base-v2 - embedding model

------------------------------------------------------------------------------------------------------------------------------------

**###Agents###**
SERVICENOW_CATEGORIZATION

------------------------------------------------------------------------------------------------------------------------------------

**###Code base ###**
-----Step 1: Setup Vector Search on Training Data
--First, let's create embeddings for your training data:
-- Add embedding column to training examples table
ALTER TABLE SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.INCIDENT_TRAINING_EXAMPLES 
DROP COLUMN DESCRIPTION_EMBEDDING; --one time step


ALTER TABLE SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.INCIDENT_TRAINING_EXAMPLES 
ADD COLUMN DESCRIPTION_EMBEDDING VECTOR(FLOAT, 768); --one time step

UPDATE SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.INCIDENT_TRAINING_EXAMPLES
SET DESCRIPTION_EMBEDDING = SNOWFLAKE.CORTEX.EMBED_TEXT_768(
    'e5-base-v2',
    COALESCE(SHORT_DESCRIPTION, '') || ' ' || 
    COALESCE(CATEGORY, '')
)
WHERE DESCRIPTION_EMBEDDING IS NULL;


----------------------------------------------------------------------------------------------------------
--Step 2: Create Stream to Monitor New Incidents
-- Create stream to track new incidents
CREATE OR REPLACE STREAM SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.INCIDENTS_STREAM 
ON TABLE SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.INCIDENTS
APPEND_ONLY = TRUE;

----------------------------------------------------------------------------------------------------------
--Step 3: Create Enhanced Vector Search Function
CREATE OR REPLACE FUNCTION SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.GET_SIMILAR_INCIDENTS_BY_VECTOR(
    input_embedding VECTOR(FLOAT, 768), 
    top_k INTEGER
)
RETURNS TABLE (
    description VARCHAR,
    category VARCHAR,
    assignment_group VARCHAR,
    assigned_to VARCHAR,
    similarity_score FLOAT
)
AS
$$
    SELECT 
        SHORT_DESCRIPTION AS description,
        CATEGORY AS category,
        ASSIGNMENT_GROUP AS assignment_group,
        ASSIGNED_TO AS assigned_to,
        VECTOR_COSINE_SIMILARITY("DESCRIPTION_EMBEDDING", input_embedding) AS similarity_score
    FROM SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.INCIDENT_TRAINING_EXAMPLES
    WHERE "DESCRIPTION_EMBEDDING" IS NOT NULL
      AND ASSIGNMENT_GROUP IS NOT NULL
    QUALIFY ROW_NUMBER() OVER (ORDER BY similarity_score DESC) <= top_k
$$;

-------------------------------------------------------------------------------------------------------
---Step 4: Create Stored Procedure for LLM Processing
CREATE OR REPLACE PROCEDURE SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.PROCESS_NEW_INCIDENTS()
RETURNS STRING
LANGUAGE SQL
AS
$$
DECLARE
    processed_count INTEGER DEFAULT 0;
    curr_vec_array ARRAY; 
    similar_text VARCHAR;
    llm_out VARCHAR;
    
    -- Variables for Cursor data
    v_number VARCHAR; v_opened_at VARCHAR; v_short_desc VARCHAR; v_caller_id VARCHAR;
    v_priority VARCHAR; v_state VARCHAR; v_category VARCHAR; v_group VARCHAR; v_user VARCHAR;

    -- Variables to hold parsed AI results
    rec_group VARCHAR; rec_user VARCHAR; rec_score VARCHAR; rec_reason VARCHAR;
    
    c1 CURSOR FOR 
        SELECT 
            "NUMBER", "OPENED_AT", "SHORT_DESCRIPTION", "CALLER_ID", "PRIORITY", "STATE", "CATEGORY",
            "ASSIGNMENT_GROUP", "ASSIGNED_TO",
            CAST(SNOWFLAKE.CORTEX.EMBED_TEXT_768(
                'e5-base-v2', 
                COALESCE("SHORT_DESCRIPTION", '') || ' ' || COALESCE("CATEGORY", '')
            ) AS ARRAY) as current_embedding_arr
        FROM SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.INCIDENTS
        WHERE "SENT_TO_SERVICE_NOW" = FALSE
          AND "STATE" IN ('New', 'In Progress');
BEGIN
    -- 0. Wipe existing data for reload
    TRUNCATE TABLE SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.LLM_DATA;

    FOR rec IN c1 DO
        -- 1. Extract Cursor Data
        v_number := rec."NUMBER";
        v_opened_at := rec."OPENED_AT";
        v_short_desc := rec."SHORT_DESCRIPTION";
        v_caller_id := rec."CALLER_ID";
        v_priority := rec."PRIORITY";
        v_state := rec."STATE";
        v_category := rec."CATEGORY";
        v_group := rec."ASSIGNMENT_GROUP";
        v_user := rec."ASSIGNED_TO";
        curr_vec_array := rec.current_embedding_arr;

        -- 2. Get Similar Incidents (Added 'assigned_to' to the string)
        similar_text := (
            SELECT COALESCE(
                LISTAGG('Incident ' || description || ' -> Group: ' || assignment_group || ' | Person: ' || assigned_to, '\n'),
                'No similar incidents found.'
            )
            FROM TABLE(SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.GET_SIMILAR_INCIDENTS_BY_VECTOR(
                CAST(:curr_vec_array AS VECTOR(FLOAT, 768)), 3)
            )
        );

-- 3. Get AI Response (Updated prompt instructions)
        llm_out := (
            SELECT SNOWFLAKE.CORTEX.COMPLETE(
                'llama3-70b',
                CONCAT('Context of past tickets:\n', :similar_text, 
                       '\n\nCurrent Ticket to route: ', :v_short_desc, 
                       '\n\nTask: Based on the context above, which Assignment Group AND which specific Person should handle this? 
                       Respond ONLY in JSON format: {"assignment_group": "name", "assigned_to": "person name", "confidence_score": 0.9, "reasoning": "text"}')
            )
        );

        -- 4. Parse JSON
        rec_group := TRY_PARSE_JSON(:llm_out):assignment_group::VARCHAR;
        rec_user  := TRY_PARSE_JSON(:llm_out):assigned_to::VARCHAR;
        rec_score := TRY_PARSE_JSON(:llm_out):confidence_score::VARCHAR;
        rec_reason := TRY_PARSE_JSON(:llm_out):reasoning::VARCHAR;

        -- 5. INSERT into LLM_DATA (Explicit Aliases for "NUMBER")
        INSERT INTO SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.LLM_DATA (
            "NUMBER", "OPENED_AT", "SHORT_DESCRIPTION", "CALLER_ID", "PRIORITY", "STATE", "CATEGORY",
            "CURRENT_ASSIGNMENT_GROUP", "CURRENT_ASSIGNED_TO",
            "RECOMMENDED_ASSIGNMENT_GROUP", "RECOMMENDED_ASSIGNED_TO",
            "CONFIDENCE_SCORE", "REASONING"
        )
        SELECT 
            :v_number AS "NUMBER", :v_opened_at AS "OPENED_AT", :v_short_desc AS "SHORT_DESCRIPTION", 
            :v_caller_id AS "CALLER_ID", :v_priority AS "PRIORITY", :v_state AS "STATE", :v_category AS "CATEGORY",
            :v_group AS "CURRENT_ASSIGNMENT_GROUP", :v_user AS "CURRENT_ASSIGNED_TO", 
            :rec_group AS "RECOMMENDED_ASSIGNMENT_GROUP", :rec_user AS "RECOMMENDED_ASSIGNED_TO", 
            :rec_score AS "CONFIDENCE_SCORE", :rec_reason AS "REASONING";

        -- 6. INSERT into TRAINING_EXAMPLES (Removed "NUMBER" column from this list)
        INSERT INTO SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.INCIDENT_TRAINING_EXAMPLES (
            "SHORT_DESCRIPTION", "CATEGORY", "ASSIGNMENT_GROUP", "ASSIGNED_TO", "DESCRIPTION_EMBEDDING"
        )
        SELECT 
            :v_short_desc AS "SHORT_DESCRIPTION", 
            :v_category AS "CATEGORY", 
            :rec_group AS "ASSIGNMENT_GROUP", 
            :rec_user AS "ASSIGNED_TO", 
            CAST(:curr_vec_array AS VECTOR(FLOAT, 768)) AS "DESCRIPTION_EMBEDDING";

        -- 7. Update Source
        UPDATE SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.INCIDENTS
        SET "SENT_TO_SERVICE_NOW" = TRUE
        WHERE "NUMBER" = :v_number;

        processed_count := processed_count + 1;
    END FOR;

    RETURN 'Success: Processed ' || processed_count || ' incidents.';
END;
$$;

CALL SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.PROCESS_NEW_INCIDENTS();

---------------------------------------------------------------------------------------------
--Step 5: Create Automated Task
-- Create task to run every 5 minutes
CREATE OR REPLACE TASK SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.AUTO_PROCESS_INCIDENTS_TASK
    WAREHOUSE = SERVICENOW_WH
    SCHEDULE = '15 MINUTE'
    WHEN SYSTEM$STREAM_HAS_DATA('SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.INCIDENTS_STREAM')
AS
    CALL SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.PROCESS_NEW_INCIDENTS();

-- IMPORTANT: Tasks are created in a SUSPENDED state by default. Resume it!
ALTER TASK SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.AUTO_PROCESS_INCIDENTS_TASK RESUME;

--------------------------------------------------------------------------------------------------
SELECT COLUMN_NAME 
FROM SERVICENOW_DEST_DB.INFORMATION_SCHEMA.COLUMNS 
WHERE TABLE_NAME = 'LLM_DATA';



select * from SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.INCIDENTS order by opened_at desc;

select * from SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.INCIDENT_TRAINING_EXAMPLES ;

select * from  SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.AUTO_PROCESS_INCIDENTS_TASK;

select * from SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.INCIDENTS_STREAM order by opened_at desc ;

select * from SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.LLM_DATA ;



SELECT * FROM TABLE(SERVICENOW_DEST_DB.SERVICENOW_DEST_SCHEMA.GET_SIMILAR_INCIDENTS_BY_VECTOR(
    SNOWFLAKE.CORTEX.EMBED_TEXT_768('e5-base-v2', 'test disk issue')::VECTOR(FLOAT, 768), 3
));


