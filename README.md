
# NanoServer

## Copiar NanoServer desde ISO WIndows Server 2016
Copiar el Directorio NanoServer de la raiz de la Imagen de Windows Server 2016, Ingresamos al directorio "NanoServerImageGenerator" 
Ejecutar Windows PowerShell Administrador
## Configuracion Ejecucion Scripts no confiables
```ps1
// Windows PowerShell
// Obtener Directiva de Ejecucion

get-ExecutionPolicy

// Output
  Bypass
```

```ps1
// Windows PowerShell
// Setear Directiva de Ejecucion

Set-ExecutionPolicy Unrestricted

// Output
   Cambio de directiva de ejecución
  La directiva de ejecución te ayuda a protegerte de scripts en los que no confías. Si cambias dicha directiva, podrías exponerte a los riesgos de seguridad descritos en el tema de la Ayuda
  about_Execution_Policies en https:/go.microsoft.com/fwlink/?LinkID=135170. ¿Quieres cambiar la directiva de ejecución?
  [S] Sí  [O] Sí a todo  [N] No  [T] No a todo  [U] Suspender  [?] Ayuda (el valor predeterminado es "N"): S
```
## Importar Modulo NanoServer en PowerShell
Se debe ejecutar Windows PowerShell con permisos de administrador

```ps1
// Windows PowerShell

  Import-Module NanoServerImageGenerator.psm1 -Verbose

// Output
DETALLADO: Se está cargando el módulo desde la ruta de acceso 'D:\NanoServer\NanoServerImageGenerator\NanoServerImageGenerator.psm1'.
DETALLADO: Se está importando la función 'Edit-NanoServerImage'.
DETALLADO: Se está importando la función 'Get-NanoServerPackage'.
DETALLADO: Se está importando la función 'New-NanoServerImage'.
```
## Crear Imagen IIS Nano server
```ps1
// Windows PowerShell
New-NanoServerImage -Edition Standard -DeploymentType Guest -MediaPath E:\ -BasePath .Base -TargetPath .NanoServer01.vhd -ComputerName NanoServer01 -Package Microsoft-NanoServer-IIS-Package
```

### Ejecucion Completa
````ps1
Windows PowerShell
Copyright (C) Microsoft Corporation. Todos los derechos reservados.

PS C:\Windows\system32> get-ExecutionPolicy
Bypass
PS C:\Windows\system32> Set-ExecutionPolicy Unrestricted

Cambio de directiva de ejecución
La directiva de ejecución te ayuda a protegerte de scripts en los que no confías. Si cambias dicha directiva, podrías exponerte a los riesgos de seguridad descritos en el tema de la Ayuda
about_Execution_Policies en https:/go.microsoft.com/fwlink/?LinkID=135170. ¿Quieres cambiar la directiva de ejecución?
[S] Sí  [O] Sí a todo  [N] No  [T] No a todo  [U] Suspender  [?] Ayuda (el valor predeterminado es "N"): S
PS C:\Windows\system32> cd D:\
PS D:\> cd .\NanoServer\
PS D:\NanoServer> cd .\NanoServerImageGenerator\
PS D:\NanoServer\NanoServerImageGenerator> ls


    Directorio: D:\NanoServer\NanoServerImageGenerator


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----       30-01-2020      0:07                es-ES
-a----       15-07-2016     22:30         163433 Convert-WindowsImage.ps1
-a----       25-05-2016     17:42            478 NanoServerImageGenerator.psd1
-a----       14-07-2016     17:18         101216 NanoServerImageGenerator.psm1


PS D:\NanoServer\NanoServerImageGenerator> Import-Module .\NanoServerImageGenerator.psm1 -Verbose
DETALLADO: Se está cargando el módulo desde la ruta de acceso 'D:\NanoServer\NanoServerImageGenerator\NanoServerImageGenerator.psm1'.
DETALLADO: Se está importando la función 'Edit-NanoServerImage'.
DETALLADO: Se está importando la función 'Get-NanoServerPackage'.
DETALLADO: Se está importando la función 'New-NanoServerImage'.
PS D:\NanoServer\NanoServerImageGenerator> New-NanoServerImage -Edition Standard -DeploymentType Guest -MediaPath E:\ -BasePath .Base -TargetPath .NanoServer01.vhd -ComputerName NanoServer01 -
Package Microsoft-NanoServer-IIS-Package

cmdlet New-NanoServerImage en la posición 1 de la canalización de comandos
Proporcione valores para los parámetros siguientes:
AdministratorPassword: *********
Listo. El registro se encuentra en: D:\NanoServer\NanoServerImageGenerator\.Base\Logs\2020-01-30_00-32-55-98
PS D:\NanoServer\NanoServerImageGenerator>
````

## Añadir IIS Server Package en Nano Server de Windows Server 2016
````ps1
Windows PowerShell
  Mount-DiskImage -ImagePath D:\NanoServer\NanoServerImageGenerator\.NanoServer01.vhd
````

````ps1
Windows PowerShell
  Add-WindowsPackage -Path F:\ -PackagePath D:\NanoServer\Packages\Microsoft-NanoServer-IIS-Package.cab
````

````ps1
Windows PowerShell
  DisMount-DiskImage -ImagePath D:\NanoServer\NanoServerImageGenerator\.NanoServer01.vhd
````

## Connect Nano Server desde PowerShell
````ps1
Windows PowerShell
  Enter-PSSession -VMName NanoServer01
````


Copyright (C) Sebastian Marquez. Todos los derechos reservados.
