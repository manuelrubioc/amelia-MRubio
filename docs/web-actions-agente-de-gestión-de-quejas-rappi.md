# Web Actions: Agente de Gestión de Quejas Rappi

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
