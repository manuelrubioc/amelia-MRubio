# MRubio

Amelia AI agent artifacts for the **mrubio** domain.

## Domain Context

**Company:** Rappi  
**Industry:** logistics  
**Country:** mx  
**Use Case:** complaint_handling  

2 business opportunities. In both cases, the idea is to reduce the amount of tickets handled by humans so the interest is to have AI as the first layer and have a smooth transition to agents when the Ai cannot solve the case:

AI for social media support — specifically to classify incoming interactions and determine whether they constitute a ticket or a comment.
AI for dispute management — with a focus on image recognition and identification 


## Summary

| Artifact | Count |
|----------|-------|
| Agents | 1 |
| Functions | 4 |
| Web Actions | 4 |
| Knowledge Collections | 0 |

## [Agente de Gestión de Quejas Rappi](docs/agent-agente-de-gestión-de-quejas-rappi.md)

**Type:** experimental | **Status:** DEPLOYED | **Mode:** conversational

Agente inteligente para la clasificación y gestión inicial de quejas en redes sociales y disputas con análisis de imágenes para optimizar la resolución de casos

**Functions** ([details](docs/functions-agente-de-gestión-de-quejas-rappi.md)):

- `clasificarInteraccionSocial` — Clasifica interacciones de redes sociales como ticket válido
- `analizarImagenDisputa` — Analiza imágenes de disputas para identificar discrepancias 
- `consultarDetallesPedido` — Obtiene información detallada de un pedido específico
- `crearTicketSoporte` — Crea un nuevo ticket de soporte para seguimiento

**Web Actions** ([details](docs/web-actions-agente-de-gestión-de-quejas-rappi.md)):

- `POST` `clasificarInteraccionSocial` — https://a0b94834-4935-4e86-88a9-363f94cd1daa.mock.pstmn.io/clasificarInteraccionSocial
- `POST` `analizarImagenDisputa` — https://a0b94834-4935-4e86-88a9-363f94cd1daa.mock.pstmn.io/analizarImagenDisputa
- `GET` `consultarDetallesPedido` — https://a0b94834-4935-4e86-88a9-363f94cd1daa.mock.pstmn.io/consultarDetallesPedido
- `POST` `crearTicketSoporte` — https://a0b94834-4935-4e86-88a9-363f94cd1daa.mock.pstmn.io/crearTicketSoporte

[Agent](docs/agent-agente-de-gestión-de-quejas-rappi.md) | [Functions](docs/functions-agente-de-gestión-de-quejas-rappi.md) | [Web Actions](docs/web-actions-agente-de-gestión-de-quejas-rappi.md) | [Testing Guide](docs/testing-guide-agente-de-gestión-de-quejas-rappi.md)

---

## Repository Structure

```
amelia/           # Amelia-importable artifacts (Conductor-compatible)
  agentic/        # Agents, functions, knowledge collections
docs/             # Rich documentation per agent
support/          # API collections (Postman)
test-data/        # Mock datasets with test scenarios
scripts/          # External code (Groovy)
```

### Importing into Amelia

Clone this repo and point Conductor import to the `amelia/` directory.
