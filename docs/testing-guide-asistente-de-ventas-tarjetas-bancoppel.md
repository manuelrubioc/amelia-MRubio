# Testing Guide: Asistente de Ventas Tarjetas Bancoppel

Test scenarios with full request/response examples.

## Dataset: Generated (2026-02-25 19:04) **(Active)**

AI-generated mock data for Bancoppel_Cards_Sales_Assistant - Sales Assistant

_Source: generated_

### Web Action: `calcularBeneficios`

**edge_case** (HTTP 200)

Cálculo con gasto mensual mínimo

**Request Body:**
```json
{
  "productId": "BC-BASIC-004",
  "monthlySpending": "500"
}
```

**Request Params:**
```json
[
  {
    "name": "gastoMensualEstimado",
    "value": "500"
  },
  {
    "name": "productoId",
    "value": "BC-BASIC-004"
  }
]
```

**Response Body:**
```json
{
  "ahorroAnualidad": "$0.00",
  "cashbackAnual": "$60.00",
  "puntosRecompensa": 60,
  "detalleCalculos": {
    "gastoAnual": "$6,000.00",
    "cashbackSupermercados": "$30.00",
    "cashbackGeneral": "$30.00",
    "valorPuntos": "$60.00"
  },
  "resumenBeneficios": "Con un gasto mensual bajo de $500, los beneficios son limitados. Considere incrementar el uso de la tarjeta para maximizar recompensas"
}
```

---

**error_case** (HTTP 404)

Error por producto no encontrado

**Request Body:**
```json
{
  "productId": "BC-INVALID-999",
  "monthlySpending": "5000"
}
```

**Request Params:**
```json
[
  {
    "name": "gastoMensualEstimado",
    "value": "5000"
  },
  {
    "name": "productoId",
    "value": "BC-INVALID-999"
  }
]
```

**Response Body:**
```json
{
  "error": true,
  "codigo": "PRODUCT_NOT_FOUND",
  "mensaje": "El producto especificado no existe en nuestro catálogo",
  "ahorroAnualidad": "$0.00",
  "cashbackAnual": "$0.00",
  "puntosRecompensa": 0
}
```

---

**happy_path** (HTTP 200)

Cálculo de beneficios para gasto mensual promedio

**Request Body:**
```json
{
  "productId": "BC-GOLD-001",
  "monthlySpending": "8500"
}
```

**Request Params:**
```json
[
  {
    "name": "gastoMensualEstimado",
    "value": "8500"
  },
  {
    "name": "productoId",
    "value": "BC-GOLD-001"
  }
]
```

**Response Body:**
```json
{
  "ahorroAnualidad": "$890.00",
  "cashbackAnual": "$2,040.00",
  "puntosRecompensa": 10200,
  "detalleCalculos": {
    "gastoAnual": "$102,000.00",
    "cashbackSupermercados": "$1,530.00",
    "cashbackGeneral": "$510.00",
    "valorPuntos": "$1,020.00"
  },
  "resumenBeneficios": "Con un gasto mensual de $8,500, obtendrá $2,040 en cashback anual y 10,200 puntos recompensa"
}
```

---

### Web Action: `consultarProductosTarjetas`

**edge_case** (HTTP 200)

Consulta con ingresos muy altos que exceden productos disponibles

**Request Body:**
```json
{
  "cardType": "empresarial",
  "incomeRange": "200000+"
}
```

**Request Params:**
```json
[
  {
    "name": "rangoIngresos",
    "value": "200000+"
  },
  {
    "name": "tipoTarjeta",
    "value": "empresarial"
  }
]
```

**Response Body:**
```json
{
  "productos": [],
  "recomendacion": "Para su perfil de ingresos superiores a $200,000 pesos mensuales, le sugerimos contactar directamente con nuestro equipo de Banca Privada para productos personalizados."
}
```

---

**error_case** (HTTP 400)

Error por rango de ingresos no válido

**Request Body:**
```json
{
  "cardType": "debito",
  "incomeRange": "5000-8000"
}
```

**Request Params:**
```json
[
  {
    "name": "rangoIngresos",
    "value": "5000-8000"
  },
  {
    "name": "tipoTarjeta",
    "value": "debito"
  }
]
```

**Response Body:**
```json
{
  "error": true,
  "codigo": "INCOME_TOO_LOW",
  "mensaje": "El rango de ingresos proporcionado no cumple con los requisitos mínimos para productos de crédito. Ingreso mínimo requerido: $10,000.00",
  "productos": [],
  "recomendacion": ""
}
```

---

**happy_path** (HTTP 200)

Consulta exitosa de productos de tarjetas para ingresos medios

**Request Body:**
```json
{
  "cardType": "credito",
  "incomeRange": "15000-30000"
}
```

**Request Params:**
```json
[
  {
    "name": "rangoIngresos",
    "value": "15000-30000"
  },
  {
    "name": "tipoTarjeta",
    "value": "credito"
  }
]
```

**Response Body:**
```json
{
  "productos": [
    {
      "id": "BC-GOLD-001",
      "nombre": "Bancoppel Oro",
      "tipoTarjeta": "Crédito",
      "anualidad": "$890.00",
      "tasaInteres": "42.9%",
      "limiteCredito": "$45,000.00",
      "beneficios": [
        "2% cashback en supermercados",
        "Sin anualidad primer año",
        "Meses sin intereses en tiendas afiliadas"
      ]
    },
    {
      "id": "BC-PLAT-002",
      "nombre": "Bancoppel Platino",
      "tipoTarjeta": "Crédito",
      "anualidad": "$1,490.00",
      "tasaInteres": "39.9%",
      "limiteCredito": "$80,000.00",
      "beneficios": [
        "3% cashback en gasolineras",
        "Seguro de viaje",
        "Acceso a salas VIP"
      ]
    }
  ],
  "recomendacion": "Para su perfil de ingresos, recomendamos la tarjeta Bancoppel Oro que ofrece excelentes beneficios sin comprometer su capacidad de pago."
}
```

---

### Web Action: `evaluarPerfilCrediticio`

**edge_case** (HTTP 200)

Cliente sin historial crediticio previo

**Request Body:**
```json
{
  "rfc": "LOPA001205ABC",
  "curp": "LOPA001205HDFPRL09",
  "monthlyIncome": "12000"
}
```

**Request Params:**
```json
[
  {
    "name": "curp",
    "value": "LOPA001205HDFPRL09"
  },
  {
    "name": "ingresosMensuales",
    "value": "12000"
  },
  {
    "name": "rfc",
    "value": "LOPA001205ABC"
  }
]
```

**Response Body:**
```json
{
  "limiteAprobado": "$15,000.00",
  "productosElegibles": [
    "BC-BASIC-004"
  ],
  "scoreCredito": 0,
  "evaluacion": "APROBADO_CONDICIONAL",
  "observaciones": "Cliente sin historial crediticio. Límite inicial conservador con posibilidad de incremento tras 6 meses de buen comportamiento"
}
```

---

**error_case** (HTTP 200)

Rechazo por mal historial crediticio

**Request Body:**
```json
{
  "rfc": "PEJI790820MDF",
  "curp": "PEJI790820MDFRZN08",
  "monthlyIncome": "18000"
}
```

**Request Params:**
```json
[
  {
    "name": "curp",
    "value": "PEJI790820MDFRZN08"
  },
  {
    "name": "ingresosMensuales",
    "value": "18000"
  },
  {
    "name": "rfc",
    "value": "PEJI790820MDF"
  }
]
```

**Response Body:**
```json
{
  "limiteAprobado": "$0.00",
  "productosElegibles": [],
  "scoreCredito": 485,
  "evaluacion": "RECHAZADO",
  "observaciones": "Cliente presenta reportes negativos en Buró de Crédito. Se requiere mejorar historial crediticio antes de solicitar productos"
}
```

---

**happy_path** (HTTP 200)

Evaluación exitosa de perfil crediticio con buen score

**Request Body:**
```json
{
  "rfc": "GOME850315HDF",
  "curp": "GOME850315HDFMRN04",
  "monthlyIncome": "25000"
}
```

**Request Params:**
```json
[
  {
    "name": "curp",
    "value": "GOME850315HDFMRN04"
  },
  {
    "name": "ingresosMensuales",
    "value": "25000"
  },
  {
    "name": "rfc",
    "value": "GOME850315HDF"
  }
]
```

**Response Body:**
```json
{
  "limiteAprobado": "$65,000.00",
  "productosElegibles": [
    "BC-GOLD-001",
    "BC-PLAT-002",
    "BC-CASH-003"
  ],
  "scoreCredito": 745,
  "evaluacion": "APROBADO",
  "observaciones": "Cliente con historial crediticio favorable y capacidad de pago comprobada"
}
```

---

### Web Action: `registrarLeadVentas`

**edge_case** (HTTP 201)

Registro de lead con calificación muy baja

**Request Body:**
```json
{
  "contactInfo": "Pedro Ramírez - Tel: 5559876543 - Email: pedro.ramirez@email.com",
  "interestedProduct": "BC-BASIC-004",
  "qualificationScore": "D"
}
```

**Request Params:**
```json
[
  {
    "name": "datosContacto",
    "value": "Pedro Ramírez - Tel: 5559876543 - Email: pedro.ramirez@email.com"
  },
  {
    "name": "productoInteres",
    "value": "BC-BASIC-004"
  },
  {
    "name": "scoreCalificacion",
    "value": "D"
  }
]
```

**Response Body:**
```json
{
  "ejecutivoAsignado": "Lic. Ana Torres - Ext: 2951",
  "fechaSeguimiento": "2024-01-22T14:00:00-06:00",
  "numeroReferencia": "REF-BC-20240115-001235",
  "sucursalAsignada": "Bancoppel Centro",
  "proximosPasos": "Debido a la calificación, se programará seguimiento en 5 días hábiles para evaluar opciones de productos básicos y mejora de perfil crediticio"
}
```

---

**error_case** (HTTP 400)

Error por datos de contacto incompletos

**Request Body:**
```json
{
  "contactInfo": "Juan - Tel: 555",
  "interestedProduct": "BC-PLAT-002",
  "qualificationScore": "C"
}
```

**Request Params:**
```json
[
  {
    "name": "datosContacto",
    "value": "Juan - Tel: 555"
  },
  {
    "name": "productoInteres",
    "value": "BC-PLAT-002"
  },
  {
    "name": "scoreCalificacion",
    "value": "C"
  }
]
```

**Response Body:**
```json
{
  "error": true,
  "codigo": "INVALID_CONTACT_INFO",
  "mensaje": "Los datos de contacto proporcionados son incompletos. Se requiere nombre completo, teléfono válido y email",
  "ejecutivoAsignado": "",
  "fechaSeguimiento": "",
  "numeroReferencia": ""
}
```

---

**happy_path** (HTTP 201)

Registro exitoso de lead con alta calificación

**Request Body:**
```json
{
  "contactInfo": "María González - Tel: 5551234567 - Email: maria.gonzalez@email.com",
  "interestedProduct": "BC-GOLD-001",
  "qualificationScore": "A"
}
```

**Request Params:**
```json
[
  {
    "name": "datosContacto",
    "value": "María González - Tel: 5551234567 - Email: maria.gonzalez@email.com"
  },
  {
    "name": "productoInteres",
    "value": "BC-GOLD-001"
  },
  {
    "name": "scoreCalificacion",
    "value": "A"
  }
]
```

**Response Body:**
```json
{
  "ejecutivoAsignado": "Lic. Carlos Mendoza - Ext: 2847",
  "fechaSeguimiento": "2024-01-18T10:00:00-06:00",
  "numeroReferencia": "REF-BC-20240115-001234",
  "sucursalAsignada": "Bancoppel Insurgentes Sur",
  "proximosPasos": "El ejecutivo se comunicará dentro de 24 horas para agendar cita y completar la solicitud"
}
```

---

## Dataset: Generated (2026-02-25 06:26)

AI-generated mock data for Bancoppel_Cards_Sales_Assistant - Sales Assistant

_Source: generated_

### Web Action: `calcularBeneficios`

**edge_case** (HTTP 200)

Zero monthly spending calculation

**Request Body:**
```json
{
  "productId": "BC-CRD-001",
  "monthlySpending": "0"
}
```

**Response Body:**
```json
{
  "ahorroAnualidad": "$0.00 MXN",
  "cashbackAnual": "$0.00 MXN",
  "puntosRecompensa": {
    "puntosAnuales": 0,
    "valorPesos": "$0.00 MXN",
    "canjeables": []
  },
  "beneficiosAdicionales": [
    "Seguro de vida básico",
    "Atención telefónica 24/7"
  ],
  "resumenAnual": {
    "gastoTotal": "$0.00 MXN",
    "beneficioTotal": "$0.00 MXN",
    "porcentajeRetorno": "0.00%"
  },
  "nota": "Sin gastos registrados, los beneficios por uso no aplican. Solo beneficios incluidos por portación de tarjeta."
}
```

---

**error_case** (HTTP 404)

Product ID not found

**Request Body:**
```json
{
  "productId": "INVALID-PRODUCT",
  "monthlySpending": "5000"
}
```

**Response Body:**
```json
{
  "error": true,
  "codigo": "PRODUCTO_NO_ENCONTRADO",
  "mensaje": "El ID de producto proporcionado no existe en nuestro catálogo actual.",
  "ahorroAnualidad": "$0.00 MXN",
  "cashbackAnual": "$0.00 MXN",
  "puntosRecompensa": null
}
```

---

**happy_path** (HTTP 200)

Successful benefits calculation for platinum card

**Request Body:**
```json
{
  "productId": "BC-CRD-002",
  "monthlySpending": "8500"
}
```

**Response Body:**
```json
{
  "ahorroAnualidad": "$1,499.00 MXN",
  "cashbackAnual": "$2,550.00 MXN",
  "puntosRecompensa": {
    "puntosAnuales": 102000,
    "valorPesos": "$1,020.00 MXN",
    "canjeables": [
      "Millas aéreas",
      "Productos en tiendas afiliadas",
      "Descuentos en restaurantes"
    ]
  },
  "beneficiosAdicionales": [
    "Seguro de viaje hasta $50,000 USD",
    "Acceso a 4 salas VIP al año",
    "Concierge 24/7",
    "Protección de compras hasta $10,000 MXN"
  ],
  "resumenAnual": {
    "gastoTotal": "$102,000.00 MXN",
    "beneficioTotal": "$5,069.00 MXN",
    "porcentajeRetorno": "4.97%"
  }
}
```

---

### Web Action: `consultarProductosTarjetas`

**edge_case** (HTTP 200)

Very low income range with no eligible products

**Request Body:**
```json
{
  "cardType": "credito",
  "incomeRange": "5000-8000"
}
```

**Response Body:**
```json
{
  "productos": [],
  "recomendacion": "Lamentablemente, no contamos con productos de crédito que se ajusten a su rango de ingresos actual. Le recomendamos considerar nuestras tarjetas de débito o trabajar en incrementar sus ingresos mensuales a al menos $10,000.00 MXN para acceder a nuestros productos crediticios."
}
```

---

**error_case** (HTTP 400)

Invalid income range provided

**Request Body:**
```json
{
  "cardType": "credito",
  "incomeRange": "invalid-range"
}
```

**Response Body:**
```json
{
  "error": true,
  "codigo": "PARAM_INVALIDO",
  "mensaje": "El rango de ingresos proporcionado no es válido. Debe ser un formato como '10000-15000'.",
  "productos": [],
  "recomendacion": ""
}
```

---

**happy_path** (HTTP 200)

Successful query returning credit card products for middle income range

**Request Body:**
```json
{
  "cardType": "credito",
  "incomeRange": "15000-25000"
}
```

**Response Body:**
```json
{
  "productos": [
    {
      "id": "BC-CRD-001",
      "nombre": "Tarjeta Bancoppel Oro",
      "tipoTarjeta": "Crédito",
      "limiteCredito": "$45,000.00 MXN",
      "anualidad": "$899.00 MXN",
      "tasaInteres": "42.9% anual",
      "beneficios": [
        "2% cashback en supermercados",
        "Sin anualidad primer año",
        "Seguro de vida básico"
      ],
      "requisitos": {
        "ingresoMinimo": "$15,000.00 MXN",
        "edadMinima": 18,
        "antiguedadLaboral": "6 meses"
      }
    },
    {
      "id": "BC-CRD-002",
      "nombre": "Tarjeta Bancoppel Platino",
      "tipoTarjeta": "Crédito",
      "limiteCredito": "$80,000.00 MXN",
      "anualidad": "$1,499.00 MXN",
      "tasaInteres": "39.9% anual",
      "beneficios": [
        "3% cashback en gasolineras",
        "Acceso a salas VIP",
        "Seguro de viaje internacional"
      ],
      "requisitos": {
        "ingresoMinimo": "$20,000.00 MXN",
        "edadMinima": 21,
        "antiguedadLaboral": "12 meses"
      }
    }
  ],
  "recomendacion": "Basado en su rango de ingresos, recomendamos la Tarjeta Bancoppel Oro que se adapta perfectamente a su perfil financiero y le ofrece excelentes beneficios sin comprometer su capacidad de pago."
}
```

---

### Web Action: `evaluarPerfilCrediticio`

**edge_case** (HTTP 200)

Very low credit score with minimal approval

**Request Body:**
```json
{
  "rfc": "PEJU850615HDF",
  "curp": "PEJU850615HDFRRS04",
  "monthlyIncome": "12000"
}
```

**Response Body:**
```json
{
  "limiteAprobado": "$8,000.00 MXN",
  "productosElegibles": [
    {
      "id": "BC-CRD-003",
      "nombre": "Tarjeta Bancoppel Básica",
      "limiteOfrecido": "$8,000.00 MXN"
    }
  ],
  "scoreCredito": {
    "puntuacion": 520,
    "categoria": "Limitado",
    "factores": [
      "Historial crediticio con incidencias menores",
      "Ingresos justos para el monto solicitado",
      "Alta utilización de crédito actual"
    ],
    "observaciones": "Cliente requiere productos con límites conservadores y seguimiento cercano."
  },
  "vigenciaEvaluacion": "15 días"
}
```

---

**error_case** (HTTP 404)

CURP not found in credit bureau

**Request Body:**
```json
{
  "rfc": "INVALID123ABC",
  "curp": "INVALID123HDFRRL09",
  "monthlyIncome": "15000"
}
```

**Response Body:**
```json
{
  "error": true,
  "codigo": "CURP_NO_ENCONTRADA",
  "mensaje": "No se encontró información crediticia para la CURP proporcionada. Verifique los datos o el cliente podría no tener historial crediticio.",
  "limiteAprobado": "$0.00 MXN",
  "productosElegibles": [],
  "scoreCredito": null
}
```

---

**happy_path** (HTTP 200)

Successful credit profile evaluation with good score

**Request Body:**
```json
{
  "rfc": "GOMA801225HDF",
  "curp": "GOMA801225HDFRRL09",
  "monthlyIncome": "22000"
}
```

**Response Body:**
```json
{
  "limiteAprobado": "$65,000.00 MXN",
  "productosElegibles": [
    {
      "id": "BC-CRD-001",
      "nombre": "Tarjeta Bancoppel Oro",
      "limiteOfrecido": "$45,000.00 MXN"
    },
    {
      "id": "BC-CRD-002",
      "nombre": "Tarjeta Bancoppel Platino",
      "limiteOfrecido": "$65,000.00 MXN"
    },
    {
      "id": "BC-DEB-001",
      "nombre": "Tarjeta de Débito Premium",
      "limiteOfrecido": "N/A"
    }
  ],
  "scoreCredito": {
    "puntuacion": 720,
    "categoria": "Bueno",
    "factores": [
      "Historial crediticio limpio",
      "Ingresos estables comprobables",
      "Baja utilización de crédito actual"
    ],
    "observaciones": "Cliente con perfil crediticio favorable, elegible para productos premium."
  },
  "vigenciaEvaluacion": "30 días"
}
```

---

### Web Action: `registrarLeadVentas`

**edge_case** (HTTP 201)

Low qualification score lead registration

**Request Body:**
```json
{
  "contactInfo": "{\"nombre\": \"Pedro Sánchez López\", \"telefono\": \"55-9876-5432\", \"email\": \"pedro.sanchez@email.com\", \"ciudad\": \"Guadalajara\"}",
  "interestedProduct": "BC-CRD-003",
  "qualificationScore": "35"
}
```

**Response Body:**
```json
{
  "numeroReferencia": "BC-LEAD-2024-001848",
  "ejecutivoAsignado": {
    "nombre": "Lic. Ana Patricia Ruiz",
    "telefono": "33-2468-1357",
    "email": "ana.ruiz@bancoppel.com",
    "sucursal": "Bancoppel Guadalajara Centro",
    "horarioAtencion": "Lunes a Viernes 9:00 - 17:00"
  },
  "fechaSeguimiento": "2024-01-22T14:00:00-06:00",
  "estatusLead": "SEGUIMIENTO_ESPECIAL",
  "proximosPasos": [
    "Evaluación detallada de perfil",
    "Propuesta de productos alternativos",
    "Seguimiento en 7 días"
  ],
  "tiempoEstimadoProceso": "7-10 días hábiles",
  "observaciones": "Lead requiere evaluación adicional debido a score de calificación bajo."
}
```

---

**error_case** (HTTP 400)

Invalid contact information format

**Request Body:**
```json
{
  "contactInfo": "invalid-json-format",
  "interestedProduct": "BC-CRD-001",
  "qualificationScore": "70"
}
```

**Response Body:**
```json
{
  "error": true,
  "codigo": "DATOS_CONTACTO_INVALIDOS",
  "mensaje": "El formato de los datos de contacto es inválido. Debe ser un JSON válido con nombre, teléfono y email.",
  "numeroReferencia": null,
  "ejecutivoAsignado": null,
  "fechaSeguimiento": null
}
```

---

**happy_path** (HTTP 201)

Successful lead registration with high qualification score

**Request Body:**
```json
{
  "contactInfo": "{\"nombre\": \"María González Rodríguez\", \"telefono\": \"55-1234-5678\", \"email\": \"maria.gonzalez@email.com\", \"ciudad\": \"Ciudad de México\"}",
  "interestedProduct": "BC-CRD-002",
  "qualificationScore": "85"
}
```

**Response Body:**
```json
{
  "numeroReferencia": "BC-LEAD-2024-001847",
  "ejecutivoAsignado": {
    "nombre": "Lic. Carlos Mendoza Herrera",
    "telefono": "55-8765-4321",
    "email": "carlos.mendoza@bancoppel.com",
    "sucursal": "Bancoppel Polanco",
    "horarioAtencion": "Lunes a Viernes 9:00 - 18:00"
  },
  "fechaSeguimiento": "2024-01-15T10:00:00-06:00",
  "estatusLead": "ALTA_PRIORIDAD",
  "proximosPasos": [
    "Llamada de seguimiento en 24 horas",
    "Envío de documentación requerida",
    "Agendamiento de cita presencial"
  ],
  "tiempoEstimadoProceso": "3-5 días hábiles"
}
```

---
