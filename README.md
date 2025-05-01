# Datascribe

Datascribe is a data documentation framework to strengthen accuracy in LLM-generated analytics queries. This mcp-server provides a set of tools to instruct an LLM application on how to use the Datascribe framework to produce Datascribe documentation.

[Read the Datascribe famework overview](docs/datascribe_framework.md)

The MCP Servier helps any MCP-compatible LLM application generate rich data documentation that can support text-to-sql using the datascribe documentation framework. reason about real-world data by providing richly annotated metadata — including schema, provenance, business context, and known limitations.

## Setup

### Configure Claude desktop

Use Claude desktop to locate the `claude_desktop_config.json` file by navigating to **Settings > Developer > Edit Config**. Add the following configuration. (Note that datascribe requires the [Filesystem MCP Server](https://github.com/modelcontextprotocol/servers/tree/main/src/filesystem)):

```
{
    "mcpServers": {
        "filesystem": {
            "command": "npx",
            "args": [
            "-y",
            "@modelcontextprotocol/server-filesystem",
            "/Users/username/Desktop",
            "/path/to/other/allowed/dir"
            ]
        },
        "datascribe": {
            "command": "uv",
            "args": [
                "--directory",
                "/your/path/to/datascribe/repo",
                "run",
                "datascribe/datascribe.py"
            ]
        }
    }
}
```

Restart Claude desktop. On the new chat screen, you should see a hammer (MCP) icon appear with the new MCP tools available.

## Usage
Once configured, your AI tool will automatically make use of the MCP to interact with the datascribe tools. You may be prompted for permission before a tool can be used. Start by simply messaging "#datascribe start docs".

### Tools
The datascribe MCP Server provides the following tools for AI assistants to use:
- `datascribe_install`: prompt your bot to begin working on datascribe docs
- `datascribe_data_querying_instructions`: querying guidelines for the bot
- `run_bigquery_sql`: execute any sql against the bigquery database designated in the docs

### Limitations
- Only works currently on BigQuery databases, with service account-based authentication.

## Contributing
I welcome your collaboration in improving the developer MCP experience. Please submit issues in the [GitHub issue tracker](https://github.com/zephschafer/datadocs/issues).