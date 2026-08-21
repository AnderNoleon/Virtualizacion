HW-04 — VPN IPSec Site-to-Site (Tunnel Mode) con llamada HTTPS.

SRV-WEB                 R1-MATRIZ            R-INTERNET            R2-SUCURSAL              PC-SUCURSAL
192.168.1.10 --- SW1 --- g0/1 192.168.1.1              
                         g0/0 200.1.1.1 ---- g0/0 200.1.1.2
                                             g0/1 200.2.2.2 ---- g0/0 200.2.2.1
                                                                 g0/1 192.168.2.1 --- SW2 --- 192.168.2.10



Tabla de direccionamiento
Dispositivo	Interfaz	IP	Máscara	Gateway
R1-MATRIZ	G0/0	200.1.1.1	255.255.255.252	—
R1-MATRIZ	G0/1	192.168.1.1	255.255.255.0	—
R-INTERNET	G0/0	200.1.1.2	255.255.255.252	—
R-INTERNET	G0/1	200.2.2.2	255.255.255.252	—
R2-SUCURSAL	G0/0	200.2.2.1	255.255.255.252	—
R2-SUCURSAL	G0/1	192.168.2.1	255.255.255.0	—
SRV-WEB	NIC	192.168.1.10	255.255.255.0	192.168.1.1
PC-SUCURSAL	NIC	192.168.2.10	255.255.255.0	192.168.2.1
Equipos a arrastrar al lienzo
3 routers ISR 2911 (obligatorio: los 2911/1941 son los únicos que traen el paquete securityk9 necesario para crypto).
2 switches 2960.
1 Server-PT y 1 PC-PT.

2. Habilitar la licencia de seguridad  solo R1 y R2)

Sin esto los comandos crypto no existen. R1-MATRIZ y R2-SUCURSAL:


<img width="1136" height="621" alt="image" src="https://github.com/user-attachments/assets/b9909d12-3132-4a0e-9afe-cab7679d127b" />
<img width="746" height="676" alt="image" src="https://github.com/user-attachments/assets/79f6b9fd-33e6-4fa6-a223-4707741e67b3" />
<img width="737" height="687" alt="image" src="https://github.com/user-attachments/assets/4fde947c-064e-4530-9be7-6ef4f2cf8590" />
<img width="493" height="220" alt="image" src="https://github.com/user-attachments/assets/5eb55c11-4d96-4a1c-8d91-b3c65f1fc0c4" />

<img width="724" height="463" alt="image" src="https://github.com/user-attachments/assets/d22368a4-3b69-48b3-b2f5-a4afdee6a825" />

<img width="712" height="512" alt="image" src="https://github.com/user-attachments/assets/7a4bdda8-cb9c-4350-a95f-299e105c0b50" />
<img width="741" height="270" alt="image" src="https://github.com/user-attachments/assets/407a936a-ba2b-4107-b145-298494993af1" />

<img width="1600" height="691" alt="image" src="https://github.com/user-attachments/assets/548e1cba-2e79-4e02-98f1-4b0c00b88614" />
