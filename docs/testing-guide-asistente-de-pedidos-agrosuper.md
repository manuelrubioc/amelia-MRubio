# Testing Guide: Asistente de Pedidos Agrosuper

Test scenarios with full request/response examples.

## Dataset: Generated (2026-02-27 00:31) **(Active)**

AI-generated mock data for Agente Comercial Agrosuper

_Source: generated_

### Web Action: `calcularPrecioPublico`

**alt_happy_precio_alto_sin_iva** (HTTP 200)

Cálculo exitoso de precio público para producto premium sin IVA

> **Tester prompt:** Calcula el precio público de un producto con precio neto $89.500 sin IVA incluido.

**Request Body:**
```json
{
  "precioNeto": 89500,
  "conIva": false
}
```

**Request Params:**
```json
[
  {
    "name": "precioNeto",
    "value": "89500"
  },
  {
    "name": "conIva",
    "value": "false"
  }
]
```

**Response Body:**
```json
{
  "margenGanancia": "28.5%",
  "precioPublico": "$115.015"
}
```

---

**edge_precio_minimo_con_iva** (HTTP 200)

Cálculo con precio mínimo permitido incluyendo IVA

> **Tester prompt:** Calcula el precio público de un producto con precio neto de $1 con IVA incluido.

**Request Body:**
```json
{
  "precioNeto": 1,
  "conIva": true
}
```

**Request Params:**
```json
[
  {
    "name": "precioNeto",
    "value": "1"
  },
  {
    "name": "conIva",
    "value": "true"
  }
]
```

**Response Body:**
```json
{
  "margenGanancia": "25.0%",
  "precioPublico": "$1"
}
```

---

**error_precio_negativo** (HTTP 400)

Error por precio neto negativo

> **Tester prompt:** Intenta calcular el precio público con un precio neto de -$1.500 con IVA.

**Request Body:**
```json
{
  "precioNeto": -1500,
  "conIva": true
}
```

**Request Params:**
```json
[
  {
    "name": "precioNeto",
    "value": "-1500"
  },
  {
    "name": "conIva",
    "value": "true"
  }
]
```

**Response Body:**
```json
{
  "error": true,
  "codigo": "PRECIO_INVALIDO",
  "mensaje": "El precio neto no puede ser negativo",
  "margenGanancia": null,
  "precioPublico": null
}
```

---

### Web Action: `calcularTotalPedido`

**alt_happy_pedido_mixto** (HTTP 200)

Cálculo exitoso con productos mixtos y promociones

**Request Body:**
```json
{
  "productos": "AGS-201,AGS-105,AGS-300",
  "cantidades": "1.5,2,0.8"
}
```

**Request Params:**
```json
[
  {
    "name": "cantidades",
    "value": "1.5,2,0.8"
  },
  {
    "name": "productos",
    "value": "AGS-201,AGS-105,AGS-300"
  }
]
```

**Response Body:**
```json
{
  "detalleProductos": [
    {
      "codigo": "AGS-201",
      "nombre": "Lomo Premium",
      "cantidad": 1.5,
      "precioUnitario": 8990,
      "subtotal": 13485
    },
    {
      "codigo": "AGS-105",
      "nombre": "Pechuga Pollo",
      "cantidad": 2,
      "precioUnitario": 4990,
      "subtotal": 9980
    },
    {
      "codigo": "AGS-300",
      "nombre": "Cecinas Surtidas",
      "cantidad": 0.8,
      "precioUnitario": 3500,
      "subtotal": 2800
    }
  ],
  "sugerenciaVenta": {
    "producto": "AGS-310 - Queso Mantecoso",
    "razon": "Complementa perfectamente con cecinas"
  },
  "totalAproximado": 26265,
  "descuentos": 1315,
  "totalFinal": 24950
}
```

---

**edge_case** (HTTP 200)

Cálculo con cantidades mínimas

**Request Body:**
```json
{
  "productos": "AGS-001",
  "cantidades": "0.1"
}
```

**Request Params:**
```json
[
  {
    "name": "cantidades",
    "value": "0.1"
  },
  {
    "name": "productos",
    "value": "AGS-001"
  }
]
```

**Response Body:**
```json
{
  "detalleProductos": [
    {
      "id": "AGS-001",
      "nombre": "Pollo Entero Super Fresco",
      "cantidad": 0.1,
      "precioUnitario": 3290,
      "subtotal": 329
    }
  ],
  "sugerenciaVenta": [],
  "totalAproximado": 329
}
```

---

**edge_productos_fraccionados** (HTTP 200)

Cálculo con productos en cantidades muy pequeñas

**Request Body:**
```json
{
  "productos": "AGS-105,AGS-002",
  "cantidades": "0.25,0.1"
}
```

**Request Params:**
```json
[
  {
    "name": "cantidades",
    "value": "0.25,0.1"
  },
  {
    "name": "productos",
    "value": "AGS-105,AGS-002"
  }
]
```

**Response Body:**
```json
{
  "detalleProductos": [
    {
      "codigo": "AGS-105",
      "nombre": "Pechuga Pollo",
      "cantidad": 0.25,
      "precioUnitario": 4990,
      "subtotal": 1248
    },
    {
      "codigo": "AGS-002",
      "nombre": "Muslo Pollo",
      "cantidad": 0.1,
      "precioUnitario": 2890,
      "subtotal": 289
    }
  ],
  "sugerenciaVenta": null,
  "totalAproximado": 1537,
  "advertencia": "Pedido mínimo recomendado: $5.000 para optimizar costo de envío"
}
```

---

**error_cantidad_excesiva** (HTTP 400)

Error por cantidad que excede límites de pedido

**Request Body:**
```json
{
  "productos": "AGS-201",
  "cantidades": "50"
}
```

**Request Params:**
```json
[
  {
    "name": "cantidades",
    "value": "50"
  },
  {
    "name": "productos",
    "value": "AGS-201"
  }
]
```

**Response Body:**
```json
{
  "error": true,
  "codigo": "CANTIDAD_EXCESIVA",
  "mensaje": "La cantidad solicitada (50kg) excede el límite máximo por pedido (20kg)",
  "limiteMaximo": 20,
  "productoAfectado": "AGS-201"
}
```

---

**error_case** (HTTP 400)

Producto no disponible en stock suficiente

**Request Body:**
```json
{
  "productos": "AGS-003,AGS-004",
  "cantidades": "10,25"
}
```

**Request Params:**
```json
[
  {
    "name": "cantidades",
    "value": "10,25"
  },
  {
    "name": "productos",
    "value": "AGS-003,AGS-004"
  }
]
```

**Response Body:**
```json
{
  "error": "STOCK_INSUFICIENTE",
  "mensaje": "Stock insuficiente para algunos productos",
  "detalles": [
    {
      "producto": "AGS-004",
      "solicitado": 25,
      "disponible": 12
    }
  ]
}
```

---

**happy_path** (HTTP 200)

Cálculo exitoso del total del pedido con sugerencias

**Request Body:**
```json
{
  "productos": "AGS-001,AGS-002,AGS-015",
  "cantidades": "2,1.5,3"
}
```

**Request Params:**
```json
[
  {
    "name": "cantidades",
    "value": "2,1.5,3"
  },
  {
    "name": "productos",
    "value": "AGS-001,AGS-002,AGS-015"
  }
]
```

**Response Body:**
```json
{
  "detalleProductos": [
    {
      "id": "AGS-001",
      "nombre": "Pollo Entero Super Fresco",
      "cantidad": 2,
      "precioUnitario": 3290,
      "subtotal": 6580
    },
    {
      "id": "AGS-002",
      "nombre": "Pechuga de Pollo Deshuesada",
      "cantidad": 1.5,
      "precioUnitario": 5890,
      "subtotal": 8835
    },
    {
      "id": "AGS-015",
      "nombre": "Chorizo Parrillero",
      "cantidad": 3,
      "precioUnitario": 4250,
      "subtotal": 12750
    }
  ],
  "sugerenciaVenta": [
    {
      "id": "AGS-020",
      "nombre": "Condimento para Pollo",
      "motivo": "Complementa perfectamente con pollo fresco",
      "precio": 1890
    }
  ],
  "totalAproximado": 28165
}
```

---

### Web Action: `confirmarPedido`

**alt_happy_entrega_programada** (HTTP 201)

Confirmación exitosa con entrega programada en Valparaíso

**Request Body:**
```json
{
  "clienteId": "CLI-78901",
  "pedidoCompleto": "AGS-201:1.5kg,AGS-105:2kg,AGS-300:0.8kg",
  "fechaEntrega": "2024-02-05"
}
```

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-78901"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-02-05"
  },
  {
    "name": "pedidoCompleto",
    "value": "AGS-201:1.5kg,AGS-105:2kg,AGS-300:0.8kg"
  }
]
```

**Response Body:**
```json
{
  "fechaConfirmacion": "2024-01-30T14:25:30-03:00",
  "numeroPedido": "PED-567890",
  "resumenPedido": {
    "cliente": "CLI-78901",
    "productos": 3,
    "pesoTotal": "4.3kg",
    "valorTotal": 24950,
    "fechaEntrega": "2024-02-05",
    "horaEntrega": "09:00-12:00",
    "direccionEntrega": "Valparaíso, V Región",
    "metodoPago": "transferencia",
    "codigoSeguimiento": "AGS-567890-VP"
  }
}
```

---

**edge_case** (HTTP 201)

Confirmación de pedido con monto mínimo

**Request Body:**
```json
{
  "clienteId": "CLI-11111",
  "pedidoCompleto": "AGS-001:0.5kg",
  "fechaEntrega": "2024-01-29"
}
```

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-11111"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-01-29"
  },
  {
    "name": "pedidoCompleto",
    "value": "AGS-001:0.5kg"
  }
]
```

**Response Body:**
```json
{
  "fechaConfirmacion": "2024-01-22T14:45:00-03:00",
  "numeroPedido": "PED-789016",
  "resumenPedido": {
    "cliente": "CLI-11111",
    "productos": [
      {
        "nombre": "Pollo Entero Super Fresco",
        "cantidad": "0.5kg",
        "precio": 1645
      }
    ],
    "total": 1645,
    "fechaEntrega": "2024-01-29",
    "horarioEntrega": "14:00-17:00",
    "estado": "confirmado",
    "advertencia": "Pedido bajo monto mínimo recomendado de $10.000"
  }
}
```

---

**edge_pedido_minimo_exacto** (HTTP 201)

Confirmación con monto exacto del pedido mínimo

**Request Body:**
```json
{
  "clienteId": "CLI-33333",
  "pedidoCompleto": "AGS-002:1.73kg",
  "fechaEntrega": "2024-02-12"
}
```

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-33333"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-02-12"
  },
  {
    "name": "pedidoCompleto",
    "value": "AGS-002:1.73kg"
  }
]
```

**Response Body:**
```json
{
  "fechaConfirmacion": "2024-01-30T16:45:12-03:00",
  "numeroPedido": "PED-500000",
  "resumenPedido": {
    "cliente": "CLI-33333",
    "productos": 1,
    "pesoTotal": "1.73kg",
    "valorTotal": 5000,
    "fechaEntrega": "2024-02-12",
    "observacion": "Pedido alcanza monto mínimo exacto",
    "costoEnvio": 0,
    "tiempoEstimado": "3-4 horas"
  }
}
```

---

**error_case** (HTTP 400)

Error por fecha de entrega no válida

**Request Body:**
```json
{
  "clienteId": "CLI-67890",
  "pedidoCompleto": "AGS-005:1kg",
  "fechaEntrega": "2024-01-21"
}
```

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-67890"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-01-21"
  },
  {
    "name": "pedidoCompleto",
    "value": "AGS-005:1kg"
  }
]
```

**Response Body:**
```json
{
  "error": "FECHA_ENTREGA_INVALIDA",
  "mensaje": "La fecha de entrega debe ser al menos 24 horas desde ahora",
  "fechaMinima": "2024-01-23",
  "fechaSolicitada": "2024-01-21"
}
```

---

**error_fecha_no_disponible** (HTTP 422)

Error por fecha de entrega no disponible en la zona

**Request Body:**
```json
{
  "clienteId": "CLI-45678",
  "pedidoCompleto": "AGS-001:3kg",
  "fechaEntrega": "2024-01-31"
}
```

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-45678"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-01-31"
  },
  {
    "name": "pedidoCompleto",
    "value": "AGS-001:3kg"
  }
]
```

**Response Body:**
```json
{
  "error": true,
  "codigo": "FECHA_NO_DISPONIBLE",
  "mensaje": "La fecha 2024-01-31 no está disponible para entregas en su zona",
  "fechasDisponibles": [
    "2024-02-02",
    "2024-02-05",
    "2024-02-07"
  ],
  "zonaEntrega": "Región Metropolitana Sur"
}
```

---

**happy_path** (HTTP 201)

Confirmación exitosa del pedido

**Request Body:**
```json
{
  "clienteId": "CLI-12345",
  "pedidoCompleto": "AGS-001:2kg,AGS-002:1.5kg,AGS-015:3kg",
  "fechaEntrega": "2024-01-28"
}
```

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-12345"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-01-28"
  },
  {
    "name": "pedidoCompleto",
    "value": "AGS-001:2kg,AGS-002:1.5kg,AGS-015:3kg"
  }
]
```

**Response Body:**
```json
{
  "fechaConfirmacion": "2024-01-22T14:30:00-03:00",
  "numeroPedido": "PED-789015",
  "resumenPedido": {
    "cliente": "CLI-12345",
    "productos": [
      {
        "nombre": "Pollo Entero Super Fresco",
        "cantidad": "2kg",
        "precio": 6580
      },
      {
        "nombre": "Pechuga de Pollo Deshuesada",
        "cantidad": "1.5kg",
        "precio": 8835
      },
      {
        "nombre": "Chorizo Parrillero",
        "cantidad": "3kg",
        "precio": 12750
      }
    ],
    "total": 28165,
    "fechaEntrega": "2024-01-28",
    "horarioEntrega": "09:00-12:00",
    "estado": "confirmado"
  }
}
```

---

### Web Action: `consultarCatalogoProductos`

**alt_happy_busqueda_especifica** (HTTP 200)

Búsqueda exitosa de productos específicos con promociones vigentes

> **Tester prompt:** Busca productos de lomo vetado en la categoría vacuno.

**Request Params:**
```json
[
  {
    "name": "busqueda",
    "value": "lomo vetado"
  },
  {
    "name": "categoria",
    "value": "vacuno"
  }
]
```

**Response Body:**
```json
{
  "productos": [
    {
      "codigo": "AGS-450",
      "nombre": "Lomo Vetado Premium",
      "precio": "$18.990/kg",
      "stock": "Disponible",
      "origen": "Región de Los Lagos"
    },
    {
      "codigo": "AGS-451",
      "nombre": "Lomo Vetado Tradicional",
      "precio": "$15.490/kg",
      "stock": "Disponible",
      "origen": "Región del Maule"
    }
  ],
  "promociones": [
    {
      "tipo": "Descuento por volumen",
      "descripcion": "15% descuento comprando más de 10kg",
      "vigencia": "Hasta 28/02/2024"
    }
  ]
}
```

---

**alt_happy_premium_products** (HTTP 200)

Consulta exitosa de productos premium de cerdo

**Request Params:**
```json
[
  {
    "name": "categoria",
    "value": "cerdo"
  },
  {
    "name": "tipoProducto",
    "value": "premium"
  }
]
```

**Response Body:**
```json
{
  "productos": [
    {
      "codigo": "AGS-201",
      "nombre": "Lomo Premium Agrosuper",
      "precio": 8990,
      "unidad": "kg",
      "disponibilidad": "alta",
      "descripcion": "Lomo de cerdo premium sin aditivos"
    },
    {
      "codigo": "AGS-202",
      "nombre": "Chuletas Premium",
      "precio": 6490,
      "unidad": "kg",
      "disponibilidad": "media"
    }
  ],
  "promociones": [
    {
      "tipo": "descuento",
      "descripcion": "15% descuento en compras sobre $50.000",
      "vigencia": "2024-02-15"
    }
  ]
}
```

---

**edge_busqueda_caracteres_especiales** (HTTP 200)

Búsqueda con caracteres especiales que no arroja resultados

> **Tester prompt:** Busca 'pollo & jamón' en la categoría embutidos.

**Request Params:**
```json
[
  {
    "name": "busqueda",
    "value": "pollo & jamón"
  },
  {
    "name": "categoria",
    "value": "embutidos"
  }
]
```

**Response Body:**
```json
{
  "productos": [],
  "promociones": []
}
```

---

**edge_case** (HTTP 200)

Consulta de categoría sin productos disponibles

**Request Params:**
```json
[
  {
    "name": "categoria",
    "value": "pavo"
  },
  {
    "name": "tipoProducto",
    "value": "organico"
  }
]
```

**Response Body:**
```json
{
  "productos": [],
  "promociones": [],
  "mensaje": "No hay productos disponibles para la categoría y tipo especificado"
}
```

---

**edge_sin_stock_temporada** (HTTP 200)

Consulta con productos sin stock por temporada alta

**Request Params:**
```json
[
  {
    "name": "categoria",
    "value": "vacuno"
  },
  {
    "name": "tipoProducto",
    "value": "especial"
  }
]
```

**Response Body:**
```json
{
  "productos": [],
  "promociones": [],
  "mensaje": "Productos temporalmente sin stock debido a alta demanda navideña",
  "fechaReposicion": "2024-02-05"
}
```

---

**error_case** (HTTP 404)

Categoría de producto no encontrada

**Request Params:**
```json
[
  {
    "name": "categoria",
    "value": "mariscos"
  },
  {
    "name": "tipoProducto",
    "value": "congelado"
  }
]
```

**Response Body:**
```json
{
  "error": "CATEGORIA_NO_ENCONTRADA",
  "mensaje": "La categoría 'mariscos' no está disponible en nuestro catálogo",
  "categorias_disponibles": [
    "pollo",
    "cerdo",
    "pavo",
    "embutidos"
  ]
}
```

---

**error_categoria_discontinuada** (HTTP 404)

Error al consultar categoría discontinuada

**Request Params:**
```json
[
  {
    "name": "categoria",
    "value": "cordero"
  },
  {
    "name": "tipoProducto",
    "value": "fresco"
  }
]
```

**Response Body:**
```json
{
  "error": true,
  "codigo": "CAT_DISCONTINUADA",
  "mensaje": "La categoría 'cordero' ha sido discontinuada en nuestra línea de productos",
  "sugerencias": [
    "cerdo",
    "vacuno",
    "pollo"
  ]
}
```

---

**error_categoria_mantenimiento** (HTTP 503)

Error por categoría en mantenimiento temporal

> **Tester prompt:** Busca productos de salmón en la categoría pescados.

**Request Params:**
```json
[
  {
    "name": "busqueda",
    "value": "salmón"
  },
  {
    "name": "categoria",
    "value": "pescados"
  }
]
```

**Response Body:**
```json
{
  "error": true,
  "codigo": "CATEGORIA_MANTENIMIENTO",
  "mensaje": "Categoría pescados en mantenimiento hasta las 18:00 hrs",
  "productos": [],
  "promociones": []
}
```

---

**happy_path** (HTTP 200)

Consulta exitosa del catálogo de productos de pollo

**Request Params:**
```json
[
  {
    "name": "categoria",
    "value": "pollo"
  },
  {
    "name": "tipoProducto",
    "value": "fresco"
  }
]
```

**Response Body:**
```json
{
  "productos": [
    {
      "id": "AGS-001",
      "nombre": "Pollo Entero Super Fresco",
      "categoria": "pollo",
      "precio": 3290,
      "unidad": "kg",
      "stock": 150,
      "descripcion": "Pollo entero fresco de primera calidad"
    },
    {
      "id": "AGS-002",
      "nombre": "Pechuga de Pollo Deshuesada",
      "categoria": "pollo",
      "precio": 5890,
      "unidad": "kg",
      "stock": 85,
      "descripcion": "Pechuga de pollo sin hueso ni piel"
    }
  ],
  "promociones": [
    {
      "tipo": "descuento",
      "descripcion": "15% descuento en compras sobre $50.000",
      "vigencia": "2024-01-31"
    }
  ]
}
```

---

### Web Action: `consultarPedidoProgramado`

**alt_happy_cliente_frecuente** (HTTP 200)

Cliente frecuente con múltiples pedidos programados

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-78901"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-02-01"
  }
]
```

**Response Body:**
```json
{
  "historialPedidos": [
    {
      "numero": "PED-890123",
      "fecha": "2024-01-15",
      "total": 89500,
      "estado": "entregado"
    },
    {
      "numero": "PED-890124",
      "fecha": "2024-01-22",
      "total": 67800,
      "estado": "entregado"
    }
  ],
  "pedidoActual": {
    "numero": "PED-890125",
    "productos": [
      "AGS-201:2kg",
      "AGS-150:1kg"
    ],
    "total": 24980,
    "estado": "confirmado"
  },
  "proximaEntrega": {
    "fecha": "2024-02-08",
    "horaEstimada": "10:30",
    "direccion": "Las Condes, Santiago"
  }
}
```

---

**alt_happy_distribuidor_regional** (HTTP 200)

Consulta exitosa para distribuidor regional con múltiples productos programados

> **Tester prompt:** Dile al agente que eres el cliente CLI-85432 y consulta tu pedido programado para el 15 de febrero.

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-85432"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-02-15"
  }
]
```

**Response Body:**
```json
{
  "fechaEntrega": "2024-02-15",
  "pedidoActual": "AGS-450:25kg Lomo Vetado Premium, AGS-301:15kg Pechuga Pollo Orgánica, AGS-127:8kg Costillar Cerdo",
  "totalAproximado": "$487.650"
}
```

---

**edge_case** (HTTP 200)

Cliente sin pedidos programados

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-54321"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-01-27"
  }
]
```

**Response Body:**
```json
{
  "historialPedidos": [],
  "pedidoActual": null,
  "proximaEntrega": null,
  "mensaje": "El cliente no tiene pedidos programados"
}
```

---

**edge_cliente_nuevo_sin_historial** (HTTP 200)

Cliente nuevo sin historial de pedidos

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-99998"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-02-10"
  }
]
```

**Response Body:**
```json
{
  "historialPedidos": [],
  "pedidoActual": null,
  "proximaEntrega": null,
  "mensaje": "Cliente nuevo - sin historial de pedidos",
  "beneficiosNuevoCliente": [
    {
      "tipo": "descuento",
      "valor": "10%",
      "vigencia": "primer pedido"
    }
  ]
}
```

---

**edge_pedido_vacio_fecha_limite** (HTTP 200)

Cliente sin pedidos en fecha límite de programación

> **Tester prompt:** Consulta como cliente CLI-13579 tu pedido para el 31 de marzo.

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-13579"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-03-31"
  }
]
```

**Response Body:**
```json
{
  "fechaEntrega": "2024-03-31",
  "pedidoActual": "",
  "totalAproximado": "$0"
}
```

---

**error_case** (HTTP 404)

Cliente no encontrado en el sistema

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-99999"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-01-26"
  }
]
```

**Response Body:**
```json
{
  "error": "CLIENTE_NO_ENCONTRADO",
  "mensaje": "El cliente con ID 'CLI-99999' no existe en nuestro sistema",
  "codigo_error": "404_CLIENT_NOT_FOUND"
}
```

---

**error_cliente_suspendido** (HTTP 403)

Cliente con cuenta suspendida por pagos pendientes

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-00001"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-02-03"
  }
]
```

**Response Body:**
```json
{
  "error": true,
  "codigo": "CLIENTE_SUSPENDIDO",
  "mensaje": "Cliente suspendido por pagos pendientes",
  "montoPendiente": 125900,
  "contacto": "+56 2 2820 8000"
}
```

---

**error_fecha_formato_invalido** (HTTP 400)

Error por formato de fecha incorrecto

> **Tester prompt:** Pregunta por tu pedido como cliente CLI-76543 para la fecha 15/02/2024.

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-76543"
  },
  {
    "name": "fechaEntrega",
    "value": "15/02/2024"
  }
]
```

**Response Body:**
```json
{
  "error": true,
  "codigo": "FECHA_INVALIDA",
  "mensaje": "Formato de fecha debe ser YYYY-MM-DD",
  "fechaEntrega": null,
  "pedidoActual": null,
  "totalAproximado": null
}
```

---

**happy_path** (HTTP 200)

Consulta exitosa de pedidos programados del cliente

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-12345"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-01-25"
  }
]
```

**Response Body:**
```json
{
  "historialPedidos": [
    {
      "numeroPedido": "PED-789012",
      "fecha": "2024-01-18",
      "total": 87500,
      "estado": "entregado"
    },
    {
      "numeroPedido": "PED-789013",
      "fecha": "2024-01-11",
      "total": 65200,
      "estado": "entregado"
    }
  ],
  "pedidoActual": {
    "numeroPedido": "PED-789014",
    "fecha": "2024-01-25",
    "productos": [
      {
        "nombre": "Pollo Entero Super Fresco",
        "cantidad": 5,
        "precio": 3290
      }
    ],
    "total": 16450,
    "estado": "programado"
  },
  "proximaEntrega": {
    "fecha": "2024-02-01",
    "horario": "09:00-12:00",
    "direccion": "Av. Providencia 1234, Santiago"
  }
}
```

---

### Web Action: `registrarPedido`

**alt_happy_pedido_empresarial** (HTTP 200)

Registro exitoso de pedido empresarial con múltiples productos

> **Tester prompt:** Registra un pedido como cliente CLI-CORP001 con 50kg de AGS-450, 30kg de AGS-301, 20kg de AGS-127 y 15kg de AGS-205 para el 20 de febrero.

**Request Body:**
```json
{
  "clienteId": "CLI-CORP001",
  "productos": "AGS-450:50kg,AGS-301:30kg,AGS-127:20kg,AGS-205:15kg",
  "fechaEntrega": "2024-02-20"
}
```

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-CORP001"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-02-20"
  },
  {
    "name": "productos",
    "value": "AGS-450:50kg,AGS-301:30kg,AGS-127:20kg,AGS-205:15kg"
  }
]
```

**Response Body:**
```json
{
  "numeroOrden": "ORD-240220-8847",
  "pedidoConfirmado": true,
  "totalFinal": "$1.847.320"
}
```

---

**edge_pedido_cantidad_fraccionaria_minima** (HTTP 200)

Registro con cantidad fraccionaria mínima permitida

> **Tester prompt:** Registra un pedido mínimo como cliente CLI-24680 con solo 0.05kg de AGS-105 para el 14 de febrero.

**Request Body:**
```json
{
  "clienteId": "CLI-24680",
  "productos": "AGS-105:0.05kg",
  "fechaEntrega": "2024-02-14"
}
```

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-24680"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-02-14"
  },
  {
    "name": "productos",
    "value": "AGS-105:0.05kg"
  }
]
```

**Response Body:**
```json
{
  "numeroOrden": "ORD-240214-0051",
  "pedidoConfirmado": true,
  "totalFinal": "$425"
}
```

---

**error_producto_descontinuado** (HTTP 422)

Error por incluir producto descontinuado en el pedido

> **Tester prompt:** Intenta registrar un pedido como cliente CLI-55667 con 2kg de AGS-999 y 1kg de AGS-301 para el 18 de febrero.

**Request Body:**
```json
{
  "clienteId": "CLI-55667",
  "productos": "AGS-999:2kg,AGS-301:1kg",
  "fechaEntrega": "2024-02-18"
}
```

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-55667"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-02-18"
  },
  {
    "name": "productos",
    "value": "AGS-999:2kg,AGS-301:1kg"
  }
]
```

**Response Body:**
```json
{
  "error": true,
  "codigo": "PRODUCTO_DESCONTINUADO",
  "mensaje": "El producto AGS-999 ha sido descontinuado",
  "numeroOrden": null,
  "pedidoConfirmado": false,
  "totalFinal": null
}
```

---

## Dataset: Generated (2026-02-27 00:46)

AI-generated mock data for Agente Comercial Agrosuper

_Source: generated_

### Web Action: `calcularTotalPedido`

**edge_case** (HTTP 200)

Pedido con cantidad cero

**Request Body:**
```json
{
  "productos": "AGS001",
  "cantidades": "0"
}
```

**Request Params:**
```json
[
  {
    "name": "cantidades",
    "value": "0"
  },
  {
    "name": "productos",
    "value": "AGS001"
  }
]
```

**Response Body:**
```json
{
  "detalleProductos": [],
  "sugerenciaVenta": [
    {
      "id": "AGS001",
      "nombre": "Pechuga de Pollo Premium",
      "precio": 4590,
      "razon": "Producto popular en tu zona"
    }
  ],
  "totalAproximado": {
    "subtotal": 0,
    "descuentos": 0,
    "iva": 0,
    "total": 0
  }
}
```

---

**error_case** (HTTP 400)

Producto no disponible en inventario

**Request Body:**
```json
{
  "productos": "AGS999,AGS001",
  "cantidades": "5,2"
}
```

**Request Params:**
```json
[
  {
    "name": "cantidades",
    "value": "5,2"
  },
  {
    "name": "productos",
    "value": "AGS999,AGS001"
  }
]
```

**Response Body:**
```json
{
  "error": true,
  "mensaje": "Producto AGS999 no encontrado en catálogo",
  "codigo": "PRODUCT_NOT_FOUND",
  "detalleProductos": [],
  "sugerenciaVenta": [],
  "totalAproximado": 0
}
```

---

**happy_path** (HTTP 200)

Cálculo exitoso de pedido con múltiples productos

**Request Body:**
```json
{
  "productos": "AGS001,AGS002,AGS003",
  "cantidades": "2,3,1"
}
```

**Request Params:**
```json
[
  {
    "name": "cantidades",
    "value": "2,3,1"
  },
  {
    "name": "productos",
    "value": "AGS001,AGS002,AGS003"
  }
]
```

**Response Body:**
```json
{
  "detalleProductos": [
    {
      "id": "AGS001",
      "nombre": "Pechuga de Pollo Premium",
      "precio": 4590,
      "cantidad": 2,
      "subtotal": 9180
    },
    {
      "id": "AGS002",
      "nombre": "Muslos de Pollo",
      "precio": 2890,
      "cantidad": 3,
      "subtotal": 8670
    },
    {
      "id": "AGS003",
      "nombre": "Pollo Entero",
      "precio": 3200,
      "cantidad": 1,
      "subtotal": 3200
    }
  ],
  "sugerenciaVenta": [
    {
      "id": "AGS004",
      "nombre": "Alitas de Pollo",
      "precio": 2490,
      "razon": "Complementa perfectamente con tu selección"
    }
  ],
  "totalAproximado": {
    "subtotal": 21050,
    "descuentos": 0,
    "iva": 3999,
    "total": 25049
  }
}
```

---

### Web Action: `confirmarPedido`

**edge_case** (HTTP 201)

Pedido confirmado con monto mínimo

**Request Body:**
```json
{
  "clienteId": "CLI11111",
  "pedidoCompleto": "{\"productos\": [{\"id\": \"AGS003\", \"cantidad\": 1}], \"total\": 3200}",
  "fechaEntrega": "2024-01-22"
}
```

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI11111"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-01-22"
  },
  {
    "name": "pedidoCompleto",
    "value": "{\"productos\": [{\"id\": \"AGS003\", \"cantidad\": 1}], \"total\": 3200}"
  }
]
```

**Response Body:**
```json
{
  "fechaConfirmacion": "2024-01-12T16:45:00Z",
  "numeroPedido": "PED001346",
  "resumenPedido": {
    "cliente": "CLI11111",
    "productos": [
      {
        "id": "AGS003",
        "nombre": "Pollo Entero",
        "cantidad": 1,
        "precio": 3200,
        "subtotal": 3200
      }
    ],
    "total": 3200,
    "fechaEntrega": "2024-01-22",
    "estado": "confirmado",
    "metodoPago": "efectivo",
    "advertencia": "Pedido bajo monto mínimo recomendado de $15.000"
  }
}
```

---

**error_case** (HTTP 409)

Error por stock insuficiente al confirmar

**Request Body:**
```json
{
  "clienteId": "CLI67890",
  "pedidoCompleto": "{\"productos\": [{\"id\": \"AGS001\", \"cantidad\": 50}], \"total\": 229500}",
  "fechaEntrega": "2024-01-19"
}
```

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI67890"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-01-19"
  },
  {
    "name": "pedidoCompleto",
    "value": "{\"productos\": [{\"id\": \"AGS001\", \"cantidad\": 50}], \"total\": 229500}"
  }
]
```

**Response Body:**
```json
{
  "error": true,
  "mensaje": "Stock insuficiente para producto AGS001. Stock disponible: 15 kg",
  "codigo": "INSUFFICIENT_STOCK",
  "fechaConfirmacion": null,
  "numeroPedido": null,
  "resumenPedido": null
}
```

---

**happy_path** (HTTP 201)

Confirmación exitosa de pedido completo

**Request Body:**
```json
{
  "clienteId": "CLI12345",
  "pedidoCompleto": "{\"productos\": [{\"id\": \"AGS001\", \"cantidad\": 2}, {\"id\": \"AGS002\", \"cantidad\": 3}], \"total\": 17850}",
  "fechaEntrega": "2024-01-18"
}
```

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI12345"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-01-18"
  },
  {
    "name": "pedidoCompleto",
    "value": "{\"productos\": [{\"id\": \"AGS001\", \"cantidad\": 2}, {\"id\": \"AGS002\", \"cantidad\": 3}], \"total\": 17850}"
  }
]
```

**Response Body:**
```json
{
  "fechaConfirmacion": "2024-01-12T14:30:00Z",
  "numeroPedido": "PED001345",
  "resumenPedido": {
    "cliente": "CLI12345",
    "productos": [
      {
        "id": "AGS001",
        "nombre": "Pechuga de Pollo Premium",
        "cantidad": 2,
        "precio": 4590,
        "subtotal": 9180
      },
      {
        "id": "AGS002",
        "nombre": "Muslos de Pollo",
        "cantidad": 3,
        "precio": 2890,
        "subtotal": 8670
      }
    ],
    "total": 17850,
    "fechaEntrega": "2024-01-18",
    "estado": "confirmado",
    "metodoPago": "transferencia"
  }
}
```

---

### Web Action: `consultarCatalogoProductos`

**edge_case** (HTTP 200)

Consulta de categoría válida pero sin productos disponibles

**Request Params:**
```json
[
  {
    "name": "categoria",
    "value": "cerdo"
  },
  {
    "name": "tipoProducto",
    "value": "costillar"
  }
]
```

**Response Body:**
```json
{
  "productos": [],
  "promociones": [],
  "mensaje": "No hay productos disponibles en esta categoría temporalmente"
}
```

---

**error_case** (HTTP 404)

Categoría no encontrada en el catálogo

**Request Params:**
```json
[
  {
    "name": "categoria",
    "value": "mariscos"
  },
  {
    "name": "tipoProducto",
    "value": "salmon"
  }
]
```

**Response Body:**
```json
{
  "error": true,
  "mensaje": "Categoría 'mariscos' no disponible en nuestro catálogo",
  "codigo": "CAT_NOT_FOUND",
  "productos": [],
  "promociones": []
}
```

---

**happy_path** (HTTP 200)

Consulta exitosa del catálogo de productos cárnicos

**Request Params:**
```json
[
  {
    "name": "categoria",
    "value": "carnes"
  },
  {
    "name": "tipoProducto",
    "value": "pollo"
  }
]
```

**Response Body:**
```json
{
  "productos": [
    {
      "id": "AGS001",
      "nombre": "Pechuga de Pollo Premium",
      "precio": 4590,
      "unidad": "kg",
      "disponibilidad": true,
      "descripcion": "Pechuga de pollo fresca, sin hueso"
    },
    {
      "id": "AGS002",
      "nombre": "Muslos de Pollo",
      "precio": 2890,
      "unidad": "kg",
      "disponibilidad": true,
      "descripcion": "Muslos de pollo frescos con hueso"
    },
    {
      "id": "AGS003",
      "nombre": "Pollo Entero",
      "precio": 3200,
      "unidad": "kg",
      "disponibilidad": true,
      "descripcion": "Pollo entero fresco, peso promedio 2.5kg"
    }
  ],
  "promociones": [
    {
      "id": "PROMO001",
      "descripcion": "20% descuento en compras sobre $50.000",
      "vigencia": "2024-12-31",
      "aplicable": [
        "AGS001",
        "AGS002"
      ]
    }
  ]
}
```

---

### Web Action: `consultarPedidoProgramado`

**edge_case** (HTTP 200)

Cliente nuevo sin historial de pedidos

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI00001"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-01-25"
  }
]
```

**Response Body:**
```json
{
  "historialPedidos": [],
  "pedidoActual": null,
  "proximaEntrega": null,
  "mensaje": "Cliente nuevo sin pedidos previos"
}
```

---

**error_case** (HTTP 404)

Cliente no encontrado en el sistema

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI99999"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-01-20"
  }
]
```

**Response Body:**
```json
{
  "error": true,
  "mensaje": "Cliente no encontrado en el sistema",
  "codigo": "CLIENT_NOT_FOUND",
  "historialPedidos": [],
  "pedidoActual": null,
  "proximaEntrega": null
}
```

---

**happy_path** (HTTP 200)

Consulta exitosa de pedido programado de cliente activo

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI12345"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-01-15"
  }
]
```

**Response Body:**
```json
{
  "historialPedidos": [
    {
      "numeroPedido": "PED001234",
      "fecha": "2024-01-08",
      "total": 45600,
      "estado": "entregado"
    },
    {
      "numeroPedido": "PED001198",
      "fecha": "2024-01-01",
      "total": 38900,
      "estado": "entregado"
    }
  ],
  "pedidoActual": {
    "numeroPedido": "PED001267",
    "fechaCreacion": "2024-01-12",
    "productos": [
      {
        "id": "AGS001",
        "nombre": "Pechuga de Pollo Premium",
        "cantidad": 3,
        "precio": 4590
      }
    ],
    "total": 13770,
    "estado": "confirmado"
  },
  "proximaEntrega": {
    "fecha": "2024-01-15",
    "horaEstimada": "10:00-12:00",
    "direccion": "Av. Las Condes 1234, Las Condes, Santiago"
  }
}
```

---
