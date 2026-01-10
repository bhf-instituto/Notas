```bash 
#!/bin/bash

echo "===== USUARIOS DEL SISTEMA ====="
cut -d: -f1 /etc/passwd
echo ""

echo "===== INFO DETALLADA DE CADA USUARIO ====="
while IFS=: read -r username _ uid gid _ home shell; do
    echo "Usuario: $username"
    echo "UID: $uid, GID: $gid"
    echo "Home: $home, Shell: $shell"
    echo "Grupos: $(groups $username | cut -d: -f2)"
    echo "-----------------------------"
done < /etc/passwd
echo ""

echo "===== GRUPOS DEL SISTEMA ====="
getent group
echo ""

echo "===== USUARIOS CON PRIVILEGIOS DE SUDO ====="
getent group sudo | awk -F: '{print $4}'
echo ""

echo "===== USUARIOS CON UID 0 (root) ====="
awk -F: '($3==0){print $1}' /etc/passwd
echo ""

echo "===== PERMISOS DE DIRECTORIOS IMPORTANTES ====="
for dir in /home /etc /var /usr; do
    echo "Permisos de $dir:"
    ls -ld $dir
done
echo ""

echo "===== ARCHIVOS CON PERMISOS ESPECIALES (SUID, SGID, Sticky bit) ====="
find / -perm /6000 -type f -exec ls -l {} \; 2>/dev/null
echo ""

echo "===== USUARIOS ACTUALMENTE CONECTADOS ====="
w
echo ""

echo "===== FIN DEL REPORTE ====="

```