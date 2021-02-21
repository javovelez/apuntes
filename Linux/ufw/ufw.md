# UFW

Es un firewall para Linux.

Los Firewalls son herramientas que monitorean el tráfico de nuestras redes para identificar amenazas e impedir que afecten nuestro sistema.

La la seguridad informática es un proceso constante, así que ninguna herramienta (incluyendo el firewall) puede garantizarnos seguridad absoluta.

En Ubuntu Server podemos usar ufw (Uncomplicated Firewall) para crear algunas reglas, verificar los puertos que tenemos abiertos y realizar una protección básica de nuestro sistema:

- sudo ufw (enable, reset, status): activar, desactivar o ver el estado y reglas de nuestro firewall.
- sudo ufw allow numero-puerto: permitir el acceso por medio de un puerto específico. Recuerda que el puerto 22 es por donde trabajamos con SSH.
- sudo ufw status numbered: ver el número de nuestras reglas.
- sudo ufw delete numero-regla: borrar alguna de nuestras reglas.
- sudo ufw allow from numero-ip proto tcp to any port numero-puerto: restringir el acceso de un servicio por alguno de sus puertos a solo un número limitado de IPs específicas.

Para publicar un puerto 

    sudo ufw allow 2020/tcp
    sudo ufw allow 2020
Para saber los puerto publicados o denegados
    sudo ufw status
Para denegar un puerto
    sudo ufw deny 22