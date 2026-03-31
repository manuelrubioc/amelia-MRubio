# Functions & Web Actions: Buscador de Productos Catalogo

## Functions

## GetCatalogCategories

Retrieves the list of all categories available in the catalog.

| Setting | Value |
|---------|-------|
| Action Type | `CONSUME_WS_ACTION` |
| WS Action | `GetCatalogCategories` |
| Requires Confirmation | No |

### Output Parameters

| Name | Description |
|------|-------------|
| `categories` | An array of catalog category objects or names representing all available categories. |

---

## Demo_Flow_SearchProducts_Delete

Deletes products returned from a demo search flow, typically used for testing or cleanup of demo search results.

| Setting | Value |
|---------|-------|
| Action Type | `CONVERSATION_FLOW` |
| Flow | `Demo_Flow_SearchProducts_Delete` |
| Requires Confirmation | No |

---

## SearchProducts

Searches for products using text query and optional filters such as price range, category, brand, tags, and pagination/sorting options.

| Setting | Value |
|---------|-------|
| Action Type | `CONSUME_WS_ACTION` |
| WS Action | `SearchProducts` |
| Requires Confirmation | No |

### Input Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `limit` | NUMBER | Yes | Maximum number of products to return in a single response. |
| `q` | STRING | No | Free-text search query used to match products by name, description, or other searchable fields. |
| `summary` | BOOLEAN | Yes | If true, returns a summarized product list (e.g., fewer fields or aggregated info); if false, returns full product details. |

### Output Parameters

| Name | Description |
|------|-------------|
| `products` | Array of products that match the search query and filters, including their details or summaries depending on the 'summary' flag. |

---

## GetCategoryProducts

Retrieves a list of products for a given category or subcategory, with optional pagination, sorting, and summary-level control.

| Setting | Value |
|---------|-------|
| Action Type | `CONSUME_WS_ACTION` |
| WS Action | `GetCategoryProducts` |
| Requires Confirmation | No |

### Input Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `category` | STRING | Yes | the category id to search for products in the catalog |
| `limit` | NUMBER | Yes | The maximum number of products to return in this request. |
| `offset` | INTEGER | Yes | The zero-based index of the first product to return, used for pagination. |
| `sort` | STRING | Yes | Sorting criteria for the product list, such as a field name and direction (e.g., 'price_asc', 'price_desc', 'name_asc'). |
| `subcategory` | STRING | No | Identifier of the subcategory whose products should be retrieved. |
| `summary` | BOOLEAN | No | If true, returns a summarized view of each product (e.g., key fields only); if false, returns full product details. |

### Output Parameters

| Name | Description |
|------|-------------|
| `products` | Array of products that match the search query and filters, including their details or summaries depending on the 'summary' flag. |

---
