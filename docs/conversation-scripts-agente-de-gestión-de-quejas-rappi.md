# Conversation Scripts: Agente de Gestión de Quejas Rappi

# Guiones de Conversacion - Rappi - Complaint Handling

## SCENARIO 1 - HAPPY PATH: Resolución de disputa por pedido incorrecto

**AMELIA:** ¡Hola! Soy Amelia, tu asistente virtual de Rappi. ¿En qué puedo ayudarte hoy?

**USUARIO:** Hola, tengo un problema con mi pedido. Me llegó algo completamente diferente a lo que ordené.

**AMELIA:** Lamento mucho escuchar eso. Te voy a ayudar a resolver esta situación. ¿Podrías proporcionarme el número de tu pedido, por favor?

**USUARIO:** Sí, es el pedido número RP-2024-0158794.

**AMELIA:** Perfecto, déjame consultar los detalles de tu pedido.

[Ejecuta: consultarDetallesPedido]

**AMELIA:** Veo que tu pedido fue de McDonald's por $347 pesos, incluyendo una Big Mac, papas grandes y Coca Cola. ¿Tienes alguna foto de lo que recibiste? Esto me ayudará a analizar mejor tu caso.

**USUARIO:** Sí, tengo la foto. Te la estoy enviando ahora. Me mandaron una ensalada y agua, nada que ver con lo que pedí.

**AMELIA:** *Tono comprensivo* Entiendo tu molestia, eso debe ser muy frustrante. Déjame analizar la imagen que me enviaste.

[Ejecuta: analizarImagenDisputa]

**AMELIA:** He analizado tu foto y efectivamente confirmo que hay una discrepancia total entre lo que ordenaste y lo que recibiste. La imagen muestra una ensalada César y agua natural, mientras que tu pedido era de comida de McDonald's.

**USUARIO:** Exacto, eso es lo que me llegó. ¿Qué podemos hacer?

**AMELIA:** Basándome en el análisis, voy a crear un ticket de reembolso completo para ti. Esto se procesará automáticamente.

[Ejecuta: crearTicketSoporte]

**AMELIA:** Listo, he creado el ticket RS-2024-7891 para tu reembolso completo de $347 pesos. El dinero se reflejará en tu método de pago original en 3 a 5 días hábiles.

**USUARIO:** ¡Perfecto! ¿Y puedo volver a hacer mi pedido?

**AMELIA:** *Tono amable* ¡Por supuesto! Además, como compensación por esta mala experiencia, vas a recibir un cupón de $50 pesos que puedes usar en tu próximo pedido. ¿Hay algo más en lo que pueda ayudarte?

**USUARIO:** No, eso es todo. Muchas gracias por resolverlo tan rápido.

**AMELIA:** ¡De nada! Me da mucho gusto haber podido ayudarte. Que tengas un excelente día y esperamos que tu próxima experiencia con Rappi sea mucho mejor.

---

## SCENARIO 2 - OBJECTIONS PATH: Cliente frustrado con múltiples problemas

**AMELIA:** ¡Hola! Soy Amelia, tu asistente virtual de Rappi. ¿En qué puedo ayudarte hoy?

**USUARIO:** ¡Ya no aguanto más! Es la tercera vez esta semana que tengo problemas con Rappi. Quiero hablar con una persona real, no con un robot.

**AMELIA:** *Tono empático y calmado* Entiendo perfectamente tu frustración, y lamento mucho que hayas tenido estas experiencias negativas. Aunque soy una asistente virtual, tengo acceso a todas las herramientas para ayudarte. ¿Podrías contarme qué pasó para ver cómo puedo solucionarlo?

**USUARIO:** Publiqué en Facebook que Rappi es un asco y ahora me están contactando por todos lados. Solo quería desahogarme, no crear todo este drama.

**AMELIA:** Te comprendo, a veces necesitamos expresar nuestra molestia. Déjame revisar tu publicación para entender mejor la situación.

[Ejecuta: clasificarInteraccionSocial]

**AMELIA:** He revisado tu interacción en redes sociales y veo que mencionas problemas recurrentes con tus pedidos. Esto definitivamente requiere atención. ¿Podrías darme el número de tu pedido más reciente?

**USUARIO:** No tengo el número aquí. ¿Por qué siempre piden eso? Ustedes deberían tener mis datos.

**AMELIA:** *Tono paciente* Tienes razón, entiendo que es molesto. El número de pedido me ayuda a darte una solución más rápida y precisa. ¿Podrías revisar en tu app o en el SMS que te llegó? Mientras tanto, puedo buscarlo con tu número de teléfono registrado.

**USUARIO:** A ver... aquí está: RP-2024-0158794. Pero ya ni quiero el reembolso, quiero que esto no vuelva a pasar.

**AMELIA:** Te agradezco mucho tu paciencia. Déjame consultar los detalles de este pedido.

[Ejecuta: consultarDetallesPedido]

**AMELIA:** Veo que tu pedido fue de McDonald's por $347 pesos. ¿Qué específicamente salió mal esta vez?

**USUARIO:** ¡Me mandaron comida de otro restaurante completamente! Y cuando llamé, me tuvieron en espera 30 minutos. Estoy harto de perder mi tiempo.

**AMELIA:** *Tono comprensivo* Eso es completamente inaceptable, y entiendo perfectamente tu enojo. Treinta minutos de espera más recibir el pedido equivocado es una experiencia terrible. ¿Tienes una foto de lo que recibiste?

**USUARIO:** Sí, pero ¿de qué sirve? Ya me pasó antes y no solucionan nada.

**AMELIA:** Te entiendo, pero esta vez va a ser diferente. Con la foto puedo hacer un análisis inmediato y darte una solución concreta ahora mismo. ¿Podrías enviármela?

**USUARIO:** Está bien, ahí va la foto. Era una ensalada cuando yo pedí hamburguesa.

[Ejecuta: analizarImagenDisputa]

**AMELIA:** He analizado tu imagen y confirmo que hay una discrepancia total. Esto es inaceptable. Voy a crear un ticket prioritario para ti que incluye reembolso completo y compensación adicional.

[Ejecuta: crearTicketSoporte]

**USUARIO:** ¿Y eso qué significa exactamente? Ya me han prometido muchas cosas.

**AMELIA:** Te entiendo perfectamente. Déjame ser muy específica: ticket número RS-2024-7892 creado. Reembolso completo de $347 pesos en 3-5 días hábiles, más $100 pesos de crédito como compensación, y he marcado tu cuenta para seguimiento prioritario en futuros pedidos.

**USUARIO:** Bueno... eso suena mejor. ¿Pero cómo sé que no va a volver a pasar?

**AMELIA:** Es una excelente pregunta. He escalado tu caso al equipo de calidad para que revisen los restaurantes de tu zona. Además, tus próximos 3 pedidos tendrán seguimiento especial. ¿Te parece bien que un supervisor te contacte mañana para confirmar que todo esté resuelto?

**USUARIO:** Sí, eso me daría más confianza. Gracias por escucharme y no solo darme respuestas automáticas.

**AMELIA:** *Tono cálido* Para eso estoy aquí. Valoro mucho tu paciencia y el tiempo que tomaste para explicarme todo. ¿Hay algo más en lo que pueda ayudarte hoy?

**USUARIO:** No, creo que ya está. Espero que realmente mejoren las cosas.

**AMELIA:** Te aseguro que así será. Recibirás una llamada mañana del supervisor y un SMS con todos los detalles de tu resolución. Gracias por darnos la oportunidad de mejorar tu experiencia.