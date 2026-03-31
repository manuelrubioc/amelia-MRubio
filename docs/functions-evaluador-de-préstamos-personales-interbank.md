# Functions & Web Actions: Evaluador de Préstamos Personales Interbank

## Functions

## evaluarElegibilidadPrestamo

Evalúa la elegibilidad de préstamos personales basado en perfil financiero y criterios bancarios

| Setting | Value |
|---------|-------|
| Action Type | `CONVERSATION_FLOW` |
| Flow | `evaluacionPrestamoFlow` |
| Requires Confirmation | No |

### Input Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `ingreso_anual` | NUMBER | Yes | Ingreso anual del solicitante en soles peruanos |
| `score_crediticio` | NUMBER | Yes | Puntaje crediticio del solicitante (0-999) |
| `estado_laboral` | STRING | Yes | Estado laboral actual del solicitante |
| `monto_solicitado` | NUMBER | Yes | Monto del préstamo solicitado en soles peruanos |

### Output Parameters

| Name | Description |
|------|-------------|
| `resultado_evaluacion` | Resultado estructurado de la evaluación de elegibilidad |

---

## evaluarElegibilidadPrestamo

Evalúa la elegibilidad de préstamos personales basado en perfil financiero y criterios bancarios

| Setting | Value |
|---------|-------|
| Action Type | `CONVERSATION_FLOW` |
| Flow | `evaluacionPrestamoFlow` |
| Requires Confirmation | No |

### Input Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `ingreso_anual` | NUMBER | Yes | Ingreso anual del solicitante en soles peruanos |
| `score_crediticio` | NUMBER | Yes | Puntaje crediticio del solicitante (0-999) |
| `estado_laboral` | STRING | Yes | Estado laboral actual del solicitante |
| `monto_solicitado` | NUMBER | Yes | Monto del préstamo solicitado en soles peruanos |

### Output Parameters

| Name | Description |
|------|-------------|
| `resultado_evaluacion` | Resultado estructurado de la evaluación de elegibilidad |

---

## Web Actions

### `POST` consultarClienteExistente

**URL:** `https://3114aab6-91ec-446e-aac2-b3d003bd6a38.mock.pstmn.io/consultarClienteExistente`

**Request Body:**
```json
{"ingreso_anual": "${ingreso_anual}", "score_crediticio": "${score_crediticio}"}
```

**Response Mappings:**
```json
{
  "OK": [
    {
      "variableName": "cliente_existente",
      "extractionExpression": "$.es_cliente"
    },
    {
      "variableName": "cliente_id",
      "extractionExpression": "$.cliente_id"
    }
  ]
}
```

---

### `GET` consultarProductosPrestamos

**URL:** `https://3114aab6-91ec-446e-aac2-b3d003bd6a38.mock.pstmn.io/consultarProductosPrestamos`

**Request Params:**
```json
[
  {
    "name": "tipo",
    "value": "personal",
    "secure": false
  }
]
```

**Response Mappings:**
```json
{
  "OK": [
    {
      "variableName": "productos_disponibles",
      "extractionExpression": "$.productos"
    }
  ]
}
```

---

### `POST` verificarPrestamoPreaprobado

**URL:** `https://3114aab6-91ec-446e-aac2-b3d003bd6a38.mock.pstmn.io/verificarPrestamoPreaprobado`

**Request Body:**
```json
{"cliente_id": "${cliente_id}", "score_crediticio": "${score_crediticio}"}
```

**Response Mappings:**
```json
{
  "OK": [
    {
      "variableName": "prestamo_preaprobado",
      "extractionExpression": "$.preaprobado"
    },
    {
      "variableName": "monto_preaprobado",
      "extractionExpression": "$.monto_maximo"
    }
  ]
}
```

---

### `POST` calcularCapacidadPago

**URL:** `https://3114aab6-91ec-446e-aac2-b3d003bd6a38.mock.pstmn.io/calcularCapacidadPago`

**Request Body:**
```json
{"ingreso_anual": "${ingreso_anual}", "estado_laboral": "${estado_laboral}", "monto_solicitado": "${monto_solicitado}"}
```

**Response Mappings:**
```json
{
  "OK": [
    {
      "variableName": "capacidad_pago",
      "extractionExpression": "$.capacidad_pago"
    },
    {
      "variableName": "ratio_endeudamiento",
      "extractionExpression": "$.ratio_endeudamiento"
    }
  ]
}
```

---
