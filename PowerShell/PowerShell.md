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

> Esto funciona solo para Powershell,  pero cada SO tiene su propia manera de definir variables de entorno, para solucionar este problema de compatibilidad se puede usar el paquete de node [[cross-env]].