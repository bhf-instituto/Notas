### Ubicación de los repositorios en mis pcs
```c
// Ubicación de los repositorios en ambas pcs 
Laptop: C:\Obsidian_Notes\Instituto\Notas
Desktop: E:\Obsidian_Notes\Istituto\Notas

//para clonarlo
"https://github.com/bhf-instituto/Notas.git"
```
### **Para guardar los cambios**
```c
git add .
git commit -m "Actualicé las notas"
git push
```
### **Para cargar los cambios** 
```c
git pull
```
### **Si hay cambios en local y quiero hacerlos atrás**
```c
git reset --hard HEAD // borra todos los cambios locales y vuele al último commit
```
### **Para reemplazar directamente el contenido de la carpeta por lo que hay en el main del repositorio *Agresivo* 
```c
git fetch --all
git reset --hard origin/main
```

### **Para guardar los cambios locales pero ocultarlos : **
```c
git stash
git pull
```
**Para recuperarlos: **
```c
git stash pop
```
#### 🔹 En Windows

1. Abrí el **Bloc de notas**.
2. Pegá esto:

`@echo off
cd "C:\ruta\a\tu\vault"
git fetch --all
git reset --hard origin/main
git pull
echoVault actualizado con éxito 🚀 pause`
 
 3. Guardalo como `sync-vault.bat` (importante que sea `.bat`).
 4. Ahora, cada vez que lo quieras ejecutar → doble clic y listo.
### Qué hace `git pull --rebase`

1. Git descarga los cambios que existen en remoto (`origin/main`) que tú **no tienes todavía**.
2. Temporariamente “aparta” tus commits locales que todavía no se han subido.
3. Aplica los commits remotos.
4. Luego **aplica tus commits locales encima** de esos cambios remotos.

En otras palabras, tus últimos cambios se reescriben **sobre lo que ya está en remoto**, preservando tu trabajo.

** Puedo hacer un script para correr todo junto : ** 

