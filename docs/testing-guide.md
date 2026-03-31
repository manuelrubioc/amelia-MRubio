# Testing Guide

Test scenarios from mock data sets with full request/response examples.
Each scenario includes the expected HTTP status, tester prompt, and sample payloads.

## Agent: Asistente de Ventas Tarjetas Bancoppel

### Dataset: Generated (2026-02-25 19:04) **(Active)**

AI-generated mock data for Bancoppel_Cards_Sales_Assistant - Sales Assistant

_Source: generated_

#### Web Action: `calcularBeneficios`

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

#### Web Action: `consultarProductosTarjetas`

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

#### Web Action: `evaluarPerfilCrediticio`

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

#### Web Action: `registrarLeadVentas`

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

### Dataset: Generated (2026-02-25 06:26)

AI-generated mock data for Bancoppel_Cards_Sales_Assistant - Sales Assistant

_Source: generated_

#### Web Action: `calcularBeneficios`

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

#### Web Action: `consultarProductosTarjetas`

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

#### Web Action: `evaluarPerfilCrediticio`

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

#### Web Action: `registrarLeadVentas`

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

## Agent: Asistente de Citas Bancoppel

### Dataset: Generated (2026-02-26 01:22) **(Active)**

AI-generated mock data for Asistente de Citas Bancoppel

_Source: generated_

#### Web Action: `buscarSucursales`

**alt_happy_large_result** (HTTP 200)

Búsqueda exitosa en Valencia devolviendo múltiples sucursales

**Request Body:**
```json
{
  "ciudad": "Valencia",
  "codigoPostal": "46001",
  "activas": true
}
```

**Request Params:**
```json
[
  {
    "name": "ciudad",
    "value": "Valencia"
  },
  {
    "name": "codigoPostal",
    "value": "46001"
  },
  {
    "name": "activas",
    "value": "true"
  }
]
```

**Response Body:**
```json
{
  "sucursales": [
    {
      "id": "SUC005",
      "nombre": "Bancoppel Valencia Centro",
      "direccion": "Calle Colón 24, 46004 Valencia",
      "telefono": "963456789",
      "horario": "09:00-14:30",
      "servicios": [
        "cuentas",
        "prestamos",
        "hipotecas"
      ]
    },
    {
      "id": "SUC006",
      "nombre": "Bancoppel Valencia Norte",
      "direccion": "Avenida Blasco Ibáñez 15, 46010 Valencia",
      "telefono": "963567890",
      "horario": "08:30-14:00",
      "servicios": [
        "inversiones",
        "seguros"
      ]
    },
    {
      "id": "SUC007",
      "nombre": "Bancoppel Valencia Sur",
      "direccion": "Calle Xàtiva 89, 46007 Valencia",
      "telefono": "963678901",
      "horario": "09:30-15:00",
      "servicios": [
        "cuentas",
        "tarjetas"
      ]
    }
  ],
  "totalSucursales": 3
}
```

---

**alt_happy_rural_location** (HTTP 200)

Búsqueda exitosa en localidad rural con pocas sucursales

**Request Body:**
```json
{
  "ciudad": "Guadalajara",
  "codigoPostal": "19001",
  "activas": true
}
```

**Request Params:**
```json
[
  {
    "name": "ciudad",
    "value": "Guadalajara"
  },
  {
    "name": "codigoPostal",
    "value": "19001"
  },
  {
    "name": "activas",
    "value": "true"
  }
]
```

**Response Body:**
```json
{
  "success": true,
  "sucursales": [
    {
      "id": "SUC021",
      "nombre": "Bancoppel Guadalajara Centro",
      "direccion": "Plaza Mayor, 15",
      "telefono": "949234567",
      "horario": "L-V: 8:30-14:00",
      "activa": true,
      "servicios": [
        "cuentas",
        "prestamos"
      ]
    }
  ],
  "totalSucursales": 1
}
```

---

**edge_case** (HTTP 200)

Búsqueda con código postal vacío

**Request Body:**
```json
{
  "ciudad": "Barcelona",
  "codigoPostal": "",
  "activas": true
}
```

**Request Params:**
```json
[
  {
    "name": "ciudad",
    "value": "Barcelona"
  },
  {
    "name": "codigoPostal",
    "value": ""
  }
]
```

**Response Body:**
```json
{
  "sucursales": [],
  "totalSucursales": 0
}
```

---

**edge_special_characters** (HTTP 200)

Búsqueda con caracteres especiales en ciudad

**Request Body:**
```json
{
  "ciudad": "Alcalá de Henares",
  "codigoPostal": "28801",
  "activas": true
}
```

**Request Params:**
```json
[
  {
    "name": "ciudad",
    "value": "Alcalá de Henares"
  },
  {
    "name": "codigoPostal",
    "value": "28801"
  },
  {
    "name": "activas",
    "value": "true"
  }
]
```

**Response Body:**
```json
{
  "success": true,
  "sucursales": [],
  "totalSucursales": 0,
  "mensaje": "No se encontraron sucursales activas en esta localización"
}
```

---

**error_case** (HTTP 404)

No se encontraron sucursales en la ubicación especificada

**Request Body:**
```json
{
  "ciudad": "Cuenca",
  "codigoPostal": "16001",
  "activas": true
}
```

**Request Params:**
```json
[
  {
    "name": "ciudad",
    "value": "Cuenca"
  },
  {
    "name": "codigoPostal",
    "value": "16001"
  }
]
```

**Response Body:**
```json
{
  "error": "NO_SUCURSALES_ENCONTRADAS",
  "mensaje": "No se encontraron sucursales activas en Cuenca con código postal 16001",
  "sucursales": [],
  "totalSucursales": 0
}
```

---

**error_city_not_covered** (HTTP 404)

Error por ciudad no cubierta por Bancoppel

**Request Body:**
```json
{
  "ciudad": "Pontevedra",
  "codigoPostal": "36001",
  "activas": false
}
```

**Request Params:**
```json
[
  {
    "name": "ciudad",
    "value": "Pontevedra"
  },
  {
    "name": "codigoPostal",
    "value": "36001"
  },
  {
    "name": "activas",
    "value": "false"
  }
]
```

**Response Body:**
```json
{
  "success": false,
  "error": "CIUDAD_NO_CUBIERTA",
  "message": "Bancoppel no tiene sucursales en Pontevedra",
  "sucursales": [],
  "totalSucursales": 0
}
```

---

**error_invalid_postal_code** (HTTP 404)

Error por código postal inexistente

**Request Body:**
```json
{
  "ciudad": "Zaragoza",
  "codigoPostal": "99999",
  "activas": true
}
```

**Request Params:**
```json
[
  {
    "name": "ciudad",
    "value": "Zaragoza"
  },
  {
    "name": "codigoPostal",
    "value": "99999"
  },
  {
    "name": "activas",
    "value": "true"
  }
]
```

**Response Body:**
```json
{
  "error": "CODIGO_POSTAL_INVALIDO",
  "mensaje": "El código postal 99999 no existe en España",
  "codigo": "CP_001",
  "sucursales": [],
  "totalSucursales": 0
}
```

---

**happy_path** (HTTP 200)

Búsqueda exitosa de sucursales en Madrid

**Request Body:**
```json
{
  "ciudad": "Madrid",
  "codigoPostal": "28001",
  "activas": true
}
```

**Request Params:**
```json
[
  {
    "name": "ciudad",
    "value": "Madrid"
  },
  {
    "name": "codigoPostal",
    "value": "28001"
  }
]
```

**Response Body:**
```json
{
  "sucursales": [
    {
      "id": "SUC001",
      "nombre": "Bancoppel Gran Vía",
      "direccion": "Gran Vía, 45",
      "codigoPostal": "28001",
      "ciudad": "Madrid",
      "telefono": "915234567",
      "horario": "L-V: 8:30-14:30",
      "coordenadas": {
        "lat": 40.42,
        "lng": -3.7
      }
    },
    {
      "id": "SUC002",
      "nombre": "Bancoppel Sol",
      "direccion": "Puerta del Sol, 12",
      "codigoPostal": "28001",
      "ciudad": "Madrid",
      "telefono": "915234568",
      "horario": "L-V: 8:30-14:30",
      "coordenadas": {
        "lat": 40.4165,
        "lng": -3.7026
      }
    }
  ],
  "totalSucursales": 2
}
```

---

#### Web Action: `consultarHorarios`

**alt_happy_peak_season** (HTTP 200)

Consulta exitosa durante temporada alta con horarios extendidos

**Request Body:**
```json
{
  "sucursalId": "SUC015",
  "fecha": "2024-03-15"
}
```

**Request Params:**
```json
[
  {
    "name": "sucursalId",
    "value": "SUC015"
  },
  {
    "name": "fecha",
    "value": "2024-03-15"
  }
]
```

**Response Body:**
```json
{
  "success": true,
  "horariosDisponibles": [
    "08:30",
    "09:00",
    "09:30",
    "10:00",
    "10:30",
    "11:00",
    "11:30",
    "12:00",
    "12:30",
    "15:30",
    "16:00",
    "16:30",
    "17:00",
    "17:30",
    "18:00"
  ],
  "asesoresDisponibles": [
    {
      "id": "ASE015",
      "nombre": "Carmen Jiménez López",
      "especialidad": "hipotecas"
    },
    {
      "id": "ASE016",
      "nombre": "Roberto Castillo Vega",
      "especialidad": "inversiones"
    },
    {
      "id": "ASE017",
      "nombre": "Patricia Moreno Silva",
      "especialidad": "seguros"
    }
  ]
}
```

---

**alt_happy_weekend** (HTTP 200)

Consulta exitosa para horarios de fin de semana

**Request Body:**
```json
{
  "sucursalId": "SUC003",
  "fecha": "2024-02-10"
}
```

**Request Params:**
```json
[
  {
    "name": "sucursalId",
    "value": "SUC003"
  },
  {
    "name": "fecha",
    "value": "2024-02-10"
  }
]
```

**Response Body:**
```json
{
  "horariosDisponibles": [
    "10:00",
    "11:00",
    "12:00"
  ],
  "asesoresDisponibles": [
    {
      "id": "ASE005",
      "nombre": "Carmen López Herrera",
      "especialidad": "Productos de inversión",
      "experiencia": "8 años"
    },
    {
      "id": "ASE006",
      "nombre": "Roberto Jiménez Vega",
      "especialidad": "Seguros y pensiones",
      "experiencia": "12 años"
    }
  ]
}
```

---

**edge_case** (HTTP 200)

Día sin horarios disponibles

**Request Body:**
```json
{
  "sucursalId": "SUC002",
  "fecha": "2024-12-25"
}
```

**Request Params:**
```json
[
  {
    "name": "fecha",
    "value": "2024-12-25"
  },
  {
    "name": "sucursalId",
    "value": "SUC002"
  }
]
```

**Response Body:**
```json
{
  "asesoresDisponibles": [],
  "horariosDisponibles": []
}
```

---

**edge_far_future_date** (HTTP 200)

Consulta con fecha muy futura, horarios limitados

**Request Body:**
```json
{
  "sucursalId": "SUC012",
  "fecha": "2024-08-30"
}
```

**Request Params:**
```json
[
  {
    "name": "sucursalId",
    "value": "SUC012"
  },
  {
    "name": "fecha",
    "value": "2024-08-30"
  }
]
```

**Response Body:**
```json
{
  "success": true,
  "horariosDisponibles": [],
  "asesoresDisponibles": [],
  "mensaje": "Horarios no disponibles para fechas posteriores a 60 días",
  "fechaLimite": "2024-06-15"
}
```

---

**error_case** (HTTP 404)

Fecha no válida o sucursal inexistente

**Request Body:**
```json
{
  "sucursalId": "SUC999",
  "fecha": "2024-01-20"
}
```

**Request Params:**
```json
[
  {
    "name": "fecha",
    "value": "2024-01-20"
  },
  {
    "name": "sucursalId",
    "value": "SUC999"
  }
]
```

**Response Body:**
```json
{
  "error": "SUCURSAL_NO_ENCONTRADA",
  "mensaje": "La sucursal SUC999 no existe o no está activa",
  "asesoresDisponibles": [],
  "horariosDisponibles": []
}
```

---

**error_past_date** (HTTP 400)

Error al consultar fecha en el pasado

**Request Body:**
```json
{
  "sucursalId": "SUC004",
  "fecha": "2023-12-01"
}
```

**Request Params:**
```json
[
  {
    "name": "sucursalId",
    "value": "SUC004"
  },
  {
    "name": "fecha",
    "value": "2023-12-01"
  }
]
```

**Response Body:**
```json
{
  "error": "FECHA_INVALIDA",
  "mensaje": "No es posible reservar citas en fechas pasadas",
  "codigo": "FH_002",
  "horariosDisponibles": [],
  "asesoresDisponibles": []
}
```

---

**error_sucursal_closed** (HTTP 503)

Error por sucursal temporalmente cerrada

**Request Body:**
```json
{
  "fecha": "2024-04-22",
  "sucursalId": "SUC008"
}
```

**Request Params:**
```json
[
  {
    "name": "fecha",
    "value": "2024-04-22"
  },
  {
    "name": "sucursalId",
    "value": "SUC008"
  }
]
```

**Response Body:**
```json
{
  "success": false,
  "error": "SUCURSAL_CERRADA",
  "message": "La sucursal está temporalmente cerrada por mantenimiento",
  "horariosDisponibles": [],
  "asesoresDisponibles": [],
  "fechaReapertura": "2024-04-25"
}
```

---

**happy_path** (HTTP 200)

Consulta exitosa de horarios disponibles

**Request Body:**
```json
{
  "sucursalId": "SUC001",
  "fecha": "2024-01-15"
}
```

**Request Params:**
```json
[
  {
    "name": "fecha",
    "value": "2024-01-15"
  },
  {
    "name": "sucursalId",
    "value": "SUC001"
  }
]
```

**Response Body:**
```json
{
  "asesoresDisponibles": [
    {
      "id": "ASE001",
      "nombre": "María García López",
      "especialidad": "Hipotecas"
    },
    {
      "id": "ASE002",
      "nombre": "Carlos Rodríguez Martín",
      "especialidad": "Inversiones"
    }
  ],
  "horariosDisponibles": [
    {
      "hora": "09:00",
      "asesorId": "ASE001",
      "disponible": true
    },
    {
      "hora": "09:30",
      "asesorId": "ASE001",
      "disponible": true
    },
    {
      "hora": "10:00",
      "asesorId": "ASE002",
      "disponible": true
    },
    {
      "hora": "10:30",
      "asesorId": "ASE002",
      "disponible": true
    },
    {
      "hora": "11:00",
      "asesorId": "ASE001",
      "disponible": true
    }
  ]
}
```

---

#### Web Action: `consultarServicios`

**alt_happy_business_services** (HTTP 200)

Consulta exitosa de servicios empresariales

**Request Body:**
```json
{
  "tipo": "empresas"
}
```

**Request Params:**
```json
[
  {
    "name": "tipoServicio",
    "value": "empresas"
  }
]
```

**Response Body:**
```json
{
  "success": true,
  "servicios": [
    {
      "id": "EMP001",
      "nombre": "Cuenta Corriente Empresarial",
      "categoria": "cuentas",
      "disponible": true
    },
    {
      "id": "EMP002",
      "nombre": "Línea de Crédito Comercial",
      "categoria": "financiacion",
      "disponible": true
    },
    {
      "id": "EMP003",
      "nombre": "Confirming de Proveedores",
      "categoria": "tesoreria",
      "disponible": true
    },
    {
      "id": "EMP004",
      "nombre": "Gestión de Nóminas",
      "categoria": "servicios",
      "disponible": false,
      "motivoNoDisponible": "En desarrollo"
    }
  ],
  "descripcionServicios": "Soluciones financieras integrales para empresas de todos los tamaños, desde autónomos hasta grandes corporaciones"
}
```

---

**alt_happy_insurance** (HTTP 200)

Consulta exitosa de servicios de seguros

**Request Body:**
```json
{
  "tipo": "seguros"
}
```

**Request Params:**
```json
[
  {
    "name": "tipoServicio",
    "value": "seguros"
  }
]
```

**Response Body:**
```json
{
  "servicios": [
    {
      "id": "SEG001",
      "nombre": "Seguro de Vida",
      "categoria": "seguros",
      "disponible": true
    },
    {
      "id": "SEG002",
      "nombre": "Seguro de Hogar",
      "categoria": "seguros",
      "disponible": true
    },
    {
      "id": "SEG003",
      "nombre": "Seguro de Automóvil",
      "categoria": "seguros",
      "disponible": true
    },
    {
      "id": "SEG004",
      "nombre": "Seguro de Salud",
      "categoria": "seguros",
      "disponible": false
    }
  ],
  "descripcionServicios": "Amplia gama de productos de seguros para proteger lo que más valoras: tu familia, tu hogar, tu vehículo y tu salud. Contamos con las mejores coberturas del mercado."
}
```

---

**edge_case** (HTTP 200)

Consulta de servicios generales

**Request Body:**
```json
{
  "tipo": ""
}
```

**Request Params:**
```json
[
  {
    "name": "tipoServicio",
    "value": ""
  }
]
```

**Response Body:**
```json
{
  "servicios": [],
  "descripcionServicios": "Por favor, especifique un tipo de servicio válido: hipotecas, inversiones, seguros, cuentas"
}
```

---

**edge_numeric_service_type** (HTTP 200)

Consulta con tipo de servicio numérico inesperado

**Request Body:**
```json
{
  "tipo": "123"
}
```

**Request Params:**
```json
[
  {
    "name": "tipoServicio",
    "value": "123"
  }
]
```

**Response Body:**
```json
{
  "success": true,
  "servicios": [],
  "descripcionServicios": "No se encontraron servicios para el tipo especificado",
  "sugerencias": [
    "hipotecas",
    "cuentas",
    "prestamos",
    "seguros",
    "inversiones",
    "empresas"
  ],
  "mensaje": "Utilice uno de los tipos de servicio sugeridos"
}
```

---

**error_case** (HTTP 400)

Tipo de servicio no válido

**Request Body:**
```json
{
  "tipo": "criptomonedas"
}
```

**Request Params:**
```json
[
  {
    "name": "tipoServicio",
    "value": "criptomonedas"
  }
]
```

**Response Body:**
```json
{
  "error": "TIPO_SERVICIO_INVALIDO",
  "mensaje": "El tipo de servicio 'criptomonedas' no está disponible",
  "servicios": [],
  "descripcionServicios": ""
}
```

---

**error_service_discontinued** (HTTP 410)

Error por servicio descontinuado

**Request Body:**
```json
{
  "tipo": "divisas"
}
```

**Request Params:**
```json
[
  {
    "name": "tipoServicio",
    "value": "divisas"
  }
]
```

**Response Body:**
```json
{
  "success": false,
  "error": "SERVICIO_DESCONTINUADO",
  "message": "El servicio de cambio de divisas ha sido descontinuado",
  "servicios": [],
  "descripcionServicios": null,
  "fechaDescontinuacion": "2023-12-01",
  "alternativas": [
    "Consulte con su asesor para opciones de transferencias internacionales"
  ]
}
```

---

**error_service_maintenance** (HTTP 503)

Error por servicio en mantenimiento

**Request Body:**
```json
{
  "tipo": "inversiones"
}
```

**Request Params:**
```json
[
  {
    "name": "tipoServicio",
    "value": "inversiones"
  }
]
```

**Response Body:**
```json
{
  "error": "SERVICIO_MANTENIMIENTO",
  "mensaje": "El servicio de consulta de inversiones está temporalmente no disponible por mantenimiento programado",
  "codigo": "SV_503",
  "tiempoEstimadoRecuperacion": "2024-01-16T08:00:00Z",
  "servicios": [],
  "descripcionServicios": null
}
```

---

**happy_path** (HTTP 200)

Consulta exitosa de servicios hipotecarios

**Request Body:**
```json
{
  "tipo": "hipotecas"
}
```

**Request Params:**
```json
[
  {
    "name": "tipoServicio",
    "value": "hipotecas"
  }
]
```

**Response Body:**
```json
{
  "servicios": [
    {
      "id": "HIP001",
      "nombre": "Hipoteca Fija",
      "categoria": "hipotecas",
      "duracionCita": "45 minutos",
      "documentosRequeridos": [
        "DNI",
        "Nóminas últimos 3 meses",
        "Declaración IRPF"
      ]
    },
    {
      "id": "HIP002",
      "nombre": "Hipoteca Variable",
      "categoria": "hipotecas",
      "duracionCita": "45 minutos",
      "documentosRequeridos": [
        "DNI",
        "Nóminas últimos 3 meses",
        "Declaración IRPF"
      ]
    },
    {
      "id": "HIP003",
      "nombre": "Reunificación de Deudas",
      "categoria": "hipotecas",
      "duracionCita": "60 minutos",
      "documentosRequeridos": [
        "DNI",
        "Extractos bancarios",
        "Listado de deudas"
      ]
    }
  ],
  "descripcionServicios": "Servicios hipotecarios disponibles en nuestras sucursales con asesoramiento personalizado"
}
```

---

#### Web Action: `reservarCita`

**alt_happy_business_client** (HTTP 200)

Reserva exitosa para cliente empresarial

**Request Body:**
```json
{
  "cliente": {
    "nombre": "Miguel Ángel Rodríguez Peña",
    "telefono": "689234567",
    "documento": "98765432D"
  },
  "cita": {
    "sucursalId": "SUC003",
    "fecha": "2024-02-05",
    "hora": "11:30",
    "asesorId": "ASE003"
  }
}
```

**Request Params:**
```json
[
  {
    "name": "nombreCliente",
    "value": "Miguel Ángel Rodríguez Peña"
  },
  {
    "name": "telefono",
    "value": "689234567"
  },
  {
    "name": "documentoId",
    "value": "98765432D"
  },
  {
    "name": "sucursalId",
    "value": "SUC003"
  },
  {
    "name": "fecha",
    "value": "2024-02-05"
  },
  {
    "name": "hora",
    "value": "11:30"
  },
  {
    "name": "asesorId",
    "value": "ASE003"
  }
]
```

**Response Body:**
```json
{
  "estadoReserva": "CONFIRMADA",
  "numeroConfirmacion": "BC2024020511300003",
  "detallesCita": {
    "cliente": {
      "nombre": "Miguel Ángel Rodríguez Peña",
      "telefono": "689234567",
      "documento": "98765432D"
    },
    "asesor": {
      "nombre": "Isabel Moreno Castro",
      "especialidad": "Banca empresarial"
    },
    "sucursal": {
      "nombre": "Bancoppel Sevilla Centro",
      "direccion": "Calle Sierpes 42, 41004 Sevilla"
    },
    "fechaHora": "2024-02-05T11:30:00",
    "duracionEstimada": "45 minutos",
    "recordatorio": "Se enviará SMS 24h antes"
  }
}
```

---

**alt_happy_premium_client** (HTTP 201)

Reserva exitosa para cliente premium con servicios especiales

**Request Body:**
```json
{
  "cliente": {
    "nombre": "Francisco Javier Ruiz Herrera",
    "telefono": "634567890",
    "documento": "33445566F"
  },
  "cita": {
    "sucursalId": "SUC007",
    "fecha": "2024-03-08",
    "hora": "15:00",
    "asesorId": "ASE012"
  }
}
```

**Request Params:**
```json
[
  {
    "name": "nombreCliente",
    "value": "Francisco Javier Ruiz Herrera"
  },
  {
    "name": "telefono",
    "value": "634567890"
  },
  {
    "name": "documentoId",
    "value": "33445566F"
  },
  {
    "name": "sucursalId",
    "value": "SUC007"
  },
  {
    "name": "fecha",
    "value": "2024-03-08"
  },
  {
    "name": "hora",
    "value": "15:00"
  },
  {
    "name": "asesorId",
    "value": "ASE012"
  }
]
```

**Response Body:**
```json
{
  "success": true,
  "estadoReserva": "CONFIRMADA",
  "numeroConfirmacion": "BCL-2024-789123",
  "detallesCita": {
    "cliente": {
      "nombre": "Francisco Javier Ruiz Herrera",
      "telefono": "634567890",
      "tipoCliente": "PREMIUM"
    },
    "sucursal": {
      "nombre": "Bancoppel Bilbao Centro",
      "direccion": "Gran Vía, 88"
    },
    "asesor": {
      "nombre": "Elena Navarro Prieto",
      "especialidad": "banca privada"
    },
    "fechaHora": "2024-03-08T15:00:00",
    "serviciosSolicitados": [
      "gestión patrimonial"
    ],
    "preparacionRequerida": [
      "documentos de ingresos",
      "extractos bancarios"
    ]
  }
}
```

---

**edge_case** (HTTP 200)

Reserva en fecha límite del sistema

**Request Body:**
```json
{
  "cliente": {
    "nombre": "Laura Martín Díez",
    "telefono": "655111222",
    "documento": "11223344C"
  },
  "cita": {
    "sucursalId": "SUC001",
    "fecha": "2024-12-31",
    "hora": "14:00",
    "asesorId": "ASE001"
  }
}
```

**Request Params:**
```json
[
  {
    "name": "asesorId",
    "value": "ASE001"
  },
  {
    "name": "documentoId",
    "value": "11223344C"
  },
  {
    "name": "fecha",
    "value": "2024-12-31"
  },
  {
    "name": "hora",
    "value": "14:00"
  },
  {
    "name": "nombreCliente",
    "value": "Laura Martín Díez"
  },
  {
    "name": "sucursalId",
    "value": "SUC001"
  },
  {
    "name": "telefono",
    "value": "655111222"
  }
]
```

**Response Body:**
```json
{
  "estadoReserva": "PENDIENTE_CONFIRMACION",
  "numeroConfirmacion": "CITA-2024-999999",
  "detallesCita": {
    "cliente": {
      "nombre": "Laura Martín Díez",
      "telefono": "655111222",
      "documento": "11223344C"
    },
    "sucursal": {
      "nombre": "Bancoppel Gran Vía",
      "direccion": "Gran Vía, 45, Madrid"
    },
    "asesor": {
      "nombre": "María García López",
      "especialidad": "Hipotecas"
    },
    "fechaHora": "2024-12-31T14:00:00",
    "duracionEstimada": "30 minutos",
    "instrucciones": "Cita sujeta a confirmación por horario especial"
  }
}
```

---

**edge_last_minute_booking** (HTTP 201)

Reserva de último minuto para el mismo día

**Request Body:**
```json
{
  "cliente": {
    "nombre": "Alejandro Torres Mendoza",
    "telefono": "611222333",
    "documento": "77889900G"
  },
  "cita": {
    "sucursalId": "SUC010",
    "fecha": "2024-01-30",
    "hora": "13:45",
    "asesorId": "ASE020"
  }
}
```

**Request Params:**
```json
[
  {
    "name": "nombreCliente",
    "value": "Alejandro Torres Mendoza"
  },
  {
    "name": "telefono",
    "value": "611222333"
  },
  {
    "name": "documentoId",
    "value": "77889900G"
  },
  {
    "name": "sucursalId",
    "value": "SUC010"
  },
  {
    "name": "fecha",
    "value": "2024-01-30"
  },
  {
    "name": "hora",
    "value": "13:45"
  },
  {
    "name": "asesorId",
    "value": "ASE020"
  }
]
```

**Response Body:**
```json
{
  "success": true,
  "estadoReserva": "PENDIENTE_CONFIRMACION",
  "numeroConfirmacion": "BCL-2024-456789",
  "detallesCita": {
    "cliente": {
      "nombre": "Alejandro Torres Mendoza",
      "telefono": "611222333"
    },
    "sucursal": {
      "nombre": "Bancoppel Santander Norte",
      "direccion": "Av. de los Castros, 42"
    },
    "asesor": {
      "nombre": "Isabel Fernández Cruz",
      "especialidad": "cuentas corrientes"
    },
    "fechaHora": "2024-01-30T13:45:00",
    "advertencia": "Cita de último minuto - confirme su asistencia llamando al 942567890",
    "tiempoLimiteConfirmacion": "2024-01-30T11:00:00"
  }
}
```

---

**error_case** (HTTP 409)

Horario ya ocupado

**Request Body:**
```json
{
  "cliente": {
    "nombre": "Pedro González Sánchez",
    "telefono": "677987654",
    "documento": "87654321B"
  },
  "cita": {
    "sucursalId": "SUC002",
    "fecha": "2024-01-16",
    "hora": "10:00",
    "asesorId": "ASE002"
  }
}
```

**Request Params:**
```json
[
  {
    "name": "asesorId",
    "value": "ASE002"
  },
  {
    "name": "documentoId",
    "value": "87654321B"
  },
  {
    "name": "fecha",
    "value": "2024-01-16"
  },
  {
    "name": "hora",
    "value": "10:00"
  },
  {
    "name": "nombreCliente",
    "value": "Pedro González Sánchez"
  },
  {
    "name": "sucursalId",
    "value": "SUC002"
  },
  {
    "name": "telefono",
    "value": "677987654"
  }
]
```

**Response Body:**
```json
{
  "error": "HORARIO_NO_DISPONIBLE",
  "mensaje": "El horario seleccionado ya no está disponible",
  "estadoReserva": "RECHAZADA",
  "numeroConfirmacion": null,
  "detallesCita": null
}
```

---

**error_duplicate_appointment** (HTTP 409)

Error por cita duplicada para el mismo cliente

**Request Body:**
```json
{
  "cliente": {
    "nombre": "Rosa María Vázquez Gil",
    "telefono": "612345678",
    "documento": "45678912E"
  },
  "cita": {
    "sucursalId": "SUC001",
    "fecha": "2024-01-25",
    "hora": "16:00",
    "asesorId": "ASE004"
  }
}
```

**Request Params:**
```json
[
  {
    "name": "nombreCliente",
    "value": "Rosa María Vázquez Gil"
  },
  {
    "name": "telefono",
    "value": "612345678"
  },
  {
    "name": "documentoId",
    "value": "45678912E"
  },
  {
    "name": "sucursalId",
    "value": "SUC001"
  },
  {
    "name": "fecha",
    "value": "2024-01-25"
  },
  {
    "name": "hora",
    "value": "16:00"
  },
  {
    "name": "asesorId",
    "value": "ASE004"
  }
]
```

**Response Body:**
```json
{
  "error": "CITA_DUPLICADA",
  "mensaje": "Ya tiene una cita programada para el 2024-01-25 a las 10:30",
  "codigo": "CT_003",
  "estadoReserva": "RECHAZADA",
  "citaExistente": {
    "fecha": "2024-01-25",
    "hora": "10:30",
    "numeroConfirmacion": "BC2024012510300001"
  },
  "numeroConfirmacion": null,
  "detallesCita": null
}
```

---

**error_invalid_document** (HTTP 400)

Error por documento de identidad inválido

**Request Body:**
```json
{
  "cliente": {
    "nombre": "Sofía Delgado Ramos",
    "telefono": "698765432",
    "documento": "INVALID123"
  },
  "cita": {
    "sucursalId": "SUC005",
    "fecha": "2024-02-28",
    "hora": "12:30",
    "asesorId": "ASE008"
  }
}
```

**Request Params:**
```json
[
  {
    "name": "nombreCliente",
    "value": "Sofía Delgado Ramos"
  },
  {
    "name": "telefono",
    "value": "698765432"
  },
  {
    "name": "documentoId",
    "value": "INVALID123"
  },
  {
    "name": "sucursalId",
    "value": "SUC005"
  },
  {
    "name": "fecha",
    "value": "2024-02-28"
  },
  {
    "name": "hora",
    "value": "12:30"
  },
  {
    "name": "asesorId",
    "value": "ASE008"
  }
]
```

**Response Body:**
```json
{
  "success": false,
  "error": "DOCUMENTO_INVALIDO",
  "message": "El formato del documento de identidad no es válido",
  "estadoReserva": "RECHAZADA",
  "numeroConfirmacion": null,
  "detallesError": {
    "campo": "documento",
    "valorRecibido": "INVALID123",
    "formatoEsperado": "########[A-Z] (ej: 12345678A)"
  }
}
```

---

**happy_path** (HTTP 200)

Reserva de cita exitosa

**Request Body:**
```json
{
  "cliente": {
    "nombre": "Ana Fernández Ruiz",
    "telefono": "666123456",
    "documento": "12345678A"
  },
  "cita": {
    "sucursalId": "SUC001",
    "fecha": "2024-01-15",
    "hora": "09:30",
    "asesorId": "ASE001"
  }
}
```

**Request Params:**
```json
[
  {
    "name": "asesorId",
    "value": "ASE001"
  },
  {
    "name": "documentoId",
    "value": "12345678A"
  },
  {
    "name": "fecha",
    "value": "2024-01-15"
  },
  {
    "name": "hora",
    "value": "09:30"
  },
  {
    "name": "nombreCliente",
    "value": "Ana Fernández Ruiz"
  },
  {
    "name": "sucursalId",
    "value": "SUC001"
  },
  {
    "name": "telefono",
    "value": "666123456"
  }
]
```

**Response Body:**
```json
{
  "estadoReserva": "CONFIRMADA",
  "numeroConfirmacion": "CITA-2024-001234",
  "detallesCita": {
    "cliente": {
      "nombre": "Ana Fernández Ruiz",
      "telefono": "666123456",
      "documento": "12345678A"
    },
    "sucursal": {
      "nombre": "Bancoppel Gran Vía",
      "direccion": "Gran Vía, 45, Madrid"
    },
    "asesor": {
      "nombre": "María García López",
      "especialidad": "Hipotecas"
    },
    "fechaHora": "2024-01-15T09:30:00",
    "duracionEstimada": "30 minutos",
    "instrucciones": "Traer DNI y documentación relevante"
  }
}
```

---

## Agent: Asistente de Pedidos Agrosuper

### Dataset: Generated (2026-02-27 00:31) **(Active)**

AI-generated mock data for Agente Comercial Agrosuper

_Source: generated_

#### Web Action: `calcularPrecioPublico`

**alt_happy_precio_alto_sin_iva** (HTTP 200)

Cálculo exitoso de precio público para producto premium sin IVA

> **Tester prompt:** Calcula el precio público de un producto con precio neto $89.500 sin IVA incluido.

**Request Body:**
```json
{
  "precioNeto": 89500,
  "conIva": false
}
```

**Request Params:**
```json
[
  {
    "name": "precioNeto",
    "value": "89500"
  },
  {
    "name": "conIva",
    "value": "false"
  }
]
```

**Response Body:**
```json
{
  "margenGanancia": "28.5%",
  "precioPublico": "$115.015"
}
```

---

**edge_precio_minimo_con_iva** (HTTP 200)

Cálculo con precio mínimo permitido incluyendo IVA

> **Tester prompt:** Calcula el precio público de un producto con precio neto de $1 con IVA incluido.

**Request Body:**
```json
{
  "precioNeto": 1,
  "conIva": true
}
```

**Request Params:**
```json
[
  {
    "name": "precioNeto",
    "value": "1"
  },
  {
    "name": "conIva",
    "value": "true"
  }
]
```

**Response Body:**
```json
{
  "margenGanancia": "25.0%",
  "precioPublico": "$1"
}
```

---

**error_precio_negativo** (HTTP 400)

Error por precio neto negativo

> **Tester prompt:** Intenta calcular el precio público con un precio neto de -$1.500 con IVA.

**Request Body:**
```json
{
  "precioNeto": -1500,
  "conIva": true
}
```

**Request Params:**
```json
[
  {
    "name": "precioNeto",
    "value": "-1500"
  },
  {
    "name": "conIva",
    "value": "true"
  }
]
```

**Response Body:**
```json
{
  "error": true,
  "codigo": "PRECIO_INVALIDO",
  "mensaje": "El precio neto no puede ser negativo",
  "margenGanancia": null,
  "precioPublico": null
}
```

---

#### Web Action: `calcularTotalPedido`

**alt_happy_pedido_mixto** (HTTP 200)

Cálculo exitoso con productos mixtos y promociones

**Request Body:**
```json
{
  "productos": "AGS-201,AGS-105,AGS-300",
  "cantidades": "1.5,2,0.8"
}
```

**Request Params:**
```json
[
  {
    "name": "cantidades",
    "value": "1.5,2,0.8"
  },
  {
    "name": "productos",
    "value": "AGS-201,AGS-105,AGS-300"
  }
]
```

**Response Body:**
```json
{
  "detalleProductos": [
    {
      "codigo": "AGS-201",
      "nombre": "Lomo Premium",
      "cantidad": 1.5,
      "precioUnitario": 8990,
      "subtotal": 13485
    },
    {
      "codigo": "AGS-105",
      "nombre": "Pechuga Pollo",
      "cantidad": 2,
      "precioUnitario": 4990,
      "subtotal": 9980
    },
    {
      "codigo": "AGS-300",
      "nombre": "Cecinas Surtidas",
      "cantidad": 0.8,
      "precioUnitario": 3500,
      "subtotal": 2800
    }
  ],
  "sugerenciaVenta": {
    "producto": "AGS-310 - Queso Mantecoso",
    "razon": "Complementa perfectamente con cecinas"
  },
  "totalAproximado": 26265,
  "descuentos": 1315,
  "totalFinal": 24950
}
```

---

**edge_case** (HTTP 200)

Cálculo con cantidades mínimas

**Request Body:**
```json
{
  "productos": "AGS-001",
  "cantidades": "0.1"
}
```

**Request Params:**
```json
[
  {
    "name": "cantidades",
    "value": "0.1"
  },
  {
    "name": "productos",
    "value": "AGS-001"
  }
]
```

**Response Body:**
```json
{
  "detalleProductos": [
    {
      "id": "AGS-001",
      "nombre": "Pollo Entero Super Fresco",
      "cantidad": 0.1,
      "precioUnitario": 3290,
      "subtotal": 329
    }
  ],
  "sugerenciaVenta": [],
  "totalAproximado": 329
}
```

---

**edge_productos_fraccionados** (HTTP 200)

Cálculo con productos en cantidades muy pequeñas

**Request Body:**
```json
{
  "productos": "AGS-105,AGS-002",
  "cantidades": "0.25,0.1"
}
```

**Request Params:**
```json
[
  {
    "name": "cantidades",
    "value": "0.25,0.1"
  },
  {
    "name": "productos",
    "value": "AGS-105,AGS-002"
  }
]
```

**Response Body:**
```json
{
  "detalleProductos": [
    {
      "codigo": "AGS-105",
      "nombre": "Pechuga Pollo",
      "cantidad": 0.25,
      "precioUnitario": 4990,
      "subtotal": 1248
    },
    {
      "codigo": "AGS-002",
      "nombre": "Muslo Pollo",
      "cantidad": 0.1,
      "precioUnitario": 2890,
      "subtotal": 289
    }
  ],
  "sugerenciaVenta": null,
  "totalAproximado": 1537,
  "advertencia": "Pedido mínimo recomendado: $5.000 para optimizar costo de envío"
}
```

---

**error_cantidad_excesiva** (HTTP 400)

Error por cantidad que excede límites de pedido

**Request Body:**
```json
{
  "productos": "AGS-201",
  "cantidades": "50"
}
```

**Request Params:**
```json
[
  {
    "name": "cantidades",
    "value": "50"
  },
  {
    "name": "productos",
    "value": "AGS-201"
  }
]
```

**Response Body:**
```json
{
  "error": true,
  "codigo": "CANTIDAD_EXCESIVA",
  "mensaje": "La cantidad solicitada (50kg) excede el límite máximo por pedido (20kg)",
  "limiteMaximo": 20,
  "productoAfectado": "AGS-201"
}
```

---

**error_case** (HTTP 400)

Producto no disponible en stock suficiente

**Request Body:**
```json
{
  "productos": "AGS-003,AGS-004",
  "cantidades": "10,25"
}
```

**Request Params:**
```json
[
  {
    "name": "cantidades",
    "value": "10,25"
  },
  {
    "name": "productos",
    "value": "AGS-003,AGS-004"
  }
]
```

**Response Body:**
```json
{
  "error": "STOCK_INSUFICIENTE",
  "mensaje": "Stock insuficiente para algunos productos",
  "detalles": [
    {
      "producto": "AGS-004",
      "solicitado": 25,
      "disponible": 12
    }
  ]
}
```

---

**happy_path** (HTTP 200)

Cálculo exitoso del total del pedido con sugerencias

**Request Body:**
```json
{
  "productos": "AGS-001,AGS-002,AGS-015",
  "cantidades": "2,1.5,3"
}
```

**Request Params:**
```json
[
  {
    "name": "cantidades",
    "value": "2,1.5,3"
  },
  {
    "name": "productos",
    "value": "AGS-001,AGS-002,AGS-015"
  }
]
```

**Response Body:**
```json
{
  "detalleProductos": [
    {
      "id": "AGS-001",
      "nombre": "Pollo Entero Super Fresco",
      "cantidad": 2,
      "precioUnitario": 3290,
      "subtotal": 6580
    },
    {
      "id": "AGS-002",
      "nombre": "Pechuga de Pollo Deshuesada",
      "cantidad": 1.5,
      "precioUnitario": 5890,
      "subtotal": 8835
    },
    {
      "id": "AGS-015",
      "nombre": "Chorizo Parrillero",
      "cantidad": 3,
      "precioUnitario": 4250,
      "subtotal": 12750
    }
  ],
  "sugerenciaVenta": [
    {
      "id": "AGS-020",
      "nombre": "Condimento para Pollo",
      "motivo": "Complementa perfectamente con pollo fresco",
      "precio": 1890
    }
  ],
  "totalAproximado": 28165
}
```

---

#### Web Action: `confirmarPedido`

**alt_happy_entrega_programada** (HTTP 201)

Confirmación exitosa con entrega programada en Valparaíso

**Request Body:**
```json
{
  "clienteId": "CLI-78901",
  "pedidoCompleto": "AGS-201:1.5kg,AGS-105:2kg,AGS-300:0.8kg",
  "fechaEntrega": "2024-02-05"
}
```

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-78901"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-02-05"
  },
  {
    "name": "pedidoCompleto",
    "value": "AGS-201:1.5kg,AGS-105:2kg,AGS-300:0.8kg"
  }
]
```

**Response Body:**
```json
{
  "fechaConfirmacion": "2024-01-30T14:25:30-03:00",
  "numeroPedido": "PED-567890",
  "resumenPedido": {
    "cliente": "CLI-78901",
    "productos": 3,
    "pesoTotal": "4.3kg",
    "valorTotal": 24950,
    "fechaEntrega": "2024-02-05",
    "horaEntrega": "09:00-12:00",
    "direccionEntrega": "Valparaíso, V Región",
    "metodoPago": "transferencia",
    "codigoSeguimiento": "AGS-567890-VP"
  }
}
```

---

**edge_case** (HTTP 201)

Confirmación de pedido con monto mínimo

**Request Body:**
```json
{
  "clienteId": "CLI-11111",
  "pedidoCompleto": "AGS-001:0.5kg",
  "fechaEntrega": "2024-01-29"
}
```

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-11111"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-01-29"
  },
  {
    "name": "pedidoCompleto",
    "value": "AGS-001:0.5kg"
  }
]
```

**Response Body:**
```json
{
  "fechaConfirmacion": "2024-01-22T14:45:00-03:00",
  "numeroPedido": "PED-789016",
  "resumenPedido": {
    "cliente": "CLI-11111",
    "productos": [
      {
        "nombre": "Pollo Entero Super Fresco",
        "cantidad": "0.5kg",
        "precio": 1645
      }
    ],
    "total": 1645,
    "fechaEntrega": "2024-01-29",
    "horarioEntrega": "14:00-17:00",
    "estado": "confirmado",
    "advertencia": "Pedido bajo monto mínimo recomendado de $10.000"
  }
}
```

---

**edge_pedido_minimo_exacto** (HTTP 201)

Confirmación con monto exacto del pedido mínimo

**Request Body:**
```json
{
  "clienteId": "CLI-33333",
  "pedidoCompleto": "AGS-002:1.73kg",
  "fechaEntrega": "2024-02-12"
}
```

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-33333"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-02-12"
  },
  {
    "name": "pedidoCompleto",
    "value": "AGS-002:1.73kg"
  }
]
```

**Response Body:**
```json
{
  "fechaConfirmacion": "2024-01-30T16:45:12-03:00",
  "numeroPedido": "PED-500000",
  "resumenPedido": {
    "cliente": "CLI-33333",
    "productos": 1,
    "pesoTotal": "1.73kg",
    "valorTotal": 5000,
    "fechaEntrega": "2024-02-12",
    "observacion": "Pedido alcanza monto mínimo exacto",
    "costoEnvio": 0,
    "tiempoEstimado": "3-4 horas"
  }
}
```

---

**error_case** (HTTP 400)

Error por fecha de entrega no válida

**Request Body:**
```json
{
  "clienteId": "CLI-67890",
  "pedidoCompleto": "AGS-005:1kg",
  "fechaEntrega": "2024-01-21"
}
```

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-67890"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-01-21"
  },
  {
    "name": "pedidoCompleto",
    "value": "AGS-005:1kg"
  }
]
```

**Response Body:**
```json
{
  "error": "FECHA_ENTREGA_INVALIDA",
  "mensaje": "La fecha de entrega debe ser al menos 24 horas desde ahora",
  "fechaMinima": "2024-01-23",
  "fechaSolicitada": "2024-01-21"
}
```

---

**error_fecha_no_disponible** (HTTP 422)

Error por fecha de entrega no disponible en la zona

**Request Body:**
```json
{
  "clienteId": "CLI-45678",
  "pedidoCompleto": "AGS-001:3kg",
  "fechaEntrega": "2024-01-31"
}
```

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-45678"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-01-31"
  },
  {
    "name": "pedidoCompleto",
    "value": "AGS-001:3kg"
  }
]
```

**Response Body:**
```json
{
  "error": true,
  "codigo": "FECHA_NO_DISPONIBLE",
  "mensaje": "La fecha 2024-01-31 no está disponible para entregas en su zona",
  "fechasDisponibles": [
    "2024-02-02",
    "2024-02-05",
    "2024-02-07"
  ],
  "zonaEntrega": "Región Metropolitana Sur"
}
```

---

**happy_path** (HTTP 201)

Confirmación exitosa del pedido

**Request Body:**
```json
{
  "clienteId": "CLI-12345",
  "pedidoCompleto": "AGS-001:2kg,AGS-002:1.5kg,AGS-015:3kg",
  "fechaEntrega": "2024-01-28"
}
```

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-12345"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-01-28"
  },
  {
    "name": "pedidoCompleto",
    "value": "AGS-001:2kg,AGS-002:1.5kg,AGS-015:3kg"
  }
]
```

**Response Body:**
```json
{
  "fechaConfirmacion": "2024-01-22T14:30:00-03:00",
  "numeroPedido": "PED-789015",
  "resumenPedido": {
    "cliente": "CLI-12345",
    "productos": [
      {
        "nombre": "Pollo Entero Super Fresco",
        "cantidad": "2kg",
        "precio": 6580
      },
      {
        "nombre": "Pechuga de Pollo Deshuesada",
        "cantidad": "1.5kg",
        "precio": 8835
      },
      {
        "nombre": "Chorizo Parrillero",
        "cantidad": "3kg",
        "precio": 12750
      }
    ],
    "total": 28165,
    "fechaEntrega": "2024-01-28",
    "horarioEntrega": "09:00-12:00",
    "estado": "confirmado"
  }
}
```

---

#### Web Action: `consultarCatalogoProductos`

**alt_happy_busqueda_especifica** (HTTP 200)

Búsqueda exitosa de productos específicos con promociones vigentes

> **Tester prompt:** Busca productos de lomo vetado en la categoría vacuno.

**Request Params:**
```json
[
  {
    "name": "busqueda",
    "value": "lomo vetado"
  },
  {
    "name": "categoria",
    "value": "vacuno"
  }
]
```

**Response Body:**
```json
{
  "productos": [
    {
      "codigo": "AGS-450",
      "nombre": "Lomo Vetado Premium",
      "precio": "$18.990/kg",
      "stock": "Disponible",
      "origen": "Región de Los Lagos"
    },
    {
      "codigo": "AGS-451",
      "nombre": "Lomo Vetado Tradicional",
      "precio": "$15.490/kg",
      "stock": "Disponible",
      "origen": "Región del Maule"
    }
  ],
  "promociones": [
    {
      "tipo": "Descuento por volumen",
      "descripcion": "15% descuento comprando más de 10kg",
      "vigencia": "Hasta 28/02/2024"
    }
  ]
}
```

---

**alt_happy_premium_products** (HTTP 200)

Consulta exitosa de productos premium de cerdo

**Request Params:**
```json
[
  {
    "name": "categoria",
    "value": "cerdo"
  },
  {
    "name": "tipoProducto",
    "value": "premium"
  }
]
```

**Response Body:**
```json
{
  "productos": [
    {
      "codigo": "AGS-201",
      "nombre": "Lomo Premium Agrosuper",
      "precio": 8990,
      "unidad": "kg",
      "disponibilidad": "alta",
      "descripcion": "Lomo de cerdo premium sin aditivos"
    },
    {
      "codigo": "AGS-202",
      "nombre": "Chuletas Premium",
      "precio": 6490,
      "unidad": "kg",
      "disponibilidad": "media"
    }
  ],
  "promociones": [
    {
      "tipo": "descuento",
      "descripcion": "15% descuento en compras sobre $50.000",
      "vigencia": "2024-02-15"
    }
  ]
}
```

---

**edge_busqueda_caracteres_especiales** (HTTP 200)

Búsqueda con caracteres especiales que no arroja resultados

> **Tester prompt:** Busca 'pollo & jamón' en la categoría embutidos.

**Request Params:**
```json
[
  {
    "name": "busqueda",
    "value": "pollo & jamón"
  },
  {
    "name": "categoria",
    "value": "embutidos"
  }
]
```

**Response Body:**
```json
{
  "productos": [],
  "promociones": []
}
```

---

**edge_case** (HTTP 200)

Consulta de categoría sin productos disponibles

**Request Params:**
```json
[
  {
    "name": "categoria",
    "value": "pavo"
  },
  {
    "name": "tipoProducto",
    "value": "organico"
  }
]
```

**Response Body:**
```json
{
  "productos": [],
  "promociones": [],
  "mensaje": "No hay productos disponibles para la categoría y tipo especificado"
}
```

---

**edge_sin_stock_temporada** (HTTP 200)

Consulta con productos sin stock por temporada alta

**Request Params:**
```json
[
  {
    "name": "categoria",
    "value": "vacuno"
  },
  {
    "name": "tipoProducto",
    "value": "especial"
  }
]
```

**Response Body:**
```json
{
  "productos": [],
  "promociones": [],
  "mensaje": "Productos temporalmente sin stock debido a alta demanda navideña",
  "fechaReposicion": "2024-02-05"
}
```

---

**error_case** (HTTP 404)

Categoría de producto no encontrada

**Request Params:**
```json
[
  {
    "name": "categoria",
    "value": "mariscos"
  },
  {
    "name": "tipoProducto",
    "value": "congelado"
  }
]
```

**Response Body:**
```json
{
  "error": "CATEGORIA_NO_ENCONTRADA",
  "mensaje": "La categoría 'mariscos' no está disponible en nuestro catálogo",
  "categorias_disponibles": [
    "pollo",
    "cerdo",
    "pavo",
    "embutidos"
  ]
}
```

---

**error_categoria_discontinuada** (HTTP 404)

Error al consultar categoría discontinuada

**Request Params:**
```json
[
  {
    "name": "categoria",
    "value": "cordero"
  },
  {
    "name": "tipoProducto",
    "value": "fresco"
  }
]
```

**Response Body:**
```json
{
  "error": true,
  "codigo": "CAT_DISCONTINUADA",
  "mensaje": "La categoría 'cordero' ha sido discontinuada en nuestra línea de productos",
  "sugerencias": [
    "cerdo",
    "vacuno",
    "pollo"
  ]
}
```

---

**error_categoria_mantenimiento** (HTTP 503)

Error por categoría en mantenimiento temporal

> **Tester prompt:** Busca productos de salmón en la categoría pescados.

**Request Params:**
```json
[
  {
    "name": "busqueda",
    "value": "salmón"
  },
  {
    "name": "categoria",
    "value": "pescados"
  }
]
```

**Response Body:**
```json
{
  "error": true,
  "codigo": "CATEGORIA_MANTENIMIENTO",
  "mensaje": "Categoría pescados en mantenimiento hasta las 18:00 hrs",
  "productos": [],
  "promociones": []
}
```

---

**happy_path** (HTTP 200)

Consulta exitosa del catálogo de productos de pollo

**Request Params:**
```json
[
  {
    "name": "categoria",
    "value": "pollo"
  },
  {
    "name": "tipoProducto",
    "value": "fresco"
  }
]
```

**Response Body:**
```json
{
  "productos": [
    {
      "id": "AGS-001",
      "nombre": "Pollo Entero Super Fresco",
      "categoria": "pollo",
      "precio": 3290,
      "unidad": "kg",
      "stock": 150,
      "descripcion": "Pollo entero fresco de primera calidad"
    },
    {
      "id": "AGS-002",
      "nombre": "Pechuga de Pollo Deshuesada",
      "categoria": "pollo",
      "precio": 5890,
      "unidad": "kg",
      "stock": 85,
      "descripcion": "Pechuga de pollo sin hueso ni piel"
    }
  ],
  "promociones": [
    {
      "tipo": "descuento",
      "descripcion": "15% descuento en compras sobre $50.000",
      "vigencia": "2024-01-31"
    }
  ]
}
```

---

#### Web Action: `consultarPedidoProgramado`

**alt_happy_cliente_frecuente** (HTTP 200)

Cliente frecuente con múltiples pedidos programados

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-78901"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-02-01"
  }
]
```

**Response Body:**
```json
{
  "historialPedidos": [
    {
      "numero": "PED-890123",
      "fecha": "2024-01-15",
      "total": 89500,
      "estado": "entregado"
    },
    {
      "numero": "PED-890124",
      "fecha": "2024-01-22",
      "total": 67800,
      "estado": "entregado"
    }
  ],
  "pedidoActual": {
    "numero": "PED-890125",
    "productos": [
      "AGS-201:2kg",
      "AGS-150:1kg"
    ],
    "total": 24980,
    "estado": "confirmado"
  },
  "proximaEntrega": {
    "fecha": "2024-02-08",
    "horaEstimada": "10:30",
    "direccion": "Las Condes, Santiago"
  }
}
```

---

**alt_happy_distribuidor_regional** (HTTP 200)

Consulta exitosa para distribuidor regional con múltiples productos programados

> **Tester prompt:** Dile al agente que eres el cliente CLI-85432 y consulta tu pedido programado para el 15 de febrero.

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-85432"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-02-15"
  }
]
```

**Response Body:**
```json
{
  "fechaEntrega": "2024-02-15",
  "pedidoActual": "AGS-450:25kg Lomo Vetado Premium, AGS-301:15kg Pechuga Pollo Orgánica, AGS-127:8kg Costillar Cerdo",
  "totalAproximado": "$487.650"
}
```

---

**edge_case** (HTTP 200)

Cliente sin pedidos programados

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-54321"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-01-27"
  }
]
```

**Response Body:**
```json
{
  "historialPedidos": [],
  "pedidoActual": null,
  "proximaEntrega": null,
  "mensaje": "El cliente no tiene pedidos programados"
}
```

---

**edge_cliente_nuevo_sin_historial** (HTTP 200)

Cliente nuevo sin historial de pedidos

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-99998"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-02-10"
  }
]
```

**Response Body:**
```json
{
  "historialPedidos": [],
  "pedidoActual": null,
  "proximaEntrega": null,
  "mensaje": "Cliente nuevo - sin historial de pedidos",
  "beneficiosNuevoCliente": [
    {
      "tipo": "descuento",
      "valor": "10%",
      "vigencia": "primer pedido"
    }
  ]
}
```

---

**edge_pedido_vacio_fecha_limite** (HTTP 200)

Cliente sin pedidos en fecha límite de programación

> **Tester prompt:** Consulta como cliente CLI-13579 tu pedido para el 31 de marzo.

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-13579"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-03-31"
  }
]
```

**Response Body:**
```json
{
  "fechaEntrega": "2024-03-31",
  "pedidoActual": "",
  "totalAproximado": "$0"
}
```

---

**error_case** (HTTP 404)

Cliente no encontrado en el sistema

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-99999"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-01-26"
  }
]
```

**Response Body:**
```json
{
  "error": "CLIENTE_NO_ENCONTRADO",
  "mensaje": "El cliente con ID 'CLI-99999' no existe en nuestro sistema",
  "codigo_error": "404_CLIENT_NOT_FOUND"
}
```

---

**error_cliente_suspendido** (HTTP 403)

Cliente con cuenta suspendida por pagos pendientes

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-00001"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-02-03"
  }
]
```

**Response Body:**
```json
{
  "error": true,
  "codigo": "CLIENTE_SUSPENDIDO",
  "mensaje": "Cliente suspendido por pagos pendientes",
  "montoPendiente": 125900,
  "contacto": "+56 2 2820 8000"
}
```

---

**error_fecha_formato_invalido** (HTTP 400)

Error por formato de fecha incorrecto

> **Tester prompt:** Pregunta por tu pedido como cliente CLI-76543 para la fecha 15/02/2024.

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-76543"
  },
  {
    "name": "fechaEntrega",
    "value": "15/02/2024"
  }
]
```

**Response Body:**
```json
{
  "error": true,
  "codigo": "FECHA_INVALIDA",
  "mensaje": "Formato de fecha debe ser YYYY-MM-DD",
  "fechaEntrega": null,
  "pedidoActual": null,
  "totalAproximado": null
}
```

---

**happy_path** (HTTP 200)

Consulta exitosa de pedidos programados del cliente

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-12345"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-01-25"
  }
]
```

**Response Body:**
```json
{
  "historialPedidos": [
    {
      "numeroPedido": "PED-789012",
      "fecha": "2024-01-18",
      "total": 87500,
      "estado": "entregado"
    },
    {
      "numeroPedido": "PED-789013",
      "fecha": "2024-01-11",
      "total": 65200,
      "estado": "entregado"
    }
  ],
  "pedidoActual": {
    "numeroPedido": "PED-789014",
    "fecha": "2024-01-25",
    "productos": [
      {
        "nombre": "Pollo Entero Super Fresco",
        "cantidad": 5,
        "precio": 3290
      }
    ],
    "total": 16450,
    "estado": "programado"
  },
  "proximaEntrega": {
    "fecha": "2024-02-01",
    "horario": "09:00-12:00",
    "direccion": "Av. Providencia 1234, Santiago"
  }
}
```

---

#### Web Action: `registrarPedido`

**alt_happy_pedido_empresarial** (HTTP 200)

Registro exitoso de pedido empresarial con múltiples productos

> **Tester prompt:** Registra un pedido como cliente CLI-CORP001 con 50kg de AGS-450, 30kg de AGS-301, 20kg de AGS-127 y 15kg de AGS-205 para el 20 de febrero.

**Request Body:**
```json
{
  "clienteId": "CLI-CORP001",
  "productos": "AGS-450:50kg,AGS-301:30kg,AGS-127:20kg,AGS-205:15kg",
  "fechaEntrega": "2024-02-20"
}
```

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-CORP001"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-02-20"
  },
  {
    "name": "productos",
    "value": "AGS-450:50kg,AGS-301:30kg,AGS-127:20kg,AGS-205:15kg"
  }
]
```

**Response Body:**
```json
{
  "numeroOrden": "ORD-240220-8847",
  "pedidoConfirmado": true,
  "totalFinal": "$1.847.320"
}
```

---

**edge_pedido_cantidad_fraccionaria_minima** (HTTP 200)

Registro con cantidad fraccionaria mínima permitida

> **Tester prompt:** Registra un pedido mínimo como cliente CLI-24680 con solo 0.05kg de AGS-105 para el 14 de febrero.

**Request Body:**
```json
{
  "clienteId": "CLI-24680",
  "productos": "AGS-105:0.05kg",
  "fechaEntrega": "2024-02-14"
}
```

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-24680"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-02-14"
  },
  {
    "name": "productos",
    "value": "AGS-105:0.05kg"
  }
]
```

**Response Body:**
```json
{
  "numeroOrden": "ORD-240214-0051",
  "pedidoConfirmado": true,
  "totalFinal": "$425"
}
```

---

**error_producto_descontinuado** (HTTP 422)

Error por incluir producto descontinuado en el pedido

> **Tester prompt:** Intenta registrar un pedido como cliente CLI-55667 con 2kg de AGS-999 y 1kg de AGS-301 para el 18 de febrero.

**Request Body:**
```json
{
  "clienteId": "CLI-55667",
  "productos": "AGS-999:2kg,AGS-301:1kg",
  "fechaEntrega": "2024-02-18"
}
```

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI-55667"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-02-18"
  },
  {
    "name": "productos",
    "value": "AGS-999:2kg,AGS-301:1kg"
  }
]
```

**Response Body:**
```json
{
  "error": true,
  "codigo": "PRODUCTO_DESCONTINUADO",
  "mensaje": "El producto AGS-999 ha sido descontinuado",
  "numeroOrden": null,
  "pedidoConfirmado": false,
  "totalFinal": null
}
```

---

### Dataset: Generated (2026-02-27 00:46)

AI-generated mock data for Agente Comercial Agrosuper

_Source: generated_

#### Web Action: `calcularTotalPedido`

**edge_case** (HTTP 200)

Pedido con cantidad cero

**Request Body:**
```json
{
  "productos": "AGS001",
  "cantidades": "0"
}
```

**Request Params:**
```json
[
  {
    "name": "cantidades",
    "value": "0"
  },
  {
    "name": "productos",
    "value": "AGS001"
  }
]
```

**Response Body:**
```json
{
  "detalleProductos": [],
  "sugerenciaVenta": [
    {
      "id": "AGS001",
      "nombre": "Pechuga de Pollo Premium",
      "precio": 4590,
      "razon": "Producto popular en tu zona"
    }
  ],
  "totalAproximado": {
    "subtotal": 0,
    "descuentos": 0,
    "iva": 0,
    "total": 0
  }
}
```

---

**error_case** (HTTP 400)

Producto no disponible en inventario

**Request Body:**
```json
{
  "productos": "AGS999,AGS001",
  "cantidades": "5,2"
}
```

**Request Params:**
```json
[
  {
    "name": "cantidades",
    "value": "5,2"
  },
  {
    "name": "productos",
    "value": "AGS999,AGS001"
  }
]
```

**Response Body:**
```json
{
  "error": true,
  "mensaje": "Producto AGS999 no encontrado en catálogo",
  "codigo": "PRODUCT_NOT_FOUND",
  "detalleProductos": [],
  "sugerenciaVenta": [],
  "totalAproximado": 0
}
```

---

**happy_path** (HTTP 200)

Cálculo exitoso de pedido con múltiples productos

**Request Body:**
```json
{
  "productos": "AGS001,AGS002,AGS003",
  "cantidades": "2,3,1"
}
```

**Request Params:**
```json
[
  {
    "name": "cantidades",
    "value": "2,3,1"
  },
  {
    "name": "productos",
    "value": "AGS001,AGS002,AGS003"
  }
]
```

**Response Body:**
```json
{
  "detalleProductos": [
    {
      "id": "AGS001",
      "nombre": "Pechuga de Pollo Premium",
      "precio": 4590,
      "cantidad": 2,
      "subtotal": 9180
    },
    {
      "id": "AGS002",
      "nombre": "Muslos de Pollo",
      "precio": 2890,
      "cantidad": 3,
      "subtotal": 8670
    },
    {
      "id": "AGS003",
      "nombre": "Pollo Entero",
      "precio": 3200,
      "cantidad": 1,
      "subtotal": 3200
    }
  ],
  "sugerenciaVenta": [
    {
      "id": "AGS004",
      "nombre": "Alitas de Pollo",
      "precio": 2490,
      "razon": "Complementa perfectamente con tu selección"
    }
  ],
  "totalAproximado": {
    "subtotal": 21050,
    "descuentos": 0,
    "iva": 3999,
    "total": 25049
  }
}
```

---

#### Web Action: `confirmarPedido`

**edge_case** (HTTP 201)

Pedido confirmado con monto mínimo

**Request Body:**
```json
{
  "clienteId": "CLI11111",
  "pedidoCompleto": "{\"productos\": [{\"id\": \"AGS003\", \"cantidad\": 1}], \"total\": 3200}",
  "fechaEntrega": "2024-01-22"
}
```

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI11111"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-01-22"
  },
  {
    "name": "pedidoCompleto",
    "value": "{\"productos\": [{\"id\": \"AGS003\", \"cantidad\": 1}], \"total\": 3200}"
  }
]
```

**Response Body:**
```json
{
  "fechaConfirmacion": "2024-01-12T16:45:00Z",
  "numeroPedido": "PED001346",
  "resumenPedido": {
    "cliente": "CLI11111",
    "productos": [
      {
        "id": "AGS003",
        "nombre": "Pollo Entero",
        "cantidad": 1,
        "precio": 3200,
        "subtotal": 3200
      }
    ],
    "total": 3200,
    "fechaEntrega": "2024-01-22",
    "estado": "confirmado",
    "metodoPago": "efectivo",
    "advertencia": "Pedido bajo monto mínimo recomendado de $15.000"
  }
}
```

---

**error_case** (HTTP 409)

Error por stock insuficiente al confirmar

**Request Body:**
```json
{
  "clienteId": "CLI67890",
  "pedidoCompleto": "{\"productos\": [{\"id\": \"AGS001\", \"cantidad\": 50}], \"total\": 229500}",
  "fechaEntrega": "2024-01-19"
}
```

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI67890"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-01-19"
  },
  {
    "name": "pedidoCompleto",
    "value": "{\"productos\": [{\"id\": \"AGS001\", \"cantidad\": 50}], \"total\": 229500}"
  }
]
```

**Response Body:**
```json
{
  "error": true,
  "mensaje": "Stock insuficiente para producto AGS001. Stock disponible: 15 kg",
  "codigo": "INSUFFICIENT_STOCK",
  "fechaConfirmacion": null,
  "numeroPedido": null,
  "resumenPedido": null
}
```

---

**happy_path** (HTTP 201)

Confirmación exitosa de pedido completo

**Request Body:**
```json
{
  "clienteId": "CLI12345",
  "pedidoCompleto": "{\"productos\": [{\"id\": \"AGS001\", \"cantidad\": 2}, {\"id\": \"AGS002\", \"cantidad\": 3}], \"total\": 17850}",
  "fechaEntrega": "2024-01-18"
}
```

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI12345"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-01-18"
  },
  {
    "name": "pedidoCompleto",
    "value": "{\"productos\": [{\"id\": \"AGS001\", \"cantidad\": 2}, {\"id\": \"AGS002\", \"cantidad\": 3}], \"total\": 17850}"
  }
]
```

**Response Body:**
```json
{
  "fechaConfirmacion": "2024-01-12T14:30:00Z",
  "numeroPedido": "PED001345",
  "resumenPedido": {
    "cliente": "CLI12345",
    "productos": [
      {
        "id": "AGS001",
        "nombre": "Pechuga de Pollo Premium",
        "cantidad": 2,
        "precio": 4590,
        "subtotal": 9180
      },
      {
        "id": "AGS002",
        "nombre": "Muslos de Pollo",
        "cantidad": 3,
        "precio": 2890,
        "subtotal": 8670
      }
    ],
    "total": 17850,
    "fechaEntrega": "2024-01-18",
    "estado": "confirmado",
    "metodoPago": "transferencia"
  }
}
```

---

#### Web Action: `consultarCatalogoProductos`

**edge_case** (HTTP 200)

Consulta de categoría válida pero sin productos disponibles

**Request Params:**
```json
[
  {
    "name": "categoria",
    "value": "cerdo"
  },
  {
    "name": "tipoProducto",
    "value": "costillar"
  }
]
```

**Response Body:**
```json
{
  "productos": [],
  "promociones": [],
  "mensaje": "No hay productos disponibles en esta categoría temporalmente"
}
```

---

**error_case** (HTTP 404)

Categoría no encontrada en el catálogo

**Request Params:**
```json
[
  {
    "name": "categoria",
    "value": "mariscos"
  },
  {
    "name": "tipoProducto",
    "value": "salmon"
  }
]
```

**Response Body:**
```json
{
  "error": true,
  "mensaje": "Categoría 'mariscos' no disponible en nuestro catálogo",
  "codigo": "CAT_NOT_FOUND",
  "productos": [],
  "promociones": []
}
```

---

**happy_path** (HTTP 200)

Consulta exitosa del catálogo de productos cárnicos

**Request Params:**
```json
[
  {
    "name": "categoria",
    "value": "carnes"
  },
  {
    "name": "tipoProducto",
    "value": "pollo"
  }
]
```

**Response Body:**
```json
{
  "productos": [
    {
      "id": "AGS001",
      "nombre": "Pechuga de Pollo Premium",
      "precio": 4590,
      "unidad": "kg",
      "disponibilidad": true,
      "descripcion": "Pechuga de pollo fresca, sin hueso"
    },
    {
      "id": "AGS002",
      "nombre": "Muslos de Pollo",
      "precio": 2890,
      "unidad": "kg",
      "disponibilidad": true,
      "descripcion": "Muslos de pollo frescos con hueso"
    },
    {
      "id": "AGS003",
      "nombre": "Pollo Entero",
      "precio": 3200,
      "unidad": "kg",
      "disponibilidad": true,
      "descripcion": "Pollo entero fresco, peso promedio 2.5kg"
    }
  ],
  "promociones": [
    {
      "id": "PROMO001",
      "descripcion": "20% descuento en compras sobre $50.000",
      "vigencia": "2024-12-31",
      "aplicable": [
        "AGS001",
        "AGS002"
      ]
    }
  ]
}
```

---

#### Web Action: `consultarPedidoProgramado`

**edge_case** (HTTP 200)

Cliente nuevo sin historial de pedidos

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI00001"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-01-25"
  }
]
```

**Response Body:**
```json
{
  "historialPedidos": [],
  "pedidoActual": null,
  "proximaEntrega": null,
  "mensaje": "Cliente nuevo sin pedidos previos"
}
```

---

**error_case** (HTTP 404)

Cliente no encontrado en el sistema

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI99999"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-01-20"
  }
]
```

**Response Body:**
```json
{
  "error": true,
  "mensaje": "Cliente no encontrado en el sistema",
  "codigo": "CLIENT_NOT_FOUND",
  "historialPedidos": [],
  "pedidoActual": null,
  "proximaEntrega": null
}
```

---

**happy_path** (HTTP 200)

Consulta exitosa de pedido programado de cliente activo

**Request Params:**
```json
[
  {
    "name": "clienteId",
    "value": "CLI12345"
  },
  {
    "name": "fechaEntrega",
    "value": "2024-01-15"
  }
]
```

**Response Body:**
```json
{
  "historialPedidos": [
    {
      "numeroPedido": "PED001234",
      "fecha": "2024-01-08",
      "total": 45600,
      "estado": "entregado"
    },
    {
      "numeroPedido": "PED001198",
      "fecha": "2024-01-01",
      "total": 38900,
      "estado": "entregado"
    }
  ],
  "pedidoActual": {
    "numeroPedido": "PED001267",
    "fechaCreacion": "2024-01-12",
    "productos": [
      {
        "id": "AGS001",
        "nombre": "Pechuga de Pollo Premium",
        "cantidad": 3,
        "precio": 4590
      }
    ],
    "total": 13770,
    "estado": "confirmado"
  },
  "proximaEntrega": {
    "fecha": "2024-01-15",
    "horaEstimada": "10:00-12:00",
    "direccion": "Av. Las Condes 1234, Las Condes, Santiago"
  }
}
```

---

## Agent: Agente de Verificación de Cumplimiento Regulatorio

### Dataset: Generated (2026-03-01 23:11) **(Active)**

AI-generated mock data for AgenteComplianceRegulatorio

_Source: generated_

#### Web Action: `checkRiskCompatibility`

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

#### Web Action: `getCustomerProfile`

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

#### Web Action: `validateSBSCompliance`

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

### Dataset: Generated (2026-02-28 04:12)

AI-generated mock data for AgenteComplianceRegulatorio

_Source: generated_

#### Web Action: `consultarLimitesInversion`

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

#### Web Action: `consultarPerfilCliente`

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

#### Web Action: `verificarRegulacionesProducto`

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

### Dataset: Generated (2026-02-28 15:16)

AI-generated mock data for AgenteComplianceRegulatorio

_Source: generated_

#### Web Action: `checkRegulatoryDatabase`

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

#### Web Action: `getCustomerProfile`

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

#### Web Action: `validateSbsCompliance`

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

### Dataset: Generated (2026-03-01 14:56)

AI-generated mock data for AgenteComplianceRegulatorio

_Source: generated_

#### Web Action: `checkRiskCompatibility`

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

#### Web Action: `getCustomerProfile`

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

#### Web Action: `validateSBSCompliance`

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

## Agent: Evaluador de Préstamos Personales Interbank

### Dataset: Generated (2026-03-19 03:53) **(Active)**

AI-generated mock data for Agente de Elegibilidad de Préstamos Interbank

_Source: generated_

#### Web Action: `calcularCapacidadPago`

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

#### Web Action: `consultarClienteExistente`

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

#### Web Action: `consultarProductosPrestamos`

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

#### Web Action: `verificarPrestamoPreaprobado`

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

### Dataset: Generated (2026-02-27 22:23)

AI-generated mock data for Agente de Elegibilidad de Préstamos Interbank

_Source: generated_

#### Web Action: `calcularCapacidadPago`

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

#### Web Action: `consultarClienteExistente`

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

#### Web Action: `consultarProductosPrestamos`

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

#### Web Action: `verificarPrestamoPreaprobado`

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

### Dataset: Generated (2026-03-19 02:07)

AI-generated mock data for Agente de Elegibilidad de Préstamos Interbank

_Source: generated_

#### Web Action: `calcularCapacidadPago`

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

#### Web Action: `consultarClienteExistente`

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

#### Web Action: `consultarProductosPrestamos`

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

#### Web Action: `verificarPrestamoPreaprobado`

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

### Dataset: Generated (2026-03-19 03:34)

AI-generated mock data for Agente de Elegibilidad de Préstamos Interbank

_Source: generated_

#### Web Action: `calcularCapacidadPago`

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

#### Web Action: `consultarClienteExistente`

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

#### Web Action: `consultarProductosPrestamos`

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

#### Web Action: `verificarPrestamoPreaprobado`

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

### Dataset: Generated (2026-03-19 03:41)

AI-generated mock data for Agente de Elegibilidad de Préstamos Interbank

_Source: generated_

#### Web Action: `calcularCapacidadPago`

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

#### Web Action: `consultarClienteExistente`

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

#### Web Action: `consultarProductosPrestamos`

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

#### Web Action: `verificarPrestamoPreaprobado`

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

### Dataset: Generated (2026-03-19 03:47)

AI-generated mock data for Agente de Elegibilidad de Préstamos Interbank

_Source: generated_

#### Web Action: `calcularCapacidadPago`

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

#### Web Action: `consultarClienteExistente`

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

#### Web Action: `consultarProductosPrestamos`

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

#### Web Action: `verificarPrestamoPreaprobado`

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

## Agent: Agente de Incorporación de Distribuidores Retail&Co

### Dataset: Generated (2026-03-26 03:07) **(Active)**

AI-generated mock data for Retail&Co Distributor Onboarding Assistant

_Source: generated_

#### Web Action: `consultarCatalogo`

**edge_case** (HTTP 200)

Consulta sin especificar categoría ni región

> **Tester prompt:** Pide ver el catálogo general sin especificar categoría ni región.

**Request Params:**
```json
[
  {
    "name": "categoria",
    "value": ""
  },
  {
    "name": "region",
    "value": ""
  }
]
```

**Response Body:**
```json
{
  "productos": [],
  "marcas": [
    "Coca-Cola",
    "Bimbo",
    "Lala",
    "Sabritas",
    "Nestlé"
  ],
  "condicionesDistribucion": {
    "mensaje": "Catálogo general disponible",
    "categorias": [
      "Bebidas",
      "Snacks",
      "Lácteos",
      "Panadería",
      "Limpieza",
      "Cuidado Personal"
    ],
    "regiones": [
      "CDMX",
      "Guadalajara",
      "Monterrey",
      "Puebla",
      "Tijuana",
      "León"
    ],
    "contacto": "Especifica categoría y región para ver productos y precios"
  }
}
```

---

**error_case** (HTTP 404)

Error por región no atendida

> **Tester prompt:** Consulta sobre productos electrónicos disponibles en Yucatán.

**Request Params:**
```json
[
  {
    "name": "categoria",
    "value": "Electrónicos"
  },
  {
    "name": "region",
    "value": "Yucatán"
  }
]
```

**Response Body:**
```json
{
  "productos": [],
  "marcas": [],
  "condicionesDistribucion": {
    "error": "La categoría Electrónicos no está disponible en la región Yucatán",
    "regionesDisponibles": [
      "CDMX",
      "Guadalajara",
      "Monterrey",
      "Puebla"
    ],
    "categoriasDisponibles": [
      "Bebidas",
      "Snacks",
      "Lácteos",
      "Limpieza"
    ]
  }
}
```

---

**happy_path** (HTTP 200)

Consulta exitosa de catálogo de bebidas en CDMX

> **Tester prompt:** Pregunta al asistente qué productos de bebidas están disponibles en la Ciudad de México.

**Request Params:**
```json
[
  {
    "name": "categoria",
    "value": "Bebidas"
  },
  {
    "name": "region",
    "value": "CDMX"
  }
]
```

**Response Body:**
```json
{
  "productos": [
    {
      "nombre": "Coca-Cola 355ml",
      "codigo": "CC355",
      "precio": "$12.50",
      "disponibilidad": "En stock"
    },
    {
      "nombre": "Agua Bonafont 1L",
      "codigo": "BF1L",
      "precio": "$8.90",
      "disponibilidad": "En stock"
    },
    {
      "nombre": "Cerveza Corona 355ml",
      "codigo": "CR355",
      "precio": "$18.00",
      "disponibilidad": "Stock limitado"
    }
  ],
  "marcas": [
    "Coca-Cola",
    "Bonafont",
    "Corona",
    "Pepsi",
    "Sprite"
  ],
  "condicionesDistribucion": {
    "pedidoMinimo": "$5,000.00 MXN",
    "descuentoVolumen": "5-15% según cantidad",
    "plazoEntrega": "24-48 horas",
    "terminosPago": "30 días"
  }
}
```

---

**partial_stock** (HTTP 200)

Consulta con stock limitado y productos agotados

> **Tester prompt:** Pregunta sobre la disponibilidad de productos lácteos en Monterrey.

**Request Params:**
```json
[
  {
    "name": "categoria",
    "value": "Lácteos"
  },
  {
    "name": "region",
    "value": "Monterrey"
  }
]
```

**Response Body:**
```json
{
  "productos": [
    {
      "nombre": "Leche Lala Entera 1L",
      "codigo": "LAL1L",
      "precio": "$22.00",
      "disponibilidad": "Stock limitado"
    },
    {
      "nombre": "Yogurt Danone 125g",
      "codigo": "DAN125",
      "precio": "$8.50",
      "disponibilidad": "Agotado"
    },
    {
      "nombre": "Queso Oaxaca Philadelphia 400g",
      "codigo": "PHI400",
      "precio": "$65.00",
      "disponibilidad": "En stock"
    }
  ],
  "marcas": [
    "Lala",
    "Danone",
    "Philadelphia",
    "Alpura"
  ],
  "condicionesDistribucion": {
    "pedidoMinimo": "$4,000.00 MXN",
    "descuentoVolumen": "3-12% según cantidad",
    "plazoEntrega": "48-72 horas por refrigeración",
    "terminosPago": "20 días",
    "alerta": "Algunos productos con disponibilidad limitada por alta demanda"
  }
}
```

---

**snacks_guadalajara** (HTTP 200)

Catálogo completo de snacks para Guadalajara

> **Tester prompt:** Solicita información sobre snacks disponibles para distribución en Guadalajara.

**Request Params:**
```json
[
  {
    "name": "categoria",
    "value": "Snacks"
  },
  {
    "name": "region",
    "value": "Guadalajara"
  }
]
```

**Response Body:**
```json
{
  "productos": [
    {
      "nombre": "Sabritas Clásicas 45g",
      "codigo": "SAB45",
      "precio": "$14.00",
      "disponibilidad": "En stock"
    },
    {
      "nombre": "Doritos Nacho 62g",
      "codigo": "DOR62",
      "precio": "$19.50",
      "disponibilidad": "En stock"
    },
    {
      "nombre": "Cheetos Torciditos 35g",
      "codigo": "CHE35",
      "precio": "$12.00",
      "disponibilidad": "En stock"
    },
    {
      "nombre": "Takis Fuego 113g",
      "codigo": "TAK113",
      "precio": "$25.00",
      "disponibilidad": "Stock limitado"
    }
  ],
  "marcas": [
    "Sabritas",
    "Barcel",
    "Gamesa",
    "Marinela"
  ],
  "condicionesDistribucion": {
    "pedidoMinimo": "$3,000.00 MXN",
    "descuentoVolumen": "8-20% según cantidad",
    "plazoEntrega": "24 horas",
    "terminosPago": "15 días",
    "promocionEspecial": "Compra 10 cajas, lleva 1 gratis"
  }
}
```

---

#### Web Action: `consultarRequisitos`

**edge_case** (HTTP 200)

Consulta sin especificar parámetros

> **Tester prompt:** Pregunta sobre requisitos generales sin especificar tipo de negocio ni ubicación.

**Request Params:**
```json
[
  {
    "name": "tipoNegocio",
    "value": ""
  },
  {
    "name": "ubicacion",
    "value": ""
  }
]
```

**Response Body:**
```json
{
  "requisitosBasicos": [
    "Información general disponible",
    "RFC vigente",
    "Identificación oficial",
    "Comprobante de domicilio"
  ],
  "documentosNecesarios": [
    "Especificar tipo de negocio para lista detallada"
  ],
  "beneficios": [
    "Beneficios varían según tipo de negocio y ubicación",
    "Descuentos por volumen",
    "Soporte comercial",
    "Capacitación inicial"
  ],
  "terminosComerciales": {
    "mensaje": "Términos específicos disponibles al proporcionar tipo de negocio y ubicación",
    "contacto": "Llame al 800-RETAIL-0 para asesoría personalizada"
  }
}
```

---

**error_case** (HTTP 400)

Error por tipo de negocio no soportado

> **Tester prompt:** Consulta los requisitos para un casino en Las Vegas.

**Request Params:**
```json
[
  {
    "name": "tipoNegocio",
    "value": "Casino"
  },
  {
    "name": "ubicacion",
    "value": "Las Vegas"
  }
]
```

**Response Body:**
```json
{
  "requisitosBasicos": [],
  "documentosNecesarios": [],
  "beneficios": [],
  "terminosComerciales": {
    "error": "Tipo de negocio no soportado",
    "mensaje": "El tipo de negocio 'Casino' no está dentro de nuestros segmentos objetivo",
    "tiposPermitidos": [
      "Supermercado",
      "Abarrotes",
      "Farmacia",
      "Restaurante",
      "Conveniencia",
      "Mayorista"
    ],
    "ubicacionesPermitidas": [
      "CDMX",
      "Guadalajara",
      "Monterrey",
      "Puebla",
      "Tijuana",
      "León",
      "Querétaro"
    ]
  }
}
```

---

**happy_path** (HTTP 200)

Consulta exitosa de requisitos para supermercado en CDMX

> **Tester prompt:** Pregunta qué requisitos necesitas para ser distribuidor si tienes un supermercado en la Ciudad de México.

**Request Params:**
```json
[
  {
    "name": "tipoNegocio",
    "value": "Supermercado"
  },
  {
    "name": "ubicacion",
    "value": "CDMX"
  }
]
```

**Response Body:**
```json
{
  "requisitosBasicos": [
    "RFC activo y al corriente",
    "Comprobante de domicilio fiscal",
    "Acta constitutiva (personas morales)",
    "Identificación oficial del representante legal",
    "Comprobante de ingresos o estados financieros",
    "Referencias comerciales (mínimo 2)"
  ],
  "documentosNecesarios": [
    "Cédula de identificación fiscal",
    "Licencia de funcionamiento municipal",
    "Permiso de venta de bebidas alcohólicas (si aplica)",
    "Póliza de seguro de responsabilidad civil",
    "Contrato de arrendamiento o escritura del local"
  ],
  "beneficios": [
    "Descuentos por volumen hasta 20%",
    "Crédito comercial hasta 45 días",
    "Entrega gratuita en pedidos mayores a $10,000",
    "Representante comercial dedicado",
    "Promociones exclusivas mensuales",
    "Sistema de gestión de inventarios"
  ],
  "terminosComerciales": {
    "pedidoMinimo": "$15,000.00 MXN",
    "descuentoBase": "8%",
    "plazoCredito": "30-45 días",
    "garantia": "$50,000.00 MXN o aval"
  }
}
```

---

**restaurant_chain** (HTTP 200)

Requisitos para cadena de restaurantes

> **Tester prompt:** Pregunta sobre los requisitos para una cadena de restaurantes en Monterrey.

**Request Params:**
```json
[
  {
    "name": "tipoNegocio",
    "value": "Restaurante"
  },
  {
    "name": "ubicacion",
    "value": "Monterrey"
  }
]
```

**Response Body:**
```json
{
  "requisitosBasicos": [
    "RFC de persona moral activo",
    "Estados financieros auditados",
    "Licencias sanitarias vigentes",
    "Certificación en manejo de alimentos",
    "Pólizas de seguro actualizadas",
    "Experiencia comprobable en el sector"
  ],
  "documentosNecesarios": [
    "Licencia de funcionamiento COFEPRIS",
    "Permisos municipales de operación",
    "Certificados de calidad ISO (deseable)",
    "Contratos de locales o propiedades",
    "Organigrama y estructura corporativa"
  ],
  "beneficios": [
    "Descuentos especiales hasta 25%",
    "Crédito extendido hasta 60 días",
    "Gerente de cuenta especializado",
    "Productos exclusivos para restaurantes",
    "Entrega programada y logística especializada",
    "Programa de fidelidad con bonificaciones"
  ],
  "terminosComerciales": {
    "pedidoMinimo": "$25,000.00 MXN",
    "descuentoBase": "12%",
    "plazoCredito": "45-60 días",
    "garantia": "Línea de crédito evaluada según historial"
  }
}
```

---

**small_business** (HTTP 200)

Requisitos para negocio pequeño de abarrotes

> **Tester prompt:** Consulta los requisitos para una tienda de abarrotes pequeña en Puebla.

**Request Params:**
```json
[
  {
    "name": "tipoNegocio",
    "value": "Abarrotes"
  },
  {
    "name": "ubicacion",
    "value": "Puebla"
  }
]
```

**Response Body:**
```json
{
  "requisitosBasicos": [
    "RFC con actividad comercial",
    "Identificación oficial vigente",
    "Comprobante de domicilio del negocio",
    "Experiencia mínima 6 meses en el ramo"
  ],
  "documentosNecesarios": [
    "Licencia municipal de funcionamiento",
    "Contrato de arrendamiento o propiedad",
    "Comprobante de ingresos o declaraciones",
    "Fotografías del establecimiento"
  ],
  "beneficios": [
    "Descuentos progresivos 3-12%",
    "Crédito inicial hasta 15 días",
    "Capacitación en manejo de productos",
    "Entrega sin costo en pedidos >$5,000",
    "Soporte telefónico dedicado"
  ],
  "terminosComerciales": {
    "pedidoMinimo": "$3,000.00 MXN",
    "descuentoBase": "5%",
    "plazoCredito": "15 días inicial, extensible a 30",
    "garantia": "Referencias comerciales o depósito $10,000"
  }
}
```

---

#### Web Action: `programarVisita`

**edge_case** (HTTP 200)

Programación en fecha límite (fin de semana)

> **Tester prompt:** Pide programar una visita para sábado muy temprano con tu registro DIST-MX-2024-001235.

**Request Body:**
```json
{
  "numeroRegistro": "DIST-MX-2024-001235",
  "fechaPreferida": "2024-03-16",
  "horarioPreferido": "08:00-09:00",
  "direccionVisita": "Zona muy lejana sin especificar"
}
```

**Request Params:**
```json
[
  {
    "name": "numeroRegistro",
    "value": "DIST-MX-2024-001235"
  },
  {
    "name": "fechaPreferida",
    "value": "2024-03-16"
  },
  {
    "name": "horarioPreferido",
    "value": "08:00-09:00"
  },
  {
    "name": "direccionVisita",
    "value": "Zona muy lejana sin especificar"
  }
]
```

**Response Body:**
```json
{
  "codigoVisita": "VIS-2024-ESP-0457",
  "fechaVisita": "2024-03-18",
  "horaVisita": "09:00",
  "representanteAsignado": {
    "nombre": "Lic. Miguel Hernández",
    "telefono": "55-9876-5432",
    "email": "miguel.hernandez@retailco.mx",
    "nota": "Visita reprogramada al lunes por disponibilidad de fin de semana",
    "especialidad": "Cobertura Nacional"
  }
}
```

---

**error_case** (HTTP 404)

Error por número de registro inválido

> **Tester prompt:** Intenta programar una visita con un número de registro incorrecto como DIST-FALSO-123.

**Request Body:**
```json
{
  "numeroRegistro": "DIST-FALSO-123",
  "fechaPreferida": "2024-03-20",
  "horarioPreferido": "14:00-16:00",
  "direccionVisita": "Calle Falsa 123, Ciudad Inexistente"
}
```

**Request Params:**
```json
[
  {
    "name": "numeroRegistro",
    "value": "DIST-FALSO-123"
  },
  {
    "name": "fechaPreferida",
    "value": "2024-03-20"
  },
  {
    "name": "horarioPreferido",
    "value": "14:00-16:00"
  },
  {
    "name": "direccionVisita",
    "value": "Calle Falsa 123, Ciudad Inexistente"
  }
]
```

**Response Body:**
```json
{
  "codigoVisita": null,
  "fechaVisita": null,
  "horaVisita": null,
  "representanteAsignado": {
    "error": "Número de registro no encontrado",
    "mensaje": "Verifique que el número de registro sea correcto",
    "contactoSoporte": "800-RETAIL-0 (800-738-2450)"
  }
}
```

---

**happy_path** (HTTP 200)

Programación exitosa de visita comercial

> **Tester prompt:** Solicita programar una visita para el 15 de marzo entre las 10 y 12 horas usando tu número de registro DIST-MX-2024-001234.

**Request Body:**
```json
{
  "numeroRegistro": "DIST-MX-2024-001234",
  "fechaPreferida": "2024-03-15",
  "horarioPreferido": "10:00-12:00",
  "direccionVisita": "Av. Constituyentes 1245, Col. Centro, Querétaro, Qro. CP 76000"
}
```

**Request Params:**
```json
[
  {
    "name": "numeroRegistro",
    "value": "DIST-MX-2024-001234"
  },
  {
    "name": "fechaPreferida",
    "value": "2024-03-15"
  },
  {
    "name": "horarioPreferido",
    "value": "10:00-12:00"
  },
  {
    "name": "direccionVisita",
    "value": "Av. Constituyentes 1245, Col. Centro, Querétaro, Qro. CP 76000"
  }
]
```

**Response Body:**
```json
{
  "codigoVisita": "VIS-2024-QRO-0456",
  "fechaVisita": "2024-03-15",
  "horaVisita": "10:30",
  "representanteAsignado": {
    "nombre": "Ing. Patricia Morales",
    "telefono": "442-123-4567",
    "email": "patricia.morales@retailco.mx",
    "especialidad": "Abarrotes y Conveniencia"
  }
}
```

---

**no_availability** (HTTP 200)

Sin disponibilidad en fechas solicitadas

> **Tester prompt:** Intenta programar una visita para el 24 de diciembre con tu registro DIST-MX-2024-001240.

**Request Body:**
```json
{
  "numeroRegistro": "DIST-MX-2024-001240",
  "fechaPreferida": "2024-12-24",
  "horarioPreferido": "09:00-11:00",
  "direccionVisita": "Av. Revolución 500, Col. San Pedro, Guadalajara, Jal. CP 44200"
}
```

**Request Params:**
```json
[
  {
    "name": "numeroRegistro",
    "value": "DIST-MX-2024-001240"
  },
  {
    "name": "fechaPreferida",
    "value": "2024-12-24"
  },
  {
    "name": "horarioPreferido",
    "value": "09:00-11:00"
  },
  {
    "name": "direccionVisita",
    "value": "Av. Revolución 500, Col. San Pedro, Guadalajara, Jal. CP 44200"
  }
]
```

**Response Body:**
```json
{
  "codigoVisita": "VIS-2024-ALT-0459",
  "fechaVisita": "2024-12-27",
  "horaVisita": "10:00",
  "representanteAsignado": {
    "nombre": "Lic. Carmen Vázquez",
    "telefono": "33-2468-1357",
    "email": "carmen.vazquez@retailco.mx",
    "mensaje": "Fecha original no disponible por temporada navideña",
    "fechasAlternativas": [
      "2024-12-27",
      "2024-12-30",
      "2025-01-02"
    ],
    "especialidad": "Zona Occidente"
  }
}
```

---

**urgent_visit** (HTTP 200)

Visita urgente para distribuidor premium

> **Tester prompt:** Solicita una visita urgente usando tu número premium DIST-MX-2024-PREM-001236 para mañana a cualquier hora.

**Request Body:**
```json
{
  "numeroRegistro": "DIST-MX-2024-PREM-001236",
  "fechaPreferida": "2024-03-12",
  "horarioPreferido": "URGENTE - Cualquier hora",
  "direccionVisita": "Blvd. Agua Caliente 12500, Zona Río, Tijuana, B.C. CP 22320"
}
```

**Request Params:**
```json
[
  {
    "name": "numeroRegistro",
    "value": "DIST-MX-2024-PREM-001236"
  },
  {
    "name": "fechaPreferida",
    "value": "2024-03-12"
  },
  {
    "name": "horarioPreferido",
    "value": "URGENTE - Cualquier hora"
  },
  {
    "name": "direccionVisita",
    "value": "Blvd. Agua Caliente 12500, Zona Río, Tijuana, B.C. CP 22320"
  }
]
```

**Response Body:**
```json
{
  "codigoVisita": "VIS-2024-URG-0458",
  "fechaVisita": "2024-03-12",
  "horaVisita": "16:00",
  "representanteAsignado": {
    "nombre": "Gerente Regional Alejandro Ruiz",
    "telefono": "664-555-0123",
    "email": "alejandro.ruiz@retailco.mx",
    "especialidad": "Cuentas Premium y Mayoristas",
    "nota": "Visita prioritaria confirmada para el mismo día"
  }
}
```

---

#### Web Action: `registrarDistribuidor`

**edge_case** (HTTP 200)

Registro con datos mínimos requeridos

> **Tester prompt:** Proporciona información muy básica: nombre J, empresa A, y di que no tienes información detallada aún.

**Request Body:**
```json
{
  "nombreDistribuidor": "J",
  "nombreEmpresa": "A",
  "categoriaNegocios": "Otro",
  "productosInteres": "N/A",
  "telefono": "1234567890",
  "categoriaNegocio": "Micro",
  "direccionCompleta": "Sin dirección específica",
  "horariosOperacion": "Variable"
}
```

**Request Params:**
```json
[
  {
    "name": "nombreDistribuidor",
    "value": "J"
  },
  {
    "name": "nombreEmpresa",
    "value": "A"
  },
  {
    "name": "categoriaNegocios",
    "value": "Otro"
  },
  {
    "name": "productosInteres",
    "value": "N/A"
  },
  {
    "name": "telefono",
    "value": "1234567890"
  },
  {
    "name": "categoriaNegocio",
    "value": "Micro"
  },
  {
    "name": "direccionCompleta",
    "value": "Sin dirección específica"
  },
  {
    "name": "horariosOperacion",
    "value": "Variable"
  }
]
```

**Response Body:**
```json
{
  "estatusRegistro": "PENDIENTE_REVISION",
  "numeroRegistro": "DIST-MX-2024-001235",
  "siguientesPasos": [
    "Completar información faltante",
    "Proporcionar documentos de identificación",
    "Especificar dirección comercial válida",
    "Contacto telefónico para verificación"
  ]
}
```

---

**error_case** (HTTP 409)

Error por empresa ya registrada previamente

> **Tester prompt:** Menciona que eres Carlos Mendoza de Super Mercados La Esperanza y quieres registrarte como distribuidor.

**Request Body:**
```json
{
  "nombreDistribuidor": "Carlos Mendoza",
  "nombreEmpresa": "Super Mercados La Esperanza",
  "categoriaNegocios": "Supermercado",
  "productosInteres": "Lácteos, Carnes Frías",
  "telefono": "5555123456",
  "categoriaNegocio": "Mayorista",
  "direccionCompleta": "Calz. de Tlalpan 3456, Col. Portales, CDMX CP 03300",
  "horariosOperacion": "Todos los días 6:00-23:00"
}
```

**Request Params:**
```json
[
  {
    "name": "nombreDistribuidor",
    "value": "Carlos Mendoza"
  },
  {
    "name": "nombreEmpresa",
    "value": "Super Mercados La Esperanza"
  },
  {
    "name": "categoriaNegocios",
    "value": "Supermercado"
  },
  {
    "name": "productosInteres",
    "value": "Lácteos, Carnes Frías"
  },
  {
    "name": "telefono",
    "value": "5555123456"
  },
  {
    "name": "categoriaNegocio",
    "value": "Mayorista"
  },
  {
    "name": "direccionCompleta",
    "value": "Calz. de Tlalpan 3456, Col. Portales, CDMX CP 03300"
  },
  {
    "name": "horariosOperacion",
    "value": "Todos los días 6:00-23:00"
  }
]
```

**Response Body:**
```json
{
  "estatusRegistro": "RECHAZADO",
  "numeroRegistro": null,
  "siguientesPasos": [
    "La empresa ya se encuentra registrada en nuestro sistema",
    "Contactar al representante asignado: Ana Gutiérrez (tel: 55-1234-5678)",
    "Verificar estatus de cuenta existente"
  ]
}
```

---

**happy_path** (HTTP 200)

Registro exitoso de distribuidor con datos completos

> **Tester prompt:** Dile al asistente que te llamas María Elena Rodríguez y quieres registrar tu empresa Comercializadora del Bajío para distribuir productos de abarrotes.

**Request Body:**
```json
{
  "nombreDistribuidor": "María Elena Rodríguez",
  "nombreEmpresa": "Comercializadora del Bajío SA de CV",
  "categoriaNegocios": "Abarrotes y Conveniencia",
  "productosInteres": "Bebidas, Snacks, Productos de Limpieza",
  "telefono": "4421234567",
  "categoriaNegocio": "Minorista",
  "direccionCompleta": "Av. Constituyentes 1245, Col. Centro, Querétaro, Qro. CP 76000",
  "horariosOperacion": "Lunes a Sábado 7:00-22:00, Domingo 8:00-20:00"
}
```

**Request Params:**
```json
[
  {
    "name": "nombreDistribuidor",
    "value": "María Elena Rodríguez"
  },
  {
    "name": "nombreEmpresa",
    "value": "Comercializadora del Bajío SA de CV"
  },
  {
    "name": "categoriaNegocios",
    "value": "Abarrotes y Conveniencia"
  },
  {
    "name": "productosInteres",
    "value": "Bebidas, Snacks, Productos de Limpieza"
  },
  {
    "name": "telefono",
    "value": "4421234567"
  },
  {
    "name": "categoriaNegocio",
    "value": "Minorista"
  },
  {
    "name": "direccionCompleta",
    "value": "Av. Constituyentes 1245, Col. Centro, Querétaro, Qro. CP 76000"
  },
  {
    "name": "horariosOperacion",
    "value": "Lunes a Sábado 7:00-22:00, Domingo 8:00-20:00"
  }
]
```

**Response Body:**
```json
{
  "estatusRegistro": "APROBADO",
  "numeroRegistro": "DIST-MX-2024-001234",
  "siguientesPasos": [
    "Validación de documentos en 24-48 horas",
    "Asignación de representante comercial",
    "Programar visita inicial",
    "Configuración de cuenta comercial"
  ]
}
```

---

**large_distributor** (HTTP 200)

Registro de distribuidor mayorista con múltiples categorías

> **Tester prompt:** Presenta tu empresa como Distribuidora Nacional del Pacífico, una operación mayorista grande con múltiples categorías de productos.

**Request Body:**
```json
{
  "nombreDistribuidor": "Roberto Alvarado Sánchez",
  "nombreEmpresa": "Distribuidora Nacional del Pacífico SA de CV",
  "categoriaNegocios": "Mayorista, Supermercado, Farmacia, Restaurante",
  "productosInteres": "Bebidas Alcohólicas, Bebidas No Alcohólicas, Lácteos, Productos Congelados, Medicamentos OTC, Cosméticos, Productos de Limpieza Industrial",
  "telefono": "6641987654",
  "categoriaNegocio": "Mayorista",
  "direccionCompleta": "Blvd. Agua Caliente 12500, Zona Río, Tijuana, B.C. CP 22320",
  "horariosOperacion": "24 horas, 365 días"
}
```

**Request Params:**
```json
[
  {
    "name": "nombreDistribuidor",
    "value": "Roberto Alvarado Sánchez"
  },
  {
    "name": "nombreEmpresa",
    "value": "Distribuidora Nacional del Pacífico SA de CV"
  },
  {
    "name": "categoriaNegocios",
    "value": "Mayorista, Supermercado, Farmacia, Restaurante"
  },
  {
    "name": "productosInteres",
    "value": "Bebidas Alcohólicas, Bebidas No Alcohólicas, Lácteos, Productos Congelados, Medicamentos OTC, Cosméticos, Productos de Limpieza Industrial"
  },
  {
    "name": "telefono",
    "value": "6641987654"
  },
  {
    "name": "categoriaNegocio",
    "value": "Mayorista"
  },
  {
    "name": "direccionCompleta",
    "value": "Blvd. Agua Caliente 12500, Zona Río, Tijuana, B.C. CP 22320"
  },
  {
    "name": "horariosOperacion",
    "value": "24 horas, 365 días"
  }
]
```

**Response Body:**
```json
{
  "estatusRegistro": "APROBADO_PREMIUM",
  "numeroRegistro": "DIST-MX-2024-PREM-001236",
  "siguientesPasos": [
    "Asignación de gerente de cuenta especializado",
    "Revisión de capacidad de almacenamiento",
    "Evaluación crediticia para términos preferenciales",
    "Programar reunión ejecutiva en 48 horas"
  ]
}
```

---

**validation_error** (HTTP 400)

Error por teléfono inválido

> **Tester prompt:** Registra tu tienda Mi Barrio pero proporciona un teléfono incorrecto como 123.

**Request Body:**
```json
{
  "nombreDistribuidor": "Ana Patricia López",
  "nombreEmpresa": "Tienda Mi Barrio",
  "categoriaNegocios": "Abarrotes",
  "productosInteres": "Productos Básicos",
  "telefono": "123",
  "categoriaNegocio": "Minorista",
  "direccionCompleta": "Calle Morelos 45, Col. Centro, Guadalajara, Jal. CP 44100",
  "horariosOperacion": "Lunes a Domingo 8:00-20:00"
}
```

**Request Params:**
```json
[
  {
    "name": "nombreDistribuidor",
    "value": "Ana Patricia López"
  },
  {
    "name": "nombreEmpresa",
    "value": "Tienda Mi Barrio"
  },
  {
    "name": "categoriaNegocios",
    "value": "Abarrotes"
  },
  {
    "name": "productosInteres",
    "value": "Productos Básicos"
  },
  {
    "name": "telefono",
    "value": "123"
  },
  {
    "name": "categoriaNegocio",
    "value": "Minorista"
  },
  {
    "name": "direccionCompleta",
    "value": "Calle Morelos 45, Col. Centro, Guadalajara, Jal. CP 44100"
  },
  {
    "name": "horariosOperacion",
    "value": "Lunes a Domingo 8:00-20:00"
  }
]
```

**Response Body:**
```json
{
  "estatusRegistro": "ERROR_VALIDACION",
  "numeroRegistro": null,
  "siguientesPasos": [
    "Proporcionar número telefónico válido de 10 dígitos",
    "Verificar formato: LADA + 7 u 8 dígitos",
    "Ejemplo: 3312345678 para Guadalajara"
  ]
}
```

---

## Agent: Agente de Gestión de Quejas Rappi

### Dataset: Generated (2026-03-30 02:19) **(Active)**

AI-generated mock data for Agente de Gestión de Quejas Rappi

_Source: generated_

#### Web Action: `analizarImagenDisputa`

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

#### Web Action: `clasificarInteraccionSocial`

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

#### Web Action: `consultarDetallesPedido`

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

#### Web Action: `crearTicketSoporte`

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
