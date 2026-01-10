Para instalar algo : 

```bash
> sudo apt-get install ... 
```

### Apache2: 

```bash
sudo apt-get install apache2
## para ver el estado del servidor apache : 
sudo systemctl status apache2
```

Ver IP de la  maquina: 

```bash
> ip address 
```


### 1️⃣ Usar `less` para paginar la salida
 poder navegar hacia arriba y abajo en una respuesta larga de consola

```bash
ls -l | less

## o si querés mantener colores:

ls --color=auto -l | less -R
```

### comandos de directorio

```bash   
	// Print Working Directory
	pwd
	// Listar archivos y carpetas
	ls        
	ls -l     # formato largo
	ls -a     # incluye archivos ocultos
	ls -la    # largo + ocultos
	ls -lh    # tamaños legibles (KB, MB)
	
	

```

#### Cambiar directorio

| Comando | Qué hace                      |
| ------- | ----------------------------- |
| `cd ..` | subir un nivel                |
| `cd ~`  | ir al home                    |
| `cd /`  | ir a la raíz                  |
| `cd -`  | volver al directorio anterior |

🔹 `cat` – mostrar contenido completo
`cat archivo.txt`

🔹 `less` – ver archivos largos (MUY importante)
`less archivo.log`

Atajos:

- `q` salir
- `/texto` buscar
- `n` siguiente coincidencia

🔹 `head` / `tail`

```bash
head archivo.txt     # primeras líneas
tail archivo.txt     # últimas líneas
tail -f archivo.log  # ver logs en tiempo real

```

### 📂 Manipulación básica de carpetas

🔹 `mkdir` – crear directorio
🔹 `rmdir` – borrar carpeta vacía

### 📄 Manipulación básica de archivos

🔹 `touch` – crear archivo **vacío**
🔹 `cp` – copiar
🔹 `mv` – mover o renombrar
🔹 `rm` – borrar (⚠️ peligro, en servidores **NO hay papelera**.)

### 🔎 Buscar archivos 

🔹 `find`               `find / -name "*.log"`  `find / -name archivo.txt `
🔹 `grep` – buscar texto dentro de archivos     `grep "ERROR" archivo.log` 
`grep -R "listen" /etc`

### 🧭 Ayudas rápidas

🔹 `tree` – ver estructura (si está instalado)
🔹 `stat` – info detallada `stat archivo.txt`

### 🧠 Atajos de teclado IMPORTANTE
|Atajo|Función|
|---|---|
|`Tab`|autocompletar|
|`Ctrl + C`|cancelar comando|
|`Ctrl + L`|limpiar pantalla|
|`↑ / ↓`|historial|
|`Ctrl + A`|inicio de línea|
|`Ctrl + E`|fin de línea|
### 🧩 Directorios clave en Ubuntu Server
| Ruta           | Uso                      |
| -------------- | ------------------------ |
| `/home`        | usuarios                 |
| `/etc`         | configuraciones          |
| `/var`         | archivos variables       |
| `/var/www`     | web apps                 |
| `/var/log`     | logs                     |
| `/opt`         | apps externas            |
| `/srv`         | servicios                |
| `/tmp`         | temporales               |
| `/bin` `/sbin` | comandos del sistema     |
| `/root`        | carpeta del usuario      |
| `/usr`         | apps y tools del sistema |
### 🔥 Comandos que usarás TODO el tiempo

```bash
pwd
ls -la
cd
less
nano / vim
cp
mv
rm
find
grep

```

### 🔹 ¿Qué hace `source`?

```bash
source archivo
```

👉 **Ejecuta el contenido de un archivo dentro de la shell actual**  
No abre una shell nueva.  
No corre el archivo “aparte”.

Es **exactamente lo mismo** que:
```bash
. archivo
```

🔹 ¿Por qué existe?
```bash
./script.sh
```

Linux hace esto:
- crea una **subshell**
- ejecuta el script ahí
- cuando termina → **todo se pierde**

```bash
# script.sh
MI_VAR=hola

# ./script.sh
echo $MI_VAR
# (vacío)
```
❌ La variable no existe en tu shell.

🔹 Qué cambia con `source`?
```bash
source script.sh
echo $MI_VAR
# hola
```
✔ La variable **sí queda cargada**

🔹 creo un servicio, recargo
```bash
/etc/systemd/system$ sudo nano project-1.service
/etc/systemd/system$ sudo systemctl daemon-reload
```
acá está el archivo .service 
```bash
/etc/systemd/system$
```

🔹 me doy permisos para guardar archivos en una carpeta
```bash
sudo chown -R mirko:mirko /var/www/project-1/src
```

🔹miro el log de la consola de mi app node por ejemplo `journalctl`
```bash
# a tiempo real
sudo journalctl -u project-1 -f

# logs completos
sudo journalctl -u project-1

```



## 🔹 Comandos útiles PM2

```bash
pm2 restart backend-3001
pm2 stop backend-3002 
pm2 delete backend-3002 
pm2 monit
```


###🔹 ejecutar un script

```bash
chmod +x update-cloudflare-ddns.sh`
```
- `chmod` → cambia los **permisos** de un archivo.
- `+x` → agrega el permiso de **ejecución**.
- `update-cloudflare-ddns.sh` → el archivo al que le aplicas el permiso.

Después de esto, puedes ejecutar tu script directamente así:

```bash
./update-cloudflare-ddns.sh
```
