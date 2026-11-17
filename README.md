# Laboratorios de Microsoft Entra ID (SC-300)

Repositorio donde voy documentando las prácticas que hago en mi propio tenant de laboratorio mientras repaso y amplío lo que aprendí para la certificación **SC-300 (Microsoft Identity and Access Administrator)**, que aprobé en julio de 2026.

Trabajo en soporte desde backoffice y quiero dar el salto a un puesto de administración de identidades en remoto. Aquí dejo lo que voy haciendo, con sus pasos, sus capturas y también lo que me ha fallado por el camino.

## Cómo está organizado

| Carpeta | Contenido |
|---|---|
| [`01-identidades`](01-identidades/) | Usuarios, grupos, licencias, invitados B2B, identidad híbrida |
| [`02-autenticacion-acceso`](02-autenticacion-acceso/) | MFA, SSPR, Acceso condicional, Identity Protection |
| [`03-aplicaciones`](03-aplicaciones/) | Registro de apps, aplicaciones empresariales, SSO, consentimiento |
| [`04-gobernanza`](04-gobernanza/) | PIM, revisiones de acceso, gestión de derechos, términos de uso |
| [`scripts`](scripts/) | Scripts de PowerShell con Microsoft Graph |
| [`plantillas`](plantillas/) | Plantilla que uso para cada laboratorio |

Cada laboratorio es un archivo `.md` dentro de su carpeta. Los índices de cada carpeta indican cuáles tengo hechos.

## Entorno

- Tenant de laboratorio en Azure (cuenta gratuita)
- Licencia Entra ID P2 de prueba (algunos laboratorios dependen de ella y los marco)
- PowerShell 7 con el módulo `Microsoft.Graph`
- Todo lo que se ve en las capturas es de un tenant de pruebas. No hay datos reales

## Laboratorios hechos

Voy marcando aquí a medida que los termino.

- [x] [Usuarios, grupos y licencias por grupo](01-identidades/usuarios-grupos-licencias.md)

## Contacto

- LinkedIn: [Mi perfil](https://www.linkedin.com/in/ivanmn)
- Certificación: [Microsoft Certified: Identity and Access Administrator Associate (SC-300)](https://learn.microsoft.com/es-es/users/ivanmoraisnez-7305/credentials/2b4ca728888dc50a)
