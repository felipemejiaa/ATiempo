# Prompt final · ATiempo, prototipo v2

Herramienta: Claude (asistente de IA), en conversación. No se usaron skills ni agentes adicionales.

---

Crea la versión 2 del prototipo navegable de **ATiempo** en un solo archivo HTML. Usa **React 18 cargado desde CDN** con Babel en el navegador, para que se abra con doble clic sin instalar nada. ATiempo le dice a un estudiante de la Universidad El Bosque a qué hora salir de su casa para llegar puntual a clase.

**Base:** el prototipo v1 (marco de celular con el panel de prueba al lado). El cálculo interno es el de la propuesta: D = w·H̄ + (1 − w)·E con w = n/(n + 5), Dq = D + z·σ y S = T − (Dq + M), con modo degradado cuando no hay datos de tráfico. **La fórmula no se muestra en la app.**

**Diseño:** simple, verde y blanco (#0e7a5a), tipografía Plus Jakarta Sans, tarjetas con sombras suaves y bordes redondeados. Las animaciones también son suaves: entrada escalonada de las tarjetas, desvanecido al cambiar la hora de salida, hojas que suben desde abajo y un punto de estado que late despacio.

**Cambios pedidos por los usuarios en la evaluación:**
1. Inicio centrado en la hora de salida, la hora de llegada y un estado claro: «Sal tranquilo», «Prepárate», «Sal ya» o «Vas tarde».
2. Debajo, una frase sencilla que explique por qué esa hora comparada con un día normal (por ejemplo: «Hoy sales 19 min antes de lo normal por el aguacero y los buses llenos»).
3. Una tarjeta «Tráfico en tu ruta» con:
   - la línea del recorrido por tramos (caminar, esperar, bus, caminar);
   - el tramo del bus animado, con un color y una velocidad según el nivel (fluido, moderado, pesado o muy pesado);
   - los minutos de cada tramo y lo que suma cada novedad;
   - la velocidad aproximada del bus, la franja del día y la confianza;
   - la etiqueta «SIMULADO».

   Los tramos deben sumar exactamente la duración estimada.
4. Un mapa (Leaflet con OpenStreetMap, sin clave) para marcar el origen y el destino tocando el mapa, eligiendo un barrio o usando la ubicación actual, con una nota de privacidad.
5. Campos del recorrido con explicación y ejemplo, y un botón para sugerir los tiempos según la distancia.
6. Comparar las rutas que salen del mismo origen y recomendar la de menor riesgo de llegar tarde.
7. La confianza depende de cuántos viajes hay y de qué tan parecidos son.
8. Avisos de preparación, de hora de salida y de cambio de hora, con opción de notificaciones del navegador.
9. Al registrar la llegada, preguntar cómo le fue al estudiante («Me sobró tiempo», «Llegué justo», «Llegué tarde»). Si llega tarde en 2 de los últimos 3 viajes, sugerir más margen.
10. Historial con hallazgos: el día más pesado, la ruta más confiable y cómo le ha ido con la hora.
11. Panel de prueba organizado en cinco preguntas: hora, clima, vía, transporte y conexión. Incluye escenarios reales de Bogotá: lluvia, aguacero, trancón, manifestación, obras del Metro, partido en El Campín, festivo, bus demorado, bus lleno, estación cerrada, paro de transporte y sin conexión.
12. **Modo prueba por defecto** («Día de clase, con tiempo»), para poder simular un viaje completo a cualquier hora. La hora real del celular queda como opción. Si con la hora real la clase es otro día, se ofrece «Avisarme» y un enlace para pasar al modo prueba.
13. Pantalla de «Novedades» que muestra qué cambió y quién lo pidió.

**Pantallas:** bienvenida, inicio, en camino y registro de llegada, recorridos (varios, con editar y eliminar), nuevo recorrido, horario, historial y ajustes.

**Sin backend:**
- Datos de ejemplo de Bogotá:
  - rutas desde Kennedy por SITP y por TransMilenio, y una desde el trabajo;
  - un horario variado entre las 7:00 a. m. y las 4:00 p. m., de lunes a sábado;
  - viajes de los últimos 30 días, calculados a partir de la fecha real.
- Una API falsa con espera simulada.
- `localStorage` para guardar.
