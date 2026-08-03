INFORME DE ACTIVIDAD 2: COMUNICACIÓN ENTRE SERVIDORES VIRTUALES

Se configuraron exitosamente dos servidores virtuales en VirtualBox, denominados server1 (Ubuntu Server) y server2 (Ubuntu Desktop), ambos con direcciones IP pertenecientes a la subred de la casa (192.168.9.0/24). Al servidor server1 se le asignó la IP 192.168.9.116/24 y 
al servidor server2 la IP 192.168.9.118/24, configurando el adaptador de red de ambos en modo Puente (Bridged) para garantizar que estuvieran en la misma red local. Los hostnames fueron modificados exitosamente utilizando el comando sudo hostnamectl set-hostname, asignando
server1 al primer servidor y server2 al segundo. Posteriormente, se configuró el archivo /etc/hosts en ambas máquinas agregando las entradas correspondientes para permitir la resolución de nombres entre ellos. Las pruebas de conectividad mediante ping demostraron comunicación
bidireccional exitosa: desde server1 hacia server2 (ping 192.168.9.118) con un tiempo de respuesta promedio de 1.42 ms y 0% de pérdida de paquetes, y desde server2 hacia server1 (ping 192.168.9.116) con resultados igualmente exitosos. Adicionalmente, se verificó la resolución
por nombre ejecutando ping server2 desde server1 y ping server1 desde server2, confirmando el funcionamiento correcto de la configuración. Todos los requisitos mínimos establecidos para la actividad han sido cumplidos al 100%, incluyendo la configuración de IPs en la subred
correspondiente, la asignación de hostnames y la comunicación efectiva entre ambos servidores virtuales.

<img width="784" height="590" alt="VirtualBox_ubutundes_03_08_2026_15_34_58" src="https://github.com/user-attachments/assets/cf77739f-8dea-4ee9-b69b-c7692de31795" />
<img width="1280" height="800" alt="VirtualBox_ubuntuserver_02_08_2026_20_50_28" src="https://github.com/user-attachments/assets/57a1a215-5eae-4e39-8a58-1782847dff07" />

<img width="784" height="590" alt="VirtualBox_ubutundes_03_08_2026_15_34_58" src="https://github.com/user-attachments/assets/372c59f2-b47d-4545-92f7-39a3e2961b12" />
<img width="840" height="638" alt="ubutndestok" src="https://github.com/user-attachments/assets/653c8e57-c652-4176-96a3-425087933fd1" />
<img width="1280" height="800" alt="VirtualBox_ubuntuserver_03_08_2026_15_42_05" src="https://github.com/user-attachments/assets/11cece23-3445-4695-b3cd-06c0be39b938" />
<img width="1280" height="800" alt="VirtualBox_ubuntuserver_03_08_2026_15_57_15" src="https://github.com/user-attachments/assets/6559bb64-c586-4a70-ade1-e0d96f23c926" />
<img width="968" height="694" alt="VirtualBox_ubutundes_03_08_2026_15_59_24" src="https://github.com/user-attachments/assets/75cf0793-55f6-46bf-98a6-911028d918d1" />
