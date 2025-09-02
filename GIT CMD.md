
```c
// Ubicación de los repositorios en ambas pcs 
Laptop: C:\Obsidian_Notes\Instituto\Notas
Desktop: E:\Obsidian_Notes\Istituto\Notas

//para clonarlo
"https://github.com/bhf-instituto/Notas.git"
```
**Para guardar los cambios**
```c
git add .
git commit -m "Actualicé las notas"
git push
```
**Para cargar los cambios** 
```c
git pull
```
**Si hay cambios en local y quiero hacerlos atrás**
```c
git reset --hard HEAD // borra todos los cambios locales y vuele al último commit
```
**Para reemplazar directamente el contenido de la carpeta por lo que hay en el main del repositorio *Agresivo* 
```c
git fetch --all
git reset --hard origin/main
```

**Para guardar los cambios locales pero ocultarlos : **
```c
git stash
git pull
```
**Para recuperarlos: **
```c
git stash pop
```


** Puedo hacer un script para correr todo junto : ** 

## 🔹 En Windows

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