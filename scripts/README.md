# Scripts

Scripts de PowerShell con Microsoft Graph. Antes de ejecutar nada:

```powershell
Install-Module Microsoft.Graph -Scope CurrentUser
Connect-MgGraph -Scopes "User.Read.All"
```

Pido siempre el mínimo de permisos que necesita cada script y lo indico en la cabecera.

Los resultados con datos (CSV, exportaciones) no se suben: están en el `.gitignore`.
