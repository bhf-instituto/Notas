### Modelo ambiental

El primer modelo importante que se debe desarrollar como analista es uno que no haga más que definir las interfaces entre el sistema y el resto del universo, es decir, el ambiente.

Además de determinar qué está en el interior del sistema y qué en el exterior (lo que se logra definiendo la frontera entre el sistema y el ambiente), también es críticamente importante definir las interfaces entre el sistema y el ambiente. Se necesita saber qué información entra al sistema desde el ambiente exterior, y qué información produce como salida al ambiente externo.

Otro aspecto crítico del modelo ambiental consiste en identificar los acontecimientos que ocurren en el ambiente al cual debe responder el sistema. No para todos los acontecimientos; después de todo, el ambiente en su totalidad genera un número infinito de acontecimientos. Sólo nos preocupan aquellos que ocurren en el ambiente exterior y requieren una respuesta del sistema.

### Diagrama de Contexto

El diagrama de contexto es un caso especial del diagrama de flujo de datos, en donde *una sola burbuja representa todo el sistema*.

El diagrama de contexto enfatiza varias características importantes del sistema:
- Las personas, organizaciones y sistemas con los que se comunica el sistema. Se conocen como **terminadores**.
- Los datos que el sistema recibe del mundo exterior y que deben procesarse de alguna forma.
- Los datos que el sistema produce y que se envían al mundo exterior.
- Los almacenes de datos que el sistema comparte con los terminadores. Estos almacenes de datos se crean fuera del sistema para su uso, o bien son creados en él y usados fuera.
- La frontera entre el sistema y el resto del mundo.

![[Pasted image 20251107125611.png]]