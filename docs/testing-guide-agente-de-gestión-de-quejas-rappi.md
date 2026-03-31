# Testing Guide: Agente de Gestión de Quejas Rappi

> Generated (2026-03-30 02:19)

## analizarImagenDisputa

### damaged_product ✅ (200)

Producto dañado o en mal estado detectado en imagen

**How to test:** Dile al agente que analice una imagen donde se ve una ensalada en mal estado o dañada.

### edge_case ✅ (200)

Imagen muy borrosa con confianza de análisis muy baja

**How to test:** Pide al agente analizar una imagen muy borrosa donde no se puede identificar claramente el producto.

### error_case ❌ (404)

Error al acceder a imagen no disponible o corrupta

**How to test:** Solicita al agente analizar una imagen que no se puede cargar o está corrupta.

### happy_path ✅ (200)

Análisis exitoso detectando discrepancia en producto entregado

**How to test:** Dile al agente que analice una imagen donde se entregó una pizza pepperoni mediana en lugar de hawaiana grande.

### perfect_match ✅ (200)

Producto entregado coincide exactamente con lo esperado

**How to test:** Solicita al agente analizar una imagen donde el producto entregado coincide perfectamente con lo pedido.

## clasificarInteraccionSocial

### edge_case ✅ (200)

Mensaje vacío o muy corto para clasificación

**How to test:** Pide al agente que clasifique un mensaje muy corto de Facebook que solo dice 'ok'.

### error_case ❌ (400)

Error al procesar contenido con caracteres especiales no soportados

**How to test:** Dile al agente que clasifique un mensaje de Instagram con muchos emojis y caracteres especiales.

### happy_path ✅ (200)

Clasificación exitosa de queja sobre pedido tardío en Twitter

**How to test:** Dile al agente que necesitas clasificar una queja de Twitter sobre un pedido que lleva 2 horas de retraso.

### positive_feedback ✅ (200)

Clasificación de comentario positivo sobre el servicio

**How to test:** Solicita al agente clasificar un comentario positivo de Twitter donde alguien agradece el buen servicio.

### spam_detection ✅ (200)

Detección de mensaje spam o promocional

**How to test:** Pide al agente clasificar un mensaje de Facebook que parece ser spam promocional.

## consultarDetallesPedido

### cancelled_order ✅ (200)

Pedido cancelado con detalles de reembolso

**How to test:** Dile al agente que consulte el pedido cancelado RP2024031506 para verificar el estado del reembolso.

### edge_case ✅ (200)

Pedido muy antiguo con información limitada

**How to test:** Solicita al agente consultar un pedido muy antiguo del 2023 donde la información puede estar limitada.

### error_case ❌ (404)

Pedido no encontrado o número inválido

**How to test:** Pide al agente consultar un pedido con número inexistente RP9999999999.

### happy_path ✅ (200)

Consulta exitosa de pedido activo con todos los detalles

**How to test:** Dile al agente que consulte los detalles del pedido RP2024031501 con el teléfono +52-55-1234-5678.

### large_order ✅ (200)

Pedido grande con múltiples productos y repartidores

**How to test:** Pide al agente consultar el pedido grande RP2024031507 que tiene múltiples productos.

## crearTicketSoporte

### account_security ✅ (201)

Problema de seguridad de cuenta con máxima prioridad

**How to test:** Informa al agente sobre cargos no autorizados en tu cuenta y solicita crear un ticket de seguridad urgente.

### edge_case ✅ (201)

Ticket con descripción mínima y sin evidencia

**How to test:** Pide al agente crear un ticket con muy poca información, solo diciendo 'Problema'.

### error_case ❌ (400)

Error por cliente ID inválido o no encontrado

**How to test:** Solicita al agente crear un ticket con un ID de cliente que no existe en el sistema.

### happy_path ✅ (201)

Creación exitosa de ticket por problema de entrega

**How to test:** Dile al agente que cree un ticket por un pedido que lleva más de 2 horas de retraso y es urgente.

### refund_request ✅ (201)

Solicitud de reembolso por producto defectuoso

**How to test:** Dile al agente que necesitas crear un ticket para solicitar reembolso por una pizza que llegó en mal estado.
