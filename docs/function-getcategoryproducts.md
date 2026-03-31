# Function: GetCategoryProducts

Retrieves a list of products for a given category or subcategory, with optional pagination, sorting, and summary-level control.

## Configuration

| Setting | Value |
|---------|-------|
| Action Type | `CONSUME_WS_ACTION` |
| WS Action | `GetCategoryProducts` |
| Requires Confirmation | No |

## Input Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `category` | STRING | Yes | the category id to search for products in the catalog |
| `limit` | NUMBER | Yes | The maximum number of products to return in this request. |
| `offset` | INTEGER | Yes | The zero-based index of the first product to return, used for pagination. |
| `sort` | STRING | Yes | Sorting criteria for the product list, such as a field name and direction (e.g., 'price_asc', 'price_desc', 'name_asc'). |
| `subcategory` | STRING | No | Identifier of the subcategory whose products should be retrieved. |
| `summary` | BOOLEAN | No | If true, returns a summarized view of each product (e.g., key fields only); if false, returns full product details. |

## Output Parameters

| Name | Description |
|------|-------------|
| `products` | Array of products that match the search query and filters, including their details or summaries depending on the 'summary' flag. |
