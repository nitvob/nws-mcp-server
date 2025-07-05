# Weather MCP Server

A Model Context Protocol (MCP) server that provides weather information using the National Weather Service API. This server exposes tools for getting weather forecasts and alerts for US locations.

## Features

- **🌤️ Weather Forecasts**: Get detailed weather forecasts for any US location using latitude/longitude coordinates
- **🚨 Weather Alerts**: Retrieve active weather alerts for any US state
- **🔌 MCP Integration**: Works seamlessly with Claude for Desktop and other MCP-compatible clients
- **📡 Real-time Data**: Fetches live data from the National Weather Service API

## Tools Available

### `get-forecast`

Get weather forecast for a specific location.

**Parameters:**

- `latitude` (float): Latitude of the location (-90 to 90)
- `longitude` (float): Longitude of the location (-180 to 180)

**Example usage:**

- "What's the weather forecast for San Francisco?" (Claude will use coordinates ~37.7749, -122.4194)
- "Give me the weather forecast for latitude 47.6062, longitude -122.3321" (Seattle)

### `get-alerts`

Get active weather alerts for a US state.

**Parameters:**

- `state` (string): Two-letter US state code (e.g., "CA", "NY", "TX")

**Example usage:**

- "What are the active weather alerts in California?"
- "Are there any weather warnings in Texas?"

## Prerequisites

- Python 3.10 or higher
- `uv` package manager
- Access to the internet (for NWS API calls)

## Installation

1. **Install uv** (if not already installed):

   ```bash
   # macOS/Linux
   curl -LsSf https://astral.sh/uv/install.sh | sh

   # Windows
   powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
   ```

2. **Clone or navigate to the project directory**:

   ```bash
   cd weather
   ```

3. **Install dependencies**:
   ```bash
   uv sync
   ```

## Running the Server

### Standalone Testing

To test the server directly:

```bash
uv run weather.py
```

The server will start and listen on standard input/output. You can test it using the MCP inspector or other MCP clients.

### With Claude for Desktop

1. **Install Claude for Desktop** from [claude.ai/download](https://claude.ai/download)

2. **Configure Claude for Desktop** by editing the configuration file:

   **macOS/Linux:**

   ```bash
   code ~/Library/Application\ Support/Claude/claude_desktop_config.json
   ```

   **Windows:**

   ```powershell
   code $env:AppData\Claude\claude_desktop_config.json
   ```

3. **Add the weather server configuration**:

   **macOS/Linux:**

   ```json
   {
     "mcpServers": {
       "weather": {
         "command": "uv",
         "args": [
           "--directory",
           "/ABSOLUTE/PATH/TO/YOUR/weather",
           "run",
           "weather.py"
         ]
       }
     }
   }
   ```

   **Windows:**

   ```json
   {
     "mcpServers": {
       "weather": {
         "command": "uv",
         "args": [
           "--directory",
           "C:\\ABSOLUTE\\PATH\\TO\\YOUR\\weather",
           "run",
           "weather.py"
         ]
       }
     }
   }
   ```

4. **Restart Claude for Desktop** completely

5. **Verify the integration** by looking for the "Search and tools" icon in Claude for Desktop

## Usage Examples

Once configured with Claude for Desktop, you can ask questions like:

- "What's the weather forecast for Sacramento?"
- "Give me the weather forecast for New York City"
- "What are the active weather alerts in Florida?"
- "Are there any severe weather warnings in Texas?"
- "What's the weather like at coordinates 40.7128, -74.0060?" (NYC)

## API Details

This server uses the National Weather Service API (api.weather.gov), which:

- Provides free access to US weather data
- Requires no API key
- Returns data in JSON format
- Only covers US locations

## Project Structure

```
weather/
├── main.py          # Entry point (if needed)
├── weather.py       # Main MCP server implementation
├── pyproject.toml   # Project configuration and dependencies
├── uv.lock         # Dependency lock file
└── README.md       # This file
```

## Troubleshooting

### Server Not Showing Up in Claude

1. **Check the configuration file syntax** - Ensure valid JSON
2. **Verify the absolute path** - Use full paths, not relative ones
3. **Check Claude's logs**:

   ```bash
   # macOS/Linux
   tail -f ~/Library/Logs/Claude/mcp*.log

   # Windows
   # Check logs in %AppData%\Claude\logs\
   ```

4. **Restart Claude for Desktop** completely

### Tool Calls Failing

1. **Verify the server runs standalone**:
   ```bash
   uv run weather.py
   ```
2. **Check for rate limiting** - The NWS API has rate limits
3. **Ensure coordinates are for US locations** - The NWS API only covers the US
4. **Check internet connectivity** - Server needs to reach api.weather.gov

### Common Error Messages

- **"Failed to retrieve grid point data"**: Usually means coordinates are outside the US
- **"No active alerts for this state"**: Not an error - just means no current alerts
- **"Unable to fetch forecast data"**: Network issue or invalid coordinates

## Development

### Adding New Tools

To add new weather-related tools:

1. Add the tool using the `@mcp.tool()` decorator
2. Implement the async function with proper type hints
3. Add error handling and validation
4. Test with `uv run weather.py`

### Dependencies

Key dependencies (managed by `uv`):

- `mcp`: Model Context Protocol SDK
- `httpx`: HTTP client for API requests
- `fastmcp`: Simplified MCP server framework

## License

This project is part of the Model Context Protocol ecosystem. Check individual dependencies for their licenses.

## Contributing

Feel free to submit issues and pull requests to improve the weather server functionality.

## Related Resources

- [Model Context Protocol Documentation](https://modelcontextprotocol.io/)
- [National Weather Service API](https://www.weather.gov/documentation/services-web-api)
- [Claude for Desktop](https://claude.ai/download)
- [MCP Python SDK](https://github.com/modelcontextprotocol/python-sdk)
