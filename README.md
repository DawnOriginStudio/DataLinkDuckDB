# DataLinkMySQL - UE5 MySQL Database Plugin

This is a MySQL database connection plugin developed for Unreal Engine 5, implemented based on the libmariadb library, providing complete MySQL database operation functionality.

## Features

- ✅ Support for MySQL/MariaDB database connections
- ✅ Complete CRUD operations (Create, Read, Update, Delete)
- ✅ Transaction support
- ✅ Blueprint-friendly interfaces
- ✅ JSON format query results
- ✅ SQL injection protection
- ✅ Connection pool management
- ✅ Cross-platform support (Windows/Linux/Mac)
- ✅ Detailed error handling and logging

## System Requirements

- Unreal Engine 5.0+
- MySQL 5.7+ or MariaDB 10.2+
- Windows 10+ / Linux / macOS

## Installation Instructions

1. Copy the plugin folder to your project's `Plugins` directory
2. Regenerate project files
3. Compile the project
4. Enable the `DataLinkMySQL` plugin in project settings

## Quick Start

### 1. Configure Database Connection

Edit the `Config/MySQLConfig.ini` file:

```ini
[MySQL]
Host=localhost
Port=3306
Username=your_username
Password=your_password
Database=your_database
ConnectTimeout=10
Charset=utf8mb4
```

### 2. C++ Code Example

```cpp
#include "DataLinkMySQL.h"
#include "MySQLConnection.h"
#include "MySQLBlueprintLibrary.h"

// Create connection
UMySQLConnection* Connection = UMySQLBlueprintLibrary::CreateMySQLConnection();

// Configure connection parameters
FMySQLConnectionConfig Config;
Config.Host = TEXT("localhost");
Config.Port = 3306;
Config.Username = TEXT("root");
Config.Password = TEXT("password");
Config.Database = TEXT("test_game");

// Connect to database
if (Connection->Connect(Config))
{
    // Execute query
    FMySQLQueryResult Result = Connection->ExecuteQuery(TEXT("SELECT * FROM players"));
    
    if (Result.bSuccess)
    {
        UE_LOG(LogTemp, Log, TEXT("Query successful, returned %d rows"), Result.NumRows);
        UE_LOG(LogTemp, Log, TEXT("Result data: %s"), *Result.ResultData);
    }
    
    // Disconnect
    Connection->Disconnect();
}
```

### 3. Blueprint Usage

1. Add a `MySQL Test Actor` in your blueprint
2. Configure database connection parameters
3. Call the appropriate MySQL functions

## Main Classes and Functions

### UMySQLConnection

Core connection class, providing basic database operation functionality:

- `Connect(Config)` - Connect to database
- `Disconnect()` - Disconnect from database
- `IsConnected()` - Check connection status
- `ExecuteQuery(SQL)` - Execute query
- `ExecuteStatement(SQL)` - Execute statement
- `BeginTransaction()` - Begin transaction
- `CommitTransaction()` - Commit transaction
- `RollbackTransaction()` - Rollback transaction
- `EscapeString(Input)` - Escape string

### UMySQLBlueprintLibrary

Blueprint function library, providing convenient operation interfaces:

- `CreateMySQLConnection()` - Create connection object
- `QuickConnect()` - Quick connect
- `LoadConnectionConfigFromFile()` - Load configuration from file
- `SaveConnectionConfigToFile()` - Save configuration to file
- `BuildInsertStatement()` - Build INSERT statement
- `BuildUpdateStatement()` - Build UPDATE statement
- `BuildSelectStatement()` - Build SELECT statement
- `BuildDeleteStatement()` - Build DELETE statement
- `ParseQueryResultToStringArray()` - Parse result to string array
- `GetTableList()` - Get table list
- `TestConnection()` - Test connection

### AMySQLTestActor

Example Actor class, demonstrating complete usage methods:

- Database connection management
- Table creation and data manipulation
- Transaction handling
- Error handling and logging

## Data Structures

### FMySQLConnectionConfig

```cpp
struct FMySQLConnectionConfig
{
    FString Host;           // Server address
    int32 Port;            // Port number
    FString Username;      // Username
    FString Password;      // Password
    FString Database;      // Database name
    int32 ConnectTimeout;  // Connection timeout
    FString Charset;       // Character set
};
```

### FMySQLQueryResult

```cpp
struct FMySQLQueryResult
{
    bool bSuccess;         // Whether successful
    FString ErrorMessage;  // Error message
    int32 AffectedRows;    // Number of affected rows
    FString ResultData;    // Result data (JSON format)
    int32 NumRows;         // Number of result rows
};
```

## Security Considerations

1. **SQL Injection Protection**: Always use the `EscapeString()` function to handle user input
2. **Password Security**: Don't hardcode database passwords in your code
3. **Connection Management**: Close database connections promptly when not needed
4. **Permission Control**: Create dedicated database users for your application with only necessary permissions

## Error Handling

The plugin provides detailed error information and logging:

```cpp
FMySQLQueryResult Result = Connection->ExecuteQuery(SQL);
if (!Result.bSuccess)
{
    UE_LOG(LogTemp, Error, TEXT("Query failed: %s"), *Result.ErrorMessage);
}
```

## Performance Optimization Tips

1. **Connection Reuse**: Avoid frequently creating and destroying connections
2. **Batch Operations**: Use transactions for batch data operations
3. **Index Optimization**: Create indexes for frequently queried fields
4. **Query Optimization**: Avoid using `SELECT *`, only query needed fields
5. **Connection Pool**: Use connection pools in high concurrency scenarios

## Troubleshooting

### Common Issues

1. **Connection Failure**
   - Check if MySQL service is running
   - Verify connection parameters are correct
   - Confirm firewall settings

2. **Compilation Errors**
   - Ensure libmariadb library files exist
   - Check include path settings
   - Regenerate project files

3. **Runtime Errors**
   - Check if DLL files are correctly copied
   - View UE5 output logs
   - Verify database permissions

### Debugging Tips

1. Enable verbose logging
2. Use MySQL Test Actor for testing
3. Check MySQL server logs
4. Use database management tools to verify operations

## Example Project

The plugin includes a complete example Actor (`AMySQLTestActor`), demonstrating:

- Database connection and disconnection
- Table creation and management
- Data CRUD operations
- Transaction handling
- Error handling
- Leaderboard functionality
