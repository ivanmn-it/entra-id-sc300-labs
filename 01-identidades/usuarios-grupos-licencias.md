# Usuarios, grupos y licencias por grupo

- **Área del SC-300:** Implementar identidades en Microsoft Entra ID
- **Licencia necesaria:** Entra ID P1 o superior (en mi caso, la prueba de P2)
- **Fecha:** Noviembre de 2026

## Qué quería conseguir

Crear tres usuarios de prueba, meterlos en un grupo de seguridad y que la licencia se les asigne por pertenecer al grupo, en lugar de dársela a cada uno a mano. Después, comprobar que al sacar a un usuario del grupo pierde la licencia.

## Requisitos previos

- Un tenant de laboratorio con la prueba de Entra ID P2 activa.
- Una cuenta de administrador del tenant. Para asignar licencias a un grupo en el Centro de administración de Microsoft 365 hace falta, como mínimo, el rol de Administrador de licencias.

## Pasos

### 1. Comprobar la licencia disponible

En Entra, Facturación → Licencias → Todos los productos. Tengo 25 licencias de Microsoft Entra ID P2, una asignada (la mía).

![Licencias disponibles en el tenant](img/00-licencia-p2.png)

### 2. Crear los usuarios

Identidad → Usuarios → Todos los usuarios → Nuevo usuario → Crear nuevo usuario.

![Botón para crear un usuario nuevo](img/01-boton-crear.png)

![Formulario de creación de usuario](img/01-nuevo-usuario.png)

El primer usuario, `lab.user01`, lo creé **sin ubicación de uso** a propósito, porque quería ver qué pasaba al asignarle la licencia.

![Usuario 01 creado sin ubicación de uso](img/02-sin-ubicacion-user01.png)

`lab.user02` y `lab.user03` los creé igual, pero con la ubicación de uso en España.

![Usuario con ubicación de uso en España](img/03-ubicacion-uso.png)

![Lista con los tres usuarios creados](img/04-lista-usuarios.png)

### 3. Crear el grupo

Identidad → Grupos → Todos los grupos → Nuevo grupo. Grupo de seguridad, pertenencia asignada, nombre `GRP-Licencia-P2`.

![Pantalla de nuevo grupo](img/05-crear-grupo.png)

![Propiedades del grupo](img/06-propiedades-grupo.png)

Añadí los tres usuarios como miembros.

![Selección de miembros](img/07-seleccionar-miembros-grupo.png)

![Grupo creado con sus tres miembros](img/08-grupo-creado-con-miembros.png)

### 4. Asignar la licencia al grupo

Esto no se hace en Entra sino en el **Centro de administración de Microsoft 365** (admin.microsoft.com), en Facturación → Licencias. Abrí Microsoft Entra ID P2 y usé el botón **Asignar licencias** de esa página.

![Pantalla de licencias](img/09-licencias.png)

![Botón de asignar licencias](img/10-asignar-licencias.png)

![Selección del grupo](img/11-seleccionar-grupo.png)

![Aviso de que se están asignando las licencias](img/12-resultado-asignacion.png)

Al terminar, el contador de la suscripción pasó a 4 licencias asignadas: la mía más las tres del grupo.

![Resultado de la asignación en la lista de licencias](img/13-resultado-licencias.png)

### 5. Comprobar el resultado

La pestaña **Errores y problemas** estaba vacía.

![Pestaña de errores sin ningún problema](img/14-errores.png)

Al abrir `lab.user01` en Entra, su ubicación de uso ya no estaba vacía: se había puesto en España.

![Ubicación de uso de lab.user01 después de asignar la licencia](img/15-ubicacion-user01.png)

En la ficha del usuario, la licencia aparece como heredada del grupo.

![Licencia heredada del grupo](img/16-licencia-heredada.png)

### 6. Quitar a un usuario del grupo

Quité a `lab.user03` desde el grupo → Miembros.

![Quitar a lab.user03 del grupo](img/17-eliminar-user03-de-grupo.png)

![Aviso de miembro quitado](img/18-user03-eliminado-de-grupo.png)

Al entrar en sus licencias todavía aparecía la de P2. Pulsé Actualizar y desapareció al momento.

![lab.user03 sin la licencia](img/19-licencia-retirada-user03.png)

## Cómo comprobé que funciona

- El contador de licencias asignadas pasó de 1 a 4 al asignar el grupo.
- La pestaña de errores quedó vacía.
- La ficha de cada usuario mostraba la licencia como heredada del grupo.
- Al quitar a `lab.user03`, su licencia dejó de aparecer.

## Qué me falló o me costó entender

**La asignación ya no se hace en Entra.** La propia pantalla de licencias de Entra lo avisa: añadir, quitar y reprocesar asignaciones solo está disponible en el Centro de administración de Microsoft 365. Algunas guías antiguas siguen explicando el procedimiento en Entra y ya no coincide con lo que se ve. En el centro de M365 no hay una pestaña "Grupos" como esperaba: se entra en la licencia y se usa el botón Asignar licencias.

**Esperaba un error y no lo hubo.** Dejé a `lab.user01` sin ubicación de uso pensando que la licencia fallaría. No falló. Mirando la documentación de Microsoft, cuando se asigna una licencia a un grupo, los usuarios sin ubicación de uso heredan la del directorio, y si ya tienen una no se les toca. Por eso a `lab.user01` se le rellenó España sin que yo hiciera nada.

**La licencia del usuario quitado seguía a la vista.** Después de sacar a `lab.user03` del grupo, su licencia aparecía todavía hasta que pulsé Actualizar. No era un retraso del sistema, era la pantalla sin refrescar.

## Qué haría distinto en un entorno real

- Fijaría siempre la ubicación de uso al crear al usuario. Que se herede la del directorio puede dejar un dato que no coincide con el país real de la persona, y Microsoft recomienda ponerla antes de usar licencias por grupo.
- Revisaría la pestaña de errores después de cada asignación en lugar de dar por hecho que ha ido bien.
- Con muchos usuarios usaría grupos dinámicos por departamento en vez de añadir miembros a mano. Es el siguiente laboratorio.

## Enlaces

- [Asignar licencias por pertenencia a grupo (Microsoft Learn)](https://learn.microsoft.com/entra/identity/users/licensing-groups-assign)
- [Identificar y resolver problemas de licencias de un grupo (Microsoft Learn)](https://learn.microsoft.com/en-us/entra/fundamentals/licensing-groups-resolve-problems)
