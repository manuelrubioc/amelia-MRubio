# Functions & Web Actions: Agente de Gestión de Quejas Rappi

## Functions

## clasificarInteraccionSocial

Clasifica interacciones de redes sociales como ticket válido o comentario general

| Setting | Value |
|---------|-------|
| Action Type | `CONSUME_WS_ACTION` |
| WS Action | `clasificarInteraccionSocial` |
| Requires Confirmation | No |

### Input Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `contenidoMensaje` | STRING | Yes | Texto del mensaje o comentario del usuario |
| `redSocial` | STRING | Yes | Plataforma de origen (Twitter, Facebook, Instagram) |
| `usuarioId` | STRING | No | Identificador del usuario en la red social |

### Output Parameters

| Name | Description |
|------|-------------|
| `tipoClasificacion` | TICKET_VALIDO o COMENTARIO_GENERAL |
| `nivelUrgencia` | ALTA, MEDIA, BAJA |
| `categoriaProblema` | Categoría identificada del problema |
| `requiereEscalacion` | Indica si necesita escalación inmediata |

---

## analizarImagenDisputa

Analiza imágenes de disputas para identificar discrepancias en pedidos

| Setting | Value |
|---------|-------|
| Action Type | `CONSUME_WS_ACTION` |
| WS Action | `analizarImagenDisputa` |
| Requires Confirmation | No |

### Input Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `imagenUrl` | STRING | Yes | URL de la imagen subida por el cliente |
| `numeroPedido` | STRING | Yes | Número de orden del pedido en disputa |
| `tipoProductoEsperado` | STRING | Yes | Tipo de producto que debería haber recibido |

### Output Parameters

| Name | Description |
|------|-------------|
| `discrepanciaDetectada` | Si se detectó diferencia entre pedido y entrega |
| `confianzaAnalisis` | Porcentaje de confianza del análisis |
| `tipoProductoDetectado` | Producto identificado en la imagen |
| `recomendacionCompensacion` | Tipo de compensación recomendada |

---

## consultarDetallesPedido

Obtiene información detallada de un pedido específico

| Setting | Value |
|---------|-------|
| Action Type | `CONSUME_WS_ACTION` |
| WS Action | `consultarDetallesPedido` |
| Requires Confirmation | No |

### Input Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `numeroPedido` | STRING | Yes | Número de orden del pedido |
| `telefonoCliente` | STRING | No | Teléfono del cliente para validación |
| `numero_pedido` | STRING | No | (auto-detected from web action body) |
| `telefono_cliente` | STRING | No | (auto-detected from web action body) |

### Output Parameters

| Name | Description |
|------|-------------|
| `estadoPedido` | Estado actual del pedido |
| `productosOrdenados` | Lista de productos en el pedido |
| `montoTotal` | Valor total del pedido |
| `tiempoEntrega` | Tiempo de entrega registrado |
| `repartidorAsignado` | Información del repartidor |

---

## crearTicketSoporte

Crea un nuevo ticket de soporte para seguimiento

| Setting | Value |
|---------|-------|
| Action Type | `CONSUME_WS_ACTION` |
| WS Action | `crearTicketSoporte` |
| Requires Confirmation | No |

### Input Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `tipoProblema` | STRING | Yes | Categoría del problema reportado |
| `descripcionDetallada` | STRING | Yes | Descripción completa del problema |
| `clienteId` | STRING | Yes | Identificador del cliente |
| `prioridad` | STRING | Yes | Nivel de prioridad: ALTA, MEDIA, BAJA |
| `evidenciaAdjunta` | STRING | No | URLs de imágenes o archivos adjuntos |

### Output Parameters

| Name | Description |
|------|-------------|
| `numeroTicket` | Número único del ticket creado |
| `estadoTicket` | Estado inicial del ticket |
| `tiempoEstimadoResolucion` | Tiempo estimado para resolución |
| `agenteAsignado` | Agente asignado al caso |

---

## Web Actions

### `POST` clasificarInteraccionSocial

**URL:** `https://a0b94834-4935-4e86-88a9-363f94cd1daa.mock.pstmn.io/clasificarInteraccionSocial`

**Request Body:**
```json
{"contenido_mensaje": "${contenidoMensaje}", "red_social": "${redSocial}", "usuario_id": "${usuarioId}"}
```

**Request Params:**
```json
[
  {
    "name": "contenidoMensaje",
    "value": "{{contenidoMensaje}}",
    "secure": false
  },
  {
    "name": "redSocial",
    "value": "{{redSocial}}",
    "secure": false
  },
  {
    "name": "usuarioId",
    "value": "{{usuarioId}}",
    "secure": false
  }
]
```

**Response Mappings:**
```json
{
  "OK": [
    {
      "variableName": "tipoClasificacion",
      "extractionExpression": "$.tipoClasificacion"
    },
    {
      "variableName": "nivelUrgencia",
      "extractionExpression": "$.nivelUrgencia"
    },
    {
      "variableName": "categoriaProblema",
      "extractionExpression": "$.categoriaProblema"
    },
    {
      "variableName": "requiereEscalacion",
      "extractionExpression": "$.requiereEscalacion"
    }
  ]
}
```

---

### `POST` analizarImagenDisputa

**URL:** `https://a0b94834-4935-4e86-88a9-363f94cd1daa.mock.pstmn.io/analizarImagenDisputa`

**Request Body:**
```json
{"imagen_url": "${imagenUrl}", "numero_pedido": "${numeroPedido}", "tipo_producto_esperado": "${tipoProductoEsperado}"}
```

**Request Params:**
```json
[
  {
    "name": "imagenUrl",
    "value": "{{imagenUrl}}",
    "secure": false
  },
  {
    "name": "numeroPedido",
    "value": "{{numeroPedido}}",
    "secure": false
  },
  {
    "name": "tipoProductoEsperado",
    "value": "{{tipoProductoEsperado}}",
    "secure": false
  }
]
```

**Response Mappings:**
```json
{
  "OK": [
    {
      "variableName": "discrepanciaDetectada",
      "extractionExpression": "$.discrepanciaDetectada"
    },
    {
      "variableName": "confianzaAnalisis",
      "extractionExpression": "$.confianzaAnalisis"
    },
    {
      "variableName": "tipoProductoDetectado",
      "extractionExpression": "$.tipoProductoDetectado"
    },
    {
      "variableName": "recomendacionCompensacion",
      "extractionExpression": "$.recomendacionCompensacion"
    }
  ]
}
```

---

### `GET` consultarDetallesPedido

**URL:** `https://a0b94834-4935-4e86-88a9-363f94cd1daa.mock.pstmn.io/consultarDetallesPedido`

**Request Body:**
```json
{}
```

**Request Params:**
```json
[
  {
    "name": "numeroPedido",
    "value": "{{numeroPedido}}",
    "secure": false
  },
  {
    "name": "telefonoCliente",
    "value": "{{telefonoCliente}}",
    "secure": false
  },
  {
    "name": "numero_pedido",
    "value": "{{numero_pedido}}",
    "secure": false
  },
  {
    "name": "telefono_cliente",
    "value": "{{telefono_cliente}}",
    "secure": false
  }
]
```

**Response Mappings:**
```json
{
  "OK": [
    {
      "variableName": "estadoPedido",
      "extractionExpression": "$.estadoPedido"
    },
    {
      "variableName": "productosOrdenados",
      "extractionExpression": "$.productosOrdenados"
    },
    {
      "variableName": "montoTotal",
      "extractionExpression": "$.montoTotal"
    },
    {
      "variableName": "tiempoEntrega",
      "extractionExpression": "$.tiempoEntrega"
    },
    {
      "variableName": "repartidorAsignado",
      "extractionExpression": "$.repartidorAsignado"
    }
  ]
}
```

---

### `POST` crearTicketSoporte

**URL:** `https://a0b94834-4935-4e86-88a9-363f94cd1daa.mock.pstmn.io/crearTicketSoporte`

**Request Body:**
```json
{"tipo_problema": "${tipoProblema}", "descripcion_detallada": "${descripcionDetallada}", "cliente_id": "${clienteId}", "prioridad": "${prioridad}", "evidencia_adjunta": "${evidenciaAdjunta}"}
```

**Request Params:**
```json
[
  {
    "name": "tipoProblema",
    "value": "{{tipoProblema}}",
    "secure": false
  },
  {
    "name": "descripcionDetallada",
    "value": "{{descripcionDetallada}}",
    "secure": false
  },
  {
    "name": "clienteId",
    "value": "{{clienteId}}",
    "secure": false
  },
  {
    "name": "prioridad",
    "value": "{{prioridad}}",
    "secure": false
  },
  {
    "name": "evidenciaAdjunta",
    "value": "{{evidenciaAdjunta}}",
    "secure": false
  }
]
```

**Response Mappings:**
```json
{
  "OK": [
    {
      "variableName": "numeroTicket",
      "extractionExpression": "$.numeroTicket"
    },
    {
      "variableName": "estadoTicket",
      "extractionExpression": "$.estadoTicket"
    },
    {
      "variableName": "tiempoEstimadoResolucion",
      "extractionExpression": "$.tiempoEstimadoResolucion"
    },
    {
      "variableName": "agenteAsignado",
      "extractionExpression": "$.agenteAsignado"
    }
  ]
}
```

---
