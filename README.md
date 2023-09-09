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
```alter user admin set default_role = sysadmin;```

2. The user would also have to create a database using the query ```CREATE DATABASE <enter database-name>;``` and then
choose an existing warehouse from the list of warehouses in their account 
```show warehouses;``` or create a new one ```CREATE OR REPLACE WAREHOUSE <warehouse-name> WITH WAREHOUSE_SIZE='<enter desired warehouse size>';``` 

3. Now, the user would have to enable OAuth2 for accessing the API. This can be achieved by running the below query.
```
create or replace security integration <enter security integration name>
type = oauth
enabled = true
oauth_client = custom
oauth_client_type = 'CONFIDENTIAL'
oauth_redirect_uri = '<enter callback url>'
oauth_issue_refresh_tokens = true
oauth_refresh_token_validity = 7776000; -- (90 days maximum validity) 
```
This should return a "Integration <enter security integration name> successfully created." status.

4. In order to access the details of the security integration and access the client tokens, run

```describe security integration <enter security integration name>;```

5. To further get details regarding secret tokens, run query 

``` select system$show_oauth_client_secrets('<enter security integration name>');```

6. Use an application like Postman or Insomnia to fill in the token details and activate the refresh token. Once the Authentication is enabled, the user would be able to make API calls.

## Creating an Account and Accessing the API ##

* The user should create a Snowflake account if they don't have one or login to their existing account [here](https://signup.snowflake.com/?utm_cta=trial-en-www-homepage-top-right-nav-ss-evg&_ga=2.74406678.547897382.1657561304-1006975775.1656432605&_gac=1.254279162.1656541671.Cj0KCQjw8O-VBhCpARIsACMvVLPE7vSFoPt6gqlowxPDlHT6waZ2_Kd3-4926XLVs0QvlzvTvIKg7pgaAqd2EALw_wcB).

* Once the user signs in to their account, they can go access the Snowflake API and make API calls. The API is available at https://<account_identifier>.snowflakecomputing.com/api, where <account_identifier> is the account identifier. Read more about Snowflake's Account identifier [here](https://docs.snowflake.com/en/user-guide/admin-account-identifier).

* The different endpoints offered by the API can be accessed [here](https://docs.snowflake.com/en/developer-guide/sql-api/about-endpoints).


## Kosha Connector Open Source Development

All connectors Kosha shares on the marketplace are open source. We believe in fostering collaboration and open development. Everyone is welcome to contribute their ideas, improvements, and feedback for any Kosha connector. We encourage community engagement and appreciate any contributions that align with our goals of an open and collaborative API management platform.

Refer to the contribution guidelines for details.

## Contributing

Pull requests and bug reports are welcome.

For larger changes, please create an issue in GitHub first to discuss your proposed changes and their possible implications.










