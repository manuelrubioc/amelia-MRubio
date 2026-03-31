# Functions: Agente de Gestión de Quejas Rappi

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
