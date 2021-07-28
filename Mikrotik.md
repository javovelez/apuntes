Cuando reseteo configuración del router debo desactivar todos las interfaces de red de  mi máquina para que me permita conectarme al router por winbox


1- DHCP client en ether1 ip dinámica
2- Creo bridge LAN
3- Creo ports en bridge
4- ip/addresses creo ip para el bridge
5- Creo DHCP server sobre bridge
5- Debo crear máscara para que LAN pueda salir a WAN -> IP/firewall/~NAT/+ 
	~general chain=srcnat out. interface=ether1
	~Action masquerade

!! Desde que tengo ip fija hice lo siguiente.
-Desactivé el cliente dhcp en ether1 y agregué una dirección de ip fija a eth1

Fuentes:
https://www.youtube.com/watch?v=3rOtEVXZ0Lc&ab_channel=TikAcademy
https://www.youtube.com/watch?v=03hwf2XALMA&ab_channel=GlobalysIT.Distributor
https://www.youtube.com/watch?v=v3Z9PFvrIts&ab_channel=KeftwareTel
https://www.youtube.com/watch?v=6SVeabwAoAQ&ab_channel=MacroticsSAS
https://www.youtube.com/watch?v=76nK1LXyPMA&list=PLCvN_Pl1Blxh2ejJCGI4T-xzL3VrYtsKS&ab_channel=TKSJa

portfowarding :https://www.youtube.com/watch?v=XPamzNPwMK8&ab_channel=AbcXperts

ssh admin@192.168.88.1
	/interface print
	
	
Para resetear (dentro de winbox): 
	System/ResetCionfiguration
		Seleccionamos los dos últimos items y reseteamos.
