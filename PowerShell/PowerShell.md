Para instalar csi ([[REPL]] de [[CSharp]]) 
```
 dotnet tool install --global dotnet-csi
```

### Recargar el perfil (sin reiniciar PowerShell)
```powershell
. $PROFILE
```

### Para setear variables de entorno en Node
```powershell
$env:PORT="1234"; node index.js
```