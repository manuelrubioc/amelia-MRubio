# Functions & Web Actions: Asistente de Pedidos Agrosuper

## Functions

## consultarPedidoProgramado

Consulta el pedido programado del cliente para la próxima entrega

| Setting | Value |
|---------|-------|
| Action Type | `CONSUME_WS_ACTION` |
| WS Action | `consultarPedidoProgramado` |
| Requires Confirmation | No |

### Input Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `clienteId` | STRING | Yes | ID único del cliente |
| `fechaEntrega` | STRING | Yes | Fecha de entrega programada |

### Output Parameters

| Name | Description |
|------|-------------|
| `pedidoActual` | Lista de productos en el pedido actual |
| `fechaEntrega` | Fecha de entrega confirmada |
| `totalAproximado` | Total aproximado del pedido actual |

---

## consultarCatalogoProductos

Consulta catálogo de productos cárnicos y avícolas con precios actuales

| Setting | Value |
|---------|-------|
| Action Type | `CONSUME_WS_ACTION` |
| WS Action | `consultarCatalogoProductos` |
| Requires Confirmation | No |

### Input Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `categoria` | STRING | No | Categoría de producto (pollo, cerdo, vacuno, cecinas) |
| `busqueda` | STRING | No | Término de búsqueda específico |

### Output Parameters

| Name | Description |
|------|-------------|
| `productos` | Lista de productos disponibles con precios |
| `promociones` | Productos en promoción actual |

---

## calcularPrecioPublico

Calcula precio sugerido al público con margen del 30%

| Setting | Value |
|---------|-------|
| Action Type | `CONSUME_WS_ACTION` |
| WS Action | `calcularPrecioPublico` |
| Requires Confirmation | No |

### Input Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `precioNeto` | NUMBER | Yes | Precio neto del producto |
| `conIva` | BOOLEAN | No | Si el precio incluye IVA |

### Output Parameters

| Name | Description |
|------|-------------|
| `precioPublico` | Precio sugerido de venta al público |
| `margenGanancia` | Margen de ganancia calculado |

---

## registrarPedido

Registra o modifica pedido del cliente

| Setting | Value |
|---------|-------|
| Action Type | `CONSUME_WS_ACTION` |
| WS Action | `registrarPedido` |
| Requires Confirmation | No |

### Input Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `clienteId` | STRING | Yes | ID único del cliente |
| `productos` | STRING | Yes | Lista de productos a agregar o modificar |
| `fechaEntrega` | STRING | Yes | Fecha de entrega solicitada |

### Output Parameters

| Name | Description |
|------|-------------|
| `pedidoConfirmado` | Confirmación del pedido registrado |
| `totalFinal` | Total final del pedido |
| `numeroOrden` | Número de orden generado |

---

## Web Actions

### `GET` consultarPedidoProgramado

**URL:** `https://1c583435-0d22-4b3a-9318-2763f560fb6b.mock.pstmn.io/consultarPedidoProgramado`

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "{{clienteId}}",
    "secure": false
  },
  {
    "name": "fechaEntrega",
    "value": "{{fechaEntrega}}",
    "secure": false
  }
]
```

**Response Mappings:**
```json
{
  "OK": [
    {
      "variableName": "pedidoActual",
      "extractionExpression": "$.pedidoActual"
    },
    {
      "variableName": "fechaEntrega",
      "extractionExpression": "$.fechaEntrega"
    },
    {
      "variableName": "totalAproximado",
      "extractionExpression": "$.totalAproximado"
    }
  ]
}
```

---

### `GET` consultarCatalogoProductos

**URL:** `https://1c583435-0d22-4b3a-9318-2763f560fb6b.mock.pstmn.io/consultarCatalogoProductos`

**Request Params:**
```json
[
  {
    "name": "categoria",
    "value": "{{categoria}}",
    "secure": false
  },
  {
    "name": "busqueda",
    "value": "{{busqueda}}",
    "secure": false
  }
]
```

**Response Mappings:**
```json
{
  "OK": [
    {
      "variableName": "productos",
      "extractionExpression": "$.productos"
    },
    {
      "variableName": "promociones",
      "extractionExpression": "$.promociones"
    }
  ]
}
```

---

### `POST` calcularPrecioPublico

**URL:** `https://1c583435-0d22-4b3a-9318-2763f560fb6b.mock.pstmn.io/calcularPrecioPublico`

**Request Body:**
```json
{"precioNeto": ${precioNeto}, "conIva": ${conIva}}
```

**Request Params:**
```json
[
  {
    "name": "precioNeto",
    "value": "{{precioNeto}}",
    "secure": false
  },
  {
    "name": "conIva",
    "value": "{{conIva}}",
    "secure": false
  }
]
```

**Response Mappings:**
```json
{
  "OK": [
    {
      "variableName": "precioPublico",
      "extractionExpression": "$.precioPublico"
    },
    {
      "variableName": "margenGanancia",
      "extractionExpression": "$.margenGanancia"
    }
  ]
}
```

---

### `POST` registrarPedido

**URL:** `https://1c583435-0d22-4b3a-9318-2763f560fb6b.mock.pstmn.io/registrarPedido`

**Request Body:**
```json
{"clienteId": "${clienteId}", "productos": "${productos}", "fechaEntrega": "${fechaEntrega}"}
```

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "{{clienteId}}",
    "secure": false
  },
  {
    "name": "productos",
    "value": "{{productos}}",
    "secure": false
  },
  {
    "name": "fechaEntrega",
    "value": "{{fechaEntrega}}",
    "secure": false
  }
]
```

**Response Mappings:**
```json
{
  "OK": [
    {
      "variableName": "pedidoConfirmado",
      "extractionExpression": "$.pedidoConfirmado"
    },
    {
      "variableName": "totalFinal",
      "extractionExpression": "$.totalFinal"
    },
    {
      "variableName": "numeroOrden",
      "extractionExpression": "$.numeroOrden"
    }
  ]
}
```

---
