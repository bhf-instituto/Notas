Un perfil de PowerShell es un script que se ejecuta cuando se inicia PowerShell. Puede usar el perfil como script de inicio para personalizar el entorno. Puede agregar comandos, alias, funciones, variables, módulos, unidades de PowerShell y mucho más. También puede agregar otros elementos específicos de la sesión al perfil para que estén disponibles en cada sesión sin tener que importarlos ni volver a crearlos.


## Variable **$PROFILE**

La variable automática `$PROFILE` almacena las rutas de acceso a los perfiles de PowerShell que están disponibles en la sesión actual.

Para ver una ruta de acceso de perfil, muestre el valor de la `$PROFILE` variable. También puede usar la `$PROFILE` variable en un comando para representar una ruta de acceso.

La `$PROFILE` variable almacena la ruta de acceso al perfil "Usuario actual, Host actual". Los demás perfiles se guardan en las propiedades de nota de la `$PROFILE` variable.

Por ejemplo, la `$PROFILE` variable tiene los siguientes valores en la consola de Windows PowerShell.

- Usuario actual, Host actual: `$PROFILE`
- Usuario actual, Host actual: `$PROFILE.CurrentUserCurrentHost`
- Usuario actual, todos los hosts: `$PROFILE.CurrentUserAllHosts`
- Todos los usuarios, host actual: `$PROFILE.AllUsersCurrentHost`
- Todos los usuarios, todos los hosts: `$PROFILE.AllUsersAllHosts`

Dado que los valores de la `$PROFILE` variable cambian para cada usuario y en cada aplicación host, asegúrese de mostrar los valores de las variables de perfil en cada aplicación host de PowerShell que use.

Para ver los valores actuales de la `$PROFILE` variable, escriba:
```
$PROFILE | Select-Object *
```

Puede usar la `$PROFILE` variable en muchos comandos. Por ejemplo, el siguiente comando abre el perfil "Usuario actual, Host actual" en el Bloc de notas:
```
notepad $PROFILE
```

El siguiente comando determina si se ha creado un perfil "Todos los usuarios, todos los hosts" en el equipo local:
```
Test-Path -Path $PROFILE.AllUsersAllHosts
```

## Creación de un perfil

Para crear un perfil de PowerShell, use el siguiente formato de comando:

PowerShell

```
if (!(Test-Path -Path <profile-name>)) {
  New-Item -ItemType File -Path <profile-name> -Force
}
```

Por ejemplo, para crear un perfil para el usuario actual en la aplicación host de PowerShell actual, use el siguiente comando:

PowerShell

```
if (!(Test-Path -Path $PROFILE)) {
  New-Item -ItemType File -Path $PROFILE -Force
}
```

En este comando, la `if` instrucción impide que sobrescriba un perfil existente. Reemplace el valor de la `$PROFILE` variable por la ruta de acceso al archivo de perfil que desea crear.

## Cómo editar un perfil

Puede abrir cualquier perfil de PowerShell en un editor de texto, como el Bloc de notas.

Para abrir el perfil del usuario actual en la aplicación host de PowerShell actual en el Bloc de notas, escriba:

PowerShell

```
notepad $PROFILE
```

Para abrir otros perfiles, especifique el nombre del perfil. Por ejemplo, para abrir el perfil de todos los usuarios de todas las aplicaciones host, escriba:

PowerShell

```
notepad $PROFILE.AllUsersAllHosts
```

Para aplicar los cambios, guarde el archivo de perfil y reinicie PowerShell.

[](https://learn.microsoft.com/es-es/powershell/module/microsoft.powershell.core/about/about_profiles?view=powershell-7.5#how-to-choose-a-profile)

## Cómo elegir un perfil

Si usa varias aplicaciones host, coloque los elementos que usa en todas las aplicaciones host en el `$PROFILE.CurrentUserAllHosts` perfil. Coloque elementos específicos de una aplicación host, como un comando que establece el color de fondo de una aplicación host, en un perfil específico de esa aplicación host.

Si es administrador que personaliza PowerShell para muchos usuarios, siga estas instrucciones:

- Almacenar los elementos comunes en el `$PROFILE.AllUsersAllHosts` perfil
- Almacenar elementos específicos de una aplicación host en `$PROFILE.AllUsersCurrentHost` perfiles específicos de la aplicación host
- Almacenar elementos para usuarios concretos en los perfiles específicos del usuario

Asegúrese de comprobar la documentación de la aplicación host para ver cualquier implementación especial de perfiles de PowerShell.

[](https://learn.microsoft.com/es-es/powershell/module/microsoft.powershell.core/about/about_profiles?view=powershell-7.5#how-to-use-a-profile)

## Uso de un perfil

Muchos de los elementos que se crean en PowerShell y la mayoría de los comandos que se ejecutan afectan solo a la sesión actual. Al finalizar la sesión, se eliminan los elementos.

Los comandos y elementos específicos de la sesión incluyen variables de PowerShell, variables de entorno, alias, funciones, comandos y módulos de PowerShell que agregue a la sesión.

Para guardar estos elementos y ponerlos a disposición en todas las sesiones futuras, agréguelos a un perfil de PowerShell.

Otro uso común para los perfiles es guardar funciones, alias y variables usados con frecuencia. Al guardar los elementos de un perfil, puede usarlos en cualquier sesión aplicable sin volver a crearlos.

[](https://learn.microsoft.com/es-es/powershell/module/microsoft.powershell.core/about/about_profiles?view=powershell-7.5#how-to-start-a-profile)

## Cómo iniciar un perfil

Al abrir el archivo de perfil, está en blanco. Sin embargo, puede rellenarlo con las variables, alias y comandos que se usan con frecuencia.

Estas son algunas sugerencias para empezar.

[](https://learn.microsoft.com/es-es/powershell/module/microsoft.powershell.core/about/about_profiles?view=powershell-7.5#add-a-function-that-lists-aliases-for-any-cmdlet)

### Adición de una función que muestra alias para cualquier cmdlet

PowerShell

```
function Get-CmdletAlias ($cmdletName) {
  Get-Alias |
    Where-Object -FilterScript {$_.Definition -like "$cmdletName"} |
      Format-Table -Property Definition, Name -AutoSize
}
```

[](https://learn.microsoft.com/es-es/powershell/module/microsoft.powershell.core/about/about_profiles?view=powershell-7.5#customize-your-console)

### Personalización de la consola

PowerShell

```
function CustomizeConsole {
  $hostTime = (Get-ChildItem -Path $PSHOME\pwsh.exe).CreationTime
  $hostVersion="$($Host.Version.Major)`.$($Host.Version.Minor)"
  $Host.UI.RawUI.WindowTitle = "PowerShell $hostVersion ($hostTime)"
  Clear-Host
}
CustomizeConsole
```

[](https://learn.microsoft.com/es-es/powershell/module/microsoft.powershell.core/about/about_profiles?view=powershell-7.5#add-a-customized-powershell-prompt)

### Adición de un símbolo del sistema personalizado de PowerShell

PowerShell

```
function prompt {
    $Env:COMPUTERNAME + "\" + (Get-Location) + "> "
}
```

Para obtener más información sobre el símbolo del sistema de PowerShell, consulte [about_Prompts](https://learn.microsoft.com/es-es/powershell/module/microsoft.powershell.core/about/about_prompts?view=powershell-7.5).

Para ver otros ejemplos de perfil, consulte [Personalización del entorno](https://learn.microsoft.com/es-es/powershell/scripting/learn/shell/creating-profiles) de shell.

[](https://learn.microsoft.com/es-es/powershell/module/microsoft.powershell.core/about/about_profiles?view=powershell-7.5#the-noprofile-parameter)

## Parámetro NoProfile

Para iniciar PowerShell sin perfiles, use el **parámetro NoProfile** de `pwsh.exe`, el programa que inicia PowerShell.

Para empezar, abra un programa que pueda iniciar PowerShell, como Cmd.exe o PowerShell. También puede usar el cuadro de diálogo Ejecutar en Windows.

Escriba:

PowerShell

```
pwsh -NoProfile
```

Para obtener una lista completa de los parámetros de `pwsh.exe`, escriba:

PowerShell

```
pwsh -?
```

[](https://learn.microsoft.com/es-es/powershell/module/microsoft.powershell.core/about/about_profiles?view=powershell-7.5#profiles-and-execution-policy)

## Perfiles y directiva de ejecución

La directiva de ejecución de PowerShell determina, en parte, si puede ejecutar scripts y cargar archivos de configuración, incluidos los perfiles. La **directiva de ejecución restringida** es la predeterminada. Impide que se ejecuten todos los scripts, incluidos los perfiles. Si usa la directiva "Restringida", el perfil no se ejecuta y no se aplica su contenido.

Un `Set-ExecutionPolicy` comando establece y cambia la directiva de ejecución. es uno de los pocos comandos que se aplican en todas las sesiones de PowerShell porque el valor se guarda en el Registro. No es necesario establecerlo al abrir la consola y no tiene que almacenar un `Set-ExecutionPolicy` comando en el perfil.

[](https://learn.microsoft.com/es-es/powershell/module/microsoft.powershell.core/about/about_profiles?view=powershell-7.5#profiles-and-remote-sessions)

## Perfiles y sesiones remotas

Los perfiles de PowerShell no se ejecutan automáticamente en sesiones remotas, por lo que los comandos que agregan los perfiles no están presentes en la sesión remota. Además, la `$PROFILE` variable automática no se rellena en sesiones remotas.

Para ejecutar un perfil en una sesión, use el [cmdlet Invoke-Command](https://learn.microsoft.com/es-es/powershell/module/microsoft.powershell.core/invoke-command?view=powershell-7.5) .

Por ejemplo, el siguiente comando ejecuta el perfil "Usuario actual, Host actual" desde el equipo local de la sesión en `$s`.

PowerShell

```
Invoke-Command -Session $s -FilePath $PROFILE
```

El siguiente comando ejecuta el perfil "Usuario actual, Host actual" del equipo remoto en la sesión de `$s`. Dado que la `$PROFILE` variable no se rellena, el comando usa la ruta de acceso explícita al perfil. Usamos el operador dot sourcing para que el perfil se ejecute en el ámbito actual en el equipo remoto y no en su propio ámbito.

PowerShell

```
Invoke-Command -Session $s -ScriptBlock {
  . "$HOME\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1"
}
```

Después de ejecutar este comando, los comandos que el perfil agrega a la sesión están disponibles en `$s`.