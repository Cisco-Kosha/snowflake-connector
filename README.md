# Snowflake REST API connector #

Snowflake is a cloud data warehouse that can store and analyze all your data records in one place. It can automatically scale up/down its compute resources to load, integrate, and analyze data.

The Kosha connector enables you to perform REST API operations on the Snowflake API in your Kosha workflow or custom application. Using the Kosha-Snowflake connector, you can:

* Submit SQL statements for execution.
* Check the status of the execution of a statement.
* Cancel the execution of a statement.

## Authentication ##

Kosha uses OAuth 2.0 to connect to Snowflake. If you already have an account with Snowflake, you can use your 'userid' and 'password' associated with your account along with your 'account_identifier' when provisioning the connector.

If you don’t want to use those credentials when provisioning the Snowflake connector, Kosha provides bootstrap credentials. After you sign in to your Snowflake account, Snowflake gives Kosha an access token and your connector is provisioned. Kosha automatically refreshes your access token before it expires to ensure there’s no disruption in use.

## Creating an Account and Accessing the API ##

* The user should create a Snowflake account if they don't have one or login to their existing account here: https://app.snowflake.com/?_ga=2.123302796.629466385.1689059705-1673015637.1687470140&_gac=1.229246376.1688746933.EAIaIQobChMIjpO3xoD9_wIV1AXnCh0gTwh6EAAYASAAEgJ-8vD_BwE

* Once the user signs in to their account, they can go access the Snowflake API and make API calls. The API is available at https://<account_identifier>.snowflakecomputing.com/api, where <account_identifier> is the account identifier. Read more about Snowflake's Account identifier [here](https://docs.snowflake.com/en/user-guide/admin-account-identifier).

* The different endpoints offered by the API are:

Endpoint                                    | Description
--------------------------------------------| -------------
/api/v2/statements/                         | Use this endpoint to submit SQL statements for execution.
/api/v2/statements/<statementHandle>        |  Use this endpoint to check the status of the execution of a statement. (statementHandle is a unique identifier for the statement submitted for execution.) 
/api/v2/statements/<statementHandle>/cancel | Use this endpoint to cancel the execution of a statement.











