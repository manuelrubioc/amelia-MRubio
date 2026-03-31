# Functions & Web Actions: Asistente de Citas Bancoppel

## Functions

## buscarSucursales

Busca sucursales Bancoppel disponibles según la ubicación del cliente

| Setting | Value |
|---------|-------|
| Action Type | `CONSUME_WS_ACTION` |
| WS Action | `buscarSucursales` |
| Requires Confirmation | No |

### Input Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `ciudad` | STRING | Yes | Ciudad donde buscar sucursales |
| `codigoPostal` | STRING | No | Código postal para búsqueda más precisa |
| `activas` | BOOLEAN | No | (auto-detected from web action body) |

### Output Parameters

| Name | Description |
|------|-------------|
| `sucursales` | Lista de sucursales disponibles |
| `totalSucursales` | Número total de sucursales encontradas |

---

## consultarHorarios

Consulta horarios disponibles para una sucursal específica

| Setting | Value |
|---------|-------|
| Action Type | `CONSUME_WS_ACTION` |
| WS Action | `consultarHorarios` |
| Requires Confirmation | No |

### Input Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `sucursalId` | STRING | No | ID de la sucursal seleccionada |
| `fecha` | STRING | No | Fecha deseada para la cita |

### Output Parameters

| Name | Description |
|------|-------------|
| `horariosDisponibles` | Lista de horarios disponibles |
| `asesoresDisponibles` | Lista de asesores disponibles |

---

## reservarCita

Reserva una cita con los datos del cliente

| Setting | Value |
|---------|-------|
| Action Type | `CONSUME_WS_ACTION` |
| WS Action | `reservarCita` |
| Requires Confirmation | No |

### Input Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `nombreCliente` | STRING | No | Nombre completo del cliente |
| `telefono` | STRING | No | Número de teléfono del cliente |
| `documentoId` | STRING | No | Documento de identidad del cliente |
| `sucursalId` | STRING | No | ID de la sucursal |
| `fecha` | STRING | No | Fecha de la cita |
| `hora` | STRING | No | Hora de la cita |
| `asesorId` | STRING | No | ID del asesor asignado |

### Output Parameters

| Name | Description |
|------|-------------|
| `numeroConfirmacion` | Número de confirmación de la cita |
| `estadoReserva` | Estado de la reserva |
| `detallesCita` | Detalles completos de la cita |

---

## consultarServicios

Consulta información sobre servicios bancarios disponibles

| Setting | Value |
|---------|-------|
| Action Type | `CONSUME_WS_ACTION` |
| WS Action | `consultarServicios` |
| Requires Confirmation | No |

### Input Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `tipoServicio` | STRING | No | Tipo de servicio bancario consultado |

### Output Parameters

| Name | Description |
|------|-------------|
| `servicios` | Lista de servicios disponibles |
| `descripcionServicios` | Descripción detallada de servicios |

---

## Web Actions

### `POST` buscarSucursales

**URL:** `https://a188cc91-0712-4cb3-b901-c5304b9cf79b.mock.pstmn.io/buscarSucursales`

**Request Body:**
```json
{"ciudad": "${ciudad}", "codigoPostal": "${codigoPostal}", "activas": "${activas}"}
```

**Request Params:**
```json
[
  {
    "name": "ciudad",
    "value": "{{ciudad}}",
    "secure": false
  },
  {
    "name": "codigoPostal",
    "value": "{{codigoPostal}}",
    "secure": false
  },
  {
    "name": "activas",
    "value": "{{activas}}",
    "secure": false
  }
]
```

**Response Mappings:**
```json
{
  "OK": [
    {
      "variableName": "sucursales",
      "extractionExpression": "$.sucursales"
    },
    {
      "variableName": "totalSucursales",
      "extractionExpression": "$.totalSucursales"
    }
  ]
}
```

---

### `POST` consultarHorarios

**URL:** `https://a188cc91-0712-4cb3-b901-c5304b9cf79b.mock.pstmn.io/consultarHorarios`

**Request Body:**
```json
{"sucursalId": "${sucursalId}", "fecha": "${fecha}"}
```

**Request Params:**
```json
[
  {
    "name": "sucursalId",
    "value": "{{sucursalId}}",
    "secure": false
  },
  {
    "name": "fecha",
    "value": "{{fecha}}",
    "secure": false
  }
]
```

**Response Mappings:**
```json
{
  "OK": [
    {
      "variableName": "horariosDisponibles",
      "extractionExpression": "$.horariosDisponibles"
    },
    {
      "variableName": "asesoresDisponibles",
      "extractionExpression": "$.asesoresDisponibles"
    }
  ]
}
```

---

### `POST` reservarCita

**URL:** `https://a188cc91-0712-4cb3-b901-c5304b9cf79b.mock.pstmn.io/reservarCita`

**Request Body:**
```json
{"cliente": {"nombre": "${nombreCliente}", "telefono": "${telefono}", "documento": "${documentoId}"}, "cita": {"sucursalId": "${sucursalId}", "fecha": "${fecha}", "hora": "${hora}", "asesorId": "${asesorId}"}}
```

**Request Params:**
```json
[
  {
    "name": "nombreCliente",
    "value": "{{nombreCliente}}",
    "secure": false
  },
  {
    "name": "telefono",
    "value": "{{telefono}}",
    "secure": false
  },
  {
    "name": "documentoId",
    "value": "{{documentoId}}",
    "secure": false
  },
  {
    "name": "sucursalId",
    "value": "{{sucursalId}}",
    "secure": false
  },
  {
    "name": "fecha",
    "value": "{{fecha}}",
    "secure": false
  },
  {
    "name": "hora",
    "value": "{{hora}}",
    "secure": false
  },
  {
    "name": "asesorId",
    "value": "{{asesorId}}",
    "secure": false
  }
]
```

**Response Mappings:**
```json
{
  "OK": [
    {
      "variableName": "numeroConfirmacion",
      "extractionExpression": "$.numeroConfirmacion"
    },
    {
      "variableName": "estadoReserva",
      "extractionExpression": "$.estadoReserva"
    },
    {
      "variableName": "detallesCita",
      "extractionExpression": "$.detallesCita"
    }
  ]
}
```

---

### `GET` consultarServicios

**URL:** `https://a188cc91-0712-4cb3-b901-c5304b9cf79b.mock.pstmn.io/consultarServicios`

**Request Body:**
```json
{"tipo": "${tipoServicio}"}
```

**Request Params:**
```json
[
  {
    "name": "tipoServicio",
    "value": "{{tipoServicio}}",
    "secure": false
  }
]
```

**Response Mappings:**
```json
{
  "OK": [
    {
      "variableName": "servicios",
      "extractionExpression": "$.servicios"
    },
    {
      "variableName": "descripcionServicios",
      "extractionExpression": "$.descripcionServicios"
    }
  ]
}
```

---
