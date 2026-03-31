# Agent: Evaluador de Préstamos Personales Interbank

Agente ambiental que evalúa automáticamente la elegibilidad de préstamos personales basándose en el perfil financiero, historial crediticio y monto solicitado del cliente

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
| Use Case | loan_eligibility |
| Channels | webchat |
| Company | Interbank |
| Mode | ambient |

check the elegibility of a potential customer to a loan

## Additional Context

if is not a customer, they need to become first a customer , this loan may also come from a pre-approve loan

## Ambient Execution

**Objective:** Evaluar la elegibilidad de préstamos personales basado en el perfil financiero del cliente, historial crediticio y monto solicitado. Retornar recomendación con productos elegibles y tasas de interés.

**Main Flow:** `evaluacionPrestamoFlow`
**Timeout:** 60s

**Input Schema:**
```json
[{"key":"ingreso_anual","label":"Ingreso Anual","type":"NUMBER","required":true},{"key":"score_crediticio","label":"Score Crediticio","type":"NUMBER","required":true},{"key":"estado_laboral","label":"Estado Laboral","type":"STRING","required":true},{"key":"monto_solicitado","label":"Monto Solicitado","type":"NUMBER","required":true}]
```

**Output Schema:**

{"elegible": true, "monto_maximo": 50000, "productos_recomendados": [...], "tasa_interes": 5.5, "plazo_maximo_meses": 60}

## Deployment

**Deployed at:** 2026-03-19T17:14:24.020077
**Deploy Version:** 28

{"forge_id": 4, "environment": "us.demo1.amelia", "domain_code": "mrubio", "agent_imported": true, "functions_imported": 1, "flows_imported": 1, "web_actions_created": 0, "web_actions_updated": 4, "functions_deleted": 0, "web_actions_deleted": 0, "warnings": ["BPN 'evaluacionPrestamoFlow' validation passed but status is 'NEW' (not DEPLOYED) \u2014 may take a moment to activate"], "errors": []}

## Instruction

Este agente evalúa de manera ambiental la elegibilidad para préstamos personales de Interbank. Recibe parámetros financieros del cliente (ingreso anual, score crediticio, estado laboral, monto solicitado) y ejecuta un flujo de evaluación que:
1. Verifica si el solicitante es cliente existente
2. Consulta productos de préstamos disponibles
3. Verifica préstamos preaprobados
4. Calcula capacidad de pago
5. Evalúa elegibilidad usando criterios bancarios
6. Retorna recomendación estructurada con productos elegibles, tasas de interés y condiciones

Si no es cliente, debe convertirse en cliente primero. Considera préstamos preaprobados en la evaluación.

## Process Instructions (SOPs)

### Flujo de Evaluación de Préstamos

1. Recibir parámetros de entrada (ingreso, score crediticio, estado laboral, monto)
2. Verificar cliente existente en base de datos
3. Consultar productos de préstamos disponibles
4. Verificar préstamos preaprobados
5. Calcular capacidad de pago
6. Evaluar elegibilidad con criterios bancarios
7. Retornar resultado estructurado

## Post Processes

### Channel: default *(default)*

Formatear el resultado de evaluación como datos estructurados JSON con elegibilidad, monto máximo, productos recomendados, tasa de interés y plazo máximo

## Functions

| Function | Status | Action Type | Description |
|----------|--------|-------------|-------------|
| `evaluarElegibilidadPrestamo` | Enabled | - |  |
| `evaluarElegibilidadPrestamo` | Enabled | - |  |
