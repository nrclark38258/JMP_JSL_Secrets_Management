# JSL Secrets Management for Snowflake Connections

Secure Snowflake database integration demonstrating the [enterprise security patterns](../../README.md#security-architecture) defined in the main repository documentation.

## Overview

This example implements a three-tier credential management approach optimized for database connections:

1. **Template File** - Safe to share, with placeholder credentials
2. **Raw File** - Contains actual credentials (gitignored)
3. **Encrypted File** - Encrypted version for production storage

## File Structure

```
Snowflake Example/
├── snowflake_connection_template.jsl    # Template with placeholder credentials
├── snowflake_connection_example.jsl     # Main implementation example
├── snowflake_connection_encrypted.jsl   # Encrypted credentials (production)
├── snowflake_connection_raw.jsl         # Raw credentials (gitignored)
└── docs/
    └── snowflake_usage.md               # This documentation
```

## Core Functions

### 1. `get secrets(key)`
A gatekeeper function that returns credentials only when provided with the correct access key.

**Parameters:**
- `key` - Authentication key string

**Returns:**
- Associative array containing UserName and Password

**Example:**
```jsl
secrets = get secrets("wouldntyouliketoknow");
// Returns: ["UserName" => "username", "Password" => "password"]
```

### 2. `create_snowflake_connection()`
Creates and returns a Snowflake database connection object using secured credentials.

**Parameters:** None

**Returns:**
- Data Connector object configured for Snowflake

**Configuration includes:**
- Driver: SnowflakeDSIIDriver
- Database: [YOUR_DATABASE_NAME]
- Server: [your company snowflake server]
- Warehouse: [YOUR_WAREHOUSE_NAME]
- Role: [YOUR_ROLE_NAME]

**Example:**
```jsl
dc = create_snowflake_connection();
```

### 3. `snowflake_scrubbed_query(queryName, queryString)`
Executes a SQL query and returns a clean JMP data table without embedded scripts.

**Parameters:**
- `queryName` - Name for the resulting table
- `queryString` - SQL query to execute

**Returns:**
- JMP data table with query results

**Example:**
```jsl
query = "SELECT * FROM [your_table] WHERE date >= '2025-01-01'";
tbl = snowflake_scrubbed_query("My Query Results", query);
```

## Usage Examples

### Basic Query Execution
```jsl
Names default to here(1);
include("snowflake_connection_encrypted.jsl");

// Define your SQL query
query = "SELECT DISTINCT *
FROM [database].[schema].[table_name]
WHERE [date column] BETWEEN '2025-04-25' AND CURRENT_DATE()
AND [filter column] = [filter_value]";

// Execute query and get results in a JMP table
tbl = snowflake_scrubbed_query("Scrubbed table", query);
```

### Direct SQL Query with Connection
```jsl
New SQL Query(
    Connection( create_snowflake_connection() ),
    QueryName( "CAT1_PROGCURVE" ),
    CustomSQL(
        "SELECT DISTINCT *
        FROM [database].[schema].[table_name]
        WHERE [date column] BETWEEN '2025-04-25' AND CURRENT_DATE()
        "
    )
) << Run Foreground;
```

## Quick Setup

### 1. Configure Your Database Connection
1. Copy `snowflake_connection_template.jsl` to create your working file
2. Update connection parameters for your Snowflake instance:
   - Database name, server URL, warehouse, role
3. Replace placeholder credentials with actual values
4. Choose your deployment method:
   - **Development**: Use raw file (add to `.gitignore`)
   - **Production**: Encrypt file using JMP's encryption feature

### 2. Basic Usage
```jsl
Names default to here(1);
include("your_snowflake_connection.jsl");

// Execute query and get clean results
query = "SELECT * FROM [your_table] WHERE date >= '2025-01-01'";
tbl = snowflake_scrubbed_query("My Results", query);
```

## Security Implementation

This Snowflake integration follows the [main security documentation](../../README.md#security-architecture). Database-specific considerations:

- **Minimal Database Permissions**: Use accounts with only required table/schema access
- **Connection String Security**: Avoid embedding credentials in connection strings
- **Query Parameter Safety**: Use parameterized queries when possible

## Customization

Modify the `create_snowflake_connection()` function to adjust:
- Database selection and schema
- Warehouse size and compute resources
- User roles and permissions
- Connection timeout parameters

## Troubleshooting

### Common Issues

1. **"Access denied" error**
   - Verify you're using the correct access key in `get secrets()`
   - Check that the raw file exists and contains valid credentials

2. **Connection failures**
   - Ensure SnowflakeDSIIDriver is installed
   - Verify network connectivity to Snowflake
   - Check firewall and proxy settings

3. **Query errors**
   - Escape special characters in column names with `\!"`
   - Verify database and schema names
   - Check SQL syntax compatibility with Snowflake

## Notes

- The template uses placeholder passphrase for example purposes
- The `snowflake_scrubbed_query()` function removes embedded scripts from result tables for cleaner data handling
- JMP encryption (`//-e12.1` header) provides basic credential protection

## License

This is a research and development project for demonstrating secure credential management patterns in JSL.

## Support

For questions or issues, please contact your database administrator or security team.