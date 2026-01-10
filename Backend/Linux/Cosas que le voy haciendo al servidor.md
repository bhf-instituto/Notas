* instale ubuntu server 24 claramente
* deshabilito `systemd-networkd-wait-online.service`  porque me traba el SO al iniciar
* deshabilito `motd-news.service`  porque me traba el SO al iniciar
* ```bash
  // deshabilitar
	  sudo systemctl disable systemd-networkd-wait-online.service
	  sudo systemctl disable motd-news.service
	  sudo systemctl stop motd-news.timer
	  
  // parar
	  sudo systemctl stop systemd-networkd-wait-online.service
	  sudo systemctl stop motd-news.service
	  
  //evitar que cualquier otro servicio lo abra (enmascarar)
	  sudo systemctl mask systemd-networkd-wait-online.service
	  sudo systemctl mask motd-news.service
  ```
  * instale tmux
  * desinstale tmux xd