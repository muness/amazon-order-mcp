# Amazon Orders MCP Server

An MCP (Model Context Protocol) server that wraps the [amazon-orders](https://github.com/alexdlaird/amazon-orders) Python library, allowing AI assistants to query Amazon order history.

## Installation

```bash
git clone https://github.com/muness/amazon-order-mcp.git
cd amazon-order-mcp
pip install -e .
```

Or install dependencies directly:

```bash
pip install "mcp[cli]>=1.2.0" "amazon-orders>=4.0.0" "python-dotenv>=1.0.0" "nest-asyncio>=1.6.0"
```

## Configuration

Set your Amazon credentials as environment variables:

```bash
export AMAZON_USERNAME="your-amazon-email@example.com"
export AMAZON_PASSWORD="your-amazon-password"
```

Or create a `.env` file in the project directory.

## Claude Code / Claude Desktop Configuration

Add to your MCP config (`.mcp.json` or `claude_desktop_config.json`):

```json
{
  "mcpServers": {
    "amazon-orders": {
      "command": "python",
      "args": ["/path/to/amazon-order-mcp/amazon_order_mcp.py"],
      "env": {
        "AMAZON_USERNAME": "your-amazon-email@example.com",
        "AMAZON_PASSWORD": "your-amazon-password"
      }
    }
  }
}
```

## Available Tools

### `amazon_login`

**Call this first!** Authenticates with Amazon. Required before using other tools.

**Parameters:**
- `otp_code` (optional): Your 6-digit 2FA code from your authenticator app. Required if 2FA is enabled.
- `debug` (optional): Enable debug output.

**Example flow:**
1. Call `amazon_login()`
2. If 2FA is enabled, it returns `OTP_REQUIRED`
3. Ask user for their current OTP code
4. Call `amazon_login(otp_code="123456")`

### `amazon_get_order_history`

Get order history for a specific time period.

**Parameters:**
- `year` (optional): Specific year (e.g., 2024). Defaults to current year.
- `time_filter` (optional): Use instead of year - `"last30"` for past 30 days, `"months-3"` for past 3 months.
- `full_details` (optional): If true, fetch complete order details including items (slower).

### `amazon_get_order`

Get details for a specific order.

**Parameters:**
- `order_id` (required): The Amazon order number (e.g., `"111-1234567-1234567"`).

### `amazon_search_orders`

Search orders by item title.

**Parameters:**
- `search_term` (required): Text to search for in item titles.
- `year` (optional): Year to search in.
- `time_filter` (optional): Time filter instead of year.

## Important Notes

- **No official API**: This library scrapes Amazon's website and may break if Amazon changes their site structure.
- **English .com only**: Only supports the English Amazon.com site.
- **2FA required**: Most Amazon accounts have 2FA enabled. The `amazon_login` tool handles this - just provide the OTP code when prompted.
- **Rate limiting**: Be mindful of how frequently you query - Amazon may rate limit or flag suspicious activity.

## Development

```bash
# Run the server directly
python amazon_order_mcp.py

# Or via the installed script
amazon-order-mcp
```

## Acknowledgments

This project is a thin MCP wrapper around the excellent [amazon-orders](https://github.com/alexdlaird/amazon-orders) library by [Alex Laird](https://github.com/alexdlaird). All the heavy lifting of authenticating with Amazon and parsing order data is done by that library. Thank you!

## License

MIT
