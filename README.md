ProyectoGrupalTajamar
Aplicación web para el alumnado y el profesorado de todos los másteres de Tajamar. Reúne en un mismo sitio el calendario de eventos y hackatones, la formación de equipos y la organización de viajes en coche compartido a Tajamar o a un evento.

Estado del proyecto · Diseño funcional en curso. Este README recoge los acuerdos del equipo a partir de la especificación v2 del 20 de noviembre de 2026.
Eventos y hackatones
- El calendario se abre en vista mensual y también ofrece vistas de semana, día y agenda.
- Cualquier usuario registrado y activo puede publicar eventos y hackatones. Los datos comunes incluyen nombre, dirección, fecha, hora y estado de finalización. La web es obligatoria en eventos y opcional en hackatones. Si ya existe un evento con la misma web y fecha, se muestra un aviso. La comprobación se aplica cuando se ha indicado una web.
- El botón Asistiré registra la asistencia, actualiza el contador y da acceso a los trayectos vinculados.
- Cada hackatón tiene una ficha vinculada a un único evento. La fecha y el lugar se mantienen en el evento, y la ficha añade descripción, equipos, resultados y galería. Al finalizar, se pueden añadir enlaces a repositorios y demos de los equipos.
- Los equipos son públicos. Quien crea un equipo fija su capacidad y las plazas libres se actualizan al entrar o salir miembros. Cada usuario puede pertenecer a un solo equipo por hackatón. El buscador usa perfiles de habilidades para sugerir equipos que necesitan ese perfil.
Coche compartido
El conductor confirma y publica un trayecto de ida, vuelta o ida y vuelta, con fecha, hora y número de plazas. Los usuarios activos pueden buscar trayectos a Tajamar o a un evento en un mapa con filtros, limitado a la Comunidad de Madrid. En el mapa se muestran el conductor, las plazas libres, si admite paradas y una ruta aproximada con los puntos de recogida acordados. Las direcciones particulares quedan ocultas.
Solicitud y reserva
1. El viajero solicita una plaza. La solicitud queda Pendiente y se abre un chat privado con el conductor. Puede pedir una parada si el conductor la permite.
2. Si el conductor acepta, se reserva la plaza y el viajero entra en el chat de grupo. El grupo se crea con la primera aceptación. Si rechaza la solicitud, se cierra el chat privado para nuevos mensajes.
3. Si se acepta una parada o un desvío, se recalculan la ruta y la hora estimada, se actualiza el mapa y se avisa a los ocupantes.
Estado	Cuándo se aplica
Pendiente	El viajero espera la decisión del conductor.
Aceptada	El conductor confirma y se reserva una plaza.
Rechazada	El conductor rechaza la solicitud y se cierra el chat privado.
Retirada	El viajero retira la solicitud antes de que se resuelva.
Cancelada	Un ocupante se baja del viaje, sale del grupo y libera su plaza.
Cerrada	Se cierran las solicitudes pendientes al llenarse el trayecto, al completarse o al cancelarlo el conductor. Se avisa al viajero.


Si un trayecto se llena, desaparece de las búsquedas y sigue visible para el conductor y los ocupantes. Si se libera una plaza con el trayecto aún publicado, vuelve a aparecer. Los trayectos pasados quedan fuera del mapa y de las búsquedas.

Si un evento cambia de hora o se cancela, se avisa a los asistentes y a los conductores con trayectos vinculados. Cada conductor confirma si actualiza o cancela su viaje y se comunica la decisión a los ocupantes. Si vence el plazo sin respuesta, se les avisa y el trayecto queda Pendiente de revisión.

También se han definido alertas para búsquedas guardadas, valoraciones después del viaje, puntos de encuentro sugeridos, avisos de llegada o retraso, coste compartido orientativo por kilómetros y un contador de ahorro de CO₂.
Funciones comunes
- Mi agenda reúne los eventos, hackatones, equipos y viajes del usuario en un único calendario.
- Recordatorios mediante exportación .ics, enlaces a Google Calendar y Outlook, y un bot de Telegram.
- Notificaciones push para recordatorios, mensajes de chat y cambios de ruta.
- Asistente de eventos y hackatones mediante la API de Claude, con información de la base de datos.
- Moderación mediante denuncias y bloqueo de usuarios. El rol ADMINISTRADOR puede gestionar usuarios, eventos y denuncias desde su panel.
Tecnología
Área	Tecnología definida
Aplicación y base de datos	ASP.NET Core y SQL Server
Mapa	Leaflet y OpenStreetMap
Cálculo y actualización de rutas	OpenRouteService
Chat y avisos en tiempo real	SignalR
Experiencia móvil y notificaciones	PWA y notificaciones push
Asistente	API de Claude


Datos, acceso y reservas
- Se reutilizan las tablas ROLESCHARLASTAJAMAR, CURSOSTAJAMAR, USUARIOSTAJAMAR y CURSOSUSUARIOSTAJAMAR, que tienen identificadores manuales. Los roles existentes son profesor, alumno y administrador. El acceso y la creación de contenido se limitan a usuarios con ESTADO = 1.
- El modelo propone tablas nuevas con claves IDENTITY para eventos y asistentes, hackatones y equipos, trayectos y solicitudes, paradas, chats, suscripciones push, valoraciones y denuncias.
- La pertenencia a un equipo por hackatón se protege con un índice único sobre (IDHACKATON, IDUSUARIO). La asignación de plazas se realiza en una transacción que comprueba la disponibilidad para evitar que dos usuarios ocupen la última plaza.
- Para reservar una plaza en un coche, la comprobación de disponibilidad, la reserva y la actualización del estado de la solicitud se realizan en la misma transacción. Solo puede haber una solicitud activa por usuario y trayecto. El conductor queda excluido de las solicitudes para su propio viaje.
Pendientes
- Confirmar con el responsable del sistema cómo se validan hoy las credenciales y qué representa el hash repetido. La migración a PasswordHasher de ASP.NET Identity tras el primer acceso queda como propuesta hasta aclararlo.
- Revisar las fechas del curso 3597, cuya fecha de fin es anterior a la de inicio.
- Concretar el plazo para que los conductores respondan ante cambios o cancelaciones de eventos.
- Completar el modelo de datos en SQL, preparar el plan de implementación y recoger nuevas propuestas del equipo.
