# Testing Guide: Evaluador de Préstamos Personales Interbank

Test scenarios with full request/response examples.

## Dataset: Generated (2026-03-19 03:53) **(Active)**

AI-generated mock data for Agente de Elegibilidad de Préstamos Interbank

_Source: generated_

### Web Action: `calcularCapacidadPago`

**edge_case** (HTTP 200)

Ingreso mínimo y monto mínimo

> **Tester prompt:** Menciona que tu ingreso anual es S/18,000, eres empleado dependiente y solicitas un préstamo pequeño de S/1,000.

**Request Body:**
```json
{
  "ingreso_anual": "18000",
  "estado_laboral": "empleado_dependiente",
  "monto_solicitado": "1000"
}
```

**Response Body:**
```json
{
  "success": true,
  "capacidad_pago": {
    "ingreso_mensual": 1500,
    "capacidad_maxima": 8000,
    "monto_solicitado": 1000,
    "aprobado": true,
    "cuota_maxima": 450,
    "cuota_estimada": 95,
    "porcentaje_ingresos": 6.3,
    "observaciones": "Monto conservador, aprobación probable",
    "plazo_recomendado": 12
  }
}
```

---

**error_case** (HTTP 200)

Ingresos insuficientes para el monto solicitado

> **Tester prompt:** Indica que tu ingreso anual es S/36,000, eres trabajador independiente y necesitas un préstamo de S/120,000.

**Request Body:**
```json
{
  "ingreso_anual": "36000",
  "estado_laboral": "independiente",
  "monto_solicitado": "120000"
}
```

**Response Body:**
```json
{
  "success": true,
  "capacidad_pago": {
    "ingreso_mensual": 3000,
    "capacidad_maxima": 35000,
    "monto_solicitado": 120000,
    "aprobado": false,
    "motivo": "Monto solicitado excede capacidad de pago",
    "monto_maximo_recomendado": 35000,
    "porcentaje_ingresos": 400,
    "limite_recomendado": 30
  }
}
```

---

**happy_path** (HTTP 200)

Cálculo exitoso de capacidad de pago

> **Tester prompt:** Dile al agente que tu ingreso anual es S/96,000, eres empleado dependiente y quieres un préstamo de S/75,000.

**Request Body:**
```json
{
  "ingreso_anual": "96000",
  "estado_laboral": "empleado_dependiente",
  "monto_solicitado": "75000"
}
```

**Response Body:**
```json
{
  "success": true,
  "capacidad_pago": {
    "ingreso_mensual": 8000,
    "capacidad_maxima": 85000,
    "monto_solicitado": 75000,
    "aprobado": true,
    "cuota_maxima": 2400,
    "cuota_estimada": 2150,
    "porcentaje_ingresos": 26.9,
    "observaciones": "Capacidad de pago adecuada"
  }
}
```

---

**retired_client** (HTTP 200)

Cliente jubilado con pensión

> **Tester prompt:** Comenta que eres jubilado con ingresos anuales de S/48,000 y deseas un préstamo de S/25,000.

**Request Body:**
```json
{
  "ingreso_anual": "48000",
  "estado_laboral": "jubilado",
  "monto_solicitado": "25000"
}
```

**Response Body:**
```json
{
  "success": true,
  "capacidad_pago": {
    "ingreso_mensual": 4000,
    "capacidad_maxima": 30000,
    "monto_solicitado": 25000,
    "aprobado": true,
    "cuota_maxima": 1200,
    "cuota_estimada": 850,
    "porcentaje_ingresos": 21.3,
    "observaciones": "Cliente jubilado - condiciones especiales aplicables",
    "plazo_maximo_recomendado": 36
  }
}
```

---

**validation_error** (HTTP 400)

Datos de entrada inválidos

> **Tester prompt:** Indica que no tienes ingresos, eres estudiante y quieres un préstamo de S/50,000.

**Request Body:**
```json
{
  "ingreso_anual": "0",
  "estado_laboral": "estudiante",
  "monto_solicitado": "50000"
}
```

**Response Body:**
```json
{
  "success": false,
  "error": "DATOS_INVALIDOS",
  "message": "Los datos proporcionados no son válidos para el cálculo",
  "errores": [
    "Ingreso anual debe ser mayor a cero",
    "Estado laboral no califica para préstamos"
  ],
  "codigo_error": "ERR_400_VALIDACION"
}
```

---

### Web Action: `consultarClienteExistente`

**edge_case** (HTTP 200)

Score crediticio mínimo y datos incompletos

> **Tester prompt:** Indica que tu ingreso anual es S/18,000 y pregunta sobre tu situación crediticia.

**Request Body:**
```json
{
  "ingreso_anual": "18000",
  "score_crediticio": "300"
}
```

**Response Body:**
```json
{
  "success": true,
  "cliente_existe": true,
  "datos_cliente": {
    "nombre": "Juan Carlos Pérez",
    "dni": "12345678",
    "telefono": null,
    "email": null,
    "fecha_registro": "2023-11-30",
    "productos_activos": [],
    "historial_crediticio": "Limitado",
    "observaciones": "Perfil de riesgo alto"
  }
}
```

---

**error_case** (HTTP 404)

Cliente no encontrado en el sistema

> **Tester prompt:** Menciona que tu ingreso anual es S/45,000 y consulta sobre tu elegibilidad como nuevo cliente.

**Request Body:**
```json
{
  "ingreso_anual": "45000",
  "score_crediticio": "650"
}
```

**Response Body:**
```json
{
  "success": false,
  "error": "CLIENTE_NO_ENCONTRADO",
  "message": "No se encontró información del cliente en nuestros registros",
  "codigo_error": "ERR_404_CLIENTE"
}
```

---

**happy_path** (HTTP 200)

Cliente existente con buen score crediticio

> **Tester prompt:** Dile al agente que tu ingreso anual es S/85,000 y pregunta si eres cliente existente.

**Request Body:**
```json
{
  "ingreso_anual": "85000",
  "score_crediticio": "720"
}
```

**Response Body:**
```json
{
  "success": true,
  "cliente_existe": true,
  "datos_cliente": {
    "nombre": "María Elena Rodríguez Vargas",
    "dni": "45678912",
    "telefono": "+51 987 654 321",
    "email": "maria.rodriguez@gmail.com",
    "fecha_registro": "2018-03-15",
    "productos_activos": [
      "Cuenta Corriente",
      "Tarjeta de Crédito Platinum"
    ],
    "historial_crediticio": "Excelente"
  }
}
```

---

**high_income_client** (HTTP 200)

Cliente premium con alto ingreso

> **Tester prompt:** Comenta que tu ingreso anual es S/250,000 y consulta sobre productos premium disponibles.

**Request Body:**
```json
{
  "ingreso_anual": "250000",
  "score_crediticio": "850"
}
```

**Response Body:**
```json
{
  "success": true,
  "cliente_existe": true,
  "datos_cliente": {
    "nombre": "Carlos Alberto Mendoza Silva",
    "dni": "87654321",
    "telefono": "+51 999 888 777",
    "email": "carlos.mendoza@empresa.com.pe",
    "fecha_registro": "2015-07-20",
    "productos_activos": [
      "Cuenta Premium",
      "Tarjeta Black",
      "Inversiones",
      "Seguro de Vida"
    ],
    "historial_crediticio": "Excepcional",
    "categoria_cliente": "Premium"
  }
}
```

---

**system_error** (HTTP 500)

Error interno del sistema

> **Tester prompt:** Proporciona un ingreso anual de S/60,000 y solicita información sobre tu perfil crediticio.

**Request Body:**
```json
{
  "ingreso_anual": "60000",
  "score_crediticio": "680"
}
```

**Response Body:**
```json
{
  "success": false,
  "error": "ERROR_INTERNO",
  "message": "Error temporal del sistema. Por favor intente nuevamente en unos minutos",
  "codigo_error": "ERR_500_SISTEMA",
  "timestamp": "2024-01-15T10:30:00Z"
}
```

---

### Web Action: `consultarProductosPrestamos`

**edge_case** (HTTP 200)

Catálogo vacío por mantenimiento

> **Tester prompt:** Consulta sobre los préstamos que tiene disponibles el banco.

**Response Body:**
```json
{
  "success": true,
  "productos": [],
  "message": "Catálogo en actualización",
  "fecha_actualizacion": "2024-01-15T15:00:00Z"
}
```

---

**error_case** (HTTP 503)

Servicio de productos no disponible

> **Tester prompt:** Solicita información sobre los productos de préstamos disponibles.

**Response Body:**
```json
{
  "success": false,
  "error": "SERVICIO_NO_DISPONIBLE",
  "message": "El catálogo de productos no está disponible temporalmente",
  "codigo_error": "ERR_503_PRODUCTOS",
  "tiempo_estimado_resolucion": "30 minutos"
}
```

---

**happy_path** (HTTP 200)

Lista completa de productos de préstamos disponibles

> **Tester prompt:** Pregunta al agente qué tipos de préstamos ofrece Interbank.

**Response Body:**
```json
{
  "success": true,
  "productos": [
    {
      "id": "PERS_001",
      "nombre": "Préstamo Personal Interbank",
      "tipo": "Personal",
      "monto_minimo": 3000,
      "monto_maximo": 150000,
      "tasa_minima": 18.5,
      "tasa_maxima": 35.0,
      "plazo_minimo": 12,
      "plazo_maximo": 72,
      "moneda": "PEN"
    },
    {
      "id": "HIP_001",
      "nombre": "Crédito Hipotecario Mi Vivienda",
      "tipo": "Hipotecario",
      "monto_minimo": 50000,
      "monto_maximo": 2000000,
      "tasa_minima": 7.8,
      "tasa_maxima": 12.5,
      "plazo_minimo": 60,
      "plazo_maximo": 300,
      "moneda": "PEN"
    },
    {
      "id": "VEH_001",
      "nombre": "Préstamo Vehicular",
      "tipo": "Vehicular",
      "monto_minimo": 15000,
      "monto_maximo": 500000,
      "tasa_minima": 12.9,
      "tasa_maxima": 22.0,
      "plazo_minimo": 12,
      "plazo_maximo": 84,
      "moneda": "PEN"
    }
  ]
}
```

---

**limited_products** (HTTP 200)

Solo productos básicos disponibles

> **Tester prompt:** Pregunta sobre las opciones de préstamos más sencillas que ofrece el banco.

**Response Body:**
```json
{
  "success": true,
  "productos": [
    {
      "id": "PERS_BASIC",
      "nombre": "Préstamo Personal Básico",
      "tipo": "Personal",
      "monto_minimo": 1000,
      "monto_maximo": 50000,
      "tasa_minima": 25.0,
      "tasa_maxima": 40.0,
      "plazo_minimo": 6,
      "plazo_maximo": 36,
      "moneda": "PEN",
      "requisitos": "Ingresos mínimos S/1,500"
    }
  ],
  "nota": "Productos premium requieren evaluación adicional"
}
```

---

**promotional_products** (HTTP 200)

Productos con promociones especiales

> **Tester prompt:** Consulta si hay alguna promoción especial en préstamos personales.

**Response Body:**
```json
{
  "success": true,
  "productos": [
    {
      "id": "PERS_PROMO",
      "nombre": "Préstamo Personal Campaña Navideña",
      "tipo": "Personal",
      "monto_minimo": 5000,
      "monto_maximo": 100000,
      "tasa_minima": 15.9,
      "tasa_maxima": 28.0,
      "plazo_minimo": 12,
      "plazo_maximo": 60,
      "moneda": "PEN",
      "promocion": "Tasa especial hasta 31/12/2024",
      "descuento_comisiones": true
    }
  ],
  "promociones_activas": true,
  "vigencia_promocion": "2024-12-31"
}
```

---

### Web Action: `verificarPrestamoPreaprobado`

**client_not_found** (HTTP 404)

ID de cliente no válido

> **Tester prompt:** Proporciona el ID CLI_INVALID y pregunta sobre preaprobaciones disponibles.

**Request Body:**
```json
{
  "cliente_id": "CLI_INVALID",
  "score_crediticio": "650"
}
```

**Response Body:**
```json
{
  "success": false,
  "error": "CLIENTE_NO_ENCONTRADO",
  "message": "El ID de cliente proporcionado no existe en nuestros registros",
  "codigo_error": "ERR_404_CLIENTE_ID",
  "sugerencia": "Verifique el ID de cliente o contacte a servicio al cliente"
}
```

---

**edge_case** (HTTP 200)

Cliente con preaprobación vencida

> **Tester prompt:** Menciona tu ID CLI_999888 y pregunta si tienes ofertas preaprobadas vigentes.

**Request Body:**
```json
{
  "cliente_id": "CLI_999888",
  "score_crediticio": "720"
}
```

**Response Body:**
```json
{
  "success": true,
  "preaprobado": false,
  "motivo": "Preaprobación vencida",
  "fecha_vencimiento": "2024-01-10",
  "monto_anterior": 80000,
  "renovacion_disponible": true,
  "mensaje": "Puede solicitar nueva evaluación para preaprobación"
}
```

---

**error_case** (HTTP 200)

Cliente no califica para preaprobación

> **Tester prompt:** Usa el ID de cliente CLI_123789 y consulta sobre preaprobaciones disponibles.

**Request Body:**
```json
{
  "cliente_id": "CLI_123789",
  "score_crediticio": "480"
}
```

**Response Body:**
```json
{
  "success": true,
  "preaprobado": false,
  "motivo": "Score crediticio insuficiente",
  "score_minimo_requerido": 550,
  "recomendaciones": [
    "Mejorar historial crediticio",
    "Aumentar ingresos demostrables",
    "Reducir deudas existentes"
  ],
  "proxima_evaluacion": "2024-04-01"
}
```

---

**happy_path** (HTTP 200)

Cliente con préstamo preaprobado

> **Tester prompt:** Proporciona tu ID de cliente CLI_789456 y pregunta si tienes algún préstamo preaprobado.

**Request Body:**
```json
{
  "cliente_id": "CLI_789456",
  "score_crediticio": "750"
}
```

**Response Body:**
```json
{
  "success": true,
  "preaprobado": true,
  "detalles": {
    "monto_preaprobado": 120000,
    "tasa_ofrecida": 19.5,
    "plazo_maximo": 60,
    "producto_id": "PERS_001",
    "vigencia_preaprobacion": "2024-02-15",
    "condiciones": "Sujeto a verificación de ingresos",
    "cuota_estimada": 3250
  }
}
```

---

**premium_preapproval** (HTTP 200)

Cliente premium con múltiples preaprobaciones

> **Tester prompt:** Indica que tu ID de cliente es CLI_VIP001 y consulta sobre tus preaprobaciones premium.

**Request Body:**
```json
{
  "cliente_id": "CLI_VIP001",
  "score_crediticio": "820"
}
```

**Response Body:**
```json
{
  "success": true,
  "preaprobado": true,
  "detalles": [
    {
      "producto_id": "PERS_PREMIUM",
      "monto_preaprobado": 300000,
      "tasa_ofrecida": 16.9,
      "plazo_maximo": 72
    },
    {
      "producto_id": "VEH_PREMIUM",
      "monto_preaprobado": 450000,
      "tasa_ofrecida": 11.5,
      "plazo_maximo": 84
    }
  ],
  "categoria_cliente": "Premium",
  "gestor_asignado": "Ana María Flores"
}
```

---

## Dataset: Generated (2026-02-27 22:23)

AI-generated mock data for Agente de Elegibilidad de Préstamos Interbank

_Source: generated_

### Web Action: `calcularCapacidadPago`

**edge_case** (HTTP 200)

Cliente con ingresos mínimos

**Request Body:**
```json
{
  "ingreso_anual": 18000.0,
  "gastos_estimados": 12000.0,
  "deudas_existentes": 0.0
}
```

**Response Body:**
```json
{
  "success": true,
  "capacidad_pago": {
    "ingreso_neto_mensual": 500.0,
    "capacidad_pago_mensual": 200.0,
    "ratio_endeudamiento": 0.0,
    "monto_maximo_prestamo": 12000.0,
    "cuota_maxima_recomendada": 200.0,
    "evaluacion": "CONDICIONAL",
    "observaciones": "Capacidad de pago limitada, solo productos básicos disponibles",
    "productos_disponibles": [
      "Préstamo Personal Básico"
    ],
    "moneda": "PEN"
  }
}
```

---

**error_case** (HTTP 200)

Capacidad de pago insuficiente

**Request Body:**
```json
{
  "ingreso_anual": 24000.0,
  "gastos_estimados": 20000.0,
  "deudas_existentes": 15000.0
}
```

**Response Body:**
```json
{
  "success": true,
  "capacidad_pago": {
    "ingreso_neto_mensual": 333.33,
    "capacidad_pago_mensual": 0.0,
    "ratio_endeudamiento": 0.75,
    "monto_maximo_prestamo": 0.0,
    "cuota_maxima_recomendada": 0.0,
    "evaluacion": "RECHAZADO",
    "observaciones": "Ratio de endeudamiento excede el límite permitido (75% > 60%)",
    "recomendaciones": [
      "Reducir deudas existentes",
      "Aumentar ingresos",
      "Reestructurar gastos"
    ],
    "moneda": "PEN"
  }
}
```

---

**happy_path** (HTTP 200)

Capacidad de pago calculada exitosamente

**Request Body:**
```json
{
  "ingreso_anual": 84000.0,
  "gastos_estimados": 45000.0,
  "deudas_existentes": 8500.0
}
```

**Response Body:**
```json
{
  "success": true,
  "capacidad_pago": {
    "ingreso_neto_mensual": 2550.0,
    "capacidad_pago_mensual": 765.0,
    "ratio_endeudamiento": 0.3,
    "monto_maximo_prestamo": 92000.0,
    "cuota_maxima_recomendada": 765.0,
    "evaluacion": "APROBADO",
    "observaciones": "Excelente capacidad de pago, cliente califica para productos premium",
    "moneda": "PEN"
  }
}
```

---

### Web Action: `consultarClienteExistente`

**edge_case** (HTTP 200)

Cliente con datos mínimos registrado

**Request Body:**
```json
{
  "documento_identidad": "00000001",
  "tipo_documento": "DNI"
}
```

**Response Body:**
```json
{
  "success": true,
  "cliente": {
    "id": "CLI-000001",
    "nombres": "Juan",
    "apellidos": "Perez",
    "documento_identidad": "00000001",
    "tipo_documento": "DNI",
    "email": null,
    "telefono": null,
    "direccion": null,
    "fecha_registro": "2024-01-01",
    "estado": "PENDIENTE_VERIFICACION",
    "score_crediticio": null,
    "categoria_cliente": "NUEVO"
  }
}
```

---

**error_case** (HTTP 404)

Cliente no encontrado en el sistema

**Request Body:**
```json
{
  "documento_identidad": "98765432",
  "tipo_documento": "DNI"
}
```

**Response Body:**
```json
{
  "success": false,
  "error": {
    "codigo": "CLI_NOT_FOUND",
    "mensaje": "Cliente no encontrado en el sistema",
    "detalle": "No existe un cliente registrado con el documento de identidad proporcionado"
  }
}
```

---

**happy_path** (HTTP 200)

Cliente existente encontrado exitosamente

**Request Body:**
```json
{
  "documento_identidad": "12345678",
  "tipo_documento": "DNI"
}
```

**Response Body:**
```json
{
  "success": true,
  "cliente": {
    "id": "CLI-789123",
    "nombres": "Carlos Eduardo",
    "apellidos": "Rodriguez Mendoza",
    "documento_identidad": "12345678",
    "tipo_documento": "DNI",
    "email": "carlos.rodriguez@email.com",
    "telefono": "+51987654321",
    "direccion": "Av. Javier Prado Este 4200, San Borja, Lima",
    "fecha_registro": "2019-03-15",
    "estado": "ACTIVO",
    "score_crediticio": 720,
    "categoria_cliente": "PREMIUM"
  }
}
```

---

### Web Action: `consultarProductosPrestamos`

**edge_case** (HTTP 200)

No hay productos disponibles temporalmente

**Response Body:**
```json
{
  "success": true,
  "productos": [],
  "mensaje": "No hay productos de préstamos disponibles en este momento"
}
```

---

**error_case** (HTTP 500)

Error interno del servidor al consultar productos

**Response Body:**
```json
{
  "success": false,
  "error": {
    "codigo": "INTERNAL_ERROR",
    "mensaje": "Error interno del servidor",
    "detalle": "No se pudieron cargar los productos de préstamos en este momento. Intente nuevamente más tarde."
  }
}
```

---

**happy_path** (HTTP 200)

Lista completa de productos de préstamos disponibles

**Response Body:**
```json
{
  "success": true,
  "productos": [
    {
      "id": "PRES-001",
      "nombre": "Préstamo Personal Clásico",
      "descripcion": "Préstamo personal con condiciones flexibles",
      "monto_minimo": 3000.0,
      "monto_maximo": 150000.0,
      "plazo_minimo_meses": 6,
      "plazo_maximo_meses": 60,
      "tasa_interes_anual": 18.5,
      "comision_apertura": 2.5,
      "moneda": "PEN",
      "requisitos": [
        "Ingresos mínimos S/ 1,500",
        "Antigüedad laboral mínima 6 meses"
      ]
    },
    {
      "id": "PRES-002",
      "nombre": "Préstamo Personal Premium",
      "descripcion": "Préstamo con tasas preferenciales para clientes premium",
      "monto_minimo": 10000.0,
      "monto_maximo": 300000.0,
      "plazo_minimo_meses": 12,
      "plazo_maximo_meses": 72,
      "tasa_interes_anual": 15.9,
      "comision_apertura": 1.8,
      "moneda": "PEN",
      "requisitos": [
        "Cliente premium",
        "Ingresos mínimos S/ 5,000",
        "Score crediticio mínimo 650"
      ]
    }
  ]
}
```

---

### Web Action: `verificarPrestamoPreaprobado`

**edge_case** (HTTP 200)

Cliente nuevo sin historial crediticio

**Request Body:**
```json
{
  "cliente_id": "CLI-000001",
  "score_crediticio": 0
}
```

**Response Body:**
```json
{
  "success": true,
  "preaprobado": false,
  "razon": "Sin historial crediticio suficiente",
  "alternativas": [
    {
      "producto": "Préstamo con Garantía",
      "monto_maximo": 15000.0,
      "requisitos_adicionales": [
        "Aval solidario",
        "Garantía real"
      ]
    }
  ]
}
```

---

**error_case** (HTTP 200)

Cliente no califica para preaprobación

**Request Body:**
```json
{
  "cliente_id": "CLI-456789",
  "score_crediticio": 450
}
```

**Response Body:**
```json
{
  "success": true,
  "preaprobado": false,
  "razon": "Score crediticio insuficiente",
  "score_minimo_requerido": 550,
  "recomendaciones": [
    "Mejorar historial crediticio",
    "Reducir deudas existentes",
    "Aumentar ingresos comprobables"
  ]
}
```

---

**happy_path** (HTTP 200)

Cliente tiene préstamo preaprobado

**Request Body:**
```json
{
  "cliente_id": "CLI-789123",
  "score_crediticio": 720
}
```

**Response Body:**
```json
{
  "success": true,
  "preaprobado": true,
  "detalles": {
    "monto_preaprobado": 85000.0,
    "tasa_interes": 16.5,
    "plazo_maximo_meses": 48,
    "producto_recomendado": "PRES-002",
    "vigencia_preaprobacion": "2024-06-30",
    "condiciones_especiales": [
      "Sin comisión de apertura",
      "Evaluación express"
    ],
    "moneda": "PEN"
  }
}
```

---

## Dataset: Generated (2026-03-19 02:07)

AI-generated mock data for Agente de Elegibilidad de Préstamos Interbank

_Source: generated_

### Web Action: `calcularCapacidadPago`

**alto_patrimonio** (HTTP 200)

Cliente con alto patrimonio y excelente score

> **Tester prompt:** Informa que eres empresario con ingresos de S/240,000 anuales, score 850, y necesitas S/150,000.

**Request Body:**
```json
{
  "ingreso_anual": 240000,
  "score_crediticio": 850,
  "estado_laboral": "empresario",
  "monto_solicitado": 150000
}
```

**Request Params:**
```json
[
  {
    "name": "ingreso_anual",
    "value": "240000"
  },
  {
    "name": "score_crediticio",
    "value": "850"
  },
  {
    "name": "estado_laboral",
    "value": "empresario"
  },
  {
    "name": "monto_solicitado",
    "value": "150000"
  }
]
```

**Response Body:**
```json
{
  "aprobado": true,
  "capacidad_pago": 180000,
  "monto_aprobado": 150000,
  "tasa_sugerida": 16.5,
  "plazo_recomendado": 60,
  "cuota_mensual": 3650.75,
  "ratio_endeudamiento": 18.25,
  "categoria_cliente": "Premium",
  "beneficios_adicionales": [
    "Tasa VIP",
    "Sin comisiones",
    "Gestor personalizado",
    "Aprobación express"
  ]
}
```

---

**datos_incompletos** (HTTP 400)

Faltan datos para el cálculo

> **Tester prompt:** Solicita calcular capacidad de pago pero proporciona información incompleta o en ceros.

**Request Body:**
```json
{
  "ingreso_anual": 0,
  "score_crediticio": 0,
  "estado_laboral": "",
  "monto_solicitado": 25000
}
```

**Request Params:**
```json
[
  {
    "name": "ingreso_anual",
    "value": "0"
  },
  {
    "name": "score_crediticio",
    "value": "0"
  },
  {
    "name": "estado_laboral",
    "value": ""
  },
  {
    "name": "monto_solicitado",
    "value": "25000"
  }
]
```

**Response Body:**
```json
{
  "codigo_error": "MISSING_DATA",
  "mensaje": "Datos insuficientes para realizar el cálculo",
  "campos_requeridos": [
    "ingreso_anual",
    "score_crediticio",
    "estado_laboral"
  ],
  "detalle": "Todos los campos son obligatorios para la evaluación"
}
```

---

**edge_case** (HTTP 200)

Ingreso mínimo y score límite

> **Tester prompt:** Proporciona ingresos de S/18,000 anuales, score 300, empleado dependiente, solicitando S/5,000.

**Request Body:**
```json
{
  "ingreso_anual": 18000,
  "score_crediticio": 300,
  "estado_laboral": "empleado_dependiente",
  "monto_solicitado": 5000
}
```

**Request Params:**
```json
[
  {
    "name": "ingreso_anual",
    "value": "18000"
  },
  {
    "name": "score_crediticio",
    "value": "300"
  },
  {
    "name": "estado_laboral",
    "value": "empleado_dependiente"
  },
  {
    "name": "monto_solicitado",
    "value": "5000"
  }
]
```

**Response Body:**
```json
{
  "aprobado": false,
  "capacidad_pago": 0,
  "codigo_rechazo": "SCORE_TOO_LOW",
  "mensaje": "Score crediticio insuficiente para otorgar crédito",
  "score_minimo_requerido": 350,
  "alternativas": [
    "Préstamo con garantía hipotecaria",
    "Préstamo con aval"
  ]
}
```

---

**error_case** (HTTP 200)

Capacidad de pago insuficiente

> **Tester prompt:** Menciona que ganas S/24,000 anuales, score 450, eres independiente y solicitas S/80,000.

**Request Body:**
```json
{
  "ingreso_anual": 24000,
  "score_crediticio": 450,
  "estado_laboral": "independiente",
  "monto_solicitado": 80000
}
```

**Request Params:**
```json
[
  {
    "name": "ingreso_anual",
    "value": "24000"
  },
  {
    "name": "score_crediticio",
    "value": "450"
  },
  {
    "name": "estado_laboral",
    "value": "independiente"
  },
  {
    "name": "monto_solicitado",
    "value": "80000"
  }
]
```

**Response Body:**
```json
{
  "aprobado": false,
  "capacidad_pago": 15000,
  "monto_solicitado": 80000,
  "codigo_rechazo": "INSUFFICIENT_CAPACITY",
  "mensaje": "El monto solicitado excede su capacidad de pago",
  "monto_maximo_sugerido": 12000,
  "recomendaciones": [
    "Considere un monto menor",
    "Mejore su score crediticio",
    "Incluya un co-deudor"
  ]
}
```

---

**happy_path** (HTTP 200)

Cálculo exitoso de capacidad de pago

> **Tester prompt:** Dile al agente que ganas S/84,000 al año, tienes score 720, eres empleado dependiente y quieres un préstamo de S/35,000.

**Request Body:**
```json
{
  "ingreso_anual": 84000,
  "score_crediticio": 720,
  "estado_laboral": "empleado_dependiente",
  "monto_solicitado": 35000
}
```

**Request Params:**
```json
[
  {
    "name": "ingreso_anual",
    "value": "84000"
  },
  {
    "name": "score_crediticio",
    "value": "720"
  },
  {
    "name": "estado_laboral",
    "value": "empleado_dependiente"
  },
  {
    "name": "monto_solicitado",
    "value": "35000"
  }
]
```

**Response Body:**
```json
{
  "aprobado": true,
  "capacidad_pago": 42000,
  "monto_aprobado": 35000,
  "tasa_sugerida": 19.9,
  "plazo_recomendado": 36,
  "cuota_mensual": 1285.5,
  "ratio_endeudamiento": 18.35,
  "observaciones": "Excelente perfil crediticio",
  "condiciones_especiales": [
    "Sin comisiones de evaluación",
    "Tasa preferencial por buen historial"
  ]
}
```

---

### Web Action: `consultarClienteExistente`

**cliente_inactivo** (HTTP 200)

Cliente existente pero con estado inactivo

> **Tester prompt:** Pregunta sobre tu estatus como cliente mencionando que hace tiempo no usas los servicios del banco.

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "existe": true,
  "cliente_id": "CLI987654321",
  "nombres": "María Isabel",
  "apellidos": "Torres Vargas",
  "documento": "25874163",
  "tipo_documento": "DNI",
  "estado": "inactivo",
  "fecha_registro": "2015-08-22",
  "productos_activos": [],
  "fecha_inactivacion": "2023-06-10"
}
```

---

**edge_case** (HTTP 200)

Cliente con datos incompletos

> **Tester prompt:** Consulta tu estatus como cliente mencionando que tus datos personales pueden estar incompletos.

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "existe": true,
  "cliente_id": "CLI000000001",
  "nombres": "",
  "apellidos": "Datos Incompletos",
  "documento": "00000000",
  "tipo_documento": "DNI",
  "estado": "pendiente_actualizacion",
  "fecha_registro": "2024-01-01",
  "productos_activos": []
}
```

---

**error_case** (HTTP 404)

Cliente no encontrado en la base de datos

> **Tester prompt:** Pregúntale al agente sobre tu estatus como cliente pero menciona que nunca has tenido productos con Interbank.

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "existe": false,
  "codigo_error": "CLI_NOT_FOUND",
  "mensaje": "No se encontró información del cliente en nuestros registros",
  "sugerencia": "Verifique sus datos o considere registrarse como nuevo cliente"
}
```

---

**error_sistema** (HTTP 503)

Error temporal del sistema

> **Tester prompt:** Intenta consultar tu información como cliente cuando el sistema esté experimentando problemas técnicos.

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "codigo_error": "SYS_UNAVAILABLE",
  "mensaje": "Servicio temporalmente no disponible",
  "detalle": "Estamos realizando mantenimiento programado",
  "tiempo_estimado_solucion": "15 minutos"
}
```

---

**happy_path** (HTTP 200)

Cliente existente encontrado exitosamente

> **Tester prompt:** Dile al agente que quieres consultar si eres cliente existente de Interbank.

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "existe": true,
  "cliente_id": "CLI001234567",
  "nombres": "Carlos Eduardo",
  "apellidos": "Mendoza Quispe",
  "documento": "43785629",
  "tipo_documento": "DNI",
  "estado": "activo",
  "fecha_registro": "2019-03-15",
  "productos_activos": [
    "cuenta_corriente",
    "tarjeta_credito"
  ]
}
```

---

### Web Action: `consultarProductosPrestamos`

**edge_case** (HTTP 200)

Lista vacía de productos

> **Tester prompt:** Solicita información sobre productos de préstamos cuando la lista esté vacía.

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "productos": [],
  "mensaje": "No se encontraron productos que coincidan con los criterios",
  "total_productos": 0
}
```

---

**error_autorizacion** (HTTP 403)

Error de autorización para consultar productos

> **Tester prompt:** Intenta consultar productos de préstamos sin haberte identificado previamente con el agente.

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "codigo_error": "AUTH_REQUIRED",
  "mensaje": "Se requiere autenticación para acceder a esta información",
  "accion_requerida": "Debe iniciar sesión o proporcionar credenciales válidas"
}
```

---

**error_case** (HTTP 404)

No hay productos disponibles temporalmente

> **Tester prompt:** Consulta sobre los productos de préstamos disponibles cuando no hay ofertas activas.

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "codigo_error": "PROD_NOT_AVAILABLE",
  "mensaje": "No hay productos de préstamos disponibles en este momento",
  "detalle": "Los productos están siendo actualizados",
  "contacto_alternativo": "Llame al 311-9000 para más información"
}
```

---

**happy_path** (HTTP 200)

Lista completa de productos de préstamos disponibles

> **Tester prompt:** Pregúntale al agente qué tipos de préstamos ofrece Interbank y cuáles son sus condiciones.

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "productos": [
    {
      "codigo": "PERS001",
      "nombre": "Préstamo Personal Interbank",
      "tipo": "personal",
      "tasa_minima": 18.5,
      "tasa_maxima": 35.9,
      "monto_minimo": 3000,
      "monto_maximo": 150000,
      "plazo_minimo": 12,
      "plazo_maximo": 60,
      "moneda": "PEN"
    },
    {
      "codigo": "VEH001",
      "nombre": "Préstamo Vehicular",
      "tipo": "vehicular",
      "tasa_minima": 12.9,
      "tasa_maxima": 24.5,
      "monto_minimo": 15000,
      "monto_maximo": 500000,
      "plazo_minimo": 24,
      "plazo_maximo": 84,
      "moneda": "PEN"
    },
    {
      "codigo": "HIP001",
      "nombre": "Crédito Hipotecario MiVivienda",
      "tipo": "hipotecario",
      "tasa_minima": 7.95,
      "tasa_maxima": 12.5,
      "monto_minimo": 50000,
      "monto_maximo": 2000000,
      "plazo_minimo": 60,
      "plazo_maximo": 300,
      "moneda": "PEN"
    }
  ]
}
```

---

**productos_limitados** (HTTP 200)

Solo algunos productos disponibles por región

> **Tester prompt:** Pregunta sobre préstamos disponibles mencionando que vives en una ciudad fuera de Lima.

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "productos": [
    {
      "codigo": "PERS001",
      "nombre": "Préstamo Personal Básico",
      "tipo": "personal",
      "tasa_minima": 22.9,
      "tasa_maxima": 39.9,
      "monto_minimo": 1000,
      "monto_maximo": 50000,
      "plazo_minimo": 6,
      "plazo_maximo": 36,
      "moneda": "PEN",
      "restricciones": "Disponible solo en Lima Metropolitana"
    }
  ],
  "mensaje": "Productos limitados por ubicación geográfica"
}
```

---

### Web Action: `verificarPrestamoPreaprobado`

**edge_case** (HTTP 400)

DNI con formato inválido

> **Tester prompt:** Proporciona un DNI inválido como 00000000 y pregunta sobre préstamos preaprobados.

**Request Body:**
```json
{
  "documento_identidad": "00000000",
  "tipo_prestamo": "personal"
}
```

**Request Params:**
```json
[
  {
    "name": "documento_identidad",
    "value": "00000000"
  },
  {
    "name": "tipo_prestamo",
    "value": "personal"
  }
]
```

**Response Body:**
```json
{
  "preaprobado": false,
  "codigo_error": "INVALID_DOCUMENT",
  "mensaje": "Número de documento inválido",
  "detalle": "El DNI debe tener 8 dígitos válidos",
  "formato_esperado": "########"
}
```

---

**error_case** (HTTP 404)

Cliente sin préstamos preaprobados

> **Tester prompt:** Usa el DNI 87654321 y consulta si tienes préstamos preaprobados sabiendo que no calificas.

**Request Body:**
```json
{
  "documento_identidad": "87654321",
  "tipo_prestamo": "personal"
}
```

**Request Params:**
```json
[
  {
    "name": "documento_identidad",
    "value": "87654321"
  },
  {
    "name": "tipo_prestamo",
    "value": "personal"
  }
]
```

**Response Body:**
```json
{
  "preaprobado": false,
  "codigo_error": "NO_PREAPPROVED",
  "mensaje": "No se encontraron préstamos preaprobados para este cliente",
  "alternativas": "Puede aplicar a una evaluación crediticia estándar",
  "productos_sugeridos": [
    "Préstamo Personal Básico"
  ]
}
```

---

**happy_path** (HTTP 200)

Cliente con préstamo preaprobado exitosamente

> **Tester prompt:** Proporciona tu DNI 43785629 y pregunta si tienes algún préstamo personal preaprobado.

**Request Body:**
```json
{
  "documento_identidad": "43785629",
  "tipo_prestamo": "personal"
}
```

**Request Params:**
```json
[
  {
    "name": "documento_identidad",
    "value": "43785629"
  },
  {
    "name": "tipo_prestamo",
    "value": "personal"
  }
]
```

**Response Body:**
```json
{
  "preaprobado": true,
  "monto_preaprobado": 45000,
  "tasa_ofrecida": 21.5,
  "plazo_maximo": 48,
  "moneda": "PEN",
  "vigencia_oferta": "2024-12-31",
  "codigo_preaprobacion": "PRE2024001234",
  "requisitos": [
    "Copia de DNI",
    "Último recibo de sueldo",
    "Estados de cuenta últimos 3 meses"
  ]
}
```

---

**oferta_vencida** (HTTP 200)

Préstamo preaprobado pero oferta vencida

> **Tester prompt:** Consulta con DNI 12345678 sobre préstamos preaprobados que pudieron haber vencido.

**Request Body:**
```json
{
  "documento_identidad": "12345678",
  "tipo_prestamo": "personal"
}
```

**Request Params:**
```json
[
  {
    "name": "documento_identidad",
    "value": "12345678"
  },
  {
    "name": "tipo_prestamo",
    "value": "personal"
  }
]
```

**Response Body:**
```json
{
  "preaprobado": false,
  "monto_anterior": 30000,
  "fecha_vencimiento": "2024-01-15",
  "codigo_error": "OFFER_EXPIRED",
  "mensaje": "La oferta preaprobada ha vencido",
  "accion_sugerida": "Solicite una nueva evaluación crediticia"
}
```

---

**tipo_prestamo_invalido** (HTTP 400)

Tipo de préstamo no válido

> **Tester prompt:** Usa DNI 98765432 y pregunta sobre préstamos empresariales preaprobados.

**Request Body:**
```json
{
  "documento_identidad": "98765432",
  "tipo_prestamo": "empresarial"
}
```

**Request Params:**
```json
[
  {
    "name": "documento_identidad",
    "value": "98765432"
  },
  {
    "name": "tipo_prestamo",
    "value": "empresarial"
  }
]
```

**Response Body:**
```json
{
  "codigo_error": "INVALID_LOAN_TYPE",
  "mensaje": "Tipo de préstamo no válido para preaprobaciones",
  "tipos_validos": [
    "personal",
    "vehicular",
    "hipotecario"
  ],
  "detalle": "Los préstamos empresariales requieren evaluación especializada"
}
```

---

## Dataset: Generated (2026-03-19 03:34)

AI-generated mock data for Agente de Elegibilidad de Préstamos Interbank

_Source: generated_

### Web Action: `calcularCapacidadPago`

**edge_case** (HTTP 200)

Score crediticio mínimo con ingresos altos

> **Tester prompt:** Dile al agente que ganas S/200,000 anuales con gastos de S/50,000, y consulta tu capacidad de préstamo.

**Request Body:**
```json
{
  "ingreso_anual": 200000,
  "gastos_fijos": 50000,
  "score_crediticio": 300
}
```

**Response Body:**
```json
{
  "success": true,
  "capacidad_pago": {
    "ingreso_disponible": 150000,
    "capacidad_mensual": 3000,
    "monto_maximo_prestamo": 45000,
    "clasificacion": "LIMITADA",
    "tasa_sugerida": 45.9,
    "plazo_recomendado": 18,
    "ratio_endeudamiento": 0.25,
    "observaciones": "Score crediticio bajo limita el monto"
  }
}
```

---

**error_case** (HTTP 200)

Capacidad de pago insuficiente

> **Tester prompt:** Menciona al agente que tienes ingresos anuales de S/24,000 y gastos fijos de S/22,000, y pregunta sobre opciones de préstamo.

**Request Body:**
```json
{
  "ingreso_anual": 24000,
  "gastos_fijos": 22000,
  "score_crediticio": 450
}
```

**Response Body:**
```json
{
  "success": true,
  "capacidad_pago": {
    "ingreso_disponible": 2000,
    "capacidad_mensual": 0,
    "monto_maximo_prestamo": 0,
    "clasificacion": "INSUFICIENTE",
    "ratio_endeudamiento": 0.92,
    "recomendacion": "Ingresos insuficientes para otorgar préstamo",
    "sugerencia": "Considere reducir gastos fijos o incrementar ingresos"
  }
}
```

---

**happy_path** (HTTP 200)

Cálculo exitoso de capacidad de pago alta

> **Tester prompt:** Informa al agente que ganas S/120,000 al año, tienes gastos fijos de S/45,000 y pregunta cuánto podrías solicitar de préstamo.

**Request Body:**
```json
{
  "ingreso_anual": 120000,
  "gastos_fijos": 45000,
  "score_crediticio": 750
}
```

**Response Body:**
```json
{
  "success": true,
  "capacidad_pago": {
    "ingreso_disponible": 75000,
    "capacidad_mensual": 4500,
    "monto_maximo_prestamo": 180000,
    "clasificacion": "ALTA",
    "tasa_sugerida": 18.9,
    "plazo_recomendado": 48,
    "ratio_endeudamiento": 0.25,
    "observaciones": "Excelente perfil crediticio"
  }
}
```

---

**moderate_capacity** (HTTP 200)

Capacidad de pago moderada

> **Tester prompt:** Comparte con el agente que tus ingresos anuales son S/60,000 y gastos fijos S/35,000, y pregunta por tu capacidad de préstamo.

**Request Body:**
```json
{
  "ingreso_anual": 60000,
  "gastos_fijos": 35000,
  "score_crediticio": 650
}
```

**Response Body:**
```json
{
  "success": true,
  "capacidad_pago": {
    "ingreso_disponible": 25000,
    "capacidad_mensual": 1500,
    "monto_maximo_prestamo": 65000,
    "clasificacion": "MODERADA",
    "tasa_sugerida": 28.5,
    "plazo_recomendado": 36,
    "ratio_endeudamiento": 0.58,
    "observaciones": "Capacidad adecuada con condiciones estándar"
  }
}
```

---

**validation_error** (HTTP 400)

Error en validación de datos

> **Tester prompt:** Proporciona al agente información financiera incorrecta para probar la validación del sistema.

**Request Body:**
```json
{
  "ingreso_anual": -5000,
  "gastos_fijos": 30000,
  "score_crediticio": 850
}
```

**Response Body:**
```json
{
  "success": false,
  "error": {
    "codigo": "VALIDATION_ERROR",
    "mensaje": "Datos de entrada inválidos",
    "detalles": [
      {
        "campo": "ingreso_anual",
        "error": "Debe ser mayor a cero"
      },
      {
        "campo": "gastos_fijos",
        "error": "No pueden ser mayores a los ingresos"
      }
    ]
  }
}
```

---

### Web Action: `consultarClienteExistente`

**edge_case** (HTTP 200)

DNI con formato límite válido

> **Tester prompt:** Indica al agente que tu número de DNI es 00000001 y consulta sobre opciones de préstamos.

**Request Body:**
```json
{
  "documento_identidad": "00000001",
  "tipo_documento": "DNI"
}
```

**Response Body:**
```json
{
  "success": true,
  "cliente": {
    "id": "CLI-MIN-001",
    "nombre_completo": "Ana Lucia Torres",
    "documento_identidad": "00000001",
    "tipo_documento": "DNI",
    "telefono": "+51 900000001",
    "email": "ana.torres@hotmail.com",
    "estado": "ACTIVO",
    "fecha_registro": "2024-01-01",
    "segmento": "NUEVO"
  }
}
```

---

**error_case** (HTTP 404)

Cliente no encontrado en el sistema

> **Tester prompt:** Menciona al agente que tu DNI es 87654321 y pregunta sobre préstamos disponibles.

**Request Body:**
```json
{
  "documento_identidad": "87654321",
  "tipo_documento": "DNI"
}
```

**Response Body:**
```json
{
  "success": false,
  "error": {
    "codigo": "CLI_NOT_FOUND",
    "mensaje": "No se encontró cliente con el documento proporcionado",
    "detalle": "El DNI 87654321 no está registrado en nuestro sistema"
  }
}
```

---

**happy_path** (HTTP 200)

Cliente existente encontrado exitosamente

> **Tester prompt:** Dile al agente que tu DNI es 12345678 y que quieres consultar información de préstamos.

**Request Body:**
```json
{
  "documento_identidad": "12345678",
  "tipo_documento": "DNI"
}
```

**Response Body:**
```json
{
  "success": true,
  "cliente": {
    "id": "CLI-001-2024",
    "nombre_completo": "María Elena Rodriguez Vargas",
    "documento_identidad": "12345678",
    "tipo_documento": "DNI",
    "telefono": "+51 987654321",
    "email": "maria.rodriguez@gmail.com",
    "estado": "ACTIVO",
    "fecha_registro": "2020-03-15",
    "segmento": "PREMIUM"
  }
}
```

---

**server_error** (HTTP 500)

Error interno del servidor

> **Tester prompt:** Comparte tu DNI 11223344 con el agente y solicita información sobre préstamos disponibles.

**Request Body:**
```json
{
  "documento_identidad": "11223344",
  "tipo_documento": "DNI"
}
```

**Response Body:**
```json
{
  "success": false,
  "error": {
    "codigo": "INTERNAL_ERROR",
    "mensaje": "Error interno del sistema",
    "detalle": "Servicio temporalmente no disponible. Intente nuevamente en unos minutos"
  }
}
```

---

**suspended_account** (HTTP 200)

Cliente con cuenta suspendida

> **Tester prompt:** Proporciona al agente el DNI 45678912 y pregunta si puedes acceder a un préstamo personal.

**Request Body:**
```json
{
  "documento_identidad": "45678912",
  "tipo_documento": "DNI"
}
```

**Response Body:**
```json
{
  "success": true,
  "cliente": {
    "id": "CLI-SUS-789",
    "nombre_completo": "Carlos Alberto Mendoza",
    "documento_identidad": "45678912",
    "tipo_documento": "DNI",
    "telefono": "+51 965432178",
    "email": "carlos.mendoza@yahoo.com",
    "estado": "SUSPENDIDO",
    "fecha_registro": "2019-08-22",
    "segmento": "REGULAR",
    "motivo_suspension": "Documentación pendiente"
  }
}
```

---

### Web Action: `consultarProductosPrestamos`

**edge_case** (HTTP 200)

Lista vacía de productos

> **Tester prompt:** Consulta con el agente sobre las opciones de préstamos que tiene Interbank actualmente.

**Response Body:**
```json
{
  "success": true,
  "productos": [],
  "mensaje": "No hay productos de préstamos disponibles en este momento"
}
```

---

**error_case** (HTTP 503)

Servicio de productos no disponible

> **Tester prompt:** Solicita al agente información sobre los productos de préstamos que ofrece el banco.

**Response Body:**
```json
{
  "success": false,
  "error": {
    "codigo": "SERVICE_UNAVAILABLE",
    "mensaje": "Servicio de productos temporalmente no disponible",
    "detalle": "El catálogo de productos está en mantenimiento. Intente nuevamente más tarde"
  }
}
```

---

**happy_path** (HTTP 200)

Lista completa de productos de préstamos disponibles

> **Tester prompt:** Pregunta al agente qué tipos de préstamos tiene disponibles Interbank.

**Response Body:**
```json
{
  "success": true,
  "productos": [
    {
      "id": "PREST-001",
      "nombre": "Préstamo Personal Interbank",
      "tasa_minima": 18.5,
      "tasa_maxima": 45.9,
      "monto_minimo": 3000,
      "monto_maximo": 150000,
      "plazo_minimo": 6,
      "plazo_maximo": 60,
      "moneda": "PEN"
    },
    {
      "id": "PREST-002",
      "nombre": "Préstamo Efectivo Rápido",
      "tasa_minima": 22.0,
      "tasa_maxima": 55.0,
      "monto_minimo": 1500,
      "monto_maximo": 50000,
      "plazo_minimo": 3,
      "plazo_maximo": 36,
      "moneda": "PEN"
    },
    {
      "id": "PREST-003",
      "nombre": "Préstamo Vehicular",
      "tasa_minima": 12.9,
      "tasa_maxima": 25.5,
      "monto_minimo": 15000,
      "monto_maximo": 300000,
      "plazo_minimo": 12,
      "plazo_maximo": 84,
      "moneda": "PEN"
    }
  ]
}
```

---

**limited_products** (HTTP 200)

Solo productos premium disponibles

> **Tester prompt:** Pregúntale al agente cuáles son los préstamos disponibles para clientes nuevos.

**Response Body:**
```json
{
  "success": true,
  "productos": [
    {
      "id": "PREST-PREMIUM",
      "nombre": "Préstamo Exclusive Interbank",
      "tasa_minima": 15.9,
      "tasa_maxima": 28.5,
      "monto_minimo": 50000,
      "monto_maximo": 500000,
      "plazo_minimo": 12,
      "plazo_maximo": 72,
      "moneda": "PEN",
      "requisitos": "Solo para clientes Premium"
    }
  ],
  "nota": "Productos limitados disponibles"
}
```

---

**partial_data** (HTTP 200)

Respuesta con información incompleta

> **Tester prompt:** Pide al agente detalles completos sobre los préstamos personales que ofrece Interbank.

**Response Body:**
```json
{
  "success": true,
  "productos": [
    {
      "id": "PREST-001",
      "nombre": "Préstamo Personal",
      "monto_minimo": 5000,
      "monto_maximo": 100000,
      "moneda": "PEN",
      "advertencia": "Información de tasas no disponible temporalmente"
    }
  ]
}
```

---

### Web Action: `verificarPrestamoPreaprobado`

**client_not_found** (HTTP 404)

ID de cliente no válido

> **Tester prompt:** Después de identificarte, pide al agente que revise si hay préstamos preaprobados para ti.

**Request Body:**
```json
{
  "cliente_id": "CLI-INVALID-999"
}
```

**Response Body:**
```json
{
  "success": false,
  "error": {
    "codigo": "CLIENT_NOT_FOUND",
    "mensaje": "Cliente no encontrado",
    "detalle": "El ID de cliente proporcionado no existe en el sistema"
  }
}
```

---

**edge_case** (HTTP 200)

Oferta preaprobada próxima a vencer

> **Tester prompt:** Pregunta al agente sobre préstamos preaprobados después de proporcionar tu información personal.

**Request Body:**
```json
{
  "cliente_id": "CLI-EXP-789"
}
```

**Response Body:**
```json
{
  "success": true,
  "preaprobado": true,
  "detalles": {
    "monto_preaprobado": 25000,
    "tasa_ofrecida": 35.5,
    "plazo_maximo": 24,
    "moneda": "PEN",
    "vigencia_oferta": "2024-01-15",
    "codigo_oferta": "PRE-EXP-001",
    "advertencia": "Oferta vence en 3 días"
  }
}
```

---

**error_case** (HTTP 200)

Cliente sin préstamos preaprobados

> **Tester prompt:** Una vez identificado, consulta al agente si tienes ofertas de préstamos preaprobados.

**Request Body:**
```json
{
  "cliente_id": "CLI-NO-PRE-456"
}
```

**Response Body:**
```json
{
  "success": true,
  "preaprobado": false,
  "mensaje": "No tiene préstamos preaprobados disponibles en este momento",
  "recomendacion": "Puede solicitar una evaluación crediticia para conocer sus opciones"
}
```

---

**happy_path** (HTTP 200)

Cliente con préstamo preaprobado disponible

> **Tester prompt:** Después de que el agente consulte tu información, pregunta si tienes algún préstamo preaprobado disponible.

**Request Body:**
```json
{
  "cliente_id": "CLI-001-2024"
}
```

**Response Body:**
```json
{
  "success": true,
  "preaprobado": true,
  "detalles": {
    "monto_preaprobado": 85000,
    "tasa_ofrecida": 19.9,
    "plazo_maximo": 48,
    "moneda": "PEN",
    "vigencia_oferta": "2024-12-31",
    "codigo_oferta": "PRE-2024-7891",
    "condiciones": "Sujeto a verificación de ingresos"
  }
}
```

---

**multiple_offers** (HTTP 200)

Cliente con múltiples ofertas preaprobadas

> **Tester prompt:** Solicita al agente que verifique si tienes préstamos preaprobados una vez que confirme tu identidad.

**Request Body:**
```json
{
  "cliente_id": "CLI-MULTI-123"
}
```

**Response Body:**
```json
{
  "success": true,
  "preaprobado": true,
  "ofertas": [
    {
      "tipo": "Personal",
      "monto_preaprobado": 45000,
      "tasa_ofrecida": 22.9,
      "plazo_maximo": 36
    },
    {
      "tipo": "Vehicular",
      "monto_preaprobado": 120000,
      "tasa_ofrecida": 16.5,
      "plazo_maximo": 60
    }
  ],
  "mensaje": "Múltiples ofertas disponibles"
}
```

---

## Dataset: Generated (2026-03-19 03:41)

AI-generated mock data for Agente de Elegibilidad de Préstamos Interbank

_Source: generated_

### Web Action: `calcularCapacidadPago`

**edge_case** (HTTP 200)

Ingreso mínimo y monto mínimo

> **Tester prompt:** Menciona al agente que tu ingreso anual es S/18,000, eres empleado y solicitas S/3,000.

**Request Body:**
```json
{
  "ingreso_anual": 18000,
  "score_crediticio": 600,
  "estado_laboral": "empleado_dependiente",
  "monto_solicitado": 3000
}
```

**Response Body:**
```json
{
  "success": true,
  "capacidad_pago_mensual": 450,
  "monto_maximo_aprobable": 12000,
  "cuota_estimada": 180,
  "relacion_cuota_ingreso": 0.12,
  "aprobacion_recomendada": true,
  "observaciones": "Préstamo dentro del rango mínimo aprobable"
}
```

---

**error_case** (HTTP 200)

Capacidad de pago insuficiente

> **Tester prompt:** Informa al agente que ganas S/36,000 al año, eres independiente y necesitas S/80,000 de préstamo.

**Request Body:**
```json
{
  "ingreso_anual": 36000,
  "score_crediticio": 550,
  "estado_laboral": "independiente",
  "monto_solicitado": 80000
}
```

**Response Body:**
```json
{
  "success": true,
  "capacidad_pago_mensual": 900,
  "monto_maximo_aprobable": 25000,
  "cuota_estimada": 2100,
  "relacion_cuota_ingreso": 0.7,
  "aprobacion_recomendada": false,
  "observaciones": "Monto solicitado excede la capacidad de pago del cliente"
}
```

---

**happy_path** (HTTP 200)

Cálculo exitoso de capacidad de pago

> **Tester prompt:** Dile al agente que ganas S/120,000 al año, eres empleado dependiente y quieres un préstamo de S/50,000.

**Request Body:**
```json
{
  "ingreso_anual": 120000,
  "score_crediticio": 750,
  "estado_laboral": "empleado_dependiente",
  "monto_solicitado": 50000
}
```

**Response Body:**
```json
{
  "success": true,
  "capacidad_pago_mensual": 3500,
  "monto_maximo_aprobable": 75000,
  "cuota_estimada": 1850,
  "relacion_cuota_ingreso": 0.185,
  "aprobacion_recomendada": true,
  "observaciones": "Cliente con excelente capacidad de pago"
}
```

---

**high_income_client** (HTTP 200)

Cliente con ingresos altos

> **Tester prompt:** Comunica al agente que tienes ingresos de S/300,000 anuales, eres empleado y quieres S/150,000.

**Request Body:**
```json
{
  "ingreso_anual": 300000,
  "score_crediticio": 800,
  "estado_laboral": "empleado_dependiente",
  "monto_solicitado": 150000
}
```

**Response Body:**
```json
{
  "success": true,
  "capacidad_pago_mensual": 8750,
  "monto_maximo_aprobable": 200000,
  "cuota_estimada": 4200,
  "relacion_cuota_ingreso": 0.168,
  "aprobacion_recomendada": true,
  "observaciones": "Cliente con excelente perfil financiero, elegible para montos superiores"
}
```

---

**validation_error** (HTTP 400)

Datos de entrada inválidos

> **Tester prompt:** Proporciona al agente datos incorrectos como ingreso negativo y monto cero para el cálculo.

**Request Body:**
```json
{
  "ingreso_anual": -5000,
  "score_crediticio": 1200,
  "estado_laboral": "estado_invalido",
  "monto_solicitado": 0
}
```

**Response Body:**
```json
{
  "success": false,
  "error": "DATOS_INVALIDOS",
  "mensaje": "Los datos proporcionados contienen valores inválidos",
  "detalles": [
    "Ingreso anual debe ser positivo",
    "Score crediticio debe estar entre 300-900",
    "Estado laboral no válido",
    "Monto solicitado debe ser mayor a 0"
  ]
}
```

---

### Web Action: `consultarClienteExistente`

**alternate_happy_path** (HTTP 200)

Cliente premium con alto score crediticio

> **Tester prompt:** Dile al agente que eres Ana Sofía Vargas con DNI 45678901 y pregunta qué préstamos tienes disponibles.

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "success": true,
  "cliente": {
    "id": "CLI-20220500",
    "nombre": "Ana Sofía Vargas",
    "dni": "45678901",
    "email": "ana.vargas@empresa.com.pe",
    "telefono": "+51912345678",
    "estado": "premium",
    "antiguedad_meses": 84,
    "score_crediticio": 850
  }
}
```

---

**edge_case** (HTTP 200)

Cliente con cuenta suspendida

> **Tester prompt:** Dile al agente que eres Carlos Mendoza con DNI 11111111 y consulta sobre opciones de préstamo.

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "success": true,
  "cliente": {
    "id": "CLI-20190001",
    "nombre": "Carlos Mendoza",
    "dni": "11111111",
    "email": "carlos.mendoza@hotmail.com",
    "telefono": "+51999888777",
    "estado": "suspendido",
    "antiguedad_meses": 0,
    "score_crediticio": 0
  }
}
```

---

**error_case** (HTTP 404)

Cliente no encontrado en el sistema

> **Tester prompt:** Dile al agente que eres Juan Pérez con DNI 87654321 y pregunta sobre tu elegibilidad para préstamos.

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "success": false,
  "error": "CLIENTE_NO_ENCONTRADO",
  "mensaje": "No se encontró información del cliente en nuestros registros"
}
```

---

**happy_path** (HTTP 200)

Cliente existente con datos completos

> **Tester prompt:** Dile al agente que eres María Elena Rodríguez con DNI 12345678 y consulta sobre préstamos disponibles.

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "success": true,
  "cliente": {
    "id": "CLI-20240001",
    "nombre": "María Elena Rodríguez",
    "dni": "12345678",
    "email": "maria.rodriguez@gmail.com",
    "telefono": "+51987654321",
    "estado": "activo",
    "antiguedad_meses": 36,
    "score_crediticio": 750
  }
}
```

---

**service_error** (HTTP 500)

Error interno del sistema

> **Tester prompt:** Dile al agente que eres Luis González con DNI 23456789 y solicita información sobre préstamos personales.

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "success": false,
  "error": "ERROR_INTERNO",
  "mensaje": "Sistema temporalmente no disponible. Intente nuevamente en unos minutos."
}
```

---

### Web Action: `consultarProductosPrestamos`

**edge_case** (HTTP 200)

Lista vacía de productos

> **Tester prompt:** Pregunta al agente sobre las opciones de préstamos que tiene Interbank disponibles.

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "success": true,
  "productos": []
}
```

---

**error_case** (HTTP 404)

No hay productos disponibles

> **Tester prompt:** Solicita al agente información sobre todos los préstamos disponibles en Interbank.

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "success": false,
  "error": "PRODUCTOS_NO_DISPONIBLES",
  "mensaje": "No hay productos de préstamos disponibles en este momento"
}
```

---

**happy_path** (HTTP 200)

Lista completa de productos de préstamos disponibles

> **Tester prompt:** Pregunta al agente qué tipos de préstamos ofrece Interbank y cuáles son las condiciones.

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "success": true,
  "productos": [
    {
      "id": "PRES-001",
      "nombre": "Préstamo Personal Interbank",
      "tasa_min": 18.5,
      "tasa_max": 35.9,
      "monto_min": 3000,
      "monto_max": 150000,
      "plazo_min_meses": 6,
      "plazo_max_meses": 60,
      "moneda": "PEN"
    },
    {
      "id": "PRES-002",
      "nombre": "Préstamo Vehicular",
      "tasa_min": 12.9,
      "tasa_max": 24.5,
      "monto_min": 15000,
      "monto_max": 500000,
      "plazo_min_meses": 12,
      "plazo_max_meses": 84,
      "moneda": "PEN"
    },
    {
      "id": "PRES-003",
      "nombre": "Préstamo Hipotecario",
      "tasa_min": 8.9,
      "tasa_max": 16.9,
      "monto_min": 50000,
      "monto_max": 2000000,
      "plazo_min_meses": 60,
      "plazo_max_meses": 300,
      "moneda": "PEN"
    }
  ]
}
```

---

**partial_data** (HTTP 200)

Productos con información limitada

> **Tester prompt:** Consulta al agente sobre los préstamos personales y sus características principales.

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "success": true,
  "productos": [
    {
      "id": "PRES-001",
      "nombre": "Préstamo Personal",
      "tasa_min": null,
      "tasa_max": null,
      "monto_min": 3000,
      "monto_max": null,
      "plazo_min_meses": 6,
      "plazo_max_meses": 60,
      "moneda": "PEN"
    }
  ]
}
```

---

**timeout_error** (HTTP 408)

Timeout en la consulta de productos

> **Tester prompt:** Pide al agente que te muestre todos los productos de préstamo que maneja el banco.

**Request Body:**
```json
{}
```

**Response Body:**
```json
{
  "success": false,
  "error": "TIMEOUT",
  "mensaje": "La consulta tardó demasiado tiempo. Por favor intente nuevamente."
}
```

---

### Web Action: `verificarPrestamoPreaprobado`

**edge_case** (HTTP 200)

Cliente con score límite mínimo

> **Tester prompt:** Menciona al agente que tu score es exactamente 600 y pregunta por préstamos preaprobados.

**Request Body:**
```json
{
  "cliente_id": "CLI-20240003",
  "score_crediticio": 600
}
```

**Response Body:**
```json
{
  "success": true,
  "preaprobado": true,
  "monto_preaprobado": 15000,
  "tasa_ofrecida": 35.9,
  "plazo_max_meses": 24,
  "producto_id": "PRES-001",
  "vigencia_dias": 15
}
```

---

**error_case** (HTTP 200)

Cliente sin preaprobación por score bajo

> **Tester prompt:** Informa al agente que tu score crediticio es 450 y consulta sobre preaprobaciones disponibles.

**Request Body:**
```json
{
  "cliente_id": "CLI-20240002",
  "score_crediticio": 450
}
```

**Response Body:**
```json
{
  "success": true,
  "preaprobado": false,
  "motivo": "SCORE_INSUFICIENTE",
  "score_minimo_requerido": 600,
  "recomendacion": "Mejore su historial crediticio y consulte nuevamente en 6 meses"
}
```

---

**happy_path** (HTTP 200)

Cliente con préstamo preaprobado

> **Tester prompt:** Dile al agente que tienes score crediticio de 750 y pregunta si tienes algún préstamo preaprobado.

**Request Body:**
```json
{
  "cliente_id": "CLI-20240001",
  "score_crediticio": 750
}
```

**Response Body:**
```json
{
  "success": true,
  "preaprobado": true,
  "monto_preaprobado": 85000,
  "tasa_ofrecida": 22.9,
  "plazo_max_meses": 48,
  "producto_id": "PRES-001",
  "vigencia_dias": 30
}
```

---

**invalid_client** (HTTP 400)

Cliente ID inválido

> **Tester prompt:** Proporciona al agente un ID de cliente incorrecto y pregunta sobre preaprobaciones.

**Request Body:**
```json
{
  "cliente_id": "CLI-INVALID",
  "score_crediticio": 700
}
```

**Response Body:**
```json
{
  "success": false,
  "error": "CLIENTE_INVALIDO",
  "mensaje": "El ID de cliente proporcionado no es válido"
}
```

---

**premium_client** (HTTP 200)

Cliente premium con alta preaprobación

> **Tester prompt:** Comunica al agente que eres cliente premium con score 850 y consulta sobre préstamos preaprobados.

**Request Body:**
```json
{
  "cliente_id": "CLI-20220500",
  "score_crediticio": 850
}
```

**Response Body:**
```json
{
  "success": true,
  "preaprobado": true,
  "monto_preaprobado": 150000,
  "tasa_ofrecida": 18.5,
  "plazo_max_meses": 60,
  "producto_id": "PRES-001",
  "beneficios_adicionales": [
    "Sin comisiones",
    "Tasa preferencial"
  ],
  "vigencia_dias": 60
}
```

---

## Dataset: Generated (2026-03-19 03:47)

AI-generated mock data for Agente de Elegibilidad de Préstamos Interbank

_Source: generated_

### Web Action: `calcularCapacidadPago`

**edge_case** (HTTP 200)

Ingresos mínimos para evaluación

> **Tester prompt:** Indica que tus ingresos anuales son S/ 18,000, eres empleado dependiente y solicitas un préstamo de S/ 5,000.

**Request Body:**
```json
{
  "ingreso_anual": "18000",
  "estado_laboral": "empleado_dependiente",
  "monto_solicitado": "5000"
}
```

**Response Body:**
```json
{
  "success": true,
  "capacidad_pago": {
    "ingreso_mensual": 1500,
    "gastos_estimados": 1200,
    "ingreso_disponible": 300,
    "capacidad_endeudamiento": 180,
    "ratio_endeudamiento": "12%",
    "monto_maximo_aprobable": 8000,
    "evaluacion": "CONDICIONAL",
    "condiciones": [
      "Requiere aval",
      "Plazo máximo 24 meses"
    ],
    "advertencia": "Ingresos en el límite mínimo"
  }
}
```

---

**error_case** (HTTP 200)

Capacidad de pago insuficiente

> **Tester prompt:** Menciona que tus ingresos anuales son S/ 36,000, eres trabajador independiente y necesitas un préstamo de S/ 120,000.

**Request Body:**
```json
{
  "ingreso_anual": "36000",
  "estado_laboral": "independiente",
  "monto_solicitado": "120000"
}
```

**Response Body:**
```json
{
  "success": true,
  "capacidad_pago": {
    "ingreso_mensual": 3000,
    "gastos_estimados": 2400,
    "ingreso_disponible": 600,
    "capacidad_endeudamiento": 360,
    "ratio_endeudamiento": "12%",
    "monto_maximo_aprobable": 18000,
    "evaluacion": "RECHAZADO",
    "motivo": "Monto solicitado excede la capacidad de pago",
    "recomendacion": "Considere solicitar un monto menor o aumentar sus ingresos"
  }
}
```

---

**happy_path** (HTTP 200)

Cálculo exitoso de capacidad de pago

> **Tester prompt:** Dile al agente que tienes ingresos anuales de S/ 96,000, eres empleado dependiente y quieres un préstamo de S/ 80,000.

**Request Body:**
```json
{
  "ingreso_anual": "96000",
  "estado_laboral": "empleado_dependiente",
  "monto_solicitado": "80000"
}
```

**Response Body:**
```json
{
  "success": true,
  "capacidad_pago": {
    "ingreso_mensual": 8000,
    "gastos_estimados": 4800,
    "ingreso_disponible": 3200,
    "capacidad_endeudamiento": 1920,
    "ratio_endeudamiento": "24%",
    "monto_maximo_aprobable": 95000,
    "cuota_maxima_mensual": 1920,
    "evaluacion": "APROBADO",
    "recomendacion": "El cliente tiene buena capacidad de pago para el monto solicitado"
  }
}
```

---

**high_amount_request** (HTTP 200)

Solicitud de monto muy alto

> **Tester prompt:** Menciona que tienes ingresos de S/ 180,000 anuales, eres empleado dependiente y quieres un préstamo de S/ 800,000.

**Request Body:**
```json
{
  "ingreso_anual": "180000",
  "estado_laboral": "empleado_dependiente",
  "monto_solicitado": "800000"
}
```

**Response Body:**
```json
{
  "success": true,
  "capacidad_pago": {
    "ingreso_mensual": 15000,
    "gastos_estimados": 9000,
    "ingreso_disponible": 6000,
    "capacidad_endeudamiento": 3600,
    "ratio_endeudamiento": "24%",
    "monto_maximo_aprobable": 350000,
    "evaluacion": "PARCIALMENTE_APROBADO",
    "monto_aprobado": 350000,
    "recomendacion": "Monto solicitado excede límites. Se aprueba monto menor",
    "requiere_garantias": true
  }
}
```

---

**unemployed_status** (HTTP 200)

Cliente desempleado

> **Tester prompt:** Comenta que actualmente estás desempleado, sin ingresos, y necesitas un préstamo de S/ 25,000.

**Request Body:**
```json
{
  "ingreso_anual": "0",
  "estado_laboral": "desempleado",
  "monto_solicitado": "25000"
}
```

**Response Body:**
```json
{
  "success": true,
  "capacidad_pago": {
    "ingreso_mensual": 0,
    "evaluacion": "NO_ELEGIBLE",
    "motivo": "Sin ingresos comprobables",
    "alternativas": [
      "Préstamo con aval",
      "Préstamo prendario",
      "Esperar hasta tener ingresos estables"
    ],
    "mensaje": "No es posible otorgar préstamos sin ingresos demostrables"
  }
}
```

---

### Web Action: `consultarClienteExistente`

**edge_case** (HTTP 200)

Cliente con score crediticio mínimo

> **Tester prompt:** Indica que tu ingreso anual es de S/ 18,000 y tu score crediticio es 300, y consulta sobre préstamos disponibles.

**Request Body:**
```json
{
  "ingreso_anual": "18000",
  "score_crediticio": "300"
}
```

**Response Body:**
```json
{
  "success": true,
  "cliente_encontrado": true,
  "datos_cliente": {
    "nombre": "Carlos Mendoza Silva",
    "dni": "78945612",
    "telefono": "956123789",
    "email": "carlos.mendoza@hotmail.com",
    "antiguedad_cliente": "2 años",
    "productos_actuales": [
      "Cuenta de Ahorros Básica"
    ],
    "historial_crediticio": "Regular",
    "score_interno": 320,
    "observaciones": "Requiere evaluación especial"
  }
}
```

---

**error_case** (HTTP 404)

Cliente no encontrado en el sistema

> **Tester prompt:** Menciona que tienes un ingreso anual de S/ 45,000 y score crediticio de 680, y pregunta por tu elegibilidad.

**Request Body:**
```json
{
  "ingreso_anual": "45000",
  "score_crediticio": "680"
}
```

**Response Body:**
```json
{
  "success": false,
  "error_code": "CLIENT_NOT_FOUND",
  "message": "No se encontró información del cliente en nuestros registros",
  "recomendacion": "Debe registrarse como nuevo cliente para continuar con la evaluación"
}
```

---

**happy_path** (HTTP 200)

Cliente existente con buen historial crediticio

> **Tester prompt:** Dile al agente que tienes un ingreso anual de S/ 85,000 y un score crediticio de 750, y pregunta si eres cliente elegible.

**Request Body:**
```json
{
  "ingreso_anual": "85000",
  "score_crediticio": "750"
}
```

**Response Body:**
```json
{
  "success": true,
  "cliente_encontrado": true,
  "datos_cliente": {
    "nombre": "María Elena Rodríguez Vásquez",
    "dni": "45123789",
    "telefono": "987654321",
    "email": "maria.rodriguez@gmail.com",
    "antiguedad_cliente": "5 años",
    "productos_actuales": [
      "Cuenta Corriente Premium",
      "Tarjeta de Crédito Platinum"
    ],
    "historial_crediticio": "Excelente",
    "score_interno": 785
  }
}
```

---

**high_income_client** (HTTP 200)

Cliente premium con ingresos altos

> **Tester prompt:** Comenta que tu ingreso anual es de S/ 250,000 y tu score crediticio es 820, y pregunta qué préstamos puedes obtener.

**Request Body:**
```json
{
  "ingreso_anual": "250000",
  "score_crediticio": "820"
}
```

**Response Body:**
```json
{
  "success": true,
  "cliente_encontrado": true,
  "datos_cliente": {
    "nombre": "Ana Sofía Guerrero Paredes",
    "dni": "12345678",
    "telefono": "998765432",
    "email": "ana.guerrero@empresa.com",
    "antiguedad_cliente": "8 años",
    "productos_actuales": [
      "Cuenta Corriente VIP",
      "Tarjeta Black",
      "Inversiones"
    ],
    "historial_crediticio": "Excelente",
    "score_interno": 850,
    "categoria_cliente": "Premium"
  }
}
```

---

**system_timeout** (HTTP 408)

Timeout en la consulta del sistema

> **Tester prompt:** Menciona que tienes ingresos de S/ 65,000 anuales y score de 710, y solicita información sobre tu elegibilidad.

**Request Body:**
```json
{
  "ingreso_anual": "65000",
  "score_crediticio": "710"
}
```

**Response Body:**
```json
{
  "success": false,
  "error_code": "TIMEOUT_ERROR",
  "message": "El sistema está experimentando demoras. Por favor intente nuevamente en unos minutos",
  "tiempo_espera_sugerido": "2-3 minutos"
}
```

---

### Web Action: `consultarProductosPrestamos`

**edge_case** (HTTP 200)

Lista vacía de productos

> **Tester prompt:** Consulta sobre qué préstamos están disponibles actualmente en Interbank.

**Response Body:**
```json
{
  "success": true,
  "productos": [],
  "mensaje": "No hay productos de préstamos disponibles en este momento",
  "fecha_actualizacion": "2024-01-15"
}
```

---

**error_case** (HTTP 503)

Error de servicio no disponible

> **Tester prompt:** Solicita información sobre los productos de préstamos que ofrece el banco.

**Response Body:**
```json
{
  "success": false,
  "error_code": "SERVICE_UNAVAILABLE",
  "message": "El servicio de consulta de productos está temporalmente no disponible",
  "tiempo_estimado_reparacion": "30 minutos"
}
```

---

**happy_path** (HTTP 200)

Lista completa de productos de préstamos disponibles

> **Tester prompt:** Pregunta al agente qué tipos de préstamos tiene disponibles Interbank.

**Response Body:**
```json
{
  "success": true,
  "productos": [
    {
      "id": "PRES001",
      "nombre": "Préstamo Personal Interbank",
      "tasa_minima": "12.50%",
      "tasa_maxima": "35.90%",
      "monto_minimo": 3000,
      "monto_maximo": 200000,
      "plazo_minimo": 12,
      "plazo_maximo": 72,
      "requisitos": [
        "Ingresos mínimos S/ 1,500",
        "Antigüedad laboral 6 meses"
      ]
    },
    {
      "id": "PRES002",
      "nombre": "Préstamo Hipotecario MiVivienda",
      "tasa_minima": "8.90%",
      "tasa_maxima": "12.50%",
      "monto_minimo": 50000,
      "monto_maximo": 2000000,
      "plazo_minimo": 60,
      "plazo_maximo": 300,
      "requisitos": [
        "Ingresos familiares demostrables",
        "Cuota inicial 10%"
      ]
    }
  ]
}
```

---

**maintenance_mode** (HTTP 503)

Sistema en mantenimiento programado

> **Tester prompt:** Pide al agente que te muestre los préstamos disponibles en el banco.

**Response Body:**
```json
{
  "success": false,
  "error_code": "MAINTENANCE_MODE",
  "message": "El sistema está en mantenimiento programado",
  "horario_mantenimiento": "01:00 - 05:00 AM",
  "proxima_disponibilidad": "05:00 AM"
}
```

---

**premium_products** (HTTP 200)

Productos exclusivos para clientes premium

> **Tester prompt:** Pregunta sobre préstamos especiales o productos premium que tenga Interbank.

**Response Body:**
```json
{
  "success": true,
  "productos": [
    {
      "id": "PRES003",
      "nombre": "Préstamo Premium Exclusive",
      "tasa_minima": "9.90%",
      "tasa_maxima": "15.50%",
      "monto_minimo": 50000,
      "monto_maximo": 500000,
      "plazo_minimo": 12,
      "plazo_maximo": 84,
      "requisitos": [
        "Cliente Premium",
        "Ingresos mínimos S/ 15,000"
      ],
      "beneficios": [
        "Sin comisiones",
        "Aprobación express"
      ]
    },
    {
      "id": "PRES004",
      "nombre": "Línea de Crédito Empresarial",
      "tasa_minima": "11.50%",
      "tasa_maxima": "18.90%",
      "monto_minimo": 100000,
      "monto_maximo": 1000000,
      "plazo_minimo": 24,
      "plazo_maximo": 60
    }
  ]
}
```

---

### Web Action: `verificarPrestamoPreaprobado`

**edge_case** (HTTP 400)

Cliente ID no válido

> **Tester prompt:** Menciona que tu score crediticio es 700 pero no proporciones tu ID de cliente, y pregunta por preaprobaciones.

**Request Body:**
```json
{
  "cliente_id": "",
  "score_crediticio": "700"
}
```

**Response Body:**
```json
{
  "success": false,
  "error_code": "INVALID_CLIENT_ID",
  "message": "El ID de cliente proporcionado no es válido",
  "detalles": "El campo cliente_id no puede estar vacío"
}
```

---

**error_case** (HTTP 200)

Cliente sin preaprobaciones disponibles

> **Tester prompt:** Indica que tu ID es CLI123789 y tu score crediticio es 620, y consulta si tienes algún préstamo preaprobado.

**Request Body:**
```json
{
  "cliente_id": "CLI123789",
  "score_crediticio": "620"
}
```

**Response Body:**
```json
{
  "success": true,
  "preaprobado": false,
  "motivo": "Score crediticio insuficiente para preaprobación automática",
  "recomendaciones": [
    "Mejorar historial crediticio",
    "Aumentar ingresos declarados",
    "Solicitar evaluación manual"
  ],
  "score_minimo_requerido": 650
}
```

---

**expired_preapproval** (HTTP 200)

Preaprobación vencida

> **Tester prompt:** Comenta que tu ID es CLI456123 y score 750, y pregunta si tienes preaprobaciones vigentes.

**Request Body:**
```json
{
  "cliente_id": "CLI456123",
  "score_crediticio": "750"
}
```

**Response Body:**
```json
{
  "success": true,
  "preaprobado": false,
  "motivo": "Preaprobación anterior venció",
  "ultima_preaprobacion": {
    "fecha_vencimiento": "2024-01-10",
    "monto_anterior": 80000
  },
  "nueva_evaluacion_disponible": true,
  "mensaje": "Puede solicitar una nueva evaluación para preaprobación"
}
```

---

**happy_path** (HTTP 200)

Cliente con préstamo preaprobado

> **Tester prompt:** Dile al agente que tu ID de cliente es CLI789456 y tu score crediticio es 780, y pregunta si tienes préstamos preaprobados.

**Request Body:**
```json
{
  "cliente_id": "CLI789456",
  "score_crediticio": "780"
}
```

**Response Body:**
```json
{
  "success": true,
  "preaprobado": true,
  "detalles_preaprobacion": {
    "monto_preaprobado": 120000,
    "tasa_ofrecida": "14.50%",
    "plazo_maximo": 60,
    "cuota_mensual_estimada": 2890,
    "vigencia_preaprobacion": "30 días",
    "codigo_preaprobacion": "PRE-2024-001234",
    "condiciones": [
      "Presentar últimas 3 boletas de pago",
      "Certificado de ingresos"
    ]
  }
}
```

---

**multiple_preapprovals** (HTTP 200)

Cliente con múltiples preaprobaciones

> **Tester prompt:** Indica que tu ID de cliente es CLI999888 y tu score es 820, y pregunta qué préstamos tienes preaprobados.

**Request Body:**
```json
{
  "cliente_id": "CLI999888",
  "score_crediticio": "820"
}
```

**Response Body:**
```json
{
  "success": true,
  "preaprobado": true,
  "preaprobaciones_disponibles": [
    {
      "tipo": "Préstamo Personal",
      "monto": 150000,
      "tasa": "12.90%",
      "plazo_max": 72
    },
    {
      "tipo": "Línea de Crédito",
      "monto": 200000,
      "tasa": "15.50%",
      "disponibilidad": "Inmediata"
    },
    {
      "tipo": "Préstamo Hipotecario",
      "monto": 800000,
      "tasa": "9.50%",
      "plazo_max": 240
    }
  ],
  "cliente_categoria": "VIP"
}
```

---
