HW-03 - Configuración de Redes en Máquinas Virtuales
Datos del Estudiante
Campo	Valor
Nombre	anderson omar hernandez
Hostname VM	omar
Hipervisor	Oracle VirtualBox
Interfaz	enp0s3


Escenario 1: Modo Bridge con DHCP
IP Asignada: 192.168.9.118/24
Hostname omar
<img width="952" height="679" alt="VirtualBox_ubutundes_16_08_2026_18_34_22" src="https://github.com/user-attachments/assets/dfb34282-0889-426a-9eb3-a678c91450cb" />
<img width="952" height="679" alt="VirtualBox_ubutundes_16_08_2026_18_36_14" src="https://github.com/user-attachments/assets/e698a7e9-ab72-4a57-a4e5-fa5380bdc2b7" />

Escenario 2: IP Estática Dentro de la Subred

<img width="882" height="211" alt="image" src="https://github.com/user-attachments/assets/c87d9fbc-03f4-4773-a174-49be54878c97" />

<img width="839" height="381" alt="image" src="https://github.com/user-attachments/assets/7016a61f-7a99-470f-aedc-fd2907960c0d" />

Escenario 3: IP Estática Fuera de la Subred
<img width="1069" height="653" alt="image" src="https://github.com/user-attachments/assets/33f3c3a7-b48a-4835-90d7-7045d09bc9c1" />

Conclusiones por Escenario
Escenario 1: Modo Bridge con DHCP
En este escenario, la máquina virtual obtuvo automáticamente la dirección IP 192.168.9.118/24 a través del servidor DHCP del hipervisor. Esta configuración demostró ser la más sencilla y efectiva, ya que no requirió intervención manual para asignar parámetros de red. 
El ping a Google fue exitoso con un tiempo de respuesta promedio de 15 ms, confirmando que la VM tiene conectividad completa a Internet. Este método es ideal para entornos donde la flexibilidad y la automatización son prioritarias, ya que el DHCP gestiona automáticamente 
la asignación de IP, gateway y DNS, reduciendo significativamente el margen de error humano y 
simplificando la administración de la red.

Escenario 2: Modo Bridge con IP Estática Dentro de la Subred
En este escenario, se asignó manualmente la dirección IP 192.168.9.100/24 dentro de la misma subred del hipervisor, junto con el gateway 192.168.9.1. Esta configuración también fue exitosa, logrando un ping a Google con tiempos promedio de 14.9 ms. A diferencia del DHCP, 
este método requiere conocimiento previo de los parámetros de red (subred, gateway, DNS), pero ofrece la ventaja de tener una IP fija y predecible. Esto es especialmente útil para servidores o dispositivos que necesitan ser accesibles siempre en la misma dirección. La clave 
del éxito fue que tanto la IP como el gateway pertenecían a la misma red lógica, permitiendo el enrutamiento correcto de los paquetes hacia Internet.

Escenario 3: Modo Bridge con IP Estática Fuera de la Subred
En este escenario, se asignó la dirección IP 10.0.0.100/24, que pertenece a una subred completamente diferente a la del hipervisor (192.168.9.0/24). Como resultado, el ping a Google falló con el mensaje Network is unreachable. Esto ocurre porque el gateway 192.168.9.1 no es 
accesible desde una IP en la subred 10.0.0.0/24, ya que los dispositivos solo pueden comunicarse directamente con otros que estén en su misma red lógica. Este escenario demuestra que la IP y el gateway deben estar en la misma subred para que exista conectividad, y que sin un 
enrutamiento adecuado, la VM queda aislada sin posibilidad de acceder a Internet u otras redes externas
