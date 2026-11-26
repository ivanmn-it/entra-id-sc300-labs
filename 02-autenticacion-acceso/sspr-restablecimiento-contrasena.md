# Restablecimiento de contraseña de autoservicio (SSPR)

- **Área del SC-300:** Implementar y administrar la autenticación y el acceso
- **Licencia usada:** Entra ID P2 (prueba activa)

## Qué quería conseguir

Que los usuarios de un grupo puedan restablecer su propia contraseña sin pasar por soporte, ver cómo se registran, comprobar el proceso completo con un usuario del grupo y qué pasa con uno que está fuera.

## Requisitos previos

- Un grupo piloto de seguridad con usuarios sin rol de administrador
- Un correo externo al tenant (un Gmail de pruebas) como método de verificación
- Una ventana de incógnito para las pruebas de los usuarios

## Pasos

### 1. Mirar los valores predeterminados de seguridad

Antes de empezar revisé el estado de los valores predeterminados de seguridad, porque condicionan lo que se le pide a un usuario nuevo al iniciar sesión. Estaban activados, y entre otras cosas exigen que los usuarios registren la verificación en dos pasos.

![Valores predeterminados de seguridad](img/sspr-00-valores-predeterminados.png)

### 2. Crear el grupo piloto

Creé un grupo de seguridad de pertenencia asignada, `GRP-SSPR-Piloto`, con usuarios sin rol de administrador, entre ellos `lab.user02`, y comprobé que los miembros estaban dentro.

![Nuevo grupo y selección de miembros](img/sspr-01-grupo-piloto.png)

![Grupo creado con sus miembros](img/sspr-02-grupo-piloto-creado.png)

### 3. Activar SSPR solo para ese grupo

En mi portal, Restablecimiento de contraseña aparece dentro de Identidad, no en Protección. En Propiedades elegí activarlo para usuarios seleccionados y escogí el grupo piloto.

![Propiedades del restablecimiento de contraseña](img/sspr-03-propiedades.png)

![Aviso de directiva guardada](img/sspr-04-guardado.png)

### 4. Elegir los métodos de verificación

La pantalla de métodos del restablecimiento de contraseña solo ofrece las preguntas de seguridad, y avisa de que dejarán de usarse en marzo de 2027. El resto de métodos se gestionan desde la directiva de métodos de autenticación. Dejé el número de métodos requeridos en 2 y no activé las preguntas de seguridad.

![Pantalla de métodos del restablecimiento](img/sspr-05-metodos.png)

![Directiva de métodos de autenticación](img/sspr-06-metodos-lista.png)

![Aviso de directiva aplicada](img/sspr-07-directiva-aplicada.png)

### 5. Revisar la configuración del registro

También miré la configuración de registro, que decide si los usuarios deben registrar sus datos al iniciar sesión y cada cuánto deben confirmarlos.

![Configuración del registro](img/sspr-08-datos-registro.png)

### 6. Probar el registro como usuario del grupo

En una ventana de incógnito entré con `lab.user02`. Al ser su primera vez, le pidió actualizar la contraseña.

![Pantalla de actualizar contraseña](img/sspr-09-actualizar-contrasena.png)

Después le pidió proteger la cuenta y registrar dos métodos de verificación.

![Pantalla para proteger la cuenta](img/sspr-10-verificacion.png)

El primer método fue una aplicación de autenticación.

![Método 1 de 2: instalar la aplicación](img/sspr-11-autenticador.png)

![Aplicación de autenticación añadida](img/sspr-12-autenticador-anadido.png)

El segundo fue una dirección de correo electrónico, que hay que verificar con un código que llega a esa dirección.

![Método 2 de 2: añadir un correo](img/sspr-13-anadir-correo.png)

![Verificar el correo con un código](img/sspr-14-verificar-correo.png)

![Configuración completada](img/sspr-15-configuracion-completada.png)

### 7. Restablecer la contraseña como usuario del grupo

Desde la pantalla de inicio de sesión entré por el enlace de contraseña olvidada con `lab.user02`.

![Introducir el usuario](img/sspr-16-restablecer-cuenta.png)

Hay que superar dos pasos de comprobación, con los dos métodos registrados: primero el código que llega al correo y después el de la aplicación.

![Elegir el método de verificación](img/sspr-17-verificar-correo.png)

![Código recibido por correo](img/sspr-18-codigo-correo.png)

![Código de la aplicación de autenticación](img/sspr-19-anadir-codigo-app.png)

Superadas las dos comprobaciones, Entra me dejó elegir una contraseña nueva.

![Elegir la nueva contraseña](img/sspr-20-nueva-contrasena.png)

![Contraseña restablecida](img/sspr-21-contrasena-restablecida.png)

Después llegó un correo confirmando el cambio de contraseña.

![Correo de confirmación](img/sspr-22-correo-confirmacion.png)

### 8. Probar con un usuario fuera del grupo

Repetí el proceso con `lab.user01`, que no está en el grupo piloto y no tiene ningún rol de administrador.

![Pantalla de contraseña con el enlace de contraseña olvidada](img/sspr-23-prueba-usuario-fuera-grupo.png)

![Introducir el usuario fuera del grupo](img/sspr-24-verificar-usuario-fuera-grupo.png)

En lugar del proceso de verificación, Entra responde que el restablecimiento de contraseña no está habilitado para esa cuenta.

![Error del usuario fuera del grupo](img/sspr-25-error-usuario-fuera-grupo.png)

### 9. Ver los registros de auditoría

En los registros de auditoría aparecen los dos resultados: el restablecimiento correcto del usuario dentro del grupo y el fallido del usuario de fuera, que queda registrado como un error.

![Registros de auditoría](img/sspr-26-auditoria.png)

![Restablecimiento correcto del usuario dentro del grupo](img/sspr-27-restablecimiento-correcto-usuario-dentro-grupo.png)

![Error del usuario fuera del grupo](img/sspr-28-error-usuario-fuera-grupo.png)

## Cómo comprobar que funciona

- El usuario del grupo se registró con dos métodos y pudo restablecer su contraseña sin ayuda de un administrador
- El usuario fuera del grupo no pudo hacerlo: Entra le indicó que el restablecimiento no estaba habilitado para su cuenta
- Los dos intentos quedaron registrados en la auditoría

## Qué me falló o me costó entender

**El menú no está donde dicen algunas guías.** El restablecimiento de contraseña no aparece en Protección: en mi portal está en Identidad. La documentación y algunos tutoriales lo sitúan en otro sitio, y los menús de Microsoft cambian con frecuencia.

**La pantalla de métodos ya no sirve para elegir métodos.** Esperaba marcar ahí el correo y la aplicación de autenticación, pero solo ofrece las preguntas de seguridad. El resto se gestiona desde la directiva de métodos de autenticación, y para eso hay que cambiar de pantalla.

**Un usuario nuevo tiene que hacer tres cosas antes de poder restablecer nada.** Cambiar la contraseña en su primer inicio de sesión, registrar los métodos de verificación y solo después podrá usar el autoservicio.

**Los administradores van aparte.** La propia pantalla de configuración avisa de que los administradores siempre están habilitados para el autoservicio y tienen que usar dos métodos. Por eso el usuario fuera del grupo tenía que ser uno sin ningún rol asignado.

## Qué haría distinto en un entorno real

- No usaría las preguntas de seguridad: van a retirarse en marzo de 2027 y son un método flojo, porque sus respuestas pueden adivinarse.
- Empezaría con un grupo piloto, como aquí, y ampliaría a toda la organización cuando el proceso estuviera probado.
- Avisaría a los usuarios de que registren sus métodos antes de necesitarlos, porque quien se olvida de la contraseña sin haberse registrado no puede restablecerla por sí mismo.
- Revisaría los registros de auditoría tras activarlo, para ver si hay intentos fallidos que indiquen un problema de configuración.

## Enlaces

- [Tutorial: activar el restablecimiento de contraseña de autoservicio (Microsoft Learn)](https://learn.microsoft.com/entra/identity/authentication/tutorial-enable-sspr)
- [Cómo funciona el restablecimiento de contraseña de autoservicio (Microsoft Learn)](https://learn.microsoft.com/training/modules/allow-users-reset-their-password/2-self-service-password-reset)
