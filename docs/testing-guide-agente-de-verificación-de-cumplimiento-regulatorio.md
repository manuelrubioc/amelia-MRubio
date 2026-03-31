# Testing Guide: Agente de Verificación de Cumplimiento Regulatorio

Test scenarios with full request/response examples.

## Dataset: Generated (2026-03-01 23:11) **(Active)**

AI-generated mock data for AgenteComplianceRegulatorio

_Source: generated_

### Web Action: `checkRiskCompatibility`

**edge_case** (HTTP 400)

Cliente sin perfil de riesgo definido

> **Tester prompt:** Intenta evaluar compatibilidad de un producto sin tener definido tu perfil de riesgo.

**Request Body:**
```json
{
  "customer_risk_profile": "NO_DEFINIDO",
  "product_description": "Fondo Mutuo Mixto",
  "investment_amount": "5000",
  "sbs_validation": "PENDIENTE"
}
```

**Response Body:**
```json
{
  "success": false,
  "error_code": "RISK_PROFILE_REQUIRED",
  "message": "Es necesario completar la evaluación de perfil de riesgo antes de continuar",
  "details": {
    "required_steps": [
      "Completar cuestionario de perfil",
      "Validación presencial",
      "Aprobación de asesor"
    ],
    "estimated_time": "15-20 minutos"
  },
  "scheduling_options": {
    "online_assessment": true,
    "branch_appointment": true
  }
}
```

---

**error_case** (HTTP 400)

Incompatibilidad de riesgo detectada

> **Tester prompt:** Solicita al agente evaluar un fondo de acciones internacionales siendo cliente conservador.

**Request Body:**
```json
{
  "customer_risk_profile": "CONSERVADOR",
  "product_description": "Fondo de Acciones Internacionales",
  "investment_amount": "30000",
  "sbs_validation": "APROBADO"
}
```

**Response Body:**
```json
{
  "success": false,
  "error_code": "RISK_INCOMPATIBLE",
  "message": "El producto seleccionado no es compatible con su perfil de riesgo",
  "details": {
    "product_risk_level": "ALTO",
    "customer_risk_tolerance": "BAJO",
    "compatibility_score": 25,
    "risk_mismatch": "CRITICO"
  },
  "alternative_products": [
    "Depósito a Plazo",
    "Fondo Mutuo de Renta Fija",
    "Bonos del Tesoro"
  ]
}
```

---

**happy_path** (HTTP 200)

Compatibilidad de riesgo aprobada

> **Tester prompt:** Pregunta al agente si un fondo balanceado es compatible con tu perfil moderado para S/20,000.

**Request Body:**
```json
{
  "customer_risk_profile": "MODERADO",
  "product_description": "Fondo Mutuo Balanceado",
  "investment_amount": "20000",
  "sbs_validation": "APROBADO"
}
```

**Response Body:**
```json
{
  "success": true,
  "data": {
    "compatibility_id": "RISK-COMP-20241201-001",
    "compatibility_status": "COMPATIBLE",
    "risk_assessment": {
      "product_risk_level": "MEDIO",
      "customer_risk_tolerance": "MEDIO",
      "compatibility_score": 95
    },
    "recommendations": {
      "allocation_percentage": 80,
      "suggested_amount": 16000.0,
      "diversification_advice": "Se recomienda diversificar con instrumentos de menor riesgo"
    },
    "approval_timestamp": "2024-12-01T17:15:00Z"
  }
}
```

---

**partial_compatibility** (HTTP 200)

Compatibilidad parcial con recomendaciones

> **Tester prompt:** Consulta al agente sobre invertir S/40,000 en un fondo de mercados emergentes con perfil moderado.

**Request Body:**
```json
{
  "customer_risk_profile": "MODERADO",
  "product_description": "Fondo de Renta Variable Emergente",
  "investment_amount": "40000",
  "sbs_validation": "APROBADO"
}
```

**Response Body:**
```json
{
  "success": true,
  "data": {
    "compatibility_id": "RISK-COMP-20241201-002",
    "compatibility_status": "PARCIALMENTE_COMPATIBLE",
    "risk_assessment": {
      "product_risk_level": "MEDIO_ALTO",
      "customer_risk_tolerance": "MEDIO",
      "compatibility_score": 65
    },
    "recommendations": {
      "allocation_percentage": 40,
      "suggested_amount": 16000.0,
      "warnings": [
        "Considere diversificar con activos más conservadores",
        "Monitoree regularmente la inversión"
      ]
    },
    "approval_timestamp": "2024-12-01T17:45:00Z"
  }
}
```

---

**sbs_validation_failed** (HTTP 400)

Validación SBS previa falló

> **Tester prompt:** Pregunta sobre compatibilidad de un producto cuando la validación SBS fue rechazada.

**Request Body:**
```json
{
  "customer_risk_profile": "AGRESIVO",
  "product_description": "Fondo Hedge",
  "investment_amount": "75000",
  "sbs_validation": "RECHAZADO"
}
```

**Response Body:**
```json
{
  "success": false,
  "error_code": "SBS_VALIDATION_FAILED",
  "message": "No es posible evaluar compatibilidad debido a validación SBS fallida",
  "details": {
    "sbs_status": "RECHAZADO",
    "blocking_reason": "Límites regulatorios excedidos",
    "required_action": "Resolver validación SBS primero"
  },
  "next_steps": "Contacte a su asesor para revisar los límites de inversión"
}
```

---

### Web Action: `getCustomerProfile`

**edge_case** (HTTP 200)

Cliente con perfil incompleto o en proceso de actualización

> **Tester prompt:** Consulta tu perfil como cliente nuevo que aún no ha completado toda la información requerida.

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "success": true,
  "data": {
    "customer_id": "CUST-20241201-002",
    "full_name": "José Carlos Ríos",
    "document_type": "DNI",
    "document_number": "12345678",
    "risk_profile": null,
    "customer_segment": "BANCA_PERSONAL",
    "account_opening_date": "2024-11-30",
    "monthly_income": null,
    "investment_experience": null,
    "kyc_status": "PENDIENTE",
    "last_update": "2024-12-01T08:00:00Z"
  }
}
```

---

**error_case** (HTTP 404)

Cliente no encontrado en el sistema

> **Tester prompt:** Pregunta al agente por tu perfil cuando no estés autenticado correctamente.

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "success": false,
  "error_code": "CUSTOMER_NOT_FOUND",
  "message": "No se encontró información del cliente en nuestros registros",
  "details": "Verifique que haya iniciado sesión correctamente"
}
```

---

**happy_path** (HTTP 200)

Cliente existente con perfil completo

> **Tester prompt:** Solicita al agente que obtenga tu perfil de cliente actual.

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "success": true,
  "data": {
    "customer_id": "CUST-20241201-001",
    "full_name": "María Elena Vargas Mendoza",
    "document_type": "DNI",
    "document_number": "45678912",
    "risk_profile": "MODERADO",
    "customer_segment": "BANCA_PERSONAL",
    "account_opening_date": "2019-03-15",
    "monthly_income": 8500.0,
    "investment_experience": "INTERMEDIO",
    "kyc_status": "COMPLETO",
    "last_update": "2024-11-28T10:30:00Z"
  }
}
```

---

**high_value_customer** (HTTP 200)

Cliente de banca privada con perfil completo

> **Tester prompt:** Solicita tu perfil como cliente de banca privada con experiencia en inversiones.

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "success": true,
  "data": {
    "customer_id": "CUST-20241201-003",
    "full_name": "Roberto Alejandro Castillo Herrera",
    "document_type": "DNI",
    "document_number": "87654321",
    "risk_profile": "AGRESIVO",
    "customer_segment": "BANCA_PRIVADA",
    "account_opening_date": "2015-08-20",
    "monthly_income": 45000.0,
    "investment_experience": "EXPERTO",
    "kyc_status": "COMPLETO",
    "last_update": "2024-11-25T14:20:00Z"
  }
}
```

---

**system_maintenance** (HTTP 503)

Sistema en mantenimiento temporal

> **Tester prompt:** Intenta consultar tu perfil durante una ventana de mantenimiento del sistema.

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "success": false,
  "error_code": "SERVICE_UNAVAILABLE",
  "message": "El servicio se encuentra temporalmente no disponible por mantenimiento",
  "details": "Intente nuevamente en unos minutos",
  "retry_after": 300
}
```

---

### Web Action: `validateSBSCompliance`

**edge_case** (HTTP 200)

Monto mínimo de inversión

> **Tester prompt:** Pregunta al agente sobre invertir el monto mínimo de S/500 en un depósito a plazo.

**Request Body:**
```json
{
  "customer_id": "CUST-20241201-003",
  "product_type": "Depósito a Plazo",
  "investment_amount": "500",
  "customer_profile": "CONSERVADOR"
}
```

**Response Body:**
```json
{
  "success": true,
  "data": {
    "validation_id": "SBS-VAL-20241201-002",
    "compliance_status": "APROBADO",
    "sbs_requirements_met": true,
    "regulatory_limits": {
      "max_investment_allowed": 25000.0,
      "current_exposure": 500.0,
      "remaining_capacity": 24500.0
    },
    "validation_timestamp": "2024-12-01T16:00:00Z",
    "expiry_date": "2024-12-31T23:59:59Z",
    "warnings": [
      "Monto cercano al mínimo requerido"
    ]
  }
}
```

---

**error_case** (HTTP 400)

Monto excede límites regulatorios SBS

> **Tester prompt:** Solicita al agente invertir S/100,000 en un fondo agresivo siendo cliente conservador.

**Request Body:**
```json
{
  "customer_id": "CUST-20241201-002",
  "product_type": "Fondo Mutuo Agresivo",
  "investment_amount": "100000",
  "customer_profile": "CONSERVADOR"
}
```

**Response Body:**
```json
{
  "success": false,
  "error_code": "SBS_LIMIT_EXCEEDED",
  "message": "El monto solicitado excede los límites regulatorios establecidos por la SBS",
  "details": {
    "max_allowed": 25000.0,
    "requested_amount": 100000.0,
    "customer_limit_type": "PERFIL_CONSERVADOR"
  },
  "regulatory_reference": "Circular SBS G-140-2008"
}
```

---

**happy_path** (HTTP 200)

Validación exitosa de cumplimiento SBS

> **Tester prompt:** Dile al agente que quieres invertir S/15,000 en un fondo mutuo conservador.

**Request Body:**
```json
{
  "customer_id": "CUST-20241201-001",
  "product_type": "Fondo Mutuo Conservador",
  "investment_amount": "15000",
  "customer_profile": "MODERADO"
}
```

**Response Body:**
```json
{
  "success": true,
  "data": {
    "validation_id": "SBS-VAL-20241201-001",
    "compliance_status": "APROBADO",
    "sbs_requirements_met": true,
    "regulatory_limits": {
      "max_investment_allowed": 50000.0,
      "current_exposure": 15000.0,
      "remaining_capacity": 35000.0
    },
    "validation_timestamp": "2024-12-01T15:30:00Z",
    "expiry_date": "2024-12-31T23:59:59Z"
  }
}
```

---

**kyc_incomplete** (HTTP 403)

Cliente con KYC incompleto

> **Tester prompt:** Intenta hacer una inversión cuando tu perfil KYC aún está incompleto.

**Request Body:**
```json
{
  "customer_id": "CUST-20241201-004",
  "product_type": "Fondo Mutuo Mixto",
  "investment_amount": "10000",
  "customer_profile": "PENDIENTE"
}
```

**Response Body:**
```json
{
  "success": false,
  "error_code": "KYC_INCOMPLETE",
  "message": "No es posible procesar la solicitud debido a información KYC incompleta",
  "details": {
    "missing_documents": [
      "Declaración Jurada de Ingresos",
      "Evaluación de Perfil de Riesgo"
    ],
    "required_actions": [
      "Completar evaluación en oficina",
      "Actualizar información patrimonial"
    ]
  },
  "next_steps": "Acérquese a cualquier oficina Interbank para completar su perfil"
}
```

---

**product_restricted** (HTTP 400)

Producto restringido para el segmento del cliente

> **Tester prompt:** Solicita al agente invertir en derivados financieros siendo cliente de banca personal.

**Request Body:**
```json
{
  "customer_id": "CUST-20241201-005",
  "product_type": "Derivados Financieros",
  "investment_amount": "25000",
  "customer_profile": "MODERADO"
}
```

**Response Body:**
```json
{
  "success": false,
  "error_code": "PRODUCT_RESTRICTED",
  "message": "El producto solicitado no está disponible para su segmento de cliente",
  "details": {
    "required_segment": "BANCA_PRIVADA",
    "current_segment": "BANCA_PERSONAL",
    "minimum_patrimony": 500000.0
  },
  "alternative_products": [
    "Fondo Mutuo Balanceado",
    "Depósito Estructurado"
  ]
}
```

---

## Dataset: Generated (2026-02-28 04:12)

AI-generated mock data for AgenteComplianceRegulatorio

_Source: generated_

### Web Action: `consultarLimitesInversion`

**edge_case** (HTTP 200)

Cliente nuevo sin historial de inversiones

**Request Body:**
```json
{
  "customer_id": "CL-20240120-0999",
  "product_type": "DEPOSITO_PLAZO",
  "requested_amount": "S/ 1000.00"
}
```

**Response Body:**
```json
{
  "success": true,
  "data": {
    "customer_id": "CL-20240120-0999",
    "available_limits": {
      "daily_limit": "S/ 50,000.00",
      "monthly_limit": "S/ 200,000.00",
      "annual_limit": "S/ 500,000.00",
      "product_specific_limit": "S/ 1,000,000.00"
    },
    "current_usage": {
      "daily_used": "S/ 0.00",
      "monthly_used": "S/ 0.00",
      "annual_used": "S/ 0.00",
      "product_used": "S/ 0.00"
    },
    "remaining_limits": {
      "daily_remaining": "S/ 50,000.00",
      "monthly_remaining": "S/ 200,000.00",
      "annual_remaining": "S/ 500,000.00",
      "product_remaining": "S/ 1,000,000.00"
    },
    "request_status": "DENTRO_DE_LIMITES",
    "customer_status": "NUEVO",
    "special_conditions": [
      "Límites reducidos para cliente nuevo",
      "Incremento automático después de 6 meses"
    ],
    "regulatory_basis": [
      "POLITICAS_CLIENTES_NUEVOS_INTERBANK"
    ],
    "next_review_date": "2024-07-20T00:00:00-05:00"
  }
}
```

---

**error_case** (HTTP 400)

Monto solicitado excede los límites regulatorios

**Request Body:**
```json
{
  "customer_id": "CL-20240110-0456",
  "product_type": "RENTA_VARIABLE",
  "requested_amount": "S/ 1500000.00"
}
```

**Response Body:**
```json
{
  "success": false,
  "error": {
    "code": "LIMIT_EXCEEDED",
    "message": "El monto solicitado excede los límites regulatorios establecidos",
    "details": "La inversión solicitada de S/ 1,500,000.00 supera el límite anual disponible de S/ 800,000.00",
    "limit_violations": [
      {
        "limit_type": "ANUAL",
        "limit_value": "S/ 1,000,000.00",
        "current_usage": "S/ 200,000.00",
        "requested_amount": "S/ 1,500,000.00",
        "excess_amount": "S/ 700,000.00"
      }
    ],
    "regulatory_reference": "SBS_CIRCULAR_G-140-2009_ART_25",
    "timestamp": "2024-01-20T17:30:00-05:00"
  }
}
```

---

**happy_path** (HTTP 200)

Consulta exitosa de límites de inversión disponibles

**Request Body:**
```json
{
  "customer_id": "CL-20231101-0001",
  "product_type": "FONDO_MUTUO",
  "requested_amount": "S/ 75000.00"
}
```

**Response Body:**
```json
{
  "success": true,
  "data": {
    "customer_id": "CL-20231101-0001",
    "available_limits": {
      "daily_limit": "S/ 100,000.00",
      "monthly_limit": "S/ 500,000.00",
      "annual_limit": "S/ 2,000,000.00",
      "product_specific_limit": "S/ 800,000.00"
    },
    "current_usage": {
      "daily_used": "S/ 25,000.00",
      "monthly_used": "S/ 150,000.00",
      "annual_used": "S/ 450,000.00",
      "product_used": "S/ 200,000.00"
    },
    "remaining_limits": {
      "daily_remaining": "S/ 75,000.00",
      "monthly_remaining": "S/ 350,000.00",
      "annual_remaining": "S/ 1,550,000.00",
      "product_remaining": "S/ 600,000.00"
    },
    "request_status": "DENTRO_DE_LIMITES",
    "regulatory_basis": [
      "SBS_RESOLUCION_11356-2008",
      "POLITICAS_INTERNAS_INTERBANK"
    ],
    "next_reset_dates": {
      "daily": "2024-01-21T00:00:00-05:00",
      "monthly": "2024-02-01T00:00:00-05:00",
      "annual": "2024-12-31T23:59:59-05:00"
    }
  }
}
```

---

### Web Action: `consultarPerfilCliente`

**edge_case** (HTTP 200)

Cliente con perfil mínimo y KYC pendiente

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "success": true,
  "data": {
    "customer_id": "CL-20240115-0789",
    "full_name": "Juan Carlos Pérez",
    "document_type": "DNI",
    "document_number": "12345678",
    "email": null,
    "phone": "+51 999888777",
    "risk_profile": "NO_DEFINIDO",
    "account_status": "PENDIENTE",
    "kyc_status": "INCOMPLETO",
    "last_kyc_update": null,
    "investment_experience": "NINGUNA",
    "monthly_income": "S/ 0.00",
    "net_worth": "S/ 0.00",
    "occupation": null,
    "address": null,
    "regulatory_flags": [
      "KYC_INCOMPLETO",
      "PERFIL_RIESGO_PENDIENTE"
    ],
    "pep_status": null,
    "fatca_status": "PENDIENTE"
  }
}
```

---

**error_case** (HTTP 404)

Cliente no encontrado en el sistema

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "success": false,
  "error": {
    "code": "CUSTOMER_NOT_FOUND",
    "message": "Cliente no encontrado en el sistema",
    "details": "No se pudo localizar el perfil del cliente en la base de datos",
    "timestamp": "2024-01-20T10:30:00-05:00"
  }
}
```

---

**happy_path** (HTTP 200)

Cliente activo con perfil completo

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "success": true,
  "data": {
    "customer_id": "CL-20231101-0001",
    "full_name": "María Elena Rodríguez Vásquez",
    "document_type": "DNI",
    "document_number": "43567890",
    "email": "maria.rodriguez@gmail.com",
    "phone": "+51 987654321",
    "risk_profile": "MODERADO",
    "account_status": "ACTIVO",
    "kyc_status": "COMPLETO",
    "last_kyc_update": "2024-01-15",
    "investment_experience": "INTERMEDIO",
    "monthly_income": "S/ 8,500.00",
    "net_worth": "S/ 250,000.00",
    "occupation": "Ingeniera de Sistemas",
    "address": {
      "street": "Av. Javier Prado Este 4200",
      "district": "Surco",
      "province": "Lima",
      "department": "Lima"
    },
    "regulatory_flags": [],
    "pep_status": false,
    "fatca_status": "NO_APLICA"
  }
}
```

---

### Web Action: `verificarRegulacionesProducto`

**edge_case** (HTTP 200)

Producto con monto mínimo de inversión

**Request Body:**
```json
{
  "product_description": "Depósito a Plazo Fijo 30 días",
  "investment_amount": "S/ 500.00",
  "customer_risk_profile": "CONSERVADOR"
}
```

**Response Body:**
```json
{
  "success": true,
  "data": {
    "compliance_status": "APROBADO_CON_OBSERVACIONES",
    "regulatory_checks": [
      {
        "regulation": "SBS_CIRCULAR_G-106-2005",
        "status": "CUMPLE",
        "description": "Monto mínimo para depósitos a plazo"
      }
    ],
    "required_documents": [
      "Contrato de depósito a plazo"
    ],
    "cooling_period": "0 horas",
    "maximum_investment": "S/ 1,000,000.00",
    "warnings": [
      "Monto cercano al mínimo regulatorio de S/ 500.00"
    ],
    "special_conditions": [
      "Renovación automática disponible",
      "Sin penalidad por retiro anticipado después de 15 días"
    ],
    "approval_date": "2024-01-20T16:00:00-05:00"
  }
}
```

---

**error_case** (HTTP 400)

Producto no compatible con perfil de riesgo del cliente

**Request Body:**
```json
{
  "product_description": "Fondo de Inversión Renta Variable Internacional",
  "investment_amount": "S/ 100000.00",
  "customer_risk_profile": "CONSERVADOR"
}
```

**Response Body:**
```json
{
  "success": false,
  "error": {
    "code": "RISK_PROFILE_MISMATCH",
    "message": "El producto no es compatible con el perfil de riesgo del cliente",
    "details": "Un cliente con perfil CONSERVADOR no puede invertir en productos de renta variable de alto riesgo",
    "regulatory_violations": [
      {
        "regulation": "SBS_CIRCULAR_G-140-2009_ART_15",
        "violation": "Incompatibilidad perfil-producto",
        "severity": "ALTA"
      }
    ],
    "timestamp": "2024-01-20T15:20:00-05:00"
  }
}
```

---

**happy_path** (HTTP 200)

Producto de inversión aprobado según regulaciones

**Request Body:**
```json
{
  "product_description": "Fondo Mutuo Renta Fija Soles",
  "investment_amount": "S/ 50000.00",
  "customer_risk_profile": "MODERADO"
}
```

**Response Body:**
```json
{
  "success": true,
  "data": {
    "compliance_status": "APROBADO",
    "regulatory_checks": [
      {
        "regulation": "SBS_CIRCULAR_G-140-2009",
        "status": "CUMPLE",
        "description": "Límites de inversión en fondos mutuos"
      },
      {
        "regulation": "LEY_26702_ART_221",
        "status": "CUMPLE",
        "description": "Adecuación del perfil de riesgo"
      },
      {
        "regulation": "REGLAMENTO_SMV",
        "status": "CUMPLE",
        "description": "Requisitos de información al inversionista"
      }
    ],
    "required_documents": [
      "Declaración de perfil de riesgo",
      "Contrato de inversión"
    ],
    "cooling_period": "24 horas",
    "maximum_investment": "S/ 500,000.00",
    "warnings": [],
    "approval_date": "2024-01-20T14:45:00-05:00"
  }
}
```

---

## Dataset: Generated (2026-02-28 15:16)

AI-generated mock data for AgenteComplianceRegulatorio

_Source: generated_

### Web Action: `checkRegulatoryDatabase`

**edge_case** (HTTP 200)

Regulatory check with zero investment amount

**Request Body:**
```json
{
  "customer_id": "CUS-PE-111222333",
  "product_type": "CONSULTA",
  "investment_amount": "0.00"
}
```

**Response Body:**
```json
{
  "success": true,
  "check_id": "REG-CHK-2024-001236",
  "customer_id": "CUS-PE-111222333",
  "status": "INFORMATIVO",
  "compliance_score": null,
  "checks_performed": {
    "sbs_registry": "LIMPIO",
    "uif_lists": "SIN_COINCIDENCIAS",
    "pep_verification": "NO_PEP",
    "sanctions_screening": "LIMPIO",
    "adverse_media": "SIN_ALERTAS"
  },
  "risk_assessment": "NO_APLICA",
  "recommended_action": "INFORMATIVO_SOLAMENTE",
  "additional_requirements": [],
  "alerts": [],
  "valid_until": "2024-01-15T23:59:59Z",
  "checked_at": "2024-01-15T14:35:12Z",
  "checked_by": "SISTEMA_AUTOMATICO"
}
```

---

**error_case** (HTTP 200)

Regulatory check failed due to sanctions match

**Request Body:**
```json
{
  "customer_id": "CUS-PE-555666777",
  "product_type": "CUENTA_CORRIENTE",
  "investment_amount": "50000.00"
}
```

**Response Body:**
```json
{
  "success": true,
  "check_id": "REG-CHK-2024-001235",
  "customer_id": "CUS-PE-555666777",
  "status": "RECHAZADO",
  "compliance_score": 15,
  "checks_performed": {
    "sbs_registry": "LIMPIO",
    "uif_lists": "COINCIDENCIA_PARCIAL",
    "pep_verification": "NO_PEP",
    "sanctions_screening": "COINCIDENCIA_ENCONTRADA",
    "adverse_media": "ALERTAS_ENCONTRADAS"
  },
  "risk_assessment": "ALTO",
  "recommended_action": "RECHAZAR_OPERACION",
  "additional_requirements": [
    "REVISION_MANUAL_OBLIGATORIA",
    "DOCUMENTACION_ADICIONAL"
  ],
  "alerts": [
    {
      "type": "SANCTIONS_MATCH",
      "description": "Coincidencia encontrada en lista OFAC",
      "severity": "CRITICA"
    }
  ],
  "valid_until": null,
  "checked_at": "2024-01-15T14:30:45Z",
  "checked_by": "SISTEMA_AUTOMATICO"
}
```

---

**happy_path** (HTTP 200)

Successful regulatory check with approved status

**Request Body:**
```json
{
  "customer_id": "CUS-PE-789456123",
  "product_type": "FONDO_MUTUO",
  "investment_amount": "25000.00"
}
```

**Response Body:**
```json
{
  "success": true,
  "check_id": "REG-CHK-2024-001234",
  "customer_id": "CUS-PE-789456123",
  "status": "APROBADO",
  "compliance_score": 95,
  "checks_performed": {
    "sbs_registry": "LIMPIO",
    "uif_lists": "SIN_COINCIDENCIAS",
    "pep_verification": "NO_PEP",
    "sanctions_screening": "LIMPIO",
    "adverse_media": "SIN_ALERTAS"
  },
  "risk_assessment": "BAJO",
  "recommended_action": "PROCEDER",
  "additional_requirements": [],
  "valid_until": "2024-07-15T23:59:59Z",
  "checked_at": "2024-01-15T14:25:30Z",
  "checked_by": "SISTEMA_AUTOMATICO"
}
```

---

### Web Action: `getCustomerProfile`

**edge_case** (HTTP 200)

Customer profile with minimal required data only

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "success": true,
  "customer": {
    "id": "CUS-PE-000000001",
    "name": "Juan Pérez",
    "dni": "00000001",
    "email": null,
    "phone": null,
    "address": "",
    "birth_date": "1990-01-01",
    "occupation": "",
    "monthly_income": 0.0,
    "account_opening_date": "2024-01-01",
    "risk_profile": "NO_DEFINIDO",
    "kyc_status": "PENDIENTE",
    "pep_status": null,
    "sanctions_check": "PENDIENTE",
    "account_types": [],
    "credit_score": null,
    "last_update": "2024-01-01T00:00:00Z"
  }
}
```

---

**error_case** (HTTP 404)

Customer profile not found in system

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "success": false,
  "error_code": "CUSTOMER_NOT_FOUND",
  "message": "No se encontró el perfil del cliente en la base de datos",
  "details": "El cliente puede no estar registrado o la sesión ha expirado"
}
```

---

**happy_path** (HTTP 200)

Successful retrieval of customer profile with complete information

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "success": true,
  "customer": {
    "id": "CUS-PE-789456123",
    "name": "María Elena Rodríguez Vargas",
    "dni": "45123789",
    "email": "maria.rodriguez@gmail.com",
    "phone": "+51 987654321",
    "address": "Av. Javier Prado Este 4200, San Borja, Lima",
    "birth_date": "1985-03-15",
    "occupation": "Ingeniera de Sistemas",
    "monthly_income": 8500.0,
    "account_opening_date": "2018-07-20",
    "risk_profile": "MODERADO",
    "kyc_status": "COMPLETO",
    "pep_status": false,
    "sanctions_check": "LIMPIO",
    "account_types": [
      "AHORROS",
      "CORRIENTE"
    ],
    "credit_score": 750,
    "last_update": "2024-01-15T10:30:00Z"
  }
}
```

---

### Web Action: `validateSbsCompliance`

**edge_case** (HTTP 200)

SBS compliance validation for minimum product amount

**Request Body:**
```json
{
  "bank_code": "INTERBANK",
  "customer_data": "{\"dni\": \"87654321\", \"name\": \"Ana Torres López\", \"income\": 1025.00}",
  "product_details": "{\"type\": \"CUENTA_AHORROS\", \"amount\": 50.00, \"term_months\": 0}"
}
```

**Response Body:**
```json
{
  "success": true,
  "validation_id": "SBS-VAL-2024-567892",
  "bank_code": "INTERBANK",
  "sbs_status": "CUMPLE_MINIMO",
  "compliance_details": {
    "capital_requirements": "CUMPLE",
    "liquidity_ratios": "CUMPLE",
    "customer_protection": "CUMPLE",
    "anti_money_laundering": "CUMPLE",
    "know_your_customer": "CUMPLE"
  },
  "regulatory_limits": {
    "individual_exposure": {
      "limit": 30750.0,
      "current": 0.0,
      "available": 30750.0
    },
    "minimum_opening_balance": {
      "required": 50.0,
      "provided": 50.0
    }
  },
  "required_disclosures": [
    "COMISION_MANTENIMIENTO",
    "TASA_INTERES_AHORROS"
  ],
  "special_conditions": [
    "CUENTA_BASICA_SIN_COMISIONES",
    "LIMITE_TRANSACCIONES_MENSUALES"
  ],
  "approval_conditions": [],
  "valid_until": "2024-12-31T23:59:59Z",
  "validated_at": "2024-01-15T15:20:10Z",
  "sbs_reference": "SBS-REF-2024-INT-001236"
}
```

---

**error_case** (HTTP 200)

SBS compliance validation failed due to regulatory limits exceeded

**Request Body:**
```json
{
  "bank_code": "INTERBANK",
  "customer_data": "{\"dni\": \"12345678\", \"name\": \"Carlos Mendoza Silva\", \"income\": 3500.00}",
  "product_details": "{\"type\": \"CREDITO_PERSONAL\", \"amount\": 150000.00, \"term_months\": 60}"
}
```

**Response Body:**
```json
{
  "success": false,
  "validation_id": "SBS-VAL-2024-567891",
  "bank_code": "INTERBANK",
  "sbs_status": "NO_CUMPLE",
  "compliance_details": {
    "capital_requirements": "CUMPLE",
    "liquidity_ratios": "CUMPLE",
    "customer_protection": "NO_CUMPLE",
    "anti_money_laundering": "CUMPLE",
    "know_your_customer": "CUMPLE"
  },
  "violations": [
    {
      "code": "SBS-001",
      "description": "Monto solicitado excede límite de endeudamiento permitido",
      "severity": "CRITICA"
    },
    {
      "code": "SBS-015",
      "description": "Relación cuota-ingreso supera el 30% permitido",
      "severity": "ALTA"
    }
  ],
  "regulatory_limits": {
    "individual_exposure": {
      "limit": 105000.0,
      "current": 0.0,
      "requested": 150000.0
    },
    "debt_to_income_ratio": {
      "limit": 0.3,
      "calculated": 0.85
    }
  },
  "required_actions": [
    "REDUCIR_MONTO_SOLICITADO",
    "PRESENTAR_GARANTIAS_ADICIONALES"
  ],
  "validated_at": "2024-01-15T15:15:45Z",
  "sbs_reference": "SBS-REF-2024-INT-001235"
}
```

---

**happy_path** (HTTP 200)

Successful SBS compliance validation with full approval

**Request Body:**
```json
{
  "bank_code": "INTERBANK",
  "customer_data": "{\"dni\": \"45123789\", \"name\": \"María Elena Rodríguez Vargas\", \"income\": 8500.00}",
  "product_details": "{\"type\": \"DEPOSITO_PLAZO\", \"amount\": 15000.00, \"term_months\": 12}"
}
```

**Response Body:**
```json
{
  "success": true,
  "validation_id": "SBS-VAL-2024-567890",
  "bank_code": "INTERBANK",
  "sbs_status": "CUMPLE",
  "compliance_details": {
    "capital_requirements": "CUMPLE",
    "liquidity_ratios": "CUMPLE",
    "customer_protection": "CUMPLE",
    "anti_money_laundering": "CUMPLE",
    "know_your_customer": "CUMPLE"
  },
  "regulatory_limits": {
    "individual_exposure": {
      "limit": 50000.0,
      "current": 15000.0,
      "available": 35000.0
    },
    "product_restrictions": "NINGUNA"
  },
  "required_disclosures": [
    "TASA_EFECTIVA_ANUAL",
    "COMISIONES_APLICABLES",
    "PENALIDADES_RETIRO_ANTICIPADO"
  ],
  "approval_conditions": [],
  "valid_until": "2024-12-31T23:59:59Z",
  "validated_at": "2024-01-15T15:10:20Z",
  "sbs_reference": "SBS-REF-2024-INT-001234"
}
```

---

## Dataset: Generated (2026-03-01 14:56)

AI-generated mock data for AgenteComplianceRegulatorio

_Source: generated_

### Web Action: `checkRiskCompatibility`

**alt_happy_balanced_portfolio** (HTTP 200)

Successful compatibility check for balanced portfolio

> **Tester prompt:** Consulta la compatibilidad de invertir S/ 85,000.75 en un fondo mutuo balanceado internacional.

**Request Body:**
```json
{
  "customer_risk_profile": "MODERADO",
  "product_description": "Fondo Mutuo Balanceado Internacional",
  "investment_amount": "85000.75",
  "sbs_validation": "APROBADO"
}
```

**Response Body:**
```json
{
  "compatibility_result": "COMPATIBLE",
  "risk_score": 6.5,
  "compatibility_percentage": 88,
  "recommendation": "APROBADO",
  "risk_factors": [
    "DIVERSIFICACION_INTERNACIONAL",
    "RIESGO_CAMBIARIO_MODERADO"
  ],
  "suggested_allocation": {
    "percentage": "25%",
    "max_recommended": 95000.0
  },
  "monitoring_required": false,
  "analysis_date": "2024-03-18T11:20:00-05:00",
  "next_review_date": "2024-09-18"
}
```

---

**edge_case** (HTTP 200)

Risk compatibility check for undefined customer profile

**Request Body:**
```json
{
  "customer_risk_profile": "no_definido",
  "product_description": "Cuenta de Ahorros",
  "investment_amount": "0.00",
  "sbs_validation": "pendiente"
}
```

**Response Body:**
```json
{
  "success": true,
  "data": {
    "compatibility_status": "evaluacion_requerida",
    "compatibility_score": 0,
    "risk_analysis": {
      "customer_risk_tolerance": "no_evaluado",
      "product_risk_level": "muy_bajo",
      "investment_horizon": "no_definido",
      "volatility_match": "no_aplicable"
    },
    "recommendations": [
      "Completar evaluación de perfil de riesgo",
      "Producto básico sin restricciones"
    ],
    "warnings": [
      "Perfil de riesgo no definido"
    ],
    "approval_required": true,
    "analysis_date": "2024-01-15T09:15:00-05:00"
  }
}
```

---

**edge_zero_investment** (HTTP 200)

Compatibility check with zero investment amount

> **Tester prompt:** Pregunta sobre la compatibilidad de una cuenta de ahorros sin monto de inversión.

**Request Body:**
```json
{
  "customer_risk_profile": "CONSERVADOR",
  "product_description": "Cuenta de Ahorros Premium",
  "investment_amount": "0.00",
  "sbs_validation": "APROBADO"
}
```

**Response Body:**
```json
{
  "compatibility_result": "COMPATIBLE",
  "risk_score": 1.0,
  "compatibility_percentage": 100,
  "recommendation": "APROBADO_SIN_RESTRICCIONES",
  "risk_factors": [
    "SIN_RIESGO_CAPITAL",
    "LIQUIDEZ_INMEDIATA"
  ],
  "suggested_allocation": {
    "percentage": "100%",
    "max_recommended": 999999999.99
  },
  "monitoring_required": false,
  "analysis_date": "2024-03-18T11:30:00-05:00",
  "special_note": "Producto sin riesgo de inversión - solo ahorro"
}
```

---

**error_case** (HTTP 400)

Risk incompatibility detected between customer profile and product

**Request Body:**
```json
{
  "customer_risk_profile": "conservador",
  "product_description": "Fondo de Inversión en Acciones Emergentes",
  "investment_amount": "100000.00",
  "sbs_validation": "aprobado"
}
```

**Response Body:**
```json
{
  "success": false,
  "error": {
    "code": "RISK_INCOMPATIBILITY",
    "message": "Incompatibilidad de riesgo detectada",
    "details": "El producto presenta un nivel de riesgo superior al perfil del cliente",
    "risk_analysis": {
      "customer_risk_tolerance": "bajo",
      "product_risk_level": "muy_alto",
      "compatibility_gap": 3
    },
    "required_actions": [
      "Reevaluar perfil de riesgo del cliente",
      "Considerar productos alternativos",
      "Obtener autorización expresa del cliente"
    ]
  }
}
```

---

**error_expired_validation** (HTTP 422)

Compatibility check failed due to expired SBS validation

> **Tester prompt:** Intenta verificar compatibilidad con una validación SBS que ya expiró.

**Request Body:**
```json
{
  "customer_risk_profile": "AGRESIVO",
  "product_description": "Acciones de Mercados Emergentes",
  "investment_amount": "200000.00",
  "sbs_validation": "EXPIRADO"
}
```

**Response Body:**
```json
{
  "compatibility_result": "NO_PROCESABLE",
  "error_code": "VALIDATION_EXPIRED",
  "message": "La validación SBS ha expirado. Debe renovar la validación antes de proceder",
  "required_action": "RENOVAR_VALIDACION_SBS",
  "expiry_date": "2024-02-15T10:00:00-05:00",
  "current_date": "2024-03-18T11:25:00-05:00",
  "days_expired": 32
}
```

---

**happy_path** (HTTP 200)

Successful risk compatibility check showing high compatibility

**Request Body:**
```json
{
  "customer_risk_profile": "moderado",
  "product_description": "Fondo Mutuo Balanceado",
  "investment_amount": "50000.00",
  "sbs_validation": "aprobado"
}
```

**Response Body:**
```json
{
  "success": true,
  "data": {
    "compatibility_status": "compatible",
    "compatibility_score": 88,
    "risk_analysis": {
      "customer_risk_tolerance": "medio",
      "product_risk_level": "medio",
      "investment_horizon": "mediano_plazo",
      "volatility_match": "alta"
    },
    "recommendations": [
      "Producto adecuado para el perfil del cliente",
      "Diversificación recomendada en cartera"
    ],
    "warnings": [],
    "approval_required": false,
    "analysis_date": "2024-01-15T11:20:00-05:00"
  }
}
```

---

### Web Action: `getCustomerProfile`

**alt_happy_corporate_client** (HTTP 200)

Successful profile retrieval for corporate customer

> **Tester prompt:** Pregúntale al agente sobre el perfil de un cliente corporativo.

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "customer_id": "CORP-789456",
  "customer_type": "CORPORATIVO",
  "company_name": "Inversiones San Martín S.A.C.",
  "ruc": "20567891234",
  "risk_profile": "CONSERVADOR",
  "investment_experience": "ALTA",
  "annual_income": 2500000.0,
  "net_worth": 8500000.0,
  "compliance_status": "APROBADO",
  "last_update": "2024-03-15",
  "branch_code": "LIM-007",
  "relationship_manager": "Carlos Mendoza"
}
```

---

**edge_case** (HTTP 200)

Customer with minimal profile information

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "success": true,
  "data": {
    "customer_id": "20987654321",
    "full_name": "Juan Carlos Pérez",
    "document_type": "DNI",
    "document_number": "98765432",
    "email": null,
    "phone": null,
    "address": "",
    "occupation": null,
    "monthly_income": 0.0,
    "risk_profile": "no_definido",
    "customer_segment": "básico",
    "account_opening_date": "2024-01-20",
    "kyc_status": "pendiente",
    "last_updated": "2024-01-20T09:00:00-05:00"
  }
}
```

---

**edge_minimal_profile** (HTTP 200)

Profile with minimal required data only

> **Tester prompt:** Consulta sobre un perfil de cliente con información mínima.

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "customer_id": "MIN-000001",
  "customer_type": "NATURAL",
  "full_name": "Ana Pérez",
  "dni": "12345678",
  "risk_profile": "NO_DEFINIDO",
  "investment_experience": "NINGUNA",
  "annual_income": 0.0,
  "net_worth": 0.0,
  "compliance_status": "PENDIENTE",
  "last_update": "2024-01-01",
  "branch_code": null,
  "relationship_manager": null
}
```

---

**error_case** (HTTP 404)

Customer not found in system

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "success": false,
  "error": {
    "code": "CUSTOMER_NOT_FOUND",
    "message": "Cliente no encontrado en el sistema",
    "details": "No se pudo localizar el perfil del cliente solicitado"
  }
}
```

---

**error_system_maintenance** (HTTP 503)

System maintenance error during profile retrieval

> **Tester prompt:** Solicita información del perfil del cliente durante el mantenimiento del sistema.

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "error": true,
  "code": "SYS_MAINTENANCE",
  "message": "Sistema en mantenimiento programado. Intente nuevamente en 30 minutos.",
  "retry_after": 1800
}
```

---

**happy_path** (HTTP 200)

Successful retrieval of customer profile with complete information

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "success": true,
  "data": {
    "customer_id": "20451234567",
    "full_name": "María Elena Rodríguez Vargas",
    "document_type": "DNI",
    "document_number": "45123456",
    "email": "maria.rodriguez@email.com",
    "phone": "+51987654321",
    "address": "Av. Javier Prado Este 4200, San Borja, Lima",
    "occupation": "Gerente de Ventas",
    "monthly_income": 8500.0,
    "risk_profile": "moderado",
    "customer_segment": "premium",
    "account_opening_date": "2019-03-15",
    "kyc_status": "completo",
    "last_updated": "2024-01-15T10:30:00-05:00"
  }
}
```

---

### Web Action: `validateSBSCompliance`

**alt_happy_pension_fund** (HTTP 200)

Successful SBS validation for pension fund investment

> **Tester prompt:** Dile al agente que quieres validar una inversión de S/ 125,000.50 en un fondo de pensiones mixto.

**Request Body:**
```json
{
  "customer_id": "PEN-445566",
  "product_type": "Fondo de Pensiones Mixto AFP",
  "investment_amount": "125000.50",
  "customer_profile": "MODERADO"
}
```

**Response Body:**
```json
{
  "validation_result": "APROBADO",
  "sbs_reference": "SBS-PEN-2024-0789",
  "compliance_score": 92,
  "requirements_met": [
    "CONOCIMIENTO_PRODUCTO",
    "CAPACIDAD_FINANCIERA",
    "PERFIL_ADECUADO"
  ],
  "validation_date": "2024-03-18T14:30:00-05:00",
  "expiry_date": "2024-09-18T14:30:00-05:00",
  "additional_notes": "Cliente calificado para productos de pensiones de riesgo moderado"
}
```

---

**edge_case** (HTTP 200)

SBS validation for minimum investment amount

**Request Body:**
```json
{
  "customer_id": "20111222333",
  "product_type": "Depósito a Plazo",
  "investment_amount": "100.00",
  "customer_profile": "básico"
}
```

**Response Body:**
```json
{
  "success": true,
  "data": {
    "compliance_status": "aprobado_condicional",
    "sbs_validation_id": "SBS-2024-005678",
    "validation_date": "2024-01-15T16:45:00-05:00",
    "regulations_checked": [
      "Ley N° 26702"
    ],
    "risk_assessment": "bajo",
    "required_disclosures": [
      "Seguro de depósitos FONDO DE SEGURO DE DEPÓSITOS"
    ],
    "validity_period": "30 días",
    "compliance_score": 75,
    "conditions": [
      "Monto mínimo de inversión"
    ]
  }
}
```

---

**edge_maximum_investment** (HTTP 200)

Validation at maximum allowed investment limit

> **Tester prompt:** Pregunta sobre validar una inversión máxima de S/ 10 millones en bonos corporativos.

**Request Body:**
```json
{
  "customer_id": "MAX-111222",
  "product_type": "Bonos Corporativos AAA",
  "investment_amount": "10000000.00",
  "customer_profile": "AGRESIVO"
}
```

**Response Body:**
```json
{
  "validation_result": "APROBADO_CONDICIONAL",
  "sbs_reference": "SBS-MAX-2024-1001",
  "compliance_score": 85,
  "requirements_met": [
    "CAPACIDAD_FINANCIERA_VERIFICADA",
    "EXPERIENCIA_ALTA"
  ],
  "additional_validations_required": [
    "APROBACION_GERENCIAL",
    "DOCUMENTACION_ADICIONAL"
  ],
  "validation_date": "2024-03-18T16:00:00-05:00",
  "expiry_date": "2024-03-25T16:00:00-05:00",
  "warning": "Monto en límite máximo - requiere aprobación adicional"
}
```

---

**error_case** (HTTP 400)

SBS compliance validation failed due to regulatory restrictions

**Request Body:**
```json
{
  "customer_id": "20555666777",
  "product_type": "Derivados Financieros",
  "investment_amount": "150000.00",
  "customer_profile": "conservador"
}
```

**Response Body:**
```json
{
  "success": false,
  "error": {
    "code": "SBS_COMPLIANCE_FAILED",
    "message": "Validación SBS no superada",
    "details": "El producto no es compatible con el perfil de riesgo del cliente según normativa SBS",
    "violated_regulations": [
      "Resolución SBS N° 1041-2021"
    ],
    "required_actions": [
      "Actualizar perfil de riesgo del cliente",
      "Completar evaluación de idoneidad"
    ]
  }
}
```

---

**error_high_risk_mismatch** (HTTP 400)

Validation failed due to high risk product mismatch

> **Tester prompt:** Solicita validación para invertir S/ 750,000 en derivados financieros siendo un cliente conservador.

**Request Body:**
```json
{
  "customer_id": "HRM-998877",
  "product_type": "Derivados Financieros Estructurados",
  "investment_amount": "750000.00",
  "customer_profile": "CONSERVADOR"
}
```

**Response Body:**
```json
{
  "validation_result": "RECHAZADO",
  "error_code": "RISK_MISMATCH",
  "message": "El producto solicitado no es compatible con el perfil de riesgo conservador del cliente",
  "failed_requirements": [
    "PERFIL_RIESGO_INADECUADO",
    "PRODUCTO_COMPLEJO"
  ],
  "recommendation": "Considere productos de renta fija o fondos conservadores",
  "validation_date": "2024-03-18T15:45:00-05:00"
}
```

---

**happy_path** (HTTP 200)

Successful SBS compliance validation for investment product

**Request Body:**
```json
{
  "customer_id": "20451234567",
  "product_type": "Fondo Mutuo Conservador",
  "investment_amount": "25000.00",
  "customer_profile": "moderado"
}
```

**Response Body:**
```json
{
  "success": true,
  "data": {
    "compliance_status": "aprobado",
    "sbs_validation_id": "SBS-2024-001234",
    "validation_date": "2024-01-15T14:30:00-05:00",
    "regulations_checked": [
      "Resolución SBS N° 8425-2011",
      "Circular G-140-2010",
      "Ley N° 26702"
    ],
    "risk_assessment": "compatible",
    "required_disclosures": [
      "Riesgo de mercado",
      "Comisiones aplicables",
      "Política de liquidez"
    ],
    "validity_period": "90 días",
    "compliance_score": 95
  }
}
```

---
