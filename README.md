# MySQL MCP Server

A Model Context Protocol server that provides read-only access to MySQL databases. This server enables LLMs to inspect database schemas and execute read-only queries.

## Authentication

The server supports MySQL authentication through the database URL. The URL format is:

```
mysql://username:password@host:port/database
```

Examples:

- DB: `mysql://user:pass@localhost:3306/mydb`

**Note**: Always ensure your credentials are properly secured and not exposed in public configurations.

## Components

### Tools

- **query**
  - Execute read-only SQL queries against the connected database
  - Input: `sql` (string): The SQL query to execute
  - All queries are executed within a READ ONLY transaction
  - Authentication is handled automatically using the provided credentials

### Resources

The server provides schema information for each table in the database:

- **Table Schemas** (`mysql://<host>/<table>/schema`)
  - JSON schema information for each table
  - Includes column names and data types
  - Automatically discovered from database metadata
  - Access is authenticated using the provided credentials

## Installation

1. Clone the repository:

```sh
git clone https://github.com/yourusername/mysql-mcp-server.git
cd mysql-mcp-server
```

2. Prepare and install dependencies:

```sh
npm run prepare
npm install
```

3. Create a global link:

```sh
npm link
```

Now you can use the `mysql-mcp-server` command from anywhere in your terminal:

```sh
mysql-mcp-server mysql://user:password@localhost:3306/mydb
```

## Usage with Cursor

### Configuring MCP in Cursor

1. Open Cursor's settings:

   - Click on the gear icon (⚙️) in the bottom left corner of Cursor
   - Or press `Shift + Cmd + J` on macOS

2. Configure MCP Server:
   - Click on "MCP" in the left sidebar
   - Click on "Add Global MCP Server"
   - Add the following configuration:

```json
{
  "mcpServers": {
    "mysql": {
      "command": "mysql-mcp-server",
      "args": ["mysql://user:password@localhost:3306/mydb"]
    }
  }
}
```

3. Save the configuration:
   - Click "Save" or press `Cmd + S`
   - Restart Cursor for the changes to take effect

### Security Best Practices

1. Use environment variables for sensitive credentials:

   ```json
   {
     "mcpServers": {
       "mysql": {
         "command": "mysql-mcp-server",
         "args": ["mysql://${MYSQL_USER}:${MYSQL_PASSWORD}@host:3306/mydb"]
       }
     }
   }
   ```

2. Ensure the MySQL user has minimal required permissions (READ-ONLY access)
3. Use strong passwords and follow security best practices
4. Avoid committing configuration files with credentials to version control

## License

This MCP server is licensed under the MIT License. This means you are free to use, modify, and distribute the software, subject to the terms and conditions of the MIT License. For more details, please see the LICENSE file in the project repository.
