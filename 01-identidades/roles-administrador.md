# Roles de administrador y mínimo privilegio

- **Área del SC-300:** Implementar identidades en Microsoft Entra ID
- **Licencia necesaria:** ninguna de pago para asignar roles integrados (yo lo hice con la prueba de P2 activa)

## Qué quería conseguir

Dar a un usuario de prueba un rol con pocos permisos, comprobar qué puede y qué no puede hacer, compararlo con un rol más amplio y localizar sus acciones en los registros de auditoría.

## Requisitos previos

- Una cuenta de administrador global
- El usuario de prueba `lab.user01` y otro usuario, `lab.user02`, sobre el que probar
- Una ventana de incógnito, para iniciar sesión como `lab.user01` sin cerrar mi sesión de administrador

## Pasos

### 1. Ver los roles que existen

Identidad → Roles y administradores → Roles y administradores. La lista indica para cada rol cuántas asignaciones tiene y marca los que tienen privilegios. El Administrador global solo tenía una asignación, la mía.

![Lista de roles integrados](img/roles-00-lista-roles.png)

![Asignaciones del rol Administrador global](img/roles-01-lista-admin-global.png)

### 2. Elegir un rol de pocos permisos

Elegí **Administrador del departamento de soporte técnico**, cuya descripción dice que puede restablecer las contraseñas de usuarios no administradores y de administradores del departamento de soporte técnico. No es el mismo que **Administrador de soporte técnico del servicio**, que sirve para ver el estado del servicio y gestionar solicitudes de soporte con Microsoft.

![Descripción del rol de soporte](img/roles-02-rol-soporte-descripcion.png)

### 3. Asignar el rol a lab.user01

Dentro del rol, Agregar asignaciones. Elegí a `lab.user01` y, en el formulario, el tipo de asignación activa y una justificación con el motivo del laboratorio.

![Dónde se asigna el rol](img/roles-03-agregar-asignacion.png)

![Selección de lab.user01](img/roles-03b-agregar-asignacion.png)

![Formulario de la asignación](img/roles-03c-agregar-asignacion.png)

Entra avisa de que la asignación está en curso y, después, la lista muestra al usuario con el rol.

![Aviso de asignación en curso](img/roles-04-asignacion-creada.png)

![Lista de asignaciones con lab.user01](img/roles-04b-asignacion-creada.png)

### 4. Probar como lab.user01

En una ventana de incógnito entré con `lab.user01`. Al ser su primer inicio de sesión, me pidió cambiar la contraseña.

![Pantalla para actualizar la contraseña](img/roles-05a-primer-inicio-sesion.png)

Después me pidió proteger la cuenta con verificación en dos pasos. Lo registré con una app de autenticación distinta de Microsoft Authenticator.

![Pantalla para registrar la verificación en dos pasos](img/roles-05b-primer-inicio-sesion.png)

Con el rol de soporte pude restablecer la contraseña de `lab.user02`: Entra muestra una contraseña temporal para entregársela al usuario.

![Usuario sobre el que restablecer la contraseña](img/roles-06a-restablecer-contrasena.png)

![Aviso de contraseña restablecida](img/roles-06b-restablecer-contrasena.png)

En cambio, las opciones de crear usuario y de eliminar usuario salen sombreadas, sin posibilidad de pulsarlas.

![Opción de crear usuario sombreada](img/roles-07a-crear-usuario-denegado.png)

![Opción de eliminar usuario sombreada](img/roles-07b-borrar-usuario-denegado.png)

### 5. Cambiar a un rol más amplio

Desde mi cuenta de administrador quité el rol a `lab.user01`: primero se ve el rol con la asignación y, al borrarla, desaparece.

![El rol con lab.user01 asignado](img/roles-08a-eliminar-rol.png)

![Aviso de que la asignación se ha eliminado](img/roles-08b-eliminar-rol.png)

![El rol sin la asignación](img/roles-08c-eliminar-rol.png)

Y le asigné **Administrador de usuarios**: el rol empieza sin asignaciones y termina con `lab.user01` asignado.

![El rol Administrador de usuarios, sin asignaciones](img/roles-09-agregar-nuevo-rol.png)

![Formulario para asignar el nuevo rol](img/roles-09b-agregar-nuevo-rol.png)

![Aviso de asignación del nuevo rol](img/roles-09c-agregar-nuevo-rol.png)

![El rol con lab.user01 asignado](img/roles-09d-agregar-nuevo-rol.png)

No hizo falta cerrar sesión con `lab.user01`: bastó actualizar la página para que el cambio de rol se notara.

### 6. Repetir la prueba

Con el rol de Administrador de usuarios, las opciones que antes salían sombreadas ya se podían usar. Creé un usuario de prueba y luego lo eliminé.

![Crear un usuario, captura 1](img/roles-10a-crear-usuario-permitido.png)

![Crear un usuario, captura 2](img/roles-10b-crear-usuario-permitido.png)

![Eliminar el usuario de prueba](img/roles-10c-eliminar-usuario-permitido.png)

### 7. Ver las acciones en los registros de auditoría

Identidad → Supervisión y mantenimiento → Registros de auditoría. Ahí aparecen los eventos de las pruebas: borrar un usuario, crear un usuario y restablecer una contraseña. En el detalle de cada evento, el usuario que lo inició es `lab.user01`, y en la lista aparecen todas las acciones que realizó. Eso es lo que permite saber qué hizo cada persona con su rol.

![Registros de auditoría](img/roles-11a-registros-auditoria.png)

![Registro de borrar un usuario](img/roles-11b-registro-borrar-usuario.png)

![Registro de crear un usuario](img/roles-11c-registro-crear-usuario.png)

![Registro de restablecer una contraseña](img/roles-11d-registro-reset-contrasena.png)

### 8. Retirar el rol

Para dejar el tenant limpio, quité el rol a `lab.user01`.

![Asignaciones retiradas](img/roles-12-asignaciones-retiradas.png)

![Aviso de asignación eliminada](img/roles-12b-asignaciones-retiradas.png)

## Cómo comprobé que funciona

- Con el rol de soporte pude restablecer una contraseña, pero no crear ni eliminar usuarios
- Con Administrador de usuarios sí pude crear y eliminar un usuario
- Los cambios de rol se aplicaron al momento
- Los registros de auditoría recogieron las acciones de las pruebas, con `lab.user01` como quien las realizó

## Qué me falló o me costó entender

**Los nombres de los roles se parecen mucho.** "Administrador de soporte técnico del servicio" y "Administrador del departamento de soporte técnico" suenan casi igual, pero el primero gestiona solicitudes de soporte con Microsoft y el segundo atiende a los usuarios de la organización. Hay que leer siempre la descripción antes de asignar un rol.

**Un usuario nuevo no entra directamente.** Antes de poder usar el portal, `lab.user01` tuvo que cambiar la contraseña y registrar la verificación en dos pasos. Pude usar otra app de autenticación distinta de Microsoft Authenticator.

**Los cambios de rol fueron inmediatos.** No tuve que esperar ni cerrar sesión: actualizar la página fue suficiente.

**El portal avisa antes de que falles.** Las acciones sin permiso aparecen sombreadas, a diferencia de lo que me pasó al añadir miembros a mano a un grupo dinámico, donde el error salía al confirmar.

## Qué haría distinto en un entorno real

- Daría siempre el rol más pequeño que cubra la tarea. Un rol de soporte no es inofensivo: el Administrador del departamento de soporte técnico también está marcado como rol con privilegios, porque puede restablecer contraseñas.
- Pondría fecha de fin a las asignaciones temporales, para que el acceso se retire solo y no dependa de que alguien se acuerde.
- Limitaría el rol a un grupo concreto de usuarios con una unidad administrativa, en vez de darlo sobre todo el directorio.
- Revisaría los registros de auditoría con cierta frecuencia, no solo cuando algo falla.

## Enlaces

- [Prácticas recomendadas para los roles de Microsoft Entra (Microsoft Learn)](https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/best-practices)
- [Roles con menos privilegios por tarea en Microsoft Entra ID (Microsoft Learn)](https://learn.microsoft.com/en-us/entra/identity/role-based-access-control/delegate-by-task)
- [Roles integrados de Microsoft Entra (Microsoft Learn)](https://learn.microsoft.com/azure/active-directory/roles/permissions-reference)
