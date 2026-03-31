# Testing Guide: Asistente de Citas Bancoppel

Test scenarios with full request/response examples.

## Dataset: Generated (2026-02-26 01:22) **(Active)**

AI-generated mock data for Asistente de Citas Bancoppel

_Source: generated_

### Web Action: `buscarSucursales`

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

### Web Action: `consultarHorarios`

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

### Web Action: `consultarServicios`

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

### Web Action: `reservarCita`

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
