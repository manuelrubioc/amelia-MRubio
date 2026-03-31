# Testing Guide: Agente de Incorporación de Distribuidores Retail&Co

Test scenarios with full request/response examples.

## Dataset: Generated (2026-03-26 03:07) **(Active)**

AI-generated mock data for Retail&Co Distributor Onboarding Assistant

_Source: generated_

### Web Action: `consultarCatalogo`

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

### Web Action: `consultarRequisitos`

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

### Web Action: `programarVisita`

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

### Web Action: `registrarDistribuidor`

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
