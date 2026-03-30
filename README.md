# Domain: mrubio

This repository contains the Amelia agent artifacts for the **mrubio** domain.

## Agents

- **Asistente de Regalos Coppel** - Un agente experto diseñado para ayudar a los clientes de Coppel a encontrar el regalo perfecto. Se especializa en comprender solicitudes complejas y vagas (p. ej., regalos de cumpleaños), realizando preguntas de desambiguación para entender las preferencias del destinatario. Utiliza el catálogo de productos de Coppel para ofrecer recomendaciones calificadas, comparar artículos, responder preguntas y guiar al usuario hasta la confirmación de la compra, momento en el que transfiere a un asesor humano para completar la transacción.
- **Asistente de Seguimiento Coppel** - Agente virtual especializado en atención al cliente para consultas de seguimiento de pedidos de electrodomésticos de línea blanca de Coppel. Proporciona información precisa sobre el estado de entregas, fechas estimadas y resuelve consultas relacionadas con pedidos utilizando múltiples métodos de identificación (número de pedido, datos del cliente). Capaz de manejar conversaciones no lineales, escalar a agentes humanos cuando es necesario y registrar todas las interacciones en Salesforce para seguimiento completo del cliente.
- **AVI Interbank - Asistente Virtual Inteligente** - AVI es el asistente virtual especializado de Interbank que ayuda a los clientes con consultas sobre Cuenta Sueldo y membresías de tarjetas de crédito. Proporciona información precisa sobre requisitos, beneficios, procesos de apertura, exoneraciones de membresía y resuelve dudas frecuentes de manera empática y conversacional, siguiendo las políticas y procedimientos establecidos por Interbank.
- **SoundBank Assistant** - Agente conversacional especializado en banca digital que proporciona atención integral al cliente de SoundBank. Gestiona consultas sobre productos financieros, operaciones bancarias, préstamos, tarjetas, soporte y recomendaciones personalizadas. Diseñado para ofrecer respuestas precisas, experiencia conversacional fluida y resolución eficiente de necesidades bancarias complejas a través de canal de texto.

- **Asistente de Test de Flows** - Ejecuta la funcion JSON que es un Workflow
- **Buscador de Productos Catalogo** - Agente especializado en buscar y filtrar productos dentro de un catálogo utilizando categorías de Google y múltiples atributos de producto como título, descripción, categoría, subcategoría, marca, precio, disponibilidad, condición, imagen, enlace a la web, oferta y popularidad. Permite al usuario encontrar productos relevantes según sus necesidades y preferencias.
- **Asistente de Ventas Tarjetas Bancoppel** - Asistente virtual especializado en la venta y promoción de tarjetas de crédito y débito de Bancoppel, diseñado para brindar información personalizada y guiar a los clientes en el proceso de solicitud.
- **Asistente de Citas Bancoppel** - Agente virtual que facilita la reserva de citas con asesores en sucursales Bancoppel para brindar atención personalizada y exclusiva
- **Asistente de Pedidos Agrosuper** - Agente especializado en manejo de pedidos de productos cárnicos y avícolas para clientes comerciales de Agrosuper en Chile
- **Production: mrubio** - Auto-created for 1 conversation analyses without agent
- **Agente de Verificación de Cumplimiento Regulatorio** - Agente autónomo que ejecuta verificaciones de cumplimiento y regulatorias para productos financieros, proporcionando clasificaciones y recomendaciones basadas en perfiles de cliente y normativas SBS del Perú
- **Evaluador de Préstamos Personales Interbank** - Agente ambiental que evalúa automáticamente la elegibilidad de préstamos personales basándose en el perfil financiero, historial crediticio y monto solicitado del cliente
- **Agente de Incorporación de Distribuidores Retail&Co** - Agente especializado en ayudar a potenciales distribuidores y vendedores a unirse a la red de distribución de Retail&Co en el sector de alimentos y bebidas en México. Recopila información completa, valida datos y programa visitas comerciales.
- **Agente de Gestión de Quejas Rappi** - Agente inteligente para la clasificación y gestión inicial de quejas en redes sociales y disputas con análisis de imágenes para optimizar la resolución de casos

## Functions

- `User ID` - This function retrieves user identification details, including their affiliation and contact information.
- `Demo_CardBlock` - A demo function representing a card-style content block with no inputs or outputs, typically used for UI or layout demonstrations.
- `Demo_ChatNote` - A demo function placeholder for chat-related notes or annotations. Currently it does not accept any inputs and does not return any outputs; it can be used as a structural or illustrative example in a larger system.
- `Demo_Custom_Cards` - Funcion para mostrar la tarjeta (card) de producto , contine su imagen public ULR, nombre y contenido con un boton de accion.
- `Demo_Flow_JSON_Card` - A demo function representing a JSON-based card flow, used as a placeholder or example with no inputs or outputs.
- `Demo_Flow_SearchProducts_Delete` - Deletes products returned from a demo search flow, typically used for testing or cleanup of demo search results.
- `Demo_Formulario_Registro` - Demonstration function representing a registration form workflow. It can be used as a template or placeholder for implementing user registration logic, such as collecting user details, validating input, and storing registration data.
- `Demo_Seguro_Premium` - Demonstration function for premium insurance (seguro premium) flows or features. This function does not accept inputs or produce outputs and is intended for use as a placeholder, trigger, or example within an insurance-related system.
- `Demo_Widget_Form` - A demo function representing a widget form with no inputs or outputs, typically used as a placeholder or example in a system.
- `Demo_Widgets_Contacto` - Demonstration function for handling or showcasing contact-related widgets, such as contact forms, contact cards, or communication UI elements within a demo or testing environment.
- `GetCatalogCategories` - Retrieves the list of all categories available in the catalog.
- `GetCategoryProducts` - Retrieves a list of products for a given category or subcategory, with optional pagination, sorting, and summary-level control.
- `SearchProducts` - Searches for products using text query and optional filters such as price range, category, brand, tags, and pagination/sorting options.

## Flows

_No flows found._

## Web Actions

_No web actions found._

## Repository Structure

- `amelia/` - Amelia-importable artifacts (Conductor-compatible format)
- `docs/` - Agent documentation
- `library/` - Reference documents (PDFs, DOCX)
- `support/` - API collections (Postman, Baserow)
- `scripts/` - External code (Groovy scripts)

### Importing into Amelia

Clone this repo and point Conductor import to the `amelia/` directory to import all artifacts.
