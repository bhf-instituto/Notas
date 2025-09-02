
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
	// despues se hace un pull y ya 
```