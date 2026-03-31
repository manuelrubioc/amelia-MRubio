# Function: SearchProducts

Searches for products using text query and optional filters such as price range, category, brand, tags, and pagination/sorting options.

## Configuration

| Setting | Value |
|---------|-------|
| Action Type | `CONSUME_WS_ACTION` |
| WS Action | `SearchProducts` |
| Requires Confirmation | No |

## Input Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `limit` | NUMBER | Yes | Maximum number of products to return in a single response. |
| `q` | STRING | No | Free-text search query used to match products by name, description, or other searchable fields. |
| `summary` | BOOLEAN | Yes | If true, returns a summarized product list (e.g., fewer fields or aggregated info); if false, returns full product details. |

## Output Parameters

| Name | Description |
|------|-------------|
| `products` | Array of products that match the search query and filters, including their details or summaries depending on the 'summary' flag. |
