# Amazon Orders MCP Server

An MCP (Model Context Protocol) server that wraps the [amazon-orders](https://github.com/alexdlaird/amazon-orders) Python library, allowing AI assistants to query Amazon order history.

## Installation

```bash
pip install -e .
```

Or install dependencies directly:

```bash
pip install "mcp[cli]>=1.2.0" "amazon-orders>=4.0.0"
```

## Configuration

Set your Amazon credentials as environment variables:

```bash
export AMAZON_USERNAME="your-amazon-email@example.com"
export AMAZON_PASSWORD="your-amazon-password"
```

## Claude Desktop Configuration

Add to your Claude Desktop config (`~/.claude/claude_desktop_config.json` or similar):

```json
{
  "mcpServers": {
    "amazon-orders": {
      "command": "uvx",
      "args": ["amazon-order-mcp"],
      "env": {
        "AMAZON_USERNAME": "your-amazon-email@example.com",
        "AMAZON_PASSWORD": "your-amazon-password"
      }
    }
  }
}
```

## Available Tools

### `amazon_get_order_history`

Get order history for a specific time period.

**Parameters:**
- `year` (optional): Specific year (e.g., 2024). Defaults to current year.
- `time_filter` (optional): Use instead of year - "last30" for past 30 days, "months-3" for past 3 months.
- `full_details` (optional): If true, fetch complete order details (slower).

### `amazon_get_order`

Get details for a specific order.

**Parameters:**
- `order_id` (required): The Amazon order number (e.g., "111-1234567-1234567").

### `amazon_search_orders`

Search orders by item title.

**Parameters:**
- `search_term` (required): Text to search for in item titles.
- `year` (optional): Year to search in.
- `time_filter` (optional): Time filter instead of year.

## Important Notes

- **No official API**: This library scrapes Amazon's website and may break if Amazon changes their site structure.
- **English .com only**: Only supports the English Amazon.com site.
- **2FA**: If you have two-factor authentication enabled, you may need to handle it interactively on first login.
- **Rate limiting**: Be mindful of how frequently you query - Amazon may rate limit or flag suspicious activity.

## Development

```bash
# Run the server directly
python amazon_order_mcp.py

# Or via the installed script
amazon-order-mcp
```
