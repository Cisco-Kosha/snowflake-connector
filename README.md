# Snowflake REST API connector #

Snowflake is a cloud data warehouse that can store and analyze all your data records in one place. It can automatically scale up/down its compute resources to load, integrate, and analyze data.

The Kosha connector enables you to perform REST API operations on the Snowflake API in your Kosha workflow or custom application. Using the Kosha-Snowflake connector, you can:

* Submit SQL statements for execution.
* Check the status of the execution of a statement.
* Cancel the execution of a statement.

## Authentication ##

Kosha uses OAuth 2.0 to connect to Snowflake. If you already have an account with Snowflake, you can use your 'userid' and 'password' associated with your account along with your 'account_identifier' when provisioning the connector.

If you don’t want to use those credentials when provisioning the Snowflake connector, Kosha provides bootstrap credentials. After you sign in to your Snowflake account, Snowflake gives Kosha an access token and your connector is provisioned. Kosha automatically refreshes your access token before it expires to ensure there’s no disruption in use.


Once you've logged-in to your account, you would need to authenticate to the server when using the Snowflake SQL API.

### Enabling OAuth Connection ###

1. The user would first have to set the default account role to sysadmin. This can be achieved by the below query. 
```alter user admin set default role = sysadmin;```

2. The user would also have to create a database using the query ```CREATE DATABASE <enter database-name>;``` and then
choose an existing warehouse from the list of warehouses in his account 
```show warehouses;``` or create a new one ```CREATE OR REPLACE WAREHOUSE <warehouse-name> WITH WAREHOUSE_SIZE='<enter desired warehouse size>';``` 

3. Now, the user would have to enable OAuth2 for accessing the API. This can be achieved by running the below query.
```
create or replace security integration <enter security integration name>
type = oauth
enabled = true
oauth_client = custom
oauth_client_type = 'CONFIDENTIAL'
oauth_redirect_uri = 'https://oauth.pstmn.io/v1/callback'
oauth_issue_refresh_tokens = true
oauth_refresh_token_validity = 7776000; -- (90 days maximum validity) 
```
This should return a "Integration <enter security integration name> successfully created." status.

4. In order to access the details of the security integration and access the client tokens, run 
```describe security integration <enter security integration name>;```

5. To further get details regarding secret tokens, run query 
``` select system$show_oauth_client_secrets('<enter security integration name>');```

6. Use an application like Postman or Insomnia to fill in the token details and activate the refresh token. Once the Authentication is enabled, the user can go ahead and make API calls.

## Creating an Account and Accessing the API ##

* The user should create a Snowflake account if they don't have one or login to their existing account here: https://app.snowflake.com/?_ga=2.123302796.629466385.1689059705-1673015637.1687470140&_gac=1.229246376.1688746933.EAIaIQobChMIjpO3xoD9_wIV1AXnCh0gTwh6EAAYASAAEgJ-8vD_BwE

* Once the user signs in to their account, they can go access the Snowflake API and make API calls. The API is available at https://<account_identifier>.snowflakecomputing.com/api, where <account_identifier> is the account identifier. Read more about Snowflake's Account identifier [here](https://docs.snowflake.com/en/user-guide/admin-account-identifier).

* The different endpoints offered by the API are:

Endpoint                                    | Description
--------------------------------------------| -------------
/api/v2/statements/                         | Use this endpoint to submit SQL statements for execution.
/api/v2/statements/<statementHandle>        |  Use this endpoint to check the status of the execution of a statement. (statementHandle is a unique identifier for the statement submitted for execution.) 
/api/v2/statements/<statementHandle>/cancel | Use this endpoint to cancel the execution of a statement.











