# Functions & Web Actions: Agente de Verificación de Cumplimiento Regulatorio

## Functions

## executeComplianceCheck

Ejecuta verificación completa de cumplimiento regulatorio y compatibilidad de riesgo para productos de inversión

| Setting | Value |
|---------|-------|
| Action Type | `CONVERSATION_FLOW` |
| Flow | `complianceCheckFlow` |
| Requires Confirmation | No |

### Input Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `customer_id` | STRING | Yes | Identificador único del cliente |
| `product_description` | STRING | Yes | Descripción detallada del producto de inversión |
| `total_investment_amount` | STRING | Yes | Monto total de inversión solicitado |

### Output Parameters

| Name | Description |
|------|-------------|
| `compliance_recommendation` | Recomendación final de cumplimiento con hallazgos detallados |
| `risk_classification` | Clasificación de riesgo del producto para el cliente |
| `regulatory_findings` | Hallazgos específicos de verificación regulatoria |

---

## executeComplianceCheck

Ejecuta verificación completa de cumplimiento regulatorio y compatibilidad de riesgo para productos de inversión

| Setting | Value |
|---------|-------|
| Action Type | `CONVERSATION_FLOW` |
| Flow | `complianceCheckFlow` |
| Requires Confirmation | No |

### Input Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `customer_id` | STRING | Yes | Identificador único del cliente |
| `product_description` | STRING | Yes | Descripción detallada del producto de inversión |
| `total_investment_amount` | STRING | Yes | Monto total de inversión solicitado |

### Output Parameters

| Name | Description |
|------|-------------|
| `compliance_recommendation` | Recomendación final de cumplimiento con hallazgos detallados |
| `risk_classification` | Clasificación de riesgo del producto para el cliente |
| `regulatory_findings` | Hallazgos específicos de verificación regulatoria |

---

## Web Actions

### `GET` getCustomerProfile

**URL:** `https://087ae63d-5364-4765-ab99-756a53b9c34b.mock.pstmn.io/getCustomerProfile`

**Request Params:**
```json
[
  {
    "name": "customer_id",
    "value": "${customer_id}",
    "secure": false
  }
]
```

**Response Mappings:**
```json
{
  "OK": [
    {
      "variableName": "customerProfile",
      "extractionExpression": "$.profile"
    },
    {
      "variableName": "riskProfile",
      "extractionExpression": "$.risk_profile"
    },
    {
      "variableName": "complianceStatus",
      "extractionExpression": "$.compliance_status"
    }
  ]
}
```

---

### `POST` validateSBSCompliance

**URL:** `https://087ae63d-5364-4765-ab99-756a53b9c34b.mock.pstmn.io/validateSBSCompliance`

**Request Body:**
```json
{"customer_id": "${customer_id}", "product_type": "${product_description}", "investment_amount": "${total_investment_amount}", "customer_profile": "${customerProfile}"}
```

**Response Mappings:**
```json
{
  "OK": [
    {
      "variableName": "sbsValidation",
      "extractionExpression": "$.validation_result"
    },
    {
      "variableName": "complianceScore",
      "extractionExpression": "$.compliance_score"
    },
    {
      "variableName": "regulatoryFlags",
      "extractionExpression": "$.regulatory_flags"
    }
  ]
}
```

---

### `POST` checkRiskCompatibility

**URL:** `https://087ae63d-5364-4765-ab99-756a53b9c34b.mock.pstmn.io/checkRiskCompatibility`

**Request Body:**
```json
{"customer_risk_profile": "${riskProfile}", "product_description": "${product_description}", "investment_amount": "${total_investment_amount}", "sbs_validation": "${sbsValidation}"}
```

**Response Mappings:**
```json
{
  "OK": [
    {
      "variableName": "riskCompatibility",
      "extractionExpression": "$.compatibility_result"
    },
    {
      "variableName": "riskScore",
      "extractionExpression": "$.risk_score"
    },
    {
      "variableName": "compatibilityReasons",
      "extractionExpression": "$.reasons"
    }
  ]
}
```

---
