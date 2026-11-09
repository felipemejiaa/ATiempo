# ATiempo · Prompt final del prototipo

| | |
|---|---|
| **Proyecto** | ATiempo: recomendación personalizada de la hora de salida para estudiantes que usan transporte público |
| **Artefacto** | Prototipo navegable en un solo archivo (`atiempo.html`), HTML + CSS + JavaScript vanilla |
| **Herramienta de IA** | Claude (Anthropic) |
| **Skills y agentes** | No se usaron skills ni agentes especializados |
| **Contexto entregado a la IA** | Propuesta de trabajo de grado (Figura 2: mockup de las tres pantallas), Pitch 2 · La solución, presentación de la Clase 12 · Prototipos |

---

## Prompt

```
Crea un prototipo navegable en un solo archivo HTML con CSS y JavaScript vanilla,
sin backend, que se abra con doble clic en el navegador.

CONTEXTO
ATiempo es una PWA para estudiantes de la Universidad El Bosque que llegan en
transporte público (TransMilenio y SITP). Su única función es responder, antes
de cada clase, a qué hora debe salir el estudiante. Basa las pantallas en el
mockup adjunto (Mi recorrido, Próxima salida, Mis llegadas) y conserva la barra
inferior Inicio · Recorrido · Horario · Tablero.

FLUJO PRINCIPAL
1. Bienvenida. Explica la app en tres pasos e indica que no se rastrea la
   ubicación (Ley 1581 de 2012). Opciones: configurar desde cero o cargar una
   demo con un mes de uso.
2. Mi recorrido (RF-02). Nombre del recorrido, punto de salida, caminata al
   paradero, bus habitual con trasbordo opcional, espera habitual, tiempo dentro
   del bus, destino y caminata hasta el salón. En la configuración inicial se
   pide también la primera clase del lunes.
3. Mi horario (RF-01). Clases agrupadas por día; agregar en una hoja inferior
   (materia, día, hora, salón) y eliminar. La hora de la clase es la hora de
   llegada que se usa para calcular la salida.
4. Inicio (RF-03 y RF-05). Notificación simulada ("Sal a las 6:53 para llegar a
   Redes 2"), recorrido en uso, tarjeta "Próxima salida" con la hora, la clase,
   la llegada estimada, el estado (a tiempo, sal ya, vas tarde) y el nivel de
   confianza. Sección "¿De dónde sale la hora?" con la condición del tráfico, una
   barra por tramos (caminata, espera, bus con tráfico, caminata, margen) y una
   explicación en lenguaje sencillo. Aviso cuando la hora cambia, indicando el
   motivo. Lista "Tus próximas salidas" con la hora de cada día.
5. Ajustar margen (RF-06). Nivel de seguridad (P70, P85, P95) y margen personal
   de 0 a 15 minutos, con vista previa de la nueva hora de salida.
6. Registro del viaje (RF-07). "Ya salí" pide la hora de salida y pasa al estado
   "En camino"; "Marcar llegada" pide la hora de llegada, guarda el viaje y
   actualiza la estimación.
7. Mis llegadas. Llegadas a tiempo en los últimos 30 días, error promedio de la
   estimación, viajes de la semana y exportación de registros en CSV (RF-08).

FUNCIONES DE LA APP
- Varios recorridos (desde la casa, el trabajo, otro lugar): crear, editar,
  eliminar y escoger cuál está en uso, también desde Inicio. El historial y el
  aprendizaje se llevan por recorrido.
- En Horario, al tocar una clase se muestra su hora de salida para la próxima
  vez que ocurra; cada clase muestra esa fecha y esa hora en la lista.

MOTOR DE CÁLCULO (caja blanca, según la propuesta)
- E = estimación externa de hoy: caminata + espera + bus con el tráfico de hoy
  + caminata hasta el salón.
- H = promedio de la duración real de los viajes del usuario en esa franja
  horaria y con ese recorrido; n = número de esos viajes.
- Duración esperada ponderada: D = w · H + (1 − w) · E, con w = n / (n + k) y
  k = 5 (al quinto viaje, historial y tráfico pesan 50 % cada uno).
- Incertidumbre: Dq = D + zq · σ, donde σ es la desviación estándar de los
  residuos (duración real − duración estimada) y zq depende del nivel de
  puntualidad: P70 → 0,52, P85 → 1,04, P95 → 1,645.
- Hora de salida: S = T − (Dq + M), donde T es la hora de la clase y M el margen
  personal (5 min por defecto).
- Modo degradado: si la API de tráfico no responde, w = 1 (100 % historial); si
  además hay menos de tres viajes, se usa el tiempo declarado con σ = 20 % de
  ese tiempo y confianza baja.
- El tráfico simulado depende de la franja horaria, el día de la semana y una
  variación propia de cada día.
- En Inicio, explicar en palabras cuánto pesa el historial y cuánto el tráfico
  de hoy (por ejemplo: "Tu historial pesa 69 % y el tráfico de hoy 31 %").

DATOS Y BACKEND SIMULADOS
- Datos del dominio: tres recorridos (Kennedy → B12, Chapinero → B23,
  Suba → C17 + B23), siete clases en la semana y diecinueve viajes de los
  últimos 30 días.
- API falsa con la misma forma que tendrá el backend (consultarTrafico,
  guardarRuta, guardarHorario, registrarViaje), con latencia simulada.
- Persistencia en localStorage.
- No usar etiquetas <form>; manejar los envíos con botones, para que funcione en
  vistas previas que bloquean formularios.

ESTADOS
Carga (esqueletos y botones con indicador), vacíos, errores de validación, API
sin respuesta y mensajes de éxito que indiquen cómo cambió la hora de salida.

PANEL DE PRUEBA
Fuera del teléfono y solo para quien conduce la prueba. Contiene únicamente
escenarios: día normal, lluvia, trancón, lluvia y trancón, bus demorado,
manifestación en la ruta, vías despejadas, sin conexión con la API y salida
tardía. Al elegir uno, la app vuelve a Inicio e informa el cambio. Incluye
además "Cargar demo" y "Empezar de cero".

DISEÑO
Móvil primero: dentro de un marco de celular en escritorio y a pantalla
completa en el teléfono. Tipografía Plus Jakarta Sans. Verde #0e7a5a para "a
tiempo", ámbar #d98c1f para margen y alertas, rojo #c43d33 para "tarde". Íconos
en línea, sin emojis. Textos en español de Colombia, cercanos y breves.
```

---

## Iteraciones

| # | Ajuste | Motivo |
|---|---|---|
| 1 | Revisión del flujo completo en escritorio y en celular (390 px) | Validar que la tarea principal se complete sin ayuda |
| 2 | La barra de tramos muestra solo el número en tramos cortos | Las etiquetas se cortaban en pantallas angostas |
| 3 | Ajuste de la ventana de 30 días del tablero | Coherencia con el mockup (17 de 19 llegadas a tiempo) |
| 4 | Tráfico variable por franja y día; lista de próximas salidas y aviso de cambio de hora | La hora de salida casi no reaccionaba a los cambios de tráfico o de recorrido |
| 5 | Envíos con botones en lugar de formularios y mensajes que explican el efecto de cada acción | Algunas vistas previas bloqueaban los formularios |
| 6 | Controles para simular otros días | Probar escenarios distintos al día actual |
| 7 | Panel de prueba reducido a escenarios; selección de clases en Horario, varios recorridos y exportación pasan a la app; se retira la vista del cálculo | Separar lo que es simulación de lo que es funcionalidad del producto |
| 8 | Emojis reemplazados por texto e íconos en línea | Consistencia visual con el resto de la interfaz |
| 9 | Se restituye la fórmula de la propuesta, D = w·H + (1 − w)·E, y se elimina el ajuste de incertidumbre por escenario; la variación proviene del tráfico de cada día y del peso w, que se explica en pantalla | Coherencia con el documento de propuesta |
