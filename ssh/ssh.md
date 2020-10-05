Desde la raspberry debo ejecutar  ``ssh -fNR 2020:localhost:22 userDeServer1@server1`` desde server2 para podes ingresar desde server a la raspberry. 
Luego dentro de server ejecuto ``ssh userServer2@localhost -p2020``

sudo ufw allow 2020/tcp
sudo ufw status / deny