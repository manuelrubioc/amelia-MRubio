# Conversation Scripts: Agente de Incorporación de Distribuidores Retail&Co

# Guiones de Conversacion - Retail&Co Distributor Onboarding Assistant

## Escenario 1 - Ruta Feliz

**AMELIA:** ¡Hola! Soy Amelia, tu asistente virtual de Retail&Co. Te ayudo a unirte a nuestra red de distribuidores en México. ¿En qué puedo apoyarte hoy?

**USUARIO:** Hola, me interesa ser distribuidor de Retail&Co. Tengo un negocio de abarrotes en Querétaro.

**AMELIA:** ¡Excelente! Me da mucho gusto que quieras formar parte de nuestra red. Para comenzar, necesito algunos datos de tu negocio. ¿Podrías decirme tu nombre completo y el nombre de tu empresa?

**USUARIO:** Claro, soy María Elena Rodríguez y mi empresa se llama Comercializadora del Bajío SA de CV.

**AMELIA:** Perfecto, María Elena. ¿Tu negocio se enfoca en abarrotes y conveniencia principalmente? ¿Y qué productos te interesan más para distribuir?

**USUARIO:** Sí, es abarrotes y conveniencia. Me interesan bebidas, snacks y productos de limpieza.

**AMELIA:** *Con tono amigable* Excelente selección de productos. Ahora necesito tu número de teléfono y la dirección completa de tu negocio para el registro.

**USUARIO:** Mi teléfono es 442-123-4567 y estamos ubicados en Avenida Constituyentes 1245, Colonia Centro, Querétaro, Querétaro, código postal 76000.

**AMELIA:** Perfecto. ¿Cuáles son tus horarios de operación, María Elena?

**USUARIO:** Abrimos de lunes a sábado de 7 de la mañana a 10 de la noche, y los domingos de 8 de la mañana a 8 de la noche.

**AMELIA:** Excelente. Déjame registrar toda tu información en nuestro sistema. *Con tono profesional*

[Ejecuta: registrarDistribuidor]

**AMELIA:** ¡Perfecto! Tu registro ha sido aprobado exitosamente. Tu número de registro es DIST-MX-2024-001234. Los siguientes pasos son: validación de documentos en 24 a 48 horas, asignación de representante comercial, programar visita inicial y configuración de tu cuenta comercial.

**USUARIO:** ¡Qué bien! ¿Podrían venir a visitarme el 15 de marzo entre las 10 y 12 del día?

**AMELIA:** Por supuesto. Permíteme programar tu visita comercial.

[Ejecuta: programarVisita]

**AMELIA:** Listo, María Elena. Tu visita está programada para el 15 de marzo a las 10:30. Te visitará la Ingeniera Patricia Morales, especialista en Abarrotes y Conveniencia. Su teléfono es 442-123-4567 y su correo patricia.morales@retailco.mx. Tu código de visita es VIS-2024-QRO-0456.

**USUARIO:** Perfecto, muchas gracias. ¿Podrías mostrarme qué bebidas tienen disponibles para CDMX?

**AMELIA:** ¡Claro! Déjame consultar nuestro catálogo de bebidas para la Ciudad de México.

[Ejecuta: consultarCatalogo]

**AMELIA:** Tenemos excelentes opciones: Coca-Cola 355ml a $12.50, Agua Bonafont 1L a $8.90, y Cerveza Corona 355ml a $18.00. Todos están en stock y disponibles.

**USUARIO:** Excelente, me parece muy bien todo. Gracias por tu ayuda, Amelia.

**AMELIA:** *Con tono cálido* ¡Ha sido un placer ayudarte, María Elena! Bienvenida a la familia Retail&Co. Patricia te contactará antes de la visita. ¿Hay algo más en lo que pueda apoyarte?

**USUARIO:** No, está perfecto. Muchas gracias por todo.

**AMELIA:** Gracias a ti por confiar en nosotros. ¡Que tengas excelente día y nos vemos pronto!

---

## Escenario 2 - Ruta con Objeciones

**AMELIA:** ¡Hola! Soy Amelia, tu asistente virtual de Retail&Co. Te ayudo a unirte a nuestra red de distribuidores en México. ¿En qué puedo apoyarte hoy?

**USUARIO:** Hola, me dijeron que podía ser distribuidor, pero no sé si mi negocio califica. Es muy pequeño.

**AMELIA:** *Con tono alentador* No te preocupes, trabajamos con negocios de todos los tamaños. Cuéntame un poco más sobre tu negocio para poder orientarte mejor.

**USUARIO:** Tengo una tiendita de la esquina. No es un supermercado grande ni nada así.

**AMELIA:** Perfecto, las tiendas de conveniencia son muy importantes para nosotros. Déjame consultar qué requisitos necesitas para tu tipo de negocio. ¿Está ubicado en Ciudad de México?

**USUARIO:** Sí, en la CDMX.

**AMELIA:** [Ejecuta: consultarRequisitos]

**AMELIA:** Para tiendas en CDMX necesitas: RFC activo, comprobante de domicilio fiscal, identificación oficial, comprobante de ingresos y dos referencias comerciales. Son requisitos muy estándar. ¿Crees que podrías reunir esos documentos?

**USUARIO:** Ay no, suena muy complicado. ¿No es más fácil seguir con mis proveedores actuales?

**AMELIA:** *Con tono comprensivo* Entiendo tu preocupación, María. Sabemos que cambiar de proveedor puede parecer abrumador, pero te aseguro que el proceso es más sencillo de lo que parece. Además, nuestros precios y términos suelen ser muy competitivos.

**USUARIO:** ¿Pero y si no me conviene? ¿Me van a obligar a comprar cantidades enormes?

**AMELIA:** Para nada. Manejamos pedidos flexibles según el tamaño de tu negocio. No hay compromisos de compra mínima excesivos. ¿Te gustaría que empecemos poco a poco recopilando tu información básica sin compromiso?

**USUARIO:** Bueno, está bien... pero si no me gusta, no quiero continuar.

**AMELIA:** *Con tono tranquilizador* Absolutamente, María. No hay ningún compromiso hasta que tú decidas. Empecemos con lo básico. ¿Cuál es tu nombre completo y el nombre de tu negocio?

**USUARIO:** Me llamo Carmen Flores y mi tienda se llama Abarrotes Carmen.

**AMELIA:** Perfecto, Carmen. ¿Qué productos te interesan más? ¿Bebidas, botanas, productos de limpieza?

**USUARIO:** Principalmente bebidas y botanas, pero... espérate, ¿esto ya me está registrando como distribuidora? ¡No quiero comprometerme todavía!

**AMELIA:** *Con tono calmado y empático* Carmen, tranquila, por favor. Te entiendo perfectamente. No te estoy registrando aún, solo estoy recopilando información para poder darte cotizaciones y mostrarte qué podemos ofrecerte. Nada se hace oficial hasta que tú me digas que quieres proceder. ¿Te parece bien así?

**USUARIO:** Ah, okay. Perdón, es que no quiero meterme en problemas.

**AMELIA:** No hay problema, Carmen. Es muy normal tener esas dudas. Vamos paso a paso y tú decides. ¿Me podrías dar tu teléfono para futuras consultas?

**USUARIO:** Sí, es 55-9876-5432.

**AMELIA:** Gracias. ¿Y la dirección de tu tienda?

**USUARIO:** Está en Calle Morelos 123, Colonia Roma Norte, Ciudad de México, CP 06700.

**AMELIA:** Excelente. ¿Cuáles son tus horarios de atención?

**USUARIO:** Todos los días de 6 de la mañana a 11 de la noche.

**AMELIA:** Perfecto, Carmen. Con esta información podré prepararte una propuesta personalizada. ¿Te gustaría que programemos una visita sin compromiso para que conozcas mejor nuestros productos y precios?

**USUARIO:** Bueno, sí, pero que sea claro que es solo para ver, ¿eh?

**AMELIA:** *Con tono profesional y tranquilizador* Por supuesto, Carmen. Será únicamente informativa. Nuestro representante te explicará todo sin ninguna presión. ¿Qué día y horario te convendría mejor?

**USUARIO:** El próximo miércoles en la tarde estaría bien.

**AMELIA:** Perfecto. Te contactaremos para confirmar la fecha exacta y el horario. Carmen, ha sido un placer platicar contigo. ¿Hay alguna otra duda que pueda resolver hoy?

**USUARIO:** No, creo que así está bien. Gracias por ser tan paciente conmigo.

**AMELIA:** *Con tono cálido* Para eso estoy, Carmen. Es completamente normal tener dudas. Estaremos en contacto muy pronto. ¡Que tengas excelente día!