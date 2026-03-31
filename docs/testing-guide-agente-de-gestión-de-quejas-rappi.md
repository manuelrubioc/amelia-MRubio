# Testing Guide: Agente de Gestión de Quejas Rappi

Test scenarios with full request/response examples.

## Dataset: Generated (2026-03-30 02:19) **(Active)**

AI-generated mock data for Agente de Gestión de Quejas Rappi

_Source: generated_

### Web Action: `analizarImagenDisputa`

**damaged_product** (HTTP 200)

Producto dañado o en mal estado detectado en imagen

> **Tester prompt:** Dile al agente que analice una imagen donde se ve una ensalada en mal estado o dañada.

**Request Body:**
```json
{
  "imagen_url": "https://storage.rappi.com/disputes/img_damaged_20240315.jpg",
  "numero_pedido": "RP2024031505",
  "tipo_producto_esperado": "ensalada_cesar"
}
```

**Request Params:**
```json
[
  {
    "name": "imagenUrl",
    "value": "https://storage.rappi.com/disputes/img_damaged_20240315.jpg"
  },
  {
    "name": "numeroPedido",
    "value": "RP2024031505"
  },
  {
    "name": "tipoProductoEsperado",
    "value": "ensalada_cesar"
  }
]
```

**Response Body:**
```json
{
  "confianza_analisis": 0.87,
  "discrepancia_detectada": true,
  "recomendacion_compensacion": "reembolso_completo",
  "tipo_producto_detectado": "ensalada_cesar_dañada"
}
```

---

**edge_case** (HTTP 200)

Imagen muy borrosa con confianza de análisis muy baja

> **Tester prompt:** Pide al agente analizar una imagen muy borrosa donde no se puede identificar claramente el producto.

**Request Body:**
```json
{
  "imagen_url": "https://storage.rappi.com/disputes/img_borrosa_20240315.jpg",
  "numero_pedido": "RP2024031503",
  "tipo_producto_esperado": "tacos_pastor_orden"
}
```

**Request Params:**
```json
[
  {
    "name": "imagenUrl",
    "value": "https://storage.rappi.com/disputes/img_borrosa_20240315.jpg"
  },
  {
    "name": "numeroPedido",
    "value": "RP2024031503"
  },
  {
    "name": "tipoProductoEsperado",
    "value": "tacos_pastor_orden"
  }
]
```

**Response Body:**
```json
{
  "confianza_analisis": 0.15,
  "discrepancia_detectada": false,
  "recomendacion_compensacion": "revision_manual_requerida",
  "tipo_producto_detectado": "no_identificable"
}
```

---

**error_case** (HTTP 404)

Error al acceder a imagen no disponible o corrupta

> **Tester prompt:** Solicita al agente analizar una imagen que no se puede cargar o está corrupta.

**Request Body:**
```json
{
  "imagen_url": "https://storage.rappi.com/disputes/imagen_corrupta_404.jpg",
  "numero_pedido": "RP2024031502",
  "tipo_producto_esperado": "hamburguesa_doble"
}
```

**Request Params:**
```json
[
  {
    "name": "imagenUrl",
    "value": "https://storage.rappi.com/disputes/imagen_corrupta_404.jpg"
  },
  {
    "name": "numeroPedido",
    "value": "RP2024031502"
  },
  {
    "name": "tipoProductoEsperado",
    "value": "hamburguesa_doble"
  }
]
```

**Response Body:**
```json
{
  "error": "imagen_no_accesible",
  "mensaje": "No se puede acceder a la imagen proporcionada. Verifique que la URL sea válida y que el archivo no esté corrupto",
  "codigo_error": "IMG_404"
}
```

---

**happy_path** (HTTP 200)

Análisis exitoso detectando discrepancia en producto entregado

> **Tester prompt:** Dile al agente que analice una imagen donde se entregó una pizza pepperoni mediana en lugar de hawaiana grande.

**Request Body:**
```json
{
  "imagen_url": "https://storage.rappi.com/disputes/img_20240315_143022.jpg",
  "numero_pedido": "RP2024031501",
  "tipo_producto_esperado": "pizza_hawaiana_grande"
}
```

**Request Params:**
```json
[
  {
    "name": "imagenUrl",
    "value": "https://storage.rappi.com/disputes/img_20240315_143022.jpg"
  },
  {
    "name": "numeroPedido",
    "value": "RP2024031501"
  },
  {
    "name": "tipoProductoEsperado",
    "value": "pizza_hawaiana_grande"
  }
]
```

**Response Body:**
```json
{
  "confianza_analisis": 0.89,
  "discrepancia_detectada": true,
  "recomendacion_compensacion": "reembolso_parcial_60",
  "tipo_producto_detectado": "pizza_pepperoni_mediana"
}
```

---

**perfect_match** (HTTP 200)

Producto entregado coincide exactamente con lo esperado

> **Tester prompt:** Solicita al agente analizar una imagen donde el producto entregado coincide perfectamente con lo pedido.

**Request Body:**
```json
{
  "imagen_url": "https://storage.rappi.com/disputes/img_20240315_150000.jpg",
  "numero_pedido": "RP2024031504",
  "tipo_producto_esperado": "sushi_salmon_roll"
}
```

**Request Params:**
```json
[
  {
    "name": "imagenUrl",
    "value": "https://storage.rappi.com/disputes/img_20240315_150000.jpg"
  },
  {
    "name": "numeroPedido",
    "value": "RP2024031504"
  },
  {
    "name": "tipoProductoEsperado",
    "value": "sushi_salmon_roll"
  }
]
```

**Response Body:**
```json
{
  "confianza_analisis": 0.96,
  "discrepancia_detectada": false,
  "recomendacion_compensacion": "sin_compensacion",
  "tipo_producto_detectado": "sushi_salmon_roll"
}
```

---

### Web Action: `clasificarInteraccionSocial`

**edge_case** (HTTP 200)

Mensaje vacío o muy corto para clasificación

> **Tester prompt:** Pide al agente que clasifique un mensaje muy corto de Facebook que solo dice 'ok'.

**Request Body:**
```json
{
  "contenido_mensaje": "ok",
  "red_social": "facebook",
  "usuario_id": "user_mx_000001"
}
```

**Request Params:**
```json
[
  {
    "name": "contenidoMensaje",
    "value": "ok"
  },
  {
    "name": "redSocial",
    "value": "facebook"
  },
  {
    "name": "usuarioId",
    "value": "user_mx_000001"
  }
]
```

**Response Body:**
```json
{
  "categoria_problema": "sin_clasificar",
  "nivel_urgencia": "bajo",
  "requiere_escalacion": false,
  "tipo_clasificacion": "mensaje_insuficiente"
}
```

---

**error_case** (HTTP 400)

Error al procesar contenido con caracteres especiales no soportados

> **Tester prompt:** Dile al agente que clasifique un mensaje de Instagram con muchos emojis y caracteres especiales.

**Request Body:**
```json
{
  "contenido_mensaje": "🔥💯🚀 ¡¡¡RAPPI NO SIRVE!!! 😡😡😡 #FAIL #PESIMO ████████",
  "red_social": "instagram",
  "usuario_id": "user_mx_456789"
}
```

**Request Params:**
```json
[
  {
    "name": "contenidoMensaje",
    "value": "🔥💯🚀 ¡¡¡RAPPI NO SIRVE!!! 😡😡😡 #FAIL #PESIMO ████████"
  },
  {
    "name": "redSocial",
    "value": "instagram"
  },
  {
    "name": "usuarioId",
    "value": "user_mx_456789"
  }
]
```

**Response Body:**
```json
{
  "error": "contenido_no_procesable",
  "mensaje": "El contenido contiene caracteres especiales que no pueden ser procesados por el sistema de clasificación",
  "codigo_error": "SOCIAL_001"
}
```

---

**happy_path** (HTTP 200)

Clasificación exitosa de queja sobre pedido tardío en Twitter

> **Tester prompt:** Dile al agente que necesitas clasificar una queja de Twitter sobre un pedido que lleva 2 horas de retraso.

**Request Body:**
```json
{
  "contenido_mensaje": "@RappiMexico Mi pedido #RP2024031501 lleva 2 horas de retraso y no puedo contactar al repartidor. Necesito una solución urgente por favor.",
  "red_social": "twitter",
  "usuario_id": "user_mx_789123"
}
```

**Request Params:**
```json
[
  {
    "name": "contenidoMensaje",
    "value": "@RappiMexico Mi pedido #RP2024031501 lleva 2 horas de retraso y no puedo contactar al repartidor. Necesito una solución urgente por favor."
  },
  {
    "name": "redSocial",
    "value": "twitter"
  },
  {
    "name": "usuarioId",
    "value": "user_mx_789123"
  }
]
```

**Response Body:**
```json
{
  "categoria_problema": "entrega_tardia",
  "nivel_urgencia": "alto",
  "requiere_escalacion": true,
  "tipo_clasificacion": "queja_operativa"
}
```

---

**positive_feedback** (HTTP 200)

Clasificación de comentario positivo sobre el servicio

> **Tester prompt:** Solicita al agente clasificar un comentario positivo de Twitter donde alguien agradece el buen servicio.

**Request Body:**
```json
{
  "contenido_mensaje": "Excelente servicio de Rappi hoy! Mi pedido llegó en 25 minutos y el repartidor fue muy amable. Gracias @RappiMexico 👏",
  "red_social": "twitter",
  "usuario_id": "user_mx_555888"
}
```

**Request Params:**
```json
[
  {
    "name": "contenidoMensaje",
    "value": "Excelente servicio de Rappi hoy! Mi pedido llegó en 25 minutos y el repartidor fue muy amable. Gracias @RappiMexico 👏"
  },
  {
    "name": "redSocial",
    "value": "twitter"
  },
  {
    "name": "usuarioId",
    "value": "user_mx_555888"
  }
]
```

**Response Body:**
```json
{
  "categoria_problema": "feedback_positivo",
  "nivel_urgencia": "bajo",
  "requiere_escalacion": false,
  "tipo_clasificacion": "reconocimiento"
}
```

---

**spam_detection** (HTTP 200)

Detección de mensaje spam o promocional

> **Tester prompt:** Pide al agente clasificar un mensaje de Facebook que parece ser spam promocional.

**Request Body:**
```json
{
  "contenido_mensaje": "¡GANA DINERO FÁCIL! Únete a nuestro sistema de referidos y gana $5000 pesos al día sin trabajar. Contacta por WhatsApp 55-1234-5678",
  "red_social": "facebook",
  "usuario_id": "user_mx_spam001"
}
```

**Request Params:**
```json
[
  {
    "name": "contenidoMensaje",
    "value": "¡GANA DINERO FÁCIL! Únete a nuestro sistema de referidos y gana $5000 pesos al día sin trabajar. Contacta por WhatsApp 55-1234-5678"
  },
  {
    "name": "redSocial",
    "value": "facebook"
  },
  {
    "name": "usuarioId",
    "value": "user_mx_spam001"
  }
]
```

**Response Body:**
```json
{
  "categoria_problema": "spam",
  "nivel_urgencia": "bajo",
  "requiere_escalacion": false,
  "tipo_clasificacion": "contenido_no_relacionado"
}
```

---

### Web Action: `consultarDetallesPedido`

**cancelled_order** (HTTP 200)

Pedido cancelado con detalles de reembolso

> **Tester prompt:** Dile al agente que consulte el pedido cancelado RP2024031506 para verificar el estado del reembolso.

**Request Body:**
```json
{}
```

**Request Params:**
```json
[
  {
    "name": "numeroPedido",
    "value": "RP2024031506"
  },
  {
    "name": "numero_pedido",
    "value": "RP2024031506"
  },
  {
    "name": "telefonoCliente",
    "value": "+52-55-2222-3333"
  },
  {
    "name": "telefono_cliente",
    "value": "+52-55-2222-3333"
  }
]
```

**Response Body:**
```json
{
  "estado_pedido": "cancelado",
  "monto_total": "$423.75 MXN",
  "productos_ordenados": [
    {
      "nombre": "Hamburguesa Doble",
      "cantidad": 2,
      "precio": "$189.50"
    },
    {
      "nombre": "Papas Grandes",
      "cantidad": 1,
      "precio": "$67.25"
    }
  ],
  "repartidor_asignado": null,
  "tiempo_entrega": "Pedido cancelado - Reembolso procesado en 3-5 días hábiles"
}
```

---

**edge_case** (HTTP 200)

Pedido muy antiguo con información limitada

> **Tester prompt:** Solicita al agente consultar un pedido muy antiguo del 2023 donde la información puede estar limitada.

**Request Body:**
```json
{}
```

**Request Params:**
```json
[
  {
    "name": "numeroPedido",
    "value": "RP2023010101"
  },
  {
    "name": "numero_pedido",
    "value": "RP2023010101"
  },
  {
    "name": "telefonoCliente",
    "value": "+52-55-5555-1111"
  },
  {
    "name": "telefono_cliente",
    "value": "+52-55-5555-1111"
  }
]
```

**Response Body:**
```json
{
  "estado_pedido": "entregado",
  "monto_total": "$156.00 MXN",
  "productos_ordenados": [
    {
      "nombre": "Información no disponible",
      "cantidad": 0,
      "precio": "$0.00"
    }
  ],
  "repartidor_asignado": {
    "nombre": "No disponible",
    "telefono": "",
    "calificacion": 0
  },
  "tiempo_entrega": "Pedido histórico - información limitada"
}
```

---

**error_case** (HTTP 404)

Pedido no encontrado o número inválido

> **Tester prompt:** Pide al agente consultar un pedido con número inexistente RP9999999999.

**Request Body:**
```json
{}
```

**Request Params:**
```json
[
  {
    "name": "numeroPedido",
    "value": "RP9999999999"
  },
  {
    "name": "numero_pedido",
    "value": "RP9999999999"
  },
  {
    "name": "telefonoCliente",
    "value": "+52-55-0000-0000"
  },
  {
    "name": "telefono_cliente",
    "value": "+52-55-0000-0000"
  }
]
```

**Response Body:**
```json
{
  "error": "pedido_no_encontrado",
  "mensaje": "No se encontró un pedido con el número proporcionado o no corresponde al teléfono registrado",
  "codigo_error": "ORDER_404"
}
```

---

**happy_path** (HTTP 200)

Consulta exitosa de pedido activo con todos los detalles

> **Tester prompt:** Dile al agente que consulte los detalles del pedido RP2024031501 con el teléfono +52-55-1234-5678.

**Request Body:**
```json
{}
```

**Request Params:**
```json
[
  {
    "name": "numeroPedido",
    "value": "RP2024031501"
  },
  {
    "name": "numero_pedido",
    "value": "RP2024031501"
  },
  {
    "name": "telefonoCliente",
    "value": "+52-55-1234-5678"
  },
  {
    "name": "telefono_cliente",
    "value": "+52-55-1234-5678"
  }
]
```

**Response Body:**
```json
{
  "estado_pedido": "en_camino",
  "monto_total": "$347.50 MXN",
  "productos_ordenados": [
    {
      "nombre": "Pizza Hawaiana Grande",
      "cantidad": 1,
      "precio": "$285.00"
    },
    {
      "nombre": "Coca Cola 600ml",
      "cantidad": 2,
      "precio": "$31.25"
    }
  ],
  "repartidor_asignado": {
    "nombre": "Carlos Mendoza",
    "telefono": "+52-55-9876-5432",
    "calificacion": 4.8
  },
  "tiempo_entrega": "15-25 minutos"
}
```

---

**large_order** (HTTP 200)

Pedido grande con múltiples productos y repartidores

> **Tester prompt:** Pide al agente consultar el pedido grande RP2024031507 que tiene múltiples productos.

**Request Body:**
```json
{}
```

**Request Params:**
```json
[
  {
    "name": "numeroPedido",
    "value": "RP2024031507"
  },
  {
    "name": "numero_pedido",
    "value": "RP2024031507"
  },
  {
    "name": "telefonoCliente",
    "value": "+52-55-4444-5555"
  },
  {
    "name": "telefono_cliente",
    "value": "+52-55-4444-5555"
  }
]
```

**Response Body:**
```json
{
  "estado_pedido": "preparando",
  "monto_total": "$1,247.80 MXN",
  "productos_ordenados": [
    {
      "nombre": "Pizza Margherita Grande",
      "cantidad": 3,
      "precio": "$267.00"
    },
    {
      "nombre": "Alitas BBQ (12 pzs)",
      "cantidad": 2,
      "precio": "$178.50"
    },
    {
      "nombre": "Cerveza Corona 355ml",
      "cantidad": 6,
      "precio": "$234.00"
    },
    {
      "nombre": "Helado Napolitano 1L",
      "cantidad": 1,
      "precio": "$89.30"
    }
  ],
  "repartidor_asignado": {
    "nombre": "Ana Rodríguez",
    "telefono": "+52-55-7777-8888",
    "calificacion": 4.9
  },
  "tiempo_entrega": "45-60 minutos (pedido grande)"
}
```

---

### Web Action: `crearTicketSoporte`

**account_security** (HTTP 201)

Problema de seguridad de cuenta con máxima prioridad

> **Tester prompt:** Informa al agente sobre cargos no autorizados en tu cuenta y solicita crear un ticket de seguridad urgente.

**Request Body:**
```json
{
  "tipo_problema": "seguridad_cuenta",
  "descripcion_detallada": "Detecté cargos no autorizados en mi cuenta por $1,250 pesos. Hay pedidos que no realicé a direcciones que no conozco. Creo que mi cuenta fue comprometida. Necesito bloquear mi cuenta inmediatamente y revisar todos los movimientos de los últimos 7 días.",
  "cliente_id": "CLI_MX_888999",
  "prioridad": "critica",
  "evidencia_adjunta": "estado_cuenta.pdf,pedidos_no_autorizados.jpg"
}
```

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI_MX_888999"
  },
  {
    "name": "descripcionDetallada",
    "value": "Detecté cargos no autorizados en mi cuenta por $1,250 pesos. Hay pedidos que no realicé a direcciones que no conozco. Creo que mi cuenta fue comprometida. Necesito bloquear mi cuenta inmediatamente y revisar todos los movimientos de los últimos 7 días."
  },
  {
    "name": "evidenciaAdjunta",
    "value": "estado_cuenta.pdf,pedidos_no_autorizados.jpg"
  },
  {
    "name": "prioridad",
    "value": "critica"
  },
  {
    "name": "tipoProblema",
    "value": "seguridad_cuenta"
  }
]
```

**Response Body:**
```json
{
  "agente_asignado": {
    "nombre": "Equipo Seguridad",
    "id": "SEC_001",
    "especialidad": "seguridad"
  },
  "estado_ticket": "escalado_urgente",
  "numero_ticket": "TKT-2024-031504",
  "tiempo_estimado_resolucion": "30-60 minutos"
}
```

---

**edge_case** (HTTP 201)

Ticket con descripción mínima y sin evidencia

> **Tester prompt:** Pide al agente crear un ticket con muy poca información, solo diciendo 'Problema'.

**Request Body:**
```json
{
  "tipo_problema": "otro",
  "descripcion_detallada": "Problema",
  "cliente_id": "CLI_MX_000001",
  "prioridad": "baja",
  "evidencia_adjunta": ""
}
```

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI_MX_000001"
  },
  {
    "name": "descripcionDetallada",
    "value": "Problema"
  },
  {
    "name": "evidenciaAdjunta",
    "value": ""
  },
  {
    "name": "prioridad",
    "value": "baja"
  },
  {
    "name": "tipoProblema",
    "value": "otro"
  }
]
```

**Response Body:**
```json
{
  "agente_asignado": {
    "nombre": "Sistema Automático",
    "id": "AUTO_001",
    "especialidad": "general"
  },
  "estado_ticket": "pendiente_informacion",
  "numero_ticket": "TKT-2024-031502",
  "tiempo_estimado_resolucion": "24-48 horas"
}
```

---

**error_case** (HTTP 400)

Error por cliente ID inválido o no encontrado

> **Tester prompt:** Solicita al agente crear un ticket con un ID de cliente que no existe en el sistema.

**Request Body:**
```json
{
  "tipo_problema": "cobro_incorrecto",
  "descripcion_detallada": "Me cobraron $500 pesos de más en mi tarjeta",
  "cliente_id": "CLI_INVALID_999",
  "prioridad": "media",
  "evidencia_adjunta": ""
}
```

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI_INVALID_999"
  },
  {
    "name": "descripcionDetallada",
    "value": "Me cobraron $500 pesos de más en mi tarjeta"
  },
  {
    "name": "evidenciaAdjunta",
    "value": ""
  },
  {
    "name": "prioridad",
    "value": "media"
  },
  {
    "name": "tipoProblema",
    "value": "cobro_incorrecto"
  }
]
```

**Response Body:**
```json
{
  "error": "cliente_no_valido",
  "mensaje": "El ID de cliente proporcionado no es válido o no existe en nuestro sistema",
  "codigo_error": "CLIENT_400"
}
```

---

**happy_path** (HTTP 201)

Creación exitosa de ticket por problema de entrega

> **Tester prompt:** Dile al agente que cree un ticket por un pedido que lleva más de 2 horas de retraso y es urgente.

**Request Body:**
```json
{
  "tipo_problema": "entrega_tardia",
  "descripcion_detallada": "Mi pedido #RP2024031501 fue programado para las 14:30 pero ya son las 16:45 y no ha llegado. El repartidor no contesta el teléfono y la app muestra que sigue en camino. Necesito una solución urgente ya que es para una reunión de trabajo.",
  "cliente_id": "CLI_MX_789456",
  "prioridad": "alta",
  "evidencia_adjunta": "screenshot_pedido_app.jpg"
}
```

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI_MX_789456"
  },
  {
    "name": "descripcionDetallada",
    "value": "Mi pedido #RP2024031501 fue programado para las 14:30 pero ya son las 16:45 y no ha llegado. El repartidor no contesta el teléfono y la app muestra que sigue en camino. Necesito una solución urgente ya que es para una reunión de trabajo."
  },
  {
    "name": "evidenciaAdjunta",
    "value": "screenshot_pedido_app.jpg"
  },
  {
    "name": "prioridad",
    "value": "alta"
  },
  {
    "name": "tipoProblema",
    "value": "entrega_tardia"
  }
]
```

**Response Body:**
```json
{
  "agente_asignado": {
    "nombre": "María González",
    "id": "AGT_001",
    "especialidad": "operaciones"
  },
  "estado_ticket": "abierto",
  "numero_ticket": "TKT-2024-031501",
  "tiempo_estimado_resolucion": "2-4 horas"
}
```

---

**refund_request** (HTTP 201)

Solicitud de reembolso por producto defectuoso

> **Tester prompt:** Dile al agente que necesitas crear un ticket para solicitar reembolso por una pizza que llegó en mal estado.

**Request Body:**
```json
{
  "tipo_problema": "producto_defectuoso",
  "descripcion_detallada": "Pedí una pizza hawaiana pero llegó quemada y fría. Además faltaba el jamón y la piña. La pizza está completamente incomible. Solicito reembolso completo del pedido #RP2024031508. Es la tercera vez que pasa esto con este restaurante.",
  "cliente_id": "CLI_MX_555777",
  "prioridad": "alta",
  "evidencia_adjunta": "foto_pizza_defectuosa.jpg,comprobante_pago.pdf"
}
```

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI_MX_555777"
  },
  {
    "name": "descripcionDetallada",
    "value": "Pedí una pizza hawaiana pero llegó quemada y fría. Además faltaba el jamón y la piña. La pizza está completamente incomible. Solicito reembolso completo del pedido #RP2024031508. Es la tercera vez que pasa esto con este restaurante."
  },
  {
    "name": "evidenciaAdjunta",
    "value": "foto_pizza_defectuosa.jpg,comprobante_pago.pdf"
  },
  {
    "name": "prioridad",
    "value": "alta"
  },
  {
    "name": "tipoProblema",
    "value": "producto_defectuoso"
  }
]
```

**Response Body:**
```json
{
  "agente_asignado": {
    "nombre": "Roberto Sánchez",
    "id": "AGT_005",
    "especialidad": "reembolsos"
  },
  "estado_ticket": "en_revision",
  "numero_ticket": "TKT-2024-031503",
  "tiempo_estimado_resolucion": "4-6 horas"
}
```

---
