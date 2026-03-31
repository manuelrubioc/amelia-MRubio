# Functions & Web Actions: Asistente de Ventas Tarjetas Bancoppel

## Functions

## consultarProductosTarjetas

Consulta el catálogo actualizado de tarjetas de crédito y débito disponibles con sus características, beneficios y requisitos

| Setting | Value |
|---------|-------|
| Action Type | `ConsumeWebService` |
| WS Action | `consultarProductosTarjetas` |
| Requires Confirmation | No |

### Input Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `tipoTarjeta` | STRING | No | Tipo de tarjeta: credito o debito |
| `rangoIngresos` | STRING | No | Rango de ingresos del cliente |

### Output Parameters

| Name | Description |
|------|-------------|
| `productos` | Lista de productos de tarjetas disponibles |
| `recomendacion` | Producto recomendado basado en el perfil |

---

## evaluarPerfilCrediticio

Realiza una pre-evaluación crediticia básica del cliente

| Setting | Value |
|---------|-------|
| Action Type | `ConsumeWebService` |
| WS Action | `evaluarPerfilCrediticio` |
| Requires Confirmation | No |

### Input Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `rfc` | STRING | No | RFC del cliente |
| `curp` | STRING | No | CURP del cliente |
| `ingresosMensuales` | STRING | No | Ingresos mensuales declarados |

### Output Parameters

| Name | Description |
|------|-------------|
| `scoreCredito` | Puntuación crediticia |
| `productosElegibles` | Lista de productos para los que califica |
| `limiteAprobado` | Límite de crédito pre-aprobado |

---

## calcularBeneficios

Calcula los beneficios y recompensas específicos para un producto y perfil de cliente

| Setting | Value |
|---------|-------|
| Action Type | `ConsumeWebService` |
| WS Action | `calcularBeneficios` |
| Requires Confirmation | No |

### Input Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `productoId` | STRING | No | Identificador del producto de tarjeta |
| `gastoMensualEstimado` | STRING | No | Gasto mensual estimado del cliente |

### Output Parameters

| Name | Description |
|------|-------------|
| `cashbackAnual` | Cashback anual estimado |
| `puntosRecompensa` | Puntos de recompensa anuales |
| `ahorroAnualidad` | Ahorro en anualidad y comisiones |

---

## registrarLeadVentas

Registra un lead calificado en el sistema de ventas

| Setting | Value |
|---------|-------|
| Action Type | `ConsumeWebService` |
| WS Action | `registrarLeadVentas` |
| Requires Confirmation | No |

### Input Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `datosContacto` | STRING | No | Información de contacto del cliente |
| `productoInteres` | STRING | No | Producto de interés |
| `scoreCalificacion` | STRING | No | Puntuación de calificación del lead |

### Output Parameters

| Name | Description |
|------|-------------|
| `numeroReferencia` | Número de referencia del lead |
| `fechaSeguimiento` | Fecha programada para seguimiento |
| `ejecutivoAsignado` | Ejecutivo asignado al lead |

---

## Web Actions

### `POST` consultarProductosTarjetas

**URL:** `https://795fca70-335b-4218-bfd0-17e871dc0fd3.mock.pstmn.io/consultarProductosTarjetas`

**Request Body:**
```json
{"cardType": "${tipoTarjeta}", "incomeRange": "${rangoIngresos}"}
```

**Request Params:**
```json
[
  {
    "name": "tipoTarjeta",
    "value": "${tipoTarjeta}",
    "secure": false
  },
  {
    "name": "rangoIngresos",
    "value": "${rangoIngresos}",
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
      "variableName": "recomendacion",
      "extractionExpression": "$.recomendacion"
    }
  ]
}
```

---

### `POST` evaluarPerfilCrediticio

**URL:** `https://795fca70-335b-4218-bfd0-17e871dc0fd3.mock.pstmn.io/evaluarPerfilCrediticio`

**Request Body:**
```json
{"rfc": "${rfc}", "curp": "${curp}", "monthlyIncome": "${ingresosMensuales}"}
```

**Request Params:**
```json
[
  {
    "name": "rfc",
    "value": "${rfc}",
    "secure": false
  },
  {
    "name": "curp",
    "value": "${curp}",
    "secure": false
  },
  {
    "name": "ingresosMensuales",
    "value": "${ingresosMensuales}",
    "secure": false
  }
]
```

**Response Mappings:**
```json
{
  "OK": [
    {
      "variableName": "scoreCredito",
      "extractionExpression": "$.scoreCredito"
    },
    {
      "variableName": "productosElegibles",
      "extractionExpression": "$.productosElegibles"
    },
    {
      "variableName": "limiteAprobado",
      "extractionExpression": "$.limiteAprobado"
    }
  ]
}
```

---

### `POST` calcularBeneficios

**URL:** `https://795fca70-335b-4218-bfd0-17e871dc0fd3.mock.pstmn.io/calcularBeneficios`

**Request Body:**
```json
{"productId": "${productoId}", "monthlySpending": "${gastoMensualEstimado}"}
```

**Request Params:**
```json
[
  {
    "name": "productoId",
    "value": "${productoId}",
    "secure": false
  },
  {
    "name": "gastoMensualEstimado",
    "value": "${gastoMensualEstimado}",
    "secure": false
  }
]
```

**Response Mappings:**
```json
{
  "OK": [
    {
      "variableName": "cashbackAnual",
      "extractionExpression": "$.cashbackAnual"
    },
    {
      "variableName": "puntosRecompensa",
      "extractionExpression": "$.puntosRecompensa"
    },
    {
      "variableName": "ahorroAnualidad",
      "extractionExpression": "$.ahorroAnualidad"
    }
  ]
}
```

---

### `POST` registrarLeadVentas

**URL:** `https://795fca70-335b-4218-bfd0-17e871dc0fd3.mock.pstmn.io/registrarLeadVentas`

**Request Body:**
```json
{"contactInfo": "${datosContacto}", "interestedProduct": "${productoInteres}", "qualificationScore": "${scoreCalificacion}"}
```

**Request Params:**
```json
[
  {
    "name": "datosContacto",
    "value": "${datosContacto}",
    "secure": false
  },
  {
    "name": "productoInteres",
    "value": "${productoInteres}",
    "secure": false
  },
  {
    "name": "scoreCalificacion",
    "value": "${scoreCalificacion}",
    "secure": false
  }
]
```

**Response Mappings:**
```json
{
  "OK": [
    {
      "variableName": "numeroReferencia",
      "extractionExpression": "$.numeroReferencia"
    },
    {
      "variableName": "fechaSeguimiento",
      "extractionExpression": "$.fechaSeguimiento"
    },
    {
      "variableName": "ejecutivoAsignado",
      "extractionExpression": "$.ejecutivoAsignado"
    }
  ]
}
```

---
