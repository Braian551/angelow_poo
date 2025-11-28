# Integrantes:

Lilliana Urbe

# Sistema de Gestión de Tienda Virtual - Arquitectura Limpia

Voy a crear un sistema de gestión de tienda virtual modular, aplicando los principios de arquitectura limpia.

## 📋 Requerimientos del Sistema

### Requerimientos Funcionales, Reglas de Negocio y Requisitos de Información

| Módulo | Subprocesos | Nro. (RF) | Descripción (RF) | Nro. (RN) | Descripción (RN) | Nro. (RI) | Descripción (RI) |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Gestión de Usuarios y Accesos | Registro de usuarios | RF-001 | El sistema debe permitir a los visitantes registrarse creando una cuenta nueva con correo electrónico, nombre completo, teléfono y contraseña. | RN-001 | Para crear una cuenta, es obligatorio aceptar los términos y condiciones de la tienda. El correo electrónico no puede estar repetido en el sistema. | RI-001 | El sistema debe capturar el Nombre y Apellidos completos del usuario, una Dirección de Correo Electrónico válida y única, un Número de Teléfono de contacto, una Contraseña secreta creada por el usuario, y registrar la Fecha y Hora exacta de la creación de la cuenta. |
| | Inicio de sesión | RF-002 | El sistema debe permitir a los usuarios registrados iniciar sesión con su correo electrónico y contraseña, con opción de "Recordar mi cuenta". | RN-002 | Si el usuario elige "Recordar mi cuenta", no tendrá que ingresar sus datos cada vez que entre por los próximos 30 días. Si no, la sesión se cierra al salir del navegador. | RI-002 | El sistema debe validar las Credenciales de Acceso (correo y contraseña) ingresadas y, si se solicita, generar y almacenar un Identificador de Sesión Seguro en el dispositivo del usuario para reconocerlo en futuras visitas. |
| | Ver perfil de cliente | RF-003 | El sistema debe permitir a los clientes ver su información personal completa en su perfil. | RN-003 | Cada cliente solo tiene acceso a su propia información privada. Nadie más puede ver sus datos personales. | RI-003 | El sistema debe mostrar la Información del Perfil incluyendo: Nombre completo, Correo electrónico registrado, Número de teléfono, Imagen de perfil actual y la Fecha del último acceso al sistema. |
| | Editar perfil de cliente | RF-004 | El sistema debe permitir a los clientes modificar su información personal: nombre, teléfono y foto de perfil. | RN-004 | El cliente puede actualizar sus datos de contacto y foto. El correo electrónico no se puede cambiar directamente por seguridad (requiere soporte). | RI-004 | El sistema debe recibir y actualizar los Datos Modificables: Nuevo Nombre completo, Nuevo Número de teléfono y el Archivo de la nueva Foto de perfil, registrando la Fecha de la última actualización. |
| | Registro con Google | RF-005 | El sistema debe permitir a los visitantes registrarse usando su cuenta de Google. | RN-005 | Al usar Google, la cuenta se crea automáticamente tomando el nombre y correo de Google. No hace falta crear una contraseña nueva. | RI-005 | El sistema debe obtener y almacenar los Datos de la Cuenta de Google: Nombre completo, Dirección de correo electrónico, Foto de perfil pública y el Identificador único proporcionado por Google para vincular la cuenta. |
| | Inicio de sesión con Google | RF-006 | El sistema debe permitir a los usuarios iniciar sesión usando su cuenta de Google. | RN-006 | El usuario entra con un solo clic validando su identidad con Google. Es más rápido y seguro que escribir contraseñas. | RI-006 | El sistema debe procesar la Confirmación de Identidad enviada por Google, que incluye el correo electrónico verificado y el token de acceso seguro para permitir el ingreso sin contraseña. |
| | Agregar dirección | RF-007 | El sistema debe permitir a los clientes agregar nuevas direcciones de envío con ubicación en el mapa. | RN-007 | Es obligatorio ubicar la dirección en el mapa para que el repartidor llegue exacto. Se debe incluir quién recibe y su teléfono. | RI-007 | El sistema debe guardar los Detalles de la Dirección: Dirección escrita completa (calle, número), Barrio, Ciudad, Coordenadas Geográficas exactas (Latitud y Longitud) seleccionadas en el mapa, Nombre de la persona que recibe y su Teléfono de contacto. |
| | Ver direcciones | RF-008 | El sistema debe permitir a los clientes ver todas sus direcciones guardadas. | RN-008 | El cliente puede tener varias direcciones (casa, oficina, padres) y verlas en una lista para elegir fácil. | RI-008 | El sistema debe listar todas las Direcciones Registradas del usuario, mostrando para cada una: Alias o nombre del lugar, Dirección completa, Ciudad, Nombre del receptor y Teléfono asociado. |
| | Editar dirección | RF-009 | El sistema debe permitir a los usuarios modificar cualquier dato de sus direcciones guardadas. | RN-009 | Si el cliente se muda o quiere cambiar indicaciones de entrega, puede actualizar la información en cualquier momento. | RI-009 | El sistema debe actualizar la Información de la Dirección seleccionada, permitiendo cambiar: Calle y número, Ubicación en el mapa, Instrucciones de entrega, Nombre del receptor y Teléfono de contacto. |
| | Eliminar dirección | RF-010 | El sistema debe permitir a los clientes eliminar direcciones que ya no utilizan. | RN-010 | El sistema preguntará antes de borrar para evitar errores. No se puede borrar una dirección si hay un pedido en camino a ella. | RI-010 | El sistema debe procesar la Solicitud de Eliminación identificando la dirección específica a borrar y confirmando que no existen pedidos activos asociados a ella antes de eliminarla permanentemente. |
| | Control de acceso por roles | RF-011 | El sistema debe dirigir a cada usuario a su panel correspondiente (cliente o administrador). | RN-011 | Los clientes van a su área de compras y pedidos; los administradores van al panel de control de la tienda. | RI-011 | El sistema debe verificar el Rol del Usuario (Cliente o Administrador) almacenado en su perfil y determinar la URL de Destino correcta (Panel de Cliente o Panel de Administración) para redirigirlo. |
| | Restricción de acceso | RF-012 | El sistema debe impedir que usuarios no autorizados entren a secciones privadas. | RN-012 | Si un cliente intenta entrar al área de administración, el sistema lo bloquea y lo envía de vuelta a la tienda. | RI-012 | El sistema debe comprobar los Permisos de Acceso requeridos para la página solicitada y compararlos con el Rol del Usuario actual para Autorizar o Denegar la visualización del contenido. |
| | Ver configuración | RF-013 | El sistema debe permitir a los clientes ver sus preferencias de cuenta y notificaciones. | RN-013 | El cliente puede revisar qué correos quiere recibir y el estado de seguridad de su cuenta. | RI-013 | El sistema debe mostrar las Preferencias Actuales del usuario: Estado de suscripción a boletines (Sí/No), Configuración de alertas de pedidos y Opciones de privacidad de la cuenta. |
| | Actualizar configuración | RF-014 | El sistema debe permitir a los clientes cambiar su contraseña y preferencias | RN-014 | Para cambiar la contraseña, se pide la actual por seguridad. El cliente decide si quiere recibir promociones o no. | RI-014 | El sistema debe recibir y guardar las Nuevas Preferencias: Cambio de contraseña (requiere contraseña actual y nueva), y cambios en la configuración de recepción de correos y notificaciones. |
| | Registro automático de cambios | RF-015 | El sistema debe guardar un historial de seguridad de cambios importantes en la cuenta. | RN-015 | Cada vez que se cambia una contraseña o dato sensible, queda registrado por seguridad. Esto no se puede borrar. | RI-015 | El sistema debe registrar automáticamente los Detalles del Evento: Tipo de cambio realizado (ej: cambio de contraseña), Fecha y Hora del cambio, Dirección IP desde donde se hizo y el Usuario que realizó la acción. |
| | Consulta de auditoría | RF-016 | El sistema debe permitir a los administradores ver el historial de cambios de los usuarios. | RN-016 | Sirve para investigar problemas de seguridad o reclamos sobre cambios en las cuentas. | RI-016 | El sistema debe generar un Reporte de Auditoría que muestre cronológicamente todas las acciones de seguridad y cambios de perfil realizados por un usuario específico. |
| | Login con email o teléfono | RF-017 | El sistema debe permitir entrar usando el correo o el número de teléfono. | RN-017 | Da flexibilidad al usuario para recordar su usuario. Ambos funcionan con la misma contraseña. | RI-017 | El sistema debe aceptar como Identificador de Acceso tanto una dirección de correo electrónico válida como un número de teléfono registrado, junto con la contraseña asociada para validar el ingreso. |
| | Recordar mi cuenta (Remember Me) | RF-018 | El sistema debe mantener la sesión abierta si el usuario lo solicita. | RN-018 | Usa una tecnología segura para recordar al usuario sin poner en riesgo su contraseña. Dura 30 días. | RI-018 | El sistema debe generar y almacenar un Token de Persistencia seguro en el navegador del usuario, asociado a su cuenta, con una fecha de expiración definida (30 días) para permitir el acceso automático. |
| | Registro de intentos fallidos | RF-019 | El sistema debe contar los intentos fallidos de contraseña para detectar ataques. | RN-019 | Si alguien se equivoca muchas veces seguidas, el sistema toma nota para proteger la cuenta. | RI-019 | El sistema debe registrar cada Intento Fallido de Acceso, guardando: el Usuario o Correo intentado, la Dirección IP de origen, y la Fecha y Hora exacta del intento para monitorear seguridad. |
| | Bloqueo automático de cuenta | RF-020 | El sistema debe bloquear la cuenta temporalmente tras 5 intentos fallidos. | RN-020 | Si fallan 5 veces en una hora, la cuenta se bloquea para evitar que adivinen la contraseña. | RI-020 | El sistema debe gestionar el Estado de Bloqueo de la cuenta, registrando el motivo (exceso de intentos), la hora de inicio del bloqueo y el tiempo de duración hasta el desbloqueo automático. |
| | Redirección post-login | RF-021 | El sistema debe llevar al cliente a la página que quería ver después de loguearse. | RN-021 | Si el usuario quería ver un producto y se le pidió login, al entrar debe volver a ese producto, no al inicio. | RI-021 | El sistema debe recordar temporalmente la URL de la Página Solicitada antes del inicio de sesión para redirigir al cliente a esa misma dirección exacta una vez que se haya identificado correctamente. |
| | Recuperar contraseña - Solicitar código | RF-022 | El sistema debe permitir que el usuario escriba su correo y pida un código para recuperar su contraseña. | RN-022 | Solo envía el código si el correo está registrado, muestra un aviso claro y limita cuántas solicitudes seguidas se pueden hacer. | RI-022 | Debe recibir el correo capturado, registrar la hora en que se generó el código y guardar el estado de la solicitud. |
| | Recuperar contraseña - Verificar código | RF-023 | El sistema debe mostrar un espacio para escribir el código recibido y confirmar que coincide. | RN-023 | El código dura unos minutos, se usa una sola vez y, si es incorrecto, el sistema explica qué pasó y deja volver a intentarlo. | RI-023 | Debe recibir el código ingresado, relacionarlo con la cuenta correcta y marcar si quedó aprobado, vencido o rechazado. |
| | Recuperar contraseña - Crear nueva contraseña | RF-024 | El sistema debe permitir escribir y confirmar una nueva contraseña cuando el código ya fue validado. | RN-024 | Solo guarda la contraseña si ambos campos coinciden, cumplen la longitud mínima y al finalizar avisa que la clave cambió. | RI-024 | Debe recibir la nueva contraseña y su confirmación, registrar la fecha del cambio y limpiar los códigos usados para evitar reutilizarlos. |
| Gestión de Administradores | Listar administradores | RF-025 | El sistema debe mostrar una lista de todas las personas con acceso administrativo. | RN-025 | Solo el administrador principal debe poder ver quién más tiene acceso al panel de control. | RI-025 | El sistema debe mostrar el Listado de Personal Administrativo incluyendo: Nombre completo, Dirección de correo electrónico corporativa o personal, Rol asignado y Estado actual de la cuenta (Activo/Inactivo). |
| | Crear administrador | RF-026 | El sistema debe permitir dar acceso de administrador a nuevas personas. | RN-026 | Se crea una cuenta especial con permisos totales para gestionar la tienda. | RI-026 | El sistema debe capturar los Datos del Nuevo Administrador: Nombre completo, Correo electrónico para acceso, y una Contraseña inicial segura, asignándole automáticamente el perfil de permisos de administrador. |
| | Editar administrador | RF-027 | El sistema debe permitir actualizar los datos de otros administradores. | RN-027 | Permite corregir nombres o actualizar correos del equipo de trabajo. | RI-027 | El sistema debe permitir modificar los Datos del Administrador existente: Nombre completo y Correo electrónico, manteniendo el historial de quién realizó la modificación. |
| | Eliminar administrador | RF-028 | El sistema debe permitir quitar el acceso a administradores que ya no trabajan. | RN-028 | Al eliminarlo, esa persona pierde inmediatamente el acceso al panel de control. | RI-028 | El sistema debe procesar la Eliminación de la Cuenta Administrativa, borrando sus credenciales de acceso y revocando todos los permisos de entrada al panel de control. |
| | Cambiar estado administrador | RF-029 | El sistema debe permitir desactivar temporalmente a un administrador. | RN-029 | Útil si alguien está de vacaciones o licencia; se le quita el acceso sin borrar su cuenta. | RI-029 | El sistema debe actualizar el Estado de la Cuenta (de Activo a Inactivo o viceversa), impidiendo el inicio de sesión mientras el estado sea Inactivo, sin borrar los datos del usuario. |
| Gestión de Productos e Inventario | Registrar producto | RF-030 | El sistema debe permitir crear nuevos productos en el catálogo. | RN-030 | Se debe ingresar toda la información necesaria para vender: nombre, precio, categoría y descripción. | RI-030 | El sistema debe capturar la Ficha del Producto completa: Nombre comercial, Descripción detallada, Precio de venta, Categoría a la que pertenece, Género (Niño/Niña/Bebé) y Estado inicial (Activo/Borrador). |
| | Mostrar productos | RF-031 | El sistema debe mostrar una lista completa de todos los productos registrados. | RN-031 | Permite al administrador ver todo su inventario de un vistazo y buscar productos específicos. | RI-031 | El sistema debe generar el Inventario de Productos mostrando: Imagen principal en miniatura, Nombre del producto, Precio actual, Categoría, Stock total disponible y Estado de publicación. |
| | Actualizar producto | RF-032 | El sistema debe permitir modificar cualquier dato de un producto existente. | RN-032 | Los cambios (como precio nuevo) se ven reflejados inmediatamente en la tienda para los clientes. | RI-032 | El sistema debe recibir y guardar los Datos Actualizados del producto: Nuevo nombre, Nueva descripción, Nuevo precio, Cambio de categoría o Cambio de estado, registrando la fecha de la modificación. |
| | Eliminar producto | RF-033 | El sistema debe permitir borrar productos que ya no se van a vender. | RN-033 | No se puede borrar un producto si hay pedidos pendientes que lo incluyen, para no perder el historial. | RI-033 | El sistema debe identificar el Producto a Eliminar y verificar en la base de datos que no existan Pedidos Activos vinculados a él antes de proceder con el borrado definitivo. |
| | Gestión de Categorías (Master) | RF-034 | El sistema debe permitir crear y organizar las categorías de la tienda. | RN-034 | Las categorías (ej: Vestidos, Zapatos) agrupan los productos. Se pueden crear, editar o borrar. | RI-034 | El sistema debe gestionar la Información de Categorías: Nombre de la categoría, Descripción breve, Imagen representativa y Estado (Visible/Oculta) para organizar el catálogo. |
| | Visualizar categorías | RF-035 | El sistema debe mostrar la lista de categorías disponibles. | RN-035 | Permite ver qué categorías existen y cuántos productos tiene cada una. | RI-035 | El sistema debe listar todas las Categorías Existentes, mostrando su Nombre, Imagen, Estado y un Contador de cuántos productos pertenecen actualmente a cada una. |
| | Gestión de Tallas (Master) | RF-036 | El sistema debe permitir definir qué tallas maneja la tienda. | RN-036 | Se crea una lista maestra de tallas (S, M, L, 36, 38) para usar en los productos. | RI-036 | El sistema debe administrar el Catálogo Maestro de Tallas, almacenando el Nombre de la talla (ej: "M", "40") y una Descripción opcional para estandarizar las medidas en la tienda. |
| | Agregar variante de color | RF-037 | El sistema debe permitir añadir colores disponibles a un producto. | RN-037 | Cada color genera una referencia única para controlar el inventario de ese color específico. | RI-037 | El sistema debe registrar la Variante de Color para un producto, guardando el Nombre del color, su Código visual (Hexadecimal) y generando un Código de Referencia (SKU) único para esa combinación. |
| | Mostrar variantes de color | RF-038 | El sistema debe mostrar qué colores tiene un producto. | RN-038 | Permite ver las opciones de color creadas y sus fotos específicas. | RI-038 | El sistema debe listar los Colores Disponibles para el producto seleccionado, mostrando la muestra de color, el nombre del color y las imágenes asociadas a esa variante. |
| | Eliminar variante de color | RF-039 | El sistema debe permitir quitar un color de un producto. | RN-039 | Si ya no se vende ese color, se elimina. El sistema revisa que no haya pedidos pendientes de ese color. | RI-039 | El sistema debe identificar la Variante de Color a eliminar y confirmar que no hay stock comprometido en pedidos pendientes antes de borrarla de las opciones del producto. |
| | Agregar variante de talla | RF-040 | El sistema debe permitir asignar tallas a un producto. | RN-040 | Se define qué tallas están disponibles para cada producto y cuánto stock hay de cada una. | RI-040 | El sistema debe vincular una Talla del catálogo maestro al producto y registrar la Cantidad de Stock Inicial disponible específicamente para esa talla en ese producto. |
| | Mostrar variantes de talla | RF-041 | El sistema debe mostrar el stock por talla de cada producto. | RN-041 | Permite ver rápidamente cuántas unidades quedan de cada talla (ej: 5 en S, 0 en M). | RI-041 | El sistema debe mostrar el Desglose de Stock por Talla, listando cada talla habilitada para el producto y la cantidad exacta de unidades disponibles en inventario. |
| | Eliminar variante de talla | RF-042 | El sistema debe permitir dejar de ofrecer una talla en un producto. | RN-042 | Se quita la talla de las opciones de compra. Verifica que no haya pedidos pendientes. | RI-042 | El sistema debe procesar la Eliminación de la Talla del producto, verificando previamente que no existan unidades de esa talla reservadas en carritos o pedidos activos. |
| | Ver inventario general | RF-043 | El sistema debe mostrar un reporte de todo el stock de la tienda. | RN-043 | Vista general para saber qué productos se están agotando y necesitan reposición. | RI-043 | El sistema debe generar un Reporte de Inventario Total que incluya: Nombre del producto, Variante (Color/Talla), Cantidad disponible y Alertas de stock bajo para reposición. |
| | Ajustar stock manualmente | RF-044 | El sistema debe permitir corregir la cantidad de stock a mano. | RN-044 | Si se rompe algo o se cuenta mal, el admin puede corregir el número. Debe decir por qué lo cambió. | RI-044 | El sistema debe registrar el Ajuste de Inventario: Producto y variante afectada, Nueva cantidad de stock, Motivo del ajuste (ej: "Daño", "Error de conteo") y Usuario que realizó el cambio. |
| | Ver historial de movimientos | RF-045 | El sistema debe mostrar el historial de entradas y salidas de mercancía. | RN-045 | Permite rastrear por qué cambió el stock (venta, devolución, ajuste manual). | RI-045 | El sistema debe mostrar el Kardex o Historial de Movimientos, detallando para cada cambio: Fecha y hora, Tipo de movimiento (Entrada/Salida), Cantidad, Saldo resultante y Responsable. |
| | Actualización automática por ventas | RF-046 | El sistema debe restar el stock automáticamente cuando se vende algo. | RN-046 | Al confirmarse una compra, las unidades se descuentan solas para no vender lo que no hay. | RI-046 | El sistema debe procesar la Deducción de Inventario, identificando los productos comprados en la orden y restando la cantidad exacta vendida del stock disponible de cada variante. |
| | Subir imágenes generales | RF-047 | El sistema debe permitir subir fotos del producto. | RN-047 | Las fotos deben ser de buena calidad. Se pueden subir varias para mostrar detalles. | RI-047 | El sistema debe recibir y almacenar los Archivos de Imagen del producto, guardando su ruta de acceso y permitiendo definir cuál será la imagen principal o de portada. |
| | Subir imágenes por variante de color | RF-048 | El sistema debe permitir subir fotos específicas para cada color. | RN-048 | Cuando el cliente elige "Rojo", la foto debe cambiar para mostrar el producto rojo. | RI-048 | El sistema debe asociar Archivos de Imagen Específicos a una Variante de Color, para que se muestren solo cuando el cliente selecciona ese color en la tienda. |
| | Visualizar imágenes | RF-049 | El sistema debe mostrar una galería con todas las fotos del producto. | RN-049 | Permite organizar el orden en que aparecen las fotos en la tienda. | RI-049 | El sistema debe desplegar la Galería de Imágenes del producto, mostrando todas las fotos cargadas (generales y por color) y permitiendo gestionar su orden de visualización. |
| | Eliminar imágenes | RF-050 | El sistema debe permitir borrar fotos del producto. | RN-050 | Si una foto ya no sirve o está mal, se puede quitar. No deja borrar todas (debe quedar una). | RI-050 | El sistema debe eliminar el Archivo de Imagen seleccionado del servidor y borrar su referencia en la base de datos, actualizando la galería del producto. |
| | Registro automático de cambios | RF-051 | El sistema debe guardar un historial de cambios en los productos. | RN-051 | Si alguien cambia un precio, queda registrado quién fue y cuándo para control. | RI-051 | El sistema debe registrar automáticamente cualquier Modificación del Producto: Campo cambiado (ej: Precio), Valor anterior, Valor nuevo, Usuario que hizo el cambio y Fecha del evento. |
| | Consulta de auditoría | RF-052 | El sistema debe permitir investigar cambios en el inventario o productos. | RN-052 | Herramienta para que el dueño sepa qué pasa con su catálogo e inventario. | RI-052 | El sistema debe permitir consultar el Registro de Actividad de Productos, filtrando por fecha o producto para ver quién modificó qué y cuándo. |
| Experiencia de Compra | Agregar al carrito | RF-053 | El sistema debe permitir al cliente poner productos en su carrito de compras. | RN-053 | El cliente elige color, talla y cantidad. El sistema revisa si hay stock antes de agregar. | RI-053 | El sistema debe capturar la Selección de Compra: Producto elegido, Color específico, Talla seleccionada y Cantidad de unidades, verificando disponibilidad antes de añadirlo al carrito. |
| | Ver carrito | RF-054 | El sistema debe mostrar qué productos tiene el cliente en su carrito. | RN-054 | Muestra un resumen con fotos, precios y el total a pagar hasta el momento. | RI-054 | El sistema debe mostrar el Detalle del Carrito: Lista de productos agregados con su foto, nombre, características (talla/color), precio unitario, cantidad y el Subtotal calculado. |
| | Actualizar cantidad | RF-055 | El sistema debe permitir cambiar la cantidad de productos en el carrito. | RN-055 | El cliente puede subir o bajar la cantidad. El total se recalcula solo. | RI-055 | El sistema debe recibir la Nueva Cantidad deseada para un producto en el carrito, verificar nuevamente el stock y recalcular el Precio Total de la compra. |
| | Eliminar del carrito | RF-056 | El sistema debe permitir sacar productos del carrito. | RN-056 | Si el cliente se arrepiente, puede quitar el producto con un clic. | RI-056 | El sistema debe procesar la Eliminación del Ítem del carrito, quitando el producto de la lista y actualizando inmediatamente el monto total a pagar. |
| | Compra inmediata | RF-057 | El sistema debe tener un botón "Comprar ahora" para ir directo al pago. | RN-057 | Agiliza la compra saltándose pasos intermedios si el cliente solo quiere ese producto. | RI-057 | El sistema debe iniciar una Orden de Compra Directa con el producto seleccionado, omitiendo la vista previa del carrito y llevando al usuario directamente al proceso de pago. |
| | Buscar productos | RF-058 | El sistema debe tener un buscador para encontrar productos por nombre. | RN-058 | Debe ser inteligente y sugerir productos mientras el cliente escribe. | RI-058 | El sistema debe procesar el Término de Búsqueda ingresado por el usuario y devolver una lista de Productos Coincidentes por nombre o descripción. |
| | Guardar historial de búsquedas | RF-059 | El sistema debe recordar qué buscan los clientes. | RN-059 | Ayuda a saber qué le interesa a la gente para mejorar el catálogo. | RI-059 | El sistema debe registrar cada Palabra Clave buscada por los usuarios, junto con la fecha y hora, para análisis estadístico de intereses. |
| | Mostrar búsquedas populares | RF-060 | El sistema debe sugerir lo que más busca la gente. | RN-060 | Muestra tendencias (ej: "Zapatos rojos") para inspirar a otros clientes. | RI-060 | El sistema debe calcular y mostrar las Tendencias de Búsqueda, listando los términos más frecuentes buscados por todos los usuarios en los últimos días. |
| | Búsquedas sin resultados | RF-061 | El sistema debe reportar qué buscan los clientes y no encuentran. | RN-061 | Si mucha gente busca "Gorras" y no hay, es una oportunidad de negocio. | RI-061 | El sistema debe registrar específicamente las Búsquedas Fallidas (términos que no arrojaron ningún producto) para identificar demandas insatisfechas en el catálogo. |
| | Filtrar por categoría | RF-062 | El sistema debe permitir ver solo productos de una categoría (ej: Solo Niñas). | RN-062 | Facilita encontrar productos navegando por grupos. | RI-062 | El sistema debe filtrar la visualización de productos basándose en la Categoría Seleccionada, mostrando únicamente los artículos que pertenecen a ese grupo. |
| | Filtrar por rango de precio | RF-063 | El sistema debe permitir filtrar por presupuesto (mínimo y máximo). | RN-063 | El cliente define cuánto quiere gastar y ve solo lo que puede pagar. | RI-063 | El sistema debe filtrar los productos mostrando solo aquellos cuyo Precio de Venta esté dentro del Rango de Precio (Mínimo y Máximo) definido por el usuario. |
| | Filtrar por género | RF-064 | El sistema debe permitir filtrar para quién es la ropa (Niño, Niña, Bebé). | RN-064 | Ayuda a ir directo a la ropa adecuada para el usuario. | RI-064 | El sistema debe filtrar los productos según el Género Objetivo (Niño, Niña, Bebé o Unisex) seleccionado por el cliente en el menú o filtros. |
| | Agregar a Wishlist | RF-065 | El sistema debe permitir guardar productos en una lista de deseos. | RN-065 | Para productos que gustan pero no se van a comprar ya. Requiere tener cuenta. | RI-065 | El sistema debe guardar la relación entre el Usuario y el Producto Favorito, añadiéndolo a su lista personal de deseos para acceso futuro. |
| | Ver mi Wishlist | RF-066 | El sistema debe mostrar la lista de productos favoritos del cliente. | RN-066 | El cliente puede revisar su lista para comprar esos productos después. | RI-066 | El sistema debe mostrar la Lista de Deseos del usuario, con la imagen, nombre, precio y disponibilidad actual de cada producto guardado. |
| | Eliminar de Wishlist | RF-067 | El sistema debe permitir quitar productos de la lista de deseos. | RN-067 | Si ya no interesa, se borra de la lista. | RI-067 | El sistema debe eliminar el Producto seleccionado de la Lista de Deseos del usuario, dejando de mostrarlo en su sección de favoritos. |
| | Hacer pregunta sobre producto | RF-068 | El sistema debe permitir preguntar dudas en la página del producto. | RN-068 | Los clientes pueden preguntar tallas, materiales, etc. El admin responde. | RI-068 | El sistema debe capturar la Pregunta del Cliente: Texto de la consulta, Producto sobre el que pregunta, Usuario que pregunta y Fecha de la consulta. |
| | Editar mi pregunta | RF-069 | El sistema debe permitir corregir una pregunta si aún no la responden. | RN-069 | Si el cliente escribió mal, puede arreglarlo antes de que le contesten. | RI-069 | El sistema debe permitir modificar el Texto de la Pregunta realizada, siempre y cuando no tenga aún una respuesta registrada. |
| | Eliminar mi pregunta | RF-070 | El sistema debe permitir borrar una pregunta propia. | RN-070 | Si el cliente ya resolvió la duda, puede borrar la pregunta. | RI-070 | El sistema debe eliminar el registro de la Pregunta del usuario y cualquier notificación asociada a ella. |
| | Responder preguntas (Admin) | RF-071 | El sistema debe permitir al administrador contestar las dudas. | RN-071 | La respuesta queda pública para ayudar a otros con la misma duda. | RI-071 | El sistema debe registrar la Respuesta del Administrador, vinculándola a la pregunta original, y notificar al cliente que su duda ha sido resuelta. |
| | Ver reseñas de producto | RF-072 | El sistema debe mostrar las opiniones y calificaciones de otros compradores. | RN-072 | Genera confianza. Muestra estrellas (1-5) y comentarios. | RI-072 | El sistema debe mostrar las Reseñas Públicas del producto: Calificación (estrellas), Comentario de texto, Fotos del cliente (si hay) y Fecha de la opinión. |
| | Escribir reseña con imágenes | RF-073 | El sistema debe permitir opinar y subir fotos del producto recibido. | RN-073 | Solo quien compró debería opinar. Las fotos reales ayudan mucho a vender. | RI-073 | El sistema debe recibir la Opinión del Cliente: Puntuación (1 a 5), Texto de la reseña y Archivos de fotos opcionales del producto recibido. |
| | Marcar reseña como útil | RF-074 | El sistema debe permitir votar si una opinión sirvió o no. | RN-074 | Las opiniones más útiles aparecen primero. | RI-074 | El sistema debe registrar el Voto de Utilidad ("Me sirve") de un usuario sobre una reseña, actualizando el contador de utilidad de esa opinión. |
| | Editar mi reseña | RF-075 | El sistema debe permitir cambiar la opinión propia. | RN-075 | Si la experiencia cambió, el cliente puede actualizar su reseña. | RI-075 | El sistema debe permitir actualizar el contenido de la Reseña (calificación, texto o fotos) manteniendo la fecha de la última edición. |
| | Eliminar mi reseña | RF-076 | El sistema debe permitir borrar una opinión propia. | RN-076 | El cliente tiene control sobre su contenido. | RI-076 | El sistema debe eliminar permanentemente la Reseña del usuario, incluyendo el texto, la calificación y las fotos asociadas. |
| | Sugerencias personalizadas | RF-077 | El sistema debe recomendar productos que le puedan gustar al cliente. | RN-077 | Basado en lo que ha visto o comprado antes. | RI-077 | El sistema debe generar una Lista de Productos Recomendados basada en el historial de navegación y compras recientes del usuario. |
| Proceso de Finalización de compra y Pagos | Revisar carrito | RF-078 | El sistema debe mostrar un resumen final antes de pagar. | RN-078 | Último paso para verificar que todo está bien (tallas, cantidades) antes de pagar. | RI-078 | El sistema debe presentar el Resumen de Orden Preliminar: Productos, cantidades, precios unitarios, subtotales y el monto total estimado antes de envíos. |
| | Seleccionar dirección | RF-079 | El sistema debe permitir elegir a dónde enviar el pedido. | RN-079 | Se elige de las guardadas o se crea una nueva ahí mismo. | RI-079 | El sistema debe registrar la Dirección de Envío Seleccionada para este pedido específico, incluyendo todos los datos de ubicación y contacto del receptor. |
| | Calcular envío | RF-080 | El sistema debe sumar el costo del envío al total. | RN-080 | Depende de a dónde va y cuánto compró. Se calcula solo. | RI-080 | El sistema debe calcular y mostrar el Costo de Envío aplicable según la dirección seleccionada y las reglas de tarifa vigentes, sumándolo al total. |
| | Aplicar descuento | RF-081 | El sistema debe tener una casilla para ingresar cupones de descuento. | RN-081 | Si el cupón es válido, se resta el dinero inmediatamente del total. | RI-081 | El sistema debe validar el Código de Cupón ingresado y, si es correcto, calcular el Monto Descontado y actualizar el Total a Pagar. |
| | Tipo de entrega | RF-082 | El sistema debe dar opciones de entrega (Domicilio o Recoger en tienda). | RN-082 | El cliente elige lo que prefiere. Recoger suele ser gratis. | RI-082 | El sistema debe registrar el Método de Entrega elegido por el cliente (Envío a domicilio o Recogida en punto físico) y ajustar el costo si aplica. |
| | Ver resumen final | RF-083 | El sistema debe mostrar cuánto se va a pagar en total con todo incluido. | RN-083 | Suma productos + envío - descuentos. Es el valor final a pagar. | RI-083 | El sistema debe mostrar el Desglose Financiero Final: Subtotal productos + Costo de envío - Descuentos aplicados = Total Neto a Pagar. |
| | Procesar pago por transferencia | RF-084 | El sistema debe permitir pagar transfiriendo al banco. | RN-084 | Muestra los datos de la cuenta y pide subir la foto del comprobante. | RI-084 | El sistema debe mostrar la Información Bancaria de la tienda y permitir subir el Archivo del Comprobante de Transferencia como prueba de pago. |
| | Validar comprobante y consentimiento | RF-085 | El sistema debe obligar a subir el comprobante y aceptar términos. | RN-085 | Sin comprobante no hay pedido. Sin aceptar términos no hay venta. | RI-085 | El sistema debe verificar que se haya cargado el Comprobante de Pago y que la casilla de Aceptación de Términos y Condiciones esté marcada antes de permitir finalizar. |
| | Confirmar orden | RF-086 | El sistema debe crear el pedido y confirmar al cliente. | RN-086 | Muestra un mensaje de éxito, envía un correo y vacía el carrito. | RI-086 | El sistema debe generar un Número de Orden Único, guardar todos los detalles de la compra y enviar una Confirmación de Pedido al correo del cliente. |
| | Confirmación inteligente | RF-087 | El sistema debe dar instrucciones según cómo se pidió (envío o recogida). | RN-087 | Si es recogida, muestra mapa de la tienda. Si es envío, dice cuándo llega. | RI-087 | El sistema debe mostrar Instrucciones Post-Compra personalizadas: Mapa de ubicación (si es recogida) o Tiempo estimado de entrega (si es envío). |
| | Configurar métodos de envío | RF-088 | El sistema debe permitir al admin definir formas de envío. | RN-088 | Crear opciones como "Envío Rápido", "Envío Normal" y sus precios. | RI-088 | El sistema debe guardar la Configuración de Métodos de Envío: Nombre del método, Costo base, Tiempo de entrega estimado y Estado (Activo/Inactivo). |
| | Configurar reglas de precio de envío | RF-089 | El sistema debe permitir reglas como "Envío gratis si compra más de $X". | RN-089 | Incentiva a comprar más. El sistema aplica la regla solo. | RI-089 | El sistema debe almacenar las Reglas de Costo de Envío: Rango de precio de compra (mínimo-máximo) y el Costo de envío asignado para ese rango (o gratuidad). |
| | Registrar cuenta bancaria | RF-090 | El sistema debe permitir guardar las cuentas bancarias de la tienda. | RN-090 | Son las cuentas que verá el cliente para transferir el dinero. | RI-090 | El sistema debe capturar los Datos de la Cuenta Bancaria: Nombre del Banco, Tipo de cuenta, Número de cuenta, Titular y Documento de identidad. |
| | Mostrar cuentas bancarias | RF-091 | El sistema debe listar las cuentas bancarias configuradas. | RN-091 | Para gestionar cuáles están activas y cuáles no. | RI-091 | El sistema debe listar todas las Cuentas Bancarias registradas, mostrando sus detalles y permitiendo activar o desactivar su visibilidad en el checkout. |
| | Actualizar cuenta bancaria | RF-092 | El sistema debe permitir cambiar los datos de las cuentas bancarias. | RN-092 | Si cambia el número de cuenta, se actualiza aquí para que los clientes lo vean bien. | RI-092 | El sistema debe permitir editar la Información de la Cuenta Bancaria existente para corregir errores o actualizar datos. |
| | Asistente de Direcciones GPS | RF-093 | El sistema debe ayudar a encontrar la dirección en el mapa. | RN-093 | Usa un buscador tipo Google Maps para poner el pin exacto. | RI-093 | El sistema debe integrar un servicio de mapas para obtener la Dirección Normalizada y las Coordenadas Geográficas precisas basadas en la búsqueda del usuario. |
| Gestión de Órdenes y Postventa | Listar todas las órdenes | RF-094 | El sistema debe mostrar una lista de todos los pedidos recibidos. | RN-094 | Panel principal para que el admin vea qué tiene que despachar. | RI-094 | El sistema debe mostrar el Listado General de Órdenes, incluyendo: Número de orden, Cliente, Fecha, Total, Estado del pago y Estado del envío. |
| | Ver detalle de orden | RF-095 | El sistema debe mostrar toda la información de un pedido específico. | RN-095 | Qué compró, quién es, a dónde va, si ya pagó. Todo en una pantalla. | RI-095 | El sistema debe desplegar la Información Completa de la Orden: Datos del cliente, Dirección de envío, Lista de productos comprados, Comprobante de pago y Estado actual. |
| | Cambiar estado de orden | RF-096 | El sistema debe permitir actualizar cómo va el pedido (Ej: Enviado). | RN-096 | El admin cambia el estado para informar al cliente (Pendiente -> Enviado). | RI-096 | El sistema debe registrar el Cambio de Estado de la Orden (ej: de "Preparando" a "Enviado"), actualizando la fecha del cambio y notificando al cliente. |
| | Actualizar estado de pago | RF-097 | El sistema debe permitir marcar si el pago ya entró o no. | RN-097 | El admin revisa el comprobante y dice "Pago Aprobado" o "Rechazado". | RI-097 | El sistema debe actualizar el Estado del Pago (Verificado o Rechazado) basado en la validación manual del comprobante realizada por el administrador. |
| | Eliminar orden | RF-098 | El sistema debe permitir borrar pedidos basura o cancelados. | RN-098 | Solo se borran pedidos cancelados para limpiar la lista. | RI-098 | El sistema debe procesar la Eliminación de la Orden, borrando permanentemente el registro del pedido y sus detalles asociados de la base de datos. |
| | Eliminar órdenes masivamente | RF-099 | El sistema debe permitir borrar varios pedidos a la vez. | RN-099 | Para limpiar rápido muchos pedidos cancelados antiguos. | RI-099 | El sistema debe permitir seleccionar Múltiples Órdenes y eliminarlas en una sola acción, previa confirmación de seguridad. |
| | Ver mis órdenes | RF-100 | El sistema debe permitir al cliente ver su historial de compras. | RN-100 | El cliente puede ver qué ha comprado antes y en qué estado están sus pedidos actuales. | RI-100 | El sistema debe mostrar el Historial de Compras del usuario, listando todos sus pedidos pasados y actuales con fecha, monto y estado. |
| | Ver detalle de mi orden | RF-101 | El sistema debe permitir al cliente ver el detalle de su compra. | RN-101 | Ver qué pidió, cuánto pagó y a dónde lo mandaron. | RI-101 | El sistema debe mostrar al cliente los Detalles de su Pedido: Productos, precios, dirección de entrega y estado del envío. |
| | Rastrear orden en tiempo real | RF-102 | El sistema debe mostrar una barra de progreso del pedido. | RN-102 | Gráfico visual (Recibido -> Preparando -> Enviado) para calmar ansiedad. | RI-102 | El sistema debe mostrar una Línea de Tiempo Visual que indique en qué etapa del proceso se encuentra el pedido (Recibido, Empacando, Enviado, Entregado). |
| | Enviar factura tras entrega | RF-103 | El sistema debe enviar la factura automáticamente cuando se entrega. | RN-103 | Al marcar "Entregado", el sistema manda el correo con la factura sin que el admin haga nada. | RI-103 | El sistema debe generar y enviar por correo electrónico el Archivo PDF de la Factura al cliente una vez que el estado del pedido cambie a "Entregado". |
| | Notificar cancelaciones y reembolsos | RF-104 | El sistema debe avisar si un pedido se cancela. | RN-104 | Envía un correo explicando que el pedido fue cancelado y qué pasa con el dinero. | RI-104 | El sistema debe enviar una Notificación de Cancelación al cliente, explicando el motivo de la cancelación y los pasos para el reembolso si aplica. |
| | Registro automático de cambios | RF-105 | El sistema debe guardar historial de todo lo que le pasa al pedido. | RN-105 | Trazabilidad total: se sabe quién cambió el estado y a qué hora. | RI-105 | El sistema debe registrar en la Bitácora de la Orden cada evento: cambio de estado, nota agregada o actualización de pago, con fecha y responsable. |
| | Consulta de auditoría | RF-106 | El sistema debe permitir investigar la historia de un pedido. | RN-106 | Si un cliente reclama, se puede ver paso a paso qué pasó con su orden. | RI-106 | El sistema debe mostrar el Historial Completo de Eventos de la orden para revisión administrativa. |
| | Marcar órdenes como vistas | RF-107 | El sistema debe saber qué pedidos nuevos ya vio el admin. | RN-107 | Ayuda a diferenciar pedidos nuevos de los que ya se están atendiendo. | RI-107 | El sistema debe actualizar el Indicador de "Visto" de la orden cuando el administrador abre el detalle del pedido por primera vez. |
| | Mostrar contador de nuevas | RF-108 | El sistema debe mostrar un numerito rojo con los pedidos nuevos. | RN-108 | Alerta visual para que el admin sepa que entraron ventas. | RI-108 | El sistema debe calcular y mostrar el Número de Órdenes Nuevas (no vistas) en el menú principal como una alerta visual. |
| | Cancelar pedido en línea | RF-109 | El sistema debe permitir al cliente cancelar si se arrepiente rápido. | RN-109 | Solo si el pedido no ha sido procesado aún. Ahorra trabajo al admin. | RI-109 | El sistema debe recibir la Solicitud de Cancelación del cliente, verificar que el pedido esté en estado "Pendiente" y proceder a cancelarlo automáticamente. |
| | Notificar entrega con factura | RF-110 | El sistema debe confirmar la entrega por correo. | RN-110 | Cierra el ciclo de venta avisando que todo salió bien. | RI-110 | El sistema debe enviar un Correo de Confirmación de Entrega al cliente, adjuntando la factura final de la compra. |
| | Gestionar cancelaciones y reembolsos | RF-111 | El sistema debe tener herramientas para manejar devoluciones. | RN-111 | Panel para registrar que se devolvió el dinero y por qué. | RI-111 | El sistema debe permitir registrar la Gestión de Devolución: Monto reembolsado, método de reembolso y notas internas sobre el caso. |
| Marketing y Contenido | Crear código de descuento | RF-112 | El sistema debe permitir crear cupones promocionales. | RN-112 | Se define el código (ej: VERANO10), cuánto descuenta y hasta cuándo sirve. | RI-112 | El sistema debe capturar la Configuración del Cupón: Código alfanumérico, Tipo de descuento (Porcentaje/Fijo), Valor del descuento, Fecha de vencimiento y Límite de usos. |
| | Mostrar códigos de descuento | RF-113 | El sistema debe listar todos los cupones existentes. | RN-113 | Permite ver qué promociones están activas y cuáles ya vencieron. | RI-113 | El sistema debe listar los Cupones Creados, mostrando su código, valor, estado (Activo/Vencido) y cuántas veces ha sido usado. |
| | Editar código de descuento | RF-114 | El sistema debe permitir modificar un cupón. | RN-114 | Se puede ampliar la fecha o cambiar el límite de usos. | RI-114 | El sistema debe permitir actualizar las Condiciones del Cupón: Fecha de expiración, cantidad de usos disponibles o estado de activación. |
| | Eliminar código de descuento | RF-115 | El sistema debe permitir borrar cupones. | RN-115 | Para quitar promociones que ya no se quieren ofrecer. | RI-115 | El sistema debe eliminar el Cupón seleccionado de la base de datos, impidiendo su uso futuro. |
| | Generar códigos masivamente | RF-116 | El sistema debe permitir crear muchos cupones únicos a la vez. | RN-116 | Útil para enviar un código diferente a cada cliente por correo. | RI-116 | El sistema debe generar un Lote de Códigos Únicos basado en los parámetros definidos (cantidad, prefijo, valor) y permitir su exportación. |
| | Asignar código a usuario | RF-117 | El sistema debe permitir regalar un cupón a un cliente específico. | RN-117 | El sistema le envía un correo al cliente con su regalo. | RI-117 | El sistema debe vincular un Cupón Específico a la cuenta de un Usuario y enviar una notificación por correo con el código de regalo. |
| | Copiar código de descuento (Admin) | RF-118 | El sistema debe facilitar copiar el código para compartirlo. | RN-118 | Botón para copiar el texto del cupón y pegarlo en WhatsApp o redes. | RI-118 | El sistema debe copiar el Texto del Código al portapapeles del dispositivo para facilitar su pegado en otras aplicaciones. |
| | Descargar PDF de descuento | RF-119 | El sistema debe generar un diseño imprimible del cupón. | RN-119 | Crea un archivo bonito para imprimir y dar en la tienda física. | RI-119 | El sistema debe generar un Archivo PDF con el diseño gráfico del cupón, incluyendo el código y las condiciones, listo para imprimir. |
| | Copiar código de descuento (Cliente) | RF-120 | El sistema debe permitir al cliente copiar el cupón fácil. | RN-120 | Botón en el correo o web para copiar y pegar en el checkout. | RI-120 | El sistema debe permitir al cliente copiar el Código de Descuento con un solo clic desde la interfaz visual. |
| | Crear regla de descuento bulk | RF-121 | El sistema debe permitir promociones por cantidad (ej: Lleva 3 y paga 2). | RN-121 | Incentiva a comprar más unidades. Se aplica solo en el carrito. | RI-121 | El sistema debe capturar la Regla de Volumen: Cantidad mínima de productos para activar la oferta y el Porcentaje o Valor de descuento a aplicar. |
| | Mostrar reglas bulk | RF-122 | El sistema debe listar las promociones por cantidad activas. | RN-122 | Gestión de las ofertas automáticas de la tienda. | RI-122 | El sistema debe listar las Reglas de Descuento por Volumen configuradas, mostrando sus condiciones y estado. |
| | Editar regla de descuento por cantidad | RF-123 | El sistema debe permitir cambiar las condiciones de la promoción. | RN-123 | Ajustar cuántos hay que comprar o cuánto se descuenta. | RI-123 | El sistema debe permitir modificar los Parámetros de la Regla (cantidad requerida o descuento otorgado) para ajustar la promoción. |
| | Eliminar regla de descuento por cantidad | RF-124 | El sistema debe permitir quitar promociones por cantidad. | RN-124 | Desactiva la oferta inmediatamente. | RI-124 | El sistema debe eliminar la Regla de Descuento seleccionada, dejando de aplicar la promoción en el carrito. |
| | Aplicación automática en carrito | RF-125 | El sistema debe aplicar la oferta sola si el cliente cumple la condición. | RN-125 | El cliente ve el descuento aparecer mágicamente al agregar productos. | RI-125 | El sistema debe calcular automáticamente el Descuento Aplicable en el carrito basándose en las reglas activas y los productos seleccionados. |
| | Crear colección | RF-126 | El sistema debe permitir agrupar productos por temas (Colecciones). | RN-126 | Ej: "Colección Navidad". Agrupa productos distintos bajo un mismo tema. | RI-126 | El sistema debe capturar los Datos de la Colección: Nombre, Descripción, Imagen de portada y la lista de Productos que pertenecen a ella. |
| | Mostrar colecciones | RF-127 | El sistema debe listar las colecciones creadas. | RN-127 | Ver qué agrupaciones especiales existen en la tienda. | RI-127 | El sistema debe listar las Colecciones Activas, mostrando su nombre, imagen y cantidad de productos incluidos. |
| | Actualizar colección | RF-128 | El sistema debe permitir cambiar qué productos están en una colección. | RN-128 | Agregar o quitar productos de la campaña. | RI-128 | El sistema debe permitir editar la Información de la Colección y modificar la selección de productos que la componen. |
| | Eliminar colección | RF-129 | El sistema debe permitir borrar una colección. | RN-129 | Borra el grupo, pero los productos siguen existiendo sueltos. | RI-129 | El sistema debe eliminar la Agrupación (Colección) sin borrar los productos individuales que contenía. |
| | Crear slider | RF-130 | El sistema debe permitir poner banners grandes en el inicio. | RN-130 | Imágenes rotativas para destacar ofertas o novedades al entrar. | RI-130 | El sistema debe capturar la Configuración del Banner: Imagen a mostrar, Título, Texto descriptivo, Enlace de destino y Orden de aparición. |
| | Mostrar sliders | RF-131 | El sistema debe listar los banners activos. | RN-131 | Gestión visual de la portada de la tienda. | RI-131 | El sistema debe listar los Banners (Sliders) configurados, mostrando su imagen previa y estado. |
| | Actualizar slider | RF-132 | El sistema debe permitir cambiar la imagen o texto de un banner. | RN-132 | Mantener la portada actualizada con lo último. | RI-132 | El sistema debe permitir reemplazar la Imagen del Banner o editar sus textos y enlaces asociados. |
| | Eliminar slider | RF-133 | El sistema debe permitir quitar banners. | RN-133 | Retirar publicidad vieja. | RI-133 | El sistema debe eliminar el Banner seleccionado de la rotación de la página de inicio. |
| | Gestión de Anuncios | RF-134 | El sistema debe permitir poner avisos de texto arriba (Barra de anuncios). | RN-134 | Mensajes cortos como "Envíos gratis hoy" que salen en todas las páginas. | RI-134 | El sistema debe capturar el Contenido del Anuncio: Texto del mensaje, Color de fondo, Enlace opcional y Estado de visibilidad. |
| | Listar anuncios | RF-135 | El sistema debe mostrar los anuncios configurados. | RN-135 | Ver historial de mensajes y activar el que se necesite. | RI-135 | El sistema debe listar los Anuncios de Barra Superior disponibles para su gestión. |
| Administración y Reportes | Gráficos de tendencias | RF-136 | El sistema debe mostrar gráficos de cómo va el negocio. | RN-136 | Curvas de ventas y barras de productos para entender rápido la situación. | RI-136 | El sistema debe generar Gráficos Visuales que representen el volumen de ventas, nuevos clientes y pedidos a lo largo del tiempo. |
| | Búsqueda global | RF-137 | El sistema debe tener un buscador general para el admin. | RN-137 | Encuentra rápido un pedido, producto o cliente desde cualquier parte. | RI-137 | El sistema debe procesar la Consulta Global y buscar coincidencias en pedidos, productos y clientes, mostrando los resultados relevantes. |
| | Acciones rápidas | RF-138 | El sistema debe tener atajos a lo más usado. | RN-138 | Botones para "Nuevo Producto", "Ver Pedidos" a un clic. | RI-138 | El sistema debe mostrar Accesos Directos a las funciones más frecuentes del panel administrativo. |
| | Informe de productos | RF-139 | El sistema debe reportar qué productos se venden más y cuáles menos. | RN-139 | Ayuda a decidir qué comprar más y qué poner en oferta. | RI-139 | El sistema debe generar un Reporte de Rendimiento de Productos, listando los artículos más vendidos, los menos vendidos y los que tienen bajo stock. |
| | Informe de ventas | RF-140 | El sistema debe dar un reporte financiero de cuánto se vendió. | RN-140 | Total de dinero que entró en un periodo de tiempo. | RI-140 | El sistema debe generar un Reporte Financiero detallando los ingresos totales, ticket promedio y cantidad de ventas en un rango de fechas. |
| | Informe de clientes | RF-141 | El sistema debe reportar quiénes son los mejores clientes. | RN-141 | Muestra quién compra más y datos demográficos. | RI-141 | El sistema debe generar un Reporte de Clientes, identificando a los usuarios con mayor volumen de compras y frecuencia de pedidos. |
| | Exportar órdenes a PDF | RF-142 | El sistema debe generar documentos PDF de los pedidos. | RN-142 | Para imprimir facturas o guías de despacho formales. | RI-142 | El sistema debe generar un Archivo PDF formateado con todos los detalles de la orden seleccionada, listo para impresión o descarga. |
| | Exportar productos a CSV | RF-143 | El sistema debe permitir bajar el inventario a Excel/CSV. | RN-143 | Para trabajar los datos en Excel o hacer inventario físico. | RI-143 | El sistema debe generar un Archivo CSV (compatible con Excel) que contenga la base de datos completa de productos e inventario. |
| | Exportar clientes a Excel | RF-144 | El sistema debe permitir bajar la lista de clientes a Excel. | RN-144 | Para usar en campañas de correo o análisis externo. | RI-144 | El sistema debe generar un Archivo Excel con la lista completa de clientes registrados y sus datos de contacto. |
| | Diagnóstico de exportación PDF | RF-145 | El sistema debe tener una prueba para ver si los PDF funcionan. | RN-145 | Herramienta técnica para asegurar que las facturas se generan bien. | RI-145 | El sistema debe mostrar el Resultado Técnico de la prueba de generación de PDF, indicando si las librerías necesarias están funcionando correctamente. |
| | Inspección de tablas | RF-146 | El sistema debe permitir ver el estado técnico de la base de datos. | RN-146 | Uso avanzado para verificar que la información se guarda bien. | RI-146 | El sistema debe mostrar Información Técnica sobre las tablas de la base de datos: cantidad de registros, tamaño y estado de salud. |
| | Bandeja de notificaciones | RF-147 | El sistema debe tener un centro de mensajes para el admin. | RN-147 | Avisa de todo: nuevos pedidos, stock bajo, preguntas nuevas. | RI-147 | El sistema debe listar las Notificaciones del Sistema, mostrando el título del evento, la descripción y la fecha de cada alerta. |
| | Contador en tiempo real | RF-148 | El sistema debe mostrar cuántas notificaciones hay sin leer. | RN-148 | Badge rojo en la campana para llamar la atención. | RI-148 | El sistema debe mantener actualizado el Contador de Notificaciones No Leídas y mostrarlo visualmente en la interfaz. |
| | Acciones sobre notificaciones | RF-149 | El sistema debe permitir marcar como leídas o borrar notificaciones. | RN-149 | Para mantener la bandeja limpia y organizada. | RI-149 | El sistema debe actualizar el Estado de Lectura de la notificación o eliminarla de la lista según la acción del usuario. |
| | Moderación de reseñas (Admin) | RF-150 | El sistema debe permitir aprobar o rechazar opiniones de clientes. | RN-150 | Evita que se publiquen insultos o spam. El admin decide qué sale. | RI-150 | El sistema debe permitir cambiar el Estado de Publicación de una reseña (Aprobada/Rechazada) para controlar su visibilidad en la tienda. |
| | Verificar compra para reseña | RF-151 | El sistema debe marcar automáticamente si la opinión es de un comprador real. | RN-151 | Pone un sello "Compra Verificada" si el usuario compró el producto. | RI-151 | El sistema debe verificar si existe una Orden Completada del usuario para el producto reseñado y asignar la Etiqueta de "Compra Verificada" si corresponde. |
| | Eliminar reseña (Admin) | RF-152 | El sistema debe permitir borrar reseñas inadecuadas. | RN-152 | Si una reseña viola las reglas, se elimina. | RI-152 | El sistema debe eliminar permanentemente la Reseña seleccionada por el administrador. |
| | Gestionar preguntas (Admin) | RF-153 | El sistema debe tener un panel para ver todas las preguntas pendientes. | RN-153 | Centraliza la atención al cliente para responder rápido. | RI-153 | El sistema debe listar las Preguntas de Clientes pendientes de respuesta, mostrando el producto, la pregunta y la fecha. |
| | Eliminar pregunta (Admin) | RF-154 | El sistema debe permitir borrar preguntas spam. | RN-154 | Limpieza de preguntas basura o repetidas. | RI-154 | El sistema debe eliminar la Pregunta seleccionada de la base de datos. |
| Configuración del Sistema | Configurar información de marca | RF-155 | El sistema debe permitir cambiar el logo y colores de la tienda. | RN-155 | Personalizar la apariencia para que coincida con la marca. | RI-155 | El sistema debe guardar la Configuración Visual de la tienda: Archivo del logo, colores principales y nombre de la marca. |
| | Configurar información de soporte | RF-156 | El sistema debe permitir poner los datos de contacto de la empresa. | RN-156 | Teléfono, correo y dirección que salen en el pie de página. | RI-156 | El sistema debe almacenar los Datos de Contacto Corporativo: Correo de soporte, Teléfono de atención y Dirección física. |
| | Configurar parámetros operacionales | RF-157 | El sistema debe permitir ajustar reglas generales (ej: moneda, zona horaria). | RN-157 | Configuración base de cómo opera el negocio. | RI-157 | El sistema debe guardar los Parámetros Globales de operación: Moneda por defecto, Zona horaria y reglas generales del negocio. |
| | Configurar redes sociales | RF-158 | El sistema debe permitir poner los links a Facebook, Instagram, etc. | RN-158 | Iconos que llevan a las redes de la tienda. | RI-158 | El sistema debe almacenar las URLs de los Perfiles Sociales de la marca (Facebook, Instagram, WhatsApp, etc.). |
| | Subir logo de marca | RF-159 | El sistema debe permitir cargar el archivo del logo. | RN-159 | Sube la imagen que aparecerá en el encabezado y correos. | RI-159 | El sistema debe procesar y guardar el Archivo de Imagen del Logo corporativo para su uso en la interfaz. |
| | Gestión de Métodos de Envío | RF-160 | El sistema debe permitir crear y gestionar diferentes formas de entrega (ej: Express, Estándar). | RN-160 | Cada método tiene su propio costo base y características. Se pueden activar o desactivar según disponibilidad. | RI-160 | El sistema debe guardar la Configuración del Método: Nombre, Descripción, Costo Base, Icono representativo y Estado. |
| | Configurar Envío Gratis por Monto | RF-161 | El sistema debe permitir definir un monto mínimo de compra para que un método de envío sea gratuito. | RN-161 | Incentiva las compras grandes. Si el cliente supera este valor, el costo de envío de este método se vuelve $0 automáticamente. | RI-161 | El sistema debe almacenar el Umbral de Gratuidad (Monto mínimo) asociado a cada método de envío específico. |
| | Reglas de Costo por Rango de Precio | RF-162 | El sistema debe permitir definir costos de envío variables según el valor total de la compra. | RN-162 | Permite cobrar tarifas diferentes dependiendo del tamaño del pedido (ej: pedidos pequeños pagan más envío). | RI-162 | El sistema debe gestionar las Reglas de Precio: Rango de valor del pedido (Mínimo y Máximo) y el Costo de envío a aplicar para ese rango (o gratuidad). |
| | Estimación de Tiempos de Entrega | RF-163 | El sistema debe permitir configurar los días estimados que tardará la entrega. | RN-163 | Informa al cliente cuándo esperar su pedido. Se define un rango de días (mínimo y máximo). | RI-163 | El sistema debe registrar los Días Hábiles Estimados (Mínimo y Máximo) para calcular y mostrar la fecha probable de entrega. |

## 🏗️ Diagrama de Arquitectura

```
📦 Proyecto
├── 🗂️ src
│   ├── 🗂️ domain
│   │   ├── entities/
│   │   ├── value_objects/
│   │   ├── exceptions/
│   │   └── interfaces/
│   ├── 🗂️ application
│   │   ├── use_cases/
│   │   ├── dtos/
│   │   └── interfaces/
│   ├── 🗂️ infrastructure
│   │   ├── persistence/
│   │   ├── serializers/
│   │   └── config/
│   └── 🗂️ presentation
│       ├── cli/
│       └── api/
```
## 🔷 Diagrama de Clases

## Diseño de la arquitectura de software

## 🐍 Implementación en Python

### Paso 1: Estructura de Carpetas

```text
Proyecto/
├── src/
│   ├── domain/
│   │   ├── entities/
│   │   │   ├── __init__.py
│   │   │   ├── cart.py
│   │   │   ├── product.py
│   │   │   └── user.py
│   │   ├── exceptions/
│   │   │   ├── __init__.py
│   │   │   ├── product_exceptions.py
│   │   │   └── user_exceptions.py
│   │   ├── interfaces/
│   │   │   ├── __init__.py
│   │   │   ├── cart_repository.py
│   │   │   └── repositories.py
│   │   └── value_objects/
│   │       └── __init__.py
│   ├── application/
│   │   ├── dtos/
│   │   └── use_cases/
│   │       ├── cart_use_cases.py
│   │       ├── product_use_cases.py
│   │       └── user_use_cases.py
│   ├── infrastructure/
│   │   ├── config/
│   │   ├── persistence/
│   │   │   ├── database.py
│   │   │   ├── mysql_cart_repository.py
│   │   │   ├── mysql_product_repository.py
│   │   │   └── mysql_user_repository.py
│   │   └── serializers/
│   └── presentation/
│       ├── api/
│       └── cli/
│           ├── commands.py
│           └── menu.py
├── tests/
├── requirements.txt
└── main.py
```

### Paso 2: Implementación del Código

#### 2.1 Dominio - Entidades

**src/domain/entities/product.py**
```python
from typing import List, Optional

class VarianteTalla:
    def __init__(self, id_variante_color: int, id_talla: int, precio: float, cantidad: int, sku: Optional[str] = None, codigo_barras: Optional[str] = None):
        self.id = None
        self.id_variante_color = id_variante_color
        self.id_talla = id_talla
        self.nombre_talla = None
        self.sku = sku
        self.codigo_barras = codigo_barras
        self.precio = precio
        self.cantidad = cantidad

    def __str__(self):
        return f"VarianteTalla(id={self.id}, id_talla={self.id_talla}, precio={self.precio}, cantidad={self.cantidad})"

class VarianteColor:
    def __init__(self, id_producto: int, id_color: int, es_defecto: bool = False):
        self.id = None
        self.id_producto = id_producto
        self.id_color = id_color
        self.nombre_color = None
        self.es_defecto = es_defecto
        self.variantes_talla: List[VarianteTalla] = []

    def agregar_variante_talla(self, variante_talla: VarianteTalla):
        self.variantes_talla.append(variante_talla)

    def __str__(self):
        return f"VarianteColor(id={self.id}, id_color={self.id_color}, tallas={len(self.variantes_talla)})"

class Producto:
    def __init__(self, nombre: str, slug: str, descripcion: str = '', marca: Optional[str] = None, genero: Optional[str] = None, precio: Optional[float] = None, id_categoria: Optional[int] = None):
        self.id = None
        self.nombre = nombre
        self.slug = slug
        self.descripcion = descripcion
        self.marca = marca
        self.genero = genero
        self.precio = precio
        self.id_categoria = id_categoria
        self.nombre_categoria = None
        self.variantes_color: List[VarianteColor] = []

    def agregar_variante_color(self, variante_color: VarianteColor):
        self.variantes_color.append(variante_color)

    def __str__(self):
        return f"Producto(id={self.id}, nombre='{self.nombre}', variantes={len(self.variantes_color)})"
```

**src/domain/entities/user.py**
```python
from typing import List, Optional
import bcrypt

class Usuario:
    """Clase base para representar a los usuarios del sistema."""
    def __init__(self, nombre: str, correo: str, password: str, telefono: Optional[str] = None, rol: str = "customer"):
        self.id = None
        self.nombre = nombre
        self.correo = correo
        self.telefono = telefono
        self.rol = rol
        self.hash_password = None
        if password:
            self.establecer_password(password)
        self._inicio_sesion = self.InicioSesion(self.correo, self.hash_password)

    def establecer_password(self, password: str):
        salt = bcrypt.gensalt()
        self.hash_password = bcrypt.hashpw(password.encode(), salt).decode()
        if hasattr(self, '_inicio_sesion'):
            self._inicio_sesion.hash_password = self.hash_password

    def verificar_password(self, password: str) -> bool:
        if not self.hash_password:
            return False
        try:
            return bcrypt.checkpw(password.encode(), self.hash_password.encode())
        except Exception:
            return False

    class InicioSesion:
        def __init__(self, correo: str, hash_password: str):
            self.correo = correo
            self.hash_password = hash_password
            self._esta_autenticado = False

        def autenticar(self, password: str) -> bool:
            if not self.hash_password:
                return False
            self._esta_autenticado = bcrypt.checkpw(password.encode(), self.hash_password.encode())
            return self._esta_autenticado

        @property
        def esta_autenticado(self) -> bool:
            return self._esta_autenticado


class Cliente(Usuario):
    def __init__(self, nombre: str, correo: str, password: str, telefono: Optional[str] = None):
        super().__init__(nombre, correo, password, telefono, rol="customer")
        # Importación local para evitar ciclos
        from .cart import Carrito
        self.carrito = Carrito(id_usuario=self.correo)
    
    def agregar_item_carrito(self, id_producto: int, cantidad: int, precio: float, id_variante_color: Optional[int] = None, id_variante_talla: Optional[int] = None):
        from .cart import ItemCarrito
        item = ItemCarrito(id_producto, cantidad, precio, id_variante_color, id_variante_talla)
        self.carrito.agregar_item(item)

    def eliminar_item_carrito(self, id_producto: int):
        """Elimina todos los items del carrito que coincidan con el id_producto."""
        self.carrito.items = [item for item in self.carrito.items if item.id_producto != id_producto]

    def vaciar_carrito(self):
        self.carrito.items = []

    def __str__(self):
        return f"Cliente(id={self.id}, nombre='{self.nombre}', correo='{self.correo}', rol='{self.rol}', items_carrito={len(self.carrito.items)})"


class Admin(Usuario):
    def __init__(self, nombre: str, correo: str, password: str, telefono: Optional[str] = None):
        super().__init__(nombre, correo, password, telefono, rol="admin")

    def crear_producto(self, nombre: str, slug: str, descripcion: str, marca: str, genero: str, precio: float, id_categoria: int):
        from .product import Producto
        return Producto(nombre, slug, descripcion, marca, genero, precio, id_categoria)

    def agregar_variante_color(self, producto, id_color: int, es_defecto: bool = False):
        from .product import VarianteColor
        variante = VarianteColor(producto.id, id_color, es_defecto)
        producto.agregar_variante_color(variante)
        return variante

    def agregar_variante_talla(self, variante_color, id_talla: int, precio: float, cantidad: int, sku: Optional[str] = None, codigo_barras: Optional[str] = None):
        from .product import VarianteTalla
        variante_talla = VarianteTalla(variante_color.id, id_talla, precio, cantidad, sku, codigo_barras)
        variante_color.agregar_variante_talla(variante_talla)
        return variante_talla

    def __str__(self):
        return f"Admin(id={self.id}, nombre='{self.nombre}', correo='{self.correo}', rol='{self.rol}')"
```

**src/domain/entities/cart.py**
```python
from typing import List, Optional

class ItemCarrito:
    def __init__(self, id_producto: int, cantidad: int, precio: float, id_variante_color: Optional[int] = None, id_variante_talla: Optional[int] = None):
        self.id = None
        self.id_carrito = None
        self.id_producto = id_producto
        self.id_variante_color = id_variante_color
        self.id_variante_talla = id_variante_talla
        self.cantidad = cantidad
        self.precio = precio
        # Metadatos opcionales para visualización
        self.nombre_producto = None
        self.nombre_color = None
        self.nombre_talla = None

    @property
    def subtotal(self) -> float:
```python
class ExcepcionProducto(Exception):
    """Excepción base para errores relacionados con productos"""
    pass

class ExcepcionProductoNoEncontrado(ExcepcionProducto):
    """Se lanza cuando no se encuentra el producto"""
    pass
```

**src/domain/exceptions/user_exceptions.py**
```python
class ExcepcionUsuario(Exception):
    """Excepción base para errores relacionados con usuarios"""
    pass

class ExcepcionDatosUsuarioInvalidos(ExcepcionUsuario):
    """Se lanza cuando los datos del usuario son inválidos"""
    pass

class ExcepcionUsuarioNoEncontrado(ExcepcionUsuario):
    """Se lanza cuando no se encuentra el usuario"""
    pass
```

#### 2.3 Interfaces de Repositorio

**src/domain/interfaces/repositories.py**
```python
from abc import ABC, abstractmethod
from typing import List, Optional
from ..entities.user import Usuario
from ..entities.product import Producto, VarianteColor, VarianteTalla

class RepositorioUsuario(ABC):
    @abstractmethod
    def guardar(self, usuario: Usuario) -> str:
        pass

    @abstractmethod
    def buscar_por_id(self, id_usuario: str) -> Optional[Usuario]:
        pass

    @abstractmethod
    def buscar_por_email(self, email: str) -> Optional[Usuario]:
        pass
    
    @abstractmethod
    def agregar_telefono(self, id_usuario: str, telefono: str) -> str:
        pass
    
    @abstractmethod
    def actualizar(self, usuario: Usuario) -> str:
        pass

class RepositorioProducto(ABC):
    @abstractmethod
    def guardar(self, producto: Producto) -> int:
        pass

    @abstractmethod
    def buscar_todos(self) -> List[Producto]:
        pass

    @abstractmethod
    def buscar_por_id_con_variantes(self, id_producto: int) -> Optional[Producto]:
        pass

    @abstractmethod
    def agregar_variante_color(self, id_producto: int, id_color: int, es_defecto: bool) -> int:
        pass

    @abstractmethod
    def agregar_variante_talla(self, id_variante_color: int, id_talla: int, precio: float, cantidad: int) -> int:
        pass
    
    @abstractmethod
    def listar_colores(self) -> List[dict]:
        pass

    @abstractmethod
    def listar_tallas(self) -> List[dict]:
        pass

    @abstractmethod
    def listar_categorias(self) -> List[dict]:
        pass
```

**src/domain/interfaces/cart_repository.py**
```python
from abc import ABC, abstractmethod
from typing import Optional
from ..entities.cart import Carrito, ItemCarrito

class RepositorioCarrito(ABC):
    @abstractmethod
    def obtener_por_id_usuario(self, id_usuario: str) -> Optional[Carrito]:
        pass

    @abstractmethod
    def crear(self, id_usuario: str) -> int:
        pass

    @abstractmethod
    def agregar_item(self, id_carrito: int, item: ItemCarrito) -> int:
        pass

    @abstractmethod
    def eliminar_item(self, id_item: int) -> bool:
        pass
    
    @abstractmethod
    def actualizar_cantidad_item(self, id_item: int, cantidad: int) -> bool:
        pass

    @abstractmethod
    def vaciar_carrito(self, id_carrito: int) -> bool:
        pass
```

#### 2.4 Casos de Uso

**src/application/use_cases/product_use_cases.py**
```python
from typing import List, Optional
from ...domain.entities.product import Producto, VarianteColor, VarianteTalla
from ...domain.interfaces.repositories import RepositorioProducto
from ...domain.exceptions.product_exceptions import ExcepcionProductoNoEncontrado

class CasosUsoProducto:
    def __init__(self, repositorio_producto: RepositorioProducto):
        self.repositorio_producto = repositorio_producto

    def crear_producto(self, nombre: str, slug: str, descripcion: str, marca: str, genero: str, precio: float, id_categoria: int) -> Producto:
        producto = Producto(nombre, slug, descripcion, marca, genero, precio, id_categoria)
        self.repositorio_producto.guardar(producto)
        return producto

    def agregar_variante_a_producto(self, producto: Producto, id_color: int, es_defecto: bool, id_talla: int, precio: float, cantidad: int) -> Producto:
        # Agregar variante de color
        id_variante_color = self.repositorio_producto.agregar_variante_color(producto.id, id_color, es_defecto)
        
        # Agregar variante de talla
        self.repositorio_producto.agregar_variante_talla(id_variante_color, id_talla, precio, cantidad)
        
        # Devolver producto actualizado
        return self.obtener_detalles_producto(producto.id)

    def agregar_variante_color(self, id_producto: int, id_color: int, es_defecto: bool) -> int:
        return self.repositorio_producto.agregar_variante_color(id_producto, id_color, es_defecto)

    def agregar_variante_talla(self, id_variante_color: int, id_talla: int, precio: float, cantidad: int) -> int:
        return self.repositorio_producto.agregar_variante_talla(id_variante_color, id_talla, precio, cantidad)

    def listar_productos(self) -> List[Producto]:
        return self.repositorio_producto.buscar_todos()

    def obtener_detalles_producto(self, id_producto: int) -> Optional[Producto]:
        return self.repositorio_producto.buscar_por_id_con_variantes(id_producto)

    def listar_categorias(self) -> List[dict]:
        return self.repositorio_producto.listar_categorias()

    def listar_colores(self) -> List[dict]:
        return self.repositorio_producto.listar_colores()

    def listar_tallas(self) -> List[dict]:
        return self.repositorio_producto.listar_tallas()
```

**src/application/use_cases/user_use_cases.py**
```python
from typing import Optional, List
from ...domain.entities.user import Usuario, Cliente, Admin
from ...domain.interfaces.repositories import RepositorioUsuario
from ...domain.exceptions.user_exceptions import ExcepcionDatosUsuarioInvalidos, ExcepcionUsuarioNoEncontrado

class CasosUsoUsuario:
    def __init__(self, repositorio_usuario: RepositorioUsuario):
        self.repositorio_usuario = repositorio_usuario

    def registrar_usuario(self, nombre: str, correo: str, contrasena: str, telefono: Optional[str] = None, rol: str = "customer") -> Usuario:
        if rol == "admin":
            usuario = Admin(nombre, correo, contrasena, telefono)
        else:
            usuario = Cliente(nombre, correo, contrasena, telefono)
        
        try:
            self.repositorio_usuario.guardar(usuario)
            return usuario
        except Exception as e:
            raise ExcepcionDatosUsuarioInvalidos(f"Error al registrar usuario: {str(e)}")

    def iniciar_sesion(self, correo: str, contrasena: str) -> Optional[Usuario]:
        usuario = self.repositorio_usuario.buscar_por_email(correo)
        if usuario and usuario._inicio_sesion.autenticar(contrasena):
            return usuario
        return None

    def agregar_telefono(self, id_usuario: str, telefono: str) -> Usuario:
        usuario = self.repositorio_usuario.buscar_por_id(id_usuario)
        if not usuario:
            raise ExcepcionUsuarioNoEncontrado(f"Usuario {id_usuario} no encontrado")
        
        usuario.agregar_telefono(telefono)
        self.repositorio_usuario.agregar_telefono(id_usuario, telefono)
        return usuario

    def actualizar_perfil(self, id_usuario: str, nombre: str, telefono: str) -> Usuario:
        usuario = self.repositorio_usuario.buscar_por_id(id_usuario)
        if not usuario:
            raise ExcepcionUsuarioNoEncontrado(f"Usuario {id_usuario} no encontrado")
        
        usuario.nombre = nombre
        usuario.telefono = telefono
        # Actualizar en repositorio
        self.repositorio_usuario.actualizar(usuario)
        return usuario
```

**src/application/use_cases/cart_use_cases.py**
```python
from typing import Optional
from ...domain.entities.cart import Carrito, ItemCarrito
from ...domain.interfaces.cart_repository import RepositorioCarrito
from ...domain.interfaces.repositories import RepositorioProducto

class CasoUsoObtenerCarrito:
    def __init__(self, repositorio_carrito: RepositorioCarrito):
        self.repositorio_carrito = repositorio_carrito

    def ejecutar(self, id_usuario: str) -> Optional[Carrito]:
        return self.repositorio_carrito.obtener_por_id_usuario(id_usuario)

class CasoUsoAgregarAlCarrito:
    def __init__(self, repositorio_carrito: RepositorioCarrito, repositorio_producto: RepositorioProducto):
        self.repositorio_carrito = repositorio_carrito
        self.repositorio_producto = repositorio_producto

    def ejecutar(self, id_usuario: str, id_producto: int, cantidad: int, id_variante_color: int, id_variante_talla: int) -> str:
        # 1. Obtener o crear carrito
        carrito = self.repositorio_carrito.obtener_por_id_usuario(id_usuario)
        if not carrito:
            id_carrito = self.repositorio_carrito.crear(id_usuario)
        else:
            id_carrito = carrito.id

        # 2. Validar producto y stock
        
        producto = self.repositorio_producto.buscar_por_id_con_variantes(id_producto)
        if not producto:
            return "Producto no encontrado"

        # Encontrar el precio de la variante específica
        precio = 0.0
        encontrado = False
        for var_color in producto.variantes_color:
            if var_color.id == id_variante_color:
                for var_talla in var_color.variantes_talla:
                    if var_talla.id == id_variante_talla:
                        precio = var_talla.precio
                        encontrado = True
                        break
            if encontrado: break
        
        if not encontrado:
            return "Variante no encontrada"

        # 3. Agregar ítem
        item = ItemCarrito(
            id_producto=id_producto,
            cantidad=cantidad,
            precio=precio,
            id_variante_color=id_variante_color,
            id_variante_talla=id_variante_talla
        )
        
        self.repositorio_carrito.agregar_item(id_carrito, item)
        return "Producto agregado al carrito exitosamente"

class CasoUsoEliminarDelCarrito:
    def __init__(self, repositorio_carrito: RepositorioCarrito):
        self.repositorio_carrito = repositorio_carrito

    def ejecutar(self, id_item: int) -> str:
        exito = self.repositorio_carrito.eliminar_item(id_item)
        return "Producto eliminado del carrito" if exito else "Error al eliminar producto"

class CasoUsoActualizarItemCarrito:
    def __init__(self, repositorio_carrito: RepositorioCarrito):
        self.repositorio_carrito = repositorio_carrito

    def ejecutar(self, id_item: int, cantidad: int) -> str:
        exito = self.repositorio_carrito.actualizar_cantidad_item(id_item, cantidad)
        return "Cantidad actualizada" if exito else "Error al actualizar cantidad"

#### 2.4.1 DTOs

**src/application/dtos/product_dtos.py**
```python
from dataclasses import dataclass
from typing import Optional, List

@dataclass
class ProductoDTO:
    id: int
    nombre: str
    slug: str
    descripcion: str
    marca: Optional[str]
    genero: Optional[str]
    precio: Optional[float]
    id_categoria: Optional[int]
    nombre_categoria: Optional[str]

    @staticmethod
    def desde_entidad(producto):
        return ProductoDTO(
            id=producto.id,
            nombre=producto.nombre,
            slug=producto.slug,
            descripcion=producto.descripcion,
            marca=producto.marca,
            genero=producto.genero,
            precio=producto.precio,
            id_categoria=producto.id_categoria,
            nombre_categoria=producto.nombre_categoria
        )
```

**src/application/dtos/user_dtos.py**
```python
from dataclasses import dataclass
from typing import Optional, List

@dataclass
class UsuarioDTO:
    id: str
    nombre: str
    correo: str
    telefono: Optional[str]
    rol: str
    telefonos: List[str]
    emails: List[str]

    @staticmethod
    def desde_entidad(usuario):
        return UsuarioDTO(
            id=usuario.id,
            nombre=usuario.nombre,
            correo=usuario.correo,
            telefono=usuario.telefono,
            rol=usuario.rol,
            telefonos=usuario.telefonos,
            emails=usuario.emails
        )
```

**src/application/dtos/cart_dtos.py**
```python
from dataclasses import dataclass
from typing import List, Optional

@dataclass
class ItemCarritoDTO:
    id: int
    id_producto: int
    nombre_producto: str
    nombre_color: str
    nombre_talla: str
    cantidad: int
    precio: float
    subtotal: float

@dataclass
class CarritoDTO:
    id: int
    id_usuario: str
    items: List[ItemCarritoDTO]
    total: float
```

```

#### 2.5 Infraestructura - Repositorios

**src/infrastructure/persistence/mysql_product_repository.py**
```python
from typing import List, Optional
from ...domain.entities.product import Producto, VarianteColor, VarianteTalla
from ...domain.interfaces.repositories import RepositorioProducto
from .database import BaseDeDatos

class RepositorioProductoMySQL(RepositorioProducto):
    def __init__(self):
        self.bd = BaseDeDatos()

    def guardar(self, producto: Producto) -> int:
        conexion = self.bd.obtener_conexion()
        cursor = conexion.cursor(buffered=True)
        try:
            cursor.execute(
                "INSERT INTO products (name, slug, description, brand, gender, price, category_id, created_at) VALUES (%s,%s,%s,%s,%s,%s,%s,NOW())",
                (producto.nombre, producto.slug, producto.descripcion, producto.marca, producto.genero, producto.precio, producto.id_categoria)
            )
            producto.id = cursor.lastrowid

            for var_color in producto.variantes_color:
                cursor.execute("INSERT INTO product_color_variants (product_id, color_id, is_default) VALUES (%s,%s,%s)", (producto.id, var_color.id_color, 1 if var_color.es_defecto else 0))
                var_color.id = cursor.lastrowid
                for var_talla in var_color.variantes_talla:
                    cursor.execute(
                        "INSERT INTO product_size_variants (color_variant_id, size_id, sku, barcode, price, compare_price, quantity, is_active) VALUES (%s,%s,%s,%s,%s,%s,%s,1)",
                        (var_color.id, var_talla.id_talla, var_talla.sku, var_talla.codigo_barras, var_talla.precio, None, var_talla.cantidad)
                    )
                    var_talla.id = cursor.lastrowid
            conexion.commit()
            return producto.id
        except Exception as e:
            conexion.rollback()
            raise e
        finally:
            cursor.close()

    def buscar_todos(self) -> List[Producto]:
        conexion = self.bd.obtener_conexion()
        cursor = conexion.cursor(dictionary=True, buffered=True)
        cursor.execute("SELECT p.id, p.name, p.slug, p.price, p.brand, p.gender, p.category_id, c.name as category_name FROM products p LEFT JOIN categories c ON p.category_id = c.id WHERE p.is_active = 1")
        filas = cursor.fetchall()
        productos = []
        for r in filas:
            p = Producto(r['name'], r['slug'], marca=r.get('brand'), genero=r.get('gender'), precio=r.get('price'), id_categoria=r.get('category_id'))
            p.nombre_categoria = r.get('category_name')
            p.id = r['id']
            productos.append(p)
        cursor.close()
        return productos
```

**src/infrastructure/persistence/mysql_user_repository.py**
```python
from typing import Optional, List
import uuid
from ...domain.entities.user import Usuario, Cliente, Admin
from ...domain.interfaces.repositories import RepositorioUsuario
from .database import BaseDeDatos

class RepositorioUsuarioMySQL(RepositorioUsuario):
    def __init__(self):
        self.bd = BaseDeDatos()

    def guardar(self, persona: Usuario) -> str:
        conexion = self.bd.obtener_conexion()
        cursor = conexion.cursor()
        
        try:
            if not persona.id:
                persona.id = uuid.uuid4().hex[:13]
            
            consulta = """
                INSERT INTO users (id, name, email, phone, password, role)
                VALUES (%s, %s, %s, %s, %s, %s)
        return id_usuario

    def actualizar(self, usuario: Usuario) -> str:
        conexion = self.bd.obtener_conexion()
        cursor = conexion.cursor()
        try:
            cursor.execute("UPDATE users SET name = %s, phone = %s WHERE id = %s", (usuario.nombre, usuario.telefono, usuario.id))
            conexion.commit()
            return usuario.id
        except Exception as e:
            conexion.rollback()
            raise e
        finally:
            cursor.close()
```

**src/infrastructure/persistence/mysql_cart_repository.py**
```python
from typing import Optional
from ...domain.entities.cart import Carrito, ItemCarrito
from ...domain.interfaces.cart_repository import RepositorioCarrito
from .database import BaseDeDatos

class RepositorioCarritoMySQL(RepositorioCarrito):
    def __init__(self):
        self.bd = BaseDeDatos()

    def obtener_por_id_usuario(self, id_usuario: str) -> Optional[Carrito]:
        conexion = self.bd.obtener_conexion()
        cursor = conexion.cursor(dictionary=True, buffered=True)
        try:
            # Obtener carrito
            cursor.execute("SELECT id, user_id, session_id FROM carts WHERE user_id = %s", (id_usuario,))
            fila_carrito = cursor.fetchone()
            
            if not fila_carrito:
                return None

            carrito = Carrito(id_usuario=fila_carrito['user_id'], id_sesion=fila_carrito['session_id'])
            carrito.id = fila_carrito['id']

            # Obtener ítems
            cursor.execute("""
                SELECT ci.id, ci.product_id, ci.quantity, ci.color_variant_id, ci.size_variant_id,
                       psv.price, p.name as product_name, c.name as color_name, s.name as size_name
                FROM cart_items ci
                JOIN products p ON ci.product_id = p.id
                LEFT JOIN product_size_variants psv ON ci.size_variant_id = psv.id
                LEFT JOIN product_color_variants pcv ON ci.color_variant_id = pcv.id
                LEFT JOIN colors c ON pcv.color_id = c.id
                LEFT JOIN sizes s ON psv.size_id = s.id
                WHERE ci.cart_id = %s
            """, (carrito.id,))
            
            filas_items = cursor.fetchall()
            for fila in filas_items:
                # Precio de respaldo si no se encuentra la variante (no debería suceder con integridad estricta)
                precio = float(fila['price']) if fila['price'] else 0.0
                item = ItemCarrito(
                    id_producto=fila['product_id'],
                    cantidad=fila['quantity'],
                    precio=precio,
                    id_variante_color=fila['color_variant_id'],
                    id_variante_talla=fila['size_variant_id']
                )
                item.id = fila['id']
                item.id_carrito = carrito.id
                item.nombre_producto = fila['product_name']
                item.nombre_color = fila['color_name']
                item.nombre_talla = fila['size_name']
                carrito.agregar_item(item)
            
            return carrito
        finally:
            cursor.close()

    def crear(self, id_usuario: str) -> int:
        conexion = self.bd.obtener_conexion()
        cursor = conexion.cursor(buffered=True)
        try:
            cursor.execute("INSERT INTO carts (user_id, created_at) VALUES (%s, NOW())", (id_usuario,))
            conexion.commit()
            return cursor.lastrowid
        except Exception as e:
            conexion.rollback()
            raise e
        finally:
            cursor.close()

    def agregar_item(self, id_carrito: int, item: ItemCarrito) -> int:
        conexion = self.bd.obtener_conexion()
        cursor = conexion.cursor(buffered=True)
        try:
            # Verificar si el ítem existe para actualizar cantidad
            cursor.execute("""
                SELECT id, quantity FROM cart_items 
                WHERE cart_id = %s AND product_id = %s AND color_variant_id = %s AND size_variant_id = %s
            """, (id_carrito, item.id_producto, item.id_variante_color, item.id_variante_talla))
            
            existente = cursor.fetchone()
            
            if existente:
                nueva_cantidad = existente[1] + item.cantidad
                cursor.execute("UPDATE cart_items SET quantity = %s WHERE id = %s", (nueva_cantidad, existente[0]))
                conexion.commit()
                return existente[0]
            else:
                cursor.execute("""
                    INSERT INTO cart_items (cart_id, product_id, color_variant_id, size_variant_id, quantity, created_at) 
                    VALUES (%s, %s, %s, %s, %s, NOW())
                """, (id_carrito, item.id_producto, item.id_variante_color, item.id_variante_talla, item.cantidad))
                conexion.commit()
                return cursor.lastrowid
        except Exception as e:
            conexion.rollback()
            raise e
        finally:
            cursor.close()

    def eliminar_item(self, id_item: int) -> bool:
        conexion = self.bd.obtener_conexion()
        cursor = conexion.cursor(buffered=True)
        try:
            cursor.execute("DELETE FROM cart_items WHERE id = %s", (id_item,))
            conexion.commit()
            return cursor.rowcount > 0
        except Exception as e:
            conexion.rollback()
            raise e
        finally:
            cursor.close()

    def actualizar_cantidad_item(self, id_item: int, cantidad: int) -> bool:
        conexion = self.bd.obtener_conexion()
        cursor = conexion.cursor(buffered=True)
        try:
            if cantidad <= 0:
                return self.eliminar_item(id_item)
            
            cursor.execute("UPDATE cart_items SET quantity = %s WHERE id = %s", (cantidad, id_item))
            conexion.commit()
            return cursor.rowcount > 0
        except Exception as e:
            conexion.rollback()
            raise e
        finally:
            cursor.close()

    def vaciar_carrito(self, id_carrito: int) -> bool:
        conexion = self.bd.obtener_conexion()
        cursor = conexion.cursor(buffered=True)
        try:
            cursor.execute("DELETE FROM cart_items WHERE cart_id = %s", (id_carrito,))
            conexion.commit()
            return True
        except Exception as e:
            conexion.rollback()
            raise e
        finally:
            cursor.close()

#### 2.5.1 Serializers

**src/infrastructure/serializers/json_serializer.py**
```python
import json
from datetime import datetime

class SerializadorJson:
    @staticmethod
    def serializar(obj):
        if isinstance(obj, datetime):
            return obj.isoformat()
        return obj.__dict__
```

```

#### 2.6 Presentación - CLI

**src/presentation/cli/commands.py**
```python
from typing import List, Optional
from ...application.use_cases.user_use_cases import CasosUsoUsuario
from ...application.use_cases.product_use_cases import CasosUsoProducto
from ...application.use_cases.cart_use_cases import CasoUsoObtenerCarrito, CasoUsoAgregarAlCarrito, CasoUsoEliminarDelCarrito, CasoUsoActualizarItemCarrito
from ...domain.entities.user import Usuario
from ...domain.entities.product import Producto
from ...domain.entities.cart import Carrito

class ComandosCLI:
    def __init__(self, casos_uso_usuario: CasosUsoUsuario, casos_uso_producto: CasosUsoProducto, 
                 caso_uso_obtener_carrito: CasoUsoObtenerCarrito, caso_uso_agregar_al_carrito: CasoUsoAgregarAlCarrito, 
                 caso_uso_eliminar_del_carrito: CasoUsoEliminarDelCarrito, caso_uso_actualizar_item_carrito: CasoUsoActualizarItemCarrito):
        self.casos_uso_usuario = casos_uso_usuario
        self.casos_uso_producto = casos_uso_producto
        self.caso_uso_obtener_carrito = caso_uso_obtener_carrito
        self.caso_uso_agregar_al_carrito = caso_uso_agregar_al_carrito
        self.caso_uso_eliminar_del_carrito = caso_uso_eliminar_del_carrito
        self.caso_uso_actualizar_item_carrito = caso_uso_actualizar_item_carrito

    def registrar_usuario(self, nombre: str, correo: str, password: str, telefono: Optional[str], rol: str) -> str:
        try:
            usuario = self.casos_uso_usuario.registrar_usuario(nombre, correo, password, telefono, rol)
            return f"✅ Persona registrada con ID: {usuario.id}"
        except Exception as e:
            return f"❌ Error: {str(e)}"

    def iniciar_sesion(self, correo: str, password: str) -> Optional[Usuario]:
        return self.casos_uso_usuario.iniciar_sesion(correo, password)

    def actualizar_perfil(self, id_usuario: str, nombre: str, telefono: str) -> str:
        try:
            self.casos_uso_usuario.actualizar_perfil(id_usuario, nombre, telefono)
            return "✅ Perfil actualizado"
        except Exception as e:
            return f"❌ Error: {str(e)}"

    def listar_productos(self) -> List[Producto]:
        return self.casos_uso_producto.listar_productos()

    def agregar_al_carrito(self, id_usuario: str, id_producto: int, cantidad: int, id_variante_color: int, id_variante_talla: int) -> str:
        try:
            return self.caso_uso_agregar_al_carrito.ejecutar(id_usuario, id_producto, cantidad, id_variante_color, id_variante_talla)
        except Exception as e:
            return f"❌ Error: {str(e)}"
```

**src/presentation/cli/menu.py**
```python
class MenuCLI:
    def __init__(self, comandos):
        self.comandos = comandos
        self.usuario_actual = None

    def mostrar_menu_principal(self):
        print("\n" + "="*50)
        print("📋 MENÚ PRINCIPAL")
        print("="*50)
        print("1. 👉Registrar nueva persona")
        print("2. 😎Iniciar sesión")
        print("3. 👣Salir")

    def mostrar_menu_admin(self):
        print("\n" + "="*50)
        print(f"👮 Bienvenido Administrador: {self.usuario_actual.nombre}")
        print("="*50)
        print("1. Ver mi información")
        print("2. Gestionar productos")
        print("3. Editar perfil (Nombre/Teléfono)")
        print("4. Cerrar sesión")

    def mostrar_menu_cliente(self):
        print("\n" + "="*50)
        print(f"👤 Bienvenido Cliente: {self.usuario_actual.nombre}")
        print("="*50)
        print("1. Ver mi información")
        print("2. Ver productos")
        print("3. Editar perfil")
        print("4. Carrito")
        print("5. Cerrar sesión")

    def ejecutar(self):
        while True:
            if self.usuario_actual is None:
                self.mostrar_menu_principal()
                opcion = input("Seleccione opción: ")
                if opcion == "1":
                    self._registrar_usuario()
                elif opcion == "2":
                    self._iniciar_sesion()
                elif opcion == "3":
                    print("👋 Hasta luego!")
                    break
            else:
                if self.usuario_actual.rol == 'admin':
                    self._ejecutar_menu_admin()
                else:
                    self._ejecutar_menu_cliente()

    def _ejecutar_menu_admin(self):
        self.mostrar_menu_admin()
        opcion = input("Seleccione opción: ")
        # ... (lógica de opciones admin)

    def _ejecutar_menu_cliente(self):
        self.mostrar_menu_cliente()
        opcion = input("Seleccione opción: ")
        # ... (lógica de opciones cliente)


    def _registrar_usuario(self):
        print("\n--- REGISTRO DE USUARIO ---")
        nombre = input("Nombre: ")
        correo = input("Email: ")
        password = input("Password: ")
        telefono = input("Teléfono (opcional): ")
        
        # Preguntar rol solo para demostración (en real sería por defecto customer o panel admin)
        es_admin = input("¿Es administrador? (s/n): ").lower() == 's'
        rol = "admin" if es_admin else "customer"
        
        print(self.comandos.registrar_usuario(nombre, correo, password, telefono, rol))

    def _iniciar_sesion(self):
        print("\n--- INICIAR SESIÓN ---")
        correo = input("Email: ")
        password = input("Password: ")
        
        usuario = self.comandos.iniciar_sesion(correo, password)
        if usuario:
            self.usuario_actual = usuario
            print(f"✅ Bienvenido, {usuario.nombre}!")
        else:
            print("❌ Credenciales inválidas")

    def _mostrar_info(self):
        print(f"\n📄 Información:")
        print(self.usuario_actual)
        print("Teléfonos:", self.usuario_actual.telefonos)
        print("Emails:", self.usuario_actual.emails)

    def _editar_perfil(self):
        print("\n--- EDITAR PERFIL ---")
        print(f"Nombre actual: {self.usuario_actual.nombre}")
        print(f"Teléfono actual: {self.usuario_actual.telefono}")
        
        nuevo_nombre = input("Nuevo nombre (dejar vacío para mantener): ") or self.usuario_actual.nombre
    repositorio_usuario = RepositorioUsuarioMySQL()
    repositorio_producto = RepositorioProductoMySQL()
    repositorio_carrito = RepositorioCarritoMySQL()

    # Inicializar casos de uso
    casos_uso_usuario = CasosUsoUsuario(repositorio_usuario)
    casos_uso_producto = CasosUsoProducto(repositorio_producto)
    caso_uso_obtener_carrito = CasoUsoObtenerCarrito(repositorio_carrito)
    caso_uso_agregar_al_carrito = CasoUsoAgregarAlCarrito(repositorio_carrito, repositorio_producto)
    caso_uso_eliminar_del_carrito = CasoUsoEliminarDelCarrito(repositorio_carrito)
    caso_uso_actualizar_item_carrito = CasoUsoActualizarItemCarrito(repositorio_carrito)
    
    # Inicializar CLI
    comandos = ComandosCLI(
        casos_uso_usuario, 
        casos_uso_producto, 
        caso_uso_obtener_carrito, 
        caso_uso_agregar_al_carrito, 
        caso_uso_eliminar_del_carrito, 
        caso_uso_actualizar_item_carrito
    )
    menu = MenuCLI(comandos)

    # Ejecutar la aplicación
    menu.ejecutar()

if __name__ == "__main__":
    main()
```

#### 2.8 Requirements

**requirements.txt**
```text
mysql-connector-python
python-dotenv
bcrypt
```
