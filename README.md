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
| Functions | 13 |
| Knowledge Collections | 0 |
| Flows | 0 |
| Web Actions | 0 |

## Agents

| Agent | Type | Status | Mode | Functions |
|-------|------|--------|------|-----------|
| [Agente de Gestión de Quejas Rappi](docs/agent-agente-de-gestión-de-quejas-rappi.md) | experimental | DEPLOYED | conversational | 4 |

## Functions

| Function | Action Type | Description |
|----------|-------------|-------------|
| [User ID](docs/function-user-id.md) | CONSUME_WS_ACTION | This function retrieves user identification details, including their affiliation |
| [Demo_CardBlock](docs/function-demo_cardblock.md) | CONVERSATION_FLOW | A demo function representing a card-style content block with no inputs or output |
| [Demo_ChatNote](docs/function-demo_chatnote.md) | CONVERSATION_FLOW | A demo function placeholder for chat-related notes or annotations. Currently it  |
| [Demo_Custom_Cards](docs/function-demo_custom_cards.md) | CONVERSATION_FLOW | Funcion para mostrar la tarjeta (card) de producto , contine su imagen public UL |
| [Demo_Flow_JSON_Card](docs/function-demo_flow_json_card.md) | CONVERSATION_FLOW | A demo function representing a JSON-based card flow, used as a placeholder or ex |
| [Demo_Flow_SearchProducts_Delete](docs/function-demo_flow_searchproducts_delete.md) | CONVERSATION_FLOW | Deletes products returned from a demo search flow, typically used for testing or |
| [Demo_Formulario_Registro](docs/function-demo_formulario_registro.md) | CONVERSATION_FLOW | Demonstration function representing a registration form workflow. It can be used |
| [Demo_Seguro_Premium](docs/function-demo_seguro_premium.md) | CONVERSATION_FLOW | Demonstration function for premium insurance (seguro premium) flows or features. |
| [Demo_Widget_Form](docs/function-demo_widget_form.md) | CONVERSATION_FLOW | A demo function representing a widget form with no inputs or outputs, typically  |
| [Demo_Widgets_Contacto](docs/function-demo_widgets_contacto.md) | CONVERSATION_FLOW | Demonstration function for handling or showcasing contact-related widgets, such  |
| [GetCatalogCategories](docs/function-getcatalogcategories.md) | CONSUME_WS_ACTION | Retrieves the list of all categories available in the catalog. |
| [GetCategoryProducts](docs/function-getcategoryproducts.md) | CONSUME_WS_ACTION | Retrieves a list of products for a given category or subcategory, with optional  |
| [SearchProducts](docs/function-searchproducts.md) | CONSUME_WS_ACTION | Searches for products using text query and optional filters such as price range, |

## Documentation

- `docs/agent-*.md` - Detailed agent documentation (instruction, SOPs, personality, deployment)
- `docs/function-*.md` - Function specifications (parameters, pre-conditions, PII rules)
- `docs/testing-guide.md` - Test scenarios with request/response examples
- `docs/conversation-scripts-*.md` - Sample conversation flows

## Repository Structure

```
amelia/           # Amelia-importable artifacts (Conductor-compatible)
  agentic/        # Agents, functions, knowledge collections
  flows/          # Flow definitions
  consumews/      # Web action specifications
  tabular/        # Tabular data
docs/             # Rich documentation (agent, function, testing)
library/          # Reference documents (PDFs, DOCX)
support/          # API collections (Postman, Baserow)
test-data/        # Mock datasets with test scenarios
scripts/          # External code (Groovy)
```

### Importing into Amelia

Clone this repo and point Conductor import to the `amelia/` directory.
