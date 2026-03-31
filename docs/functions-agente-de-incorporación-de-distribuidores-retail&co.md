# Functions & Web Actions: Agente de Incorporación de Distribuidores Retail&Co

## Functions

## registrarDistribuidor

Registra la información completa de un nuevo prospecto de distribuidor en el sistema

| Setting | Value |
|---------|-------|
| Action Type | `CONSUME_WS_ACTION` |
| WS Action | `registrarDistribuidor` |
| Requires Confirmation | No |

### Input Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `nombreDistribuidor` | STRING | Yes | Nombre completo del distribuidor |
| `nombreEmpresa` | STRING | Yes | Nombre de la empresa o negocio |
| `categoriaNegocio` | STRING | Yes | Tipo de negocio (supermercado, tienda de conveniencia, etc.) |
| `productosInteres` | STRING | Yes | Productos o categorías de interés |
| `telefono` | STRING | Yes | Número telefónico de contacto |
| `horariosOperacion` | STRING | Yes | Horarios de operación del negocio |
| `direccionCompleta` | STRING | Yes | Dirección completa incluyendo calle, número, colonia, ciudad, estado y CP |

### Output Parameters

| Name | Description |
|------|-------------|
| `numeroRegistro` | Número de registro asignado al distribuidor |
| `estatusRegistro` | Estado del registro en el sistema |
| `siguientesPasos` | Información sobre los próximos pasos del proceso |

---

## consultarCatalogo

Consulta el catálogo de productos disponibles por categoría para distribuidores

| Setting | Value |
|---------|-------|
| Action Type | `CONSUME_WS_ACTION` |
| WS Action | `consultarCatalogo` |
| Requires Confirmation | No |

### Input Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `categoria` | STRING | Yes | Categoría de productos a consultar |
| `region` | STRING | No | Región o estado donde opera el distribuidor |

### Output Parameters

| Name | Description |
|------|-------------|
| `productos` | Lista de productos disponibles en la categoría |
| `marcas` | Marcas disponibles en la categoría |
| `condicionesDistribucion` | Condiciones especiales de distribución |

---

## programarVisita

Programa una visita comercial en las instalaciones del distribuidor

| Setting | Value |
|---------|-------|
| Action Type | `CONSUME_WS_ACTION` |
| WS Action | `programarVisita` |
| Requires Confirmation | No |

### Input Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `numeroRegistro` | STRING | Yes | Número de registro del distribuidor |
| `fechaPreferida` | STRING | No | Fecha preferida para la visita |
| `horarioPreferido` | STRING | No | Horario preferido para la visita |
| `direccionVisita` | STRING | Yes | Dirección donde se realizará la visita |

### Output Parameters

| Name | Description |
|------|-------------|
| `fechaVisita` | Fecha confirmada de la visita |
| `horaVisita` | Hora confirmada de la visita |
| `representanteAsignado` | Nombre del representante comercial asignado |
| `codigoVisita` | Código de referencia de la visita |

---

## consultarRequisitos

Consulta los requisitos y términos para convertirse en distribuidor de Retail&Co

| Setting | Value |
|---------|-------|
| Action Type | `CONSUME_WS_ACTION` |
| WS Action | `consultarRequisitos` |
| Requires Confirmation | No |

### Input Parameters

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `tipoNegocio` | STRING | Yes | Tipo de negocio del prospecto |
| `ubicacion` | STRING | No | Ubicación del negocio |

### Output Parameters

| Name | Description |
|------|-------------|
| `requisitosBasicos` | Lista de requisitos básicos para ser distribuidor |
| `documentosNecesarios` | Documentos necesarios para el proceso |
| `beneficios` | Beneficios de ser distribuidor de Retail&Co |
| `terminosComerciales` | Términos comerciales generales |

---

## Web Actions

### `POST` registrarDistribuidor

**URL:** `https://aa108495-09cb-42f4-aab2-0d6260466acc.mock.pstmn.io/registrarDistribuidor`

**Request Body:**
```json
{"nombreDistribuidor": "${nombreDistribuidor}", "nombreEmpresa": "${nombreEmpresa}", "categoriaNegocio": "${categoriaNegocio}", "productosInteres": "${productosInteres}", "telefono": "${telefono}", "horariosOperacion": "${horariosOperacion}", "direccionCompleta": "${direccionCompleta}"}
```

**Request Params:**
```json
[
  {
    "name": "nombreDistribuidor",
    "value": "{{nombreDistribuidor}}",
    "secure": false
  },
  {
    "name": "nombreEmpresa",
    "value": "{{nombreEmpresa}}",
    "secure": false
  },
  {
    "name": "categoriaNegocio",
    "value": "{{categoriaNegocio}}",
    "secure": false
  },
  {
    "name": "productosInteres",
    "value": "{{productosInteres}}",
    "secure": false
  },
  {
    "name": "telefono",
    "value": "{{telefono}}",
    "secure": false
  },
  {
    "name": "horariosOperacion",
    "value": "{{horariosOperacion}}",
    "secure": false
  },
  {
    "name": "direccionCompleta",
    "value": "{{direccionCompleta}}",
    "secure": false
  }
]
```

**Response Mappings:**
```json
{
  "OK": [
    {
      "variableName": "numeroRegistro",
      "extractionExpression": "$.numeroRegistro"
    },
    {
      "variableName": "estatusRegistro",
      "extractionExpression": "$.estatusRegistro"
    },
    {
      "variableName": "siguientesPasos",
      "extractionExpression": "$.siguientesPasos"
    }
  ]
}
```

---

### `GET` consultarCatalogo

**URL:** `https://aa108495-09cb-42f4-aab2-0d6260466acc.mock.pstmn.io/consultarCatalogo`

**Request Params:**
```json
[
  {
    "name": "categoria",
    "value": "{{categoria}}",
    "secure": false
  },
  {
    "name": "region",
    "value": "{{region}}",
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
      "variableName": "marcas",
      "extractionExpression": "$.marcas"
    },
    {
      "variableName": "condicionesDistribucion",
      "extractionExpression": "$.condicionesDistribucion"
    }
  ]
}
```

---

### `POST` programarVisita

**URL:** `https://aa108495-09cb-42f4-aab2-0d6260466acc.mock.pstmn.io/programarVisita`

**Request Body:**
```json
{"numeroRegistro": "${numeroRegistro}", "fechaPreferida": "${fechaPreferida}", "horarioPreferido": "${horarioPreferido}", "direccionVisita": "${direccionVisita}"}
```

**Request Params:**
```json
[
  {
    "name": "numeroRegistro",
    "value": "{{numeroRegistro}}",
    "secure": false
  },
  {
    "name": "fechaPreferida",
    "value": "{{fechaPreferida}}",
    "secure": false
  },
  {
    "name": "horarioPreferido",
    "value": "{{horarioPreferido}}",
    "secure": false
  },
  {
    "name": "direccionVisita",
    "value": "{{direccionVisita}}",
    "secure": false
  }
]
```

**Response Mappings:**
```json
{
  "OK": [
    {
      "variableName": "fechaVisita",
      "extractionExpression": "$.fechaVisita"
    },
    {
      "variableName": "horaVisita",
      "extractionExpression": "$.horaVisita"
    },
    {
      "variableName": "representanteAsignado",
      "extractionExpression": "$.representanteAsignado"
    },
    {
      "variableName": "codigoVisita",
      "extractionExpression": "$.codigoVisita"
    }
  ]
}
```

---

### `GET` consultarRequisitos

**URL:** `https://aa108495-09cb-42f4-aab2-0d6260466acc.mock.pstmn.io/consultarRequisitos`

**Request Params:**
```json
[
  {
    "name": "tipoNegocio",
    "value": "{{tipoNegocio}}",
    "secure": false
  },
  {
    "name": "ubicacion",
    "value": "{{ubicacion}}",
    "secure": false
  }
]
```

**Response Mappings:**
```json
{
  "OK": [
    {
      "variableName": "requisitosBasicos",
      "extractionExpression": "$.requisitosBasicos"
    },
    {
      "variableName": "documentosNecesarios",
      "extractionExpression": "$.documentosNecesarios"
    },
    {
      "variableName": "beneficios",
      "extractionExpression": "$.beneficios"
    },
    {
      "variableName": "terminosComerciales",
      "extractionExpression": "$.terminosComerciales"
    }
  ]
}
```

---
