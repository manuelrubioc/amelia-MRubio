# Agent: Agente de Verificación de Cumplimiento Regulatorio

Agente autónomo que ejecuta verificaciones de cumplimiento y regulatorias para productos financieros, proporcionando clasificaciones y recomendaciones basadas en perfiles de cliente y normativas SBS del Perú

## Metadata

| Field | Value |
|-------|-------|
| Type | experimental |
| Status | DEPLOYED |
| Execution Mode | ambient |
| Domain | mrubio |
| Languages | es-PE |
| Amelia Deploy Status | DEPLOYED |

## Forge Context

| Field | Value |
|-------|-------|
| Industry | banking |
| Country | pe |
| Use Case | compliance_check |
| Channels | webchat |
| Company | Interbank |
| Mode | ambient |

a compliance an regulatory check of an existing customer for privite banking 

## Ambient Execution

**Objective:** go through the compliance & regulatory check and provide a classification back 

**Main Flow:** `complianceCheckFlow`
**Timeout:** 60s

**Input Schema:**
```json
[{"key":"customer_id","label":"Id del cliente","type":"STRING","required":true},{"key":"product_description","label":"description of the product","type":"STRING","required":true},{"key":"total_investment_amount","label":"the amount to invest ","type":"STRING","required":true}]
```

**Output Schema:**

provide the recomendation and findings for the customer on the product 

## Deployment

**Deployed at:** 2026-03-01T15:38:23.603393
**Deploy Version:** 28

{"forge_id": 6, "environment": "us.demo1.amelia", "domain_code": "mrubio", "agent_imported": true, "functions_imported": 1, "flows_imported": 1, "web_actions_created": 0, "web_actions_updated": 0, "functions_deleted": 0, "web_actions_deleted": 0, "warnings": ["Could not deploy BPN for 'complianceCheckFlow': Amelia API 400: This model cannot be updated because it was generated from a Conversation Flow.", "BPN 'complianceCheckFlow' validation passed but status is 'NEW' (not DEPLOYED) \u2014 may take a moment to activate"], "errors": ["Could not resolve parentPathId for web actions. Detail: Amelia API 400: Conversation flow folder 'Interbank_AgenteComplianceRegulatorio' already exists."]}

## Instruction

Este agente ejecuta de forma autónoma verificaciones completas de cumplimiento regulatorio para productos de inversión en Interbank. Recibe parámetros del cliente (ID, descripción del producto, monto de inversión) y ejecuta:

1. Recuperación del perfil completo del cliente desde sistemas internos
2. Validación de cumplimiento con normativas SBS peruanas
3. Verificación de compatibilidad de riesgo entre cliente y producto
4. Análisis cognitivo de factores regulatorios y de riesgo
5. Generación de recomendación final con hallazgos detallados

El agente proporciona una clasificación estructurada (APROBADO/RECHAZADO/CONDICIONAL) junto con justificaciones técnicas, recomendaciones específicas y requisitos adicionales cuando aplique. Toda la evaluación se basa en normativas bancarias peruanas vigentes y políticas internas de Interbank.

## Process Instructions (SOPs)

### Flujo de Verificación de Cumplimiento

1. Recibir parámetros de entrada (ID cliente, descripción producto, monto inversión)
2. Obtener perfil completo del cliente
3. Validar cumplimiento normativo SBS
4. Verificar compatibilidad de riesgo
5. Analizar factores regulatorios con IA
6. Generar recomendación final estructurada

## Post Processes

### Channel: default *(default)*

Formatear el resultado como datos estructurados con la recomendación de cumplimiento, clasificación de riesgo y hallazgos regulatorios detallados

## Functions

| Function | Status | Action Type | Description |
|----------|--------|-------------|-------------|
| `executeComplianceCheck` | Enabled | - |  |
| `executeComplianceCheck` | Enabled | - |  |
