# Assessment-01: Red Privada Virtual con ZeroTier

## Integrantes
- [ANDERSON OMAR NOE HERNANDEZ DE LEON 1512321]
- [Dennys Rolando Y Carreto Aguilon 2276616 ]

- Enlace de video = https://youtu.be/nVCuVjNl_F4
- 

## Descripción
Implementación de una red privada virtual (SDN) utilizando ZeroTier para interconectar cuatro máquinas virtuales, con una de ellas actuando como router.

## Configuración de la Red ZeroTier

### Network ID

### Tabla de Máquinas Virtuales

| Máquina | Hostname | IP ZeroTier | Rol |
|---------|----------|-------------|-----|
| MV1 | vm-nodo1 | 10.147.20.2 | Nodo |
| MV2 | vm-nodo2 | 10.147.20.3 | Nodo |
| MV3 | vm-nodo3 | 10.147.20.4 | Nodo |
| MV4 | vm-router | 10.147.20.1 | Router |

## Configuración del Router (vm-router)

### 1. Habilitar IP Forwarding

```bash
# Temporal
sudo sysctl -w net.ipv4.ip_forward=1

# Permanente
sudo sed -i 's/#net.ipv4.ip_forward=1/net.ipv4.ip_forward=1/g' /etc/sysctl.conf

2. Configurar iptables

# Interfaz física: enp0s3
# Interfaz ZeroTier: ztf6kxub6

# Regla NAT para enmascarar tráfico
sudo iptables -t nat -A POSTROUTING -o enp0s3 -j MASQUERADE

# Reglas FORWARD para permitir tráfico entre redes
sudo iptables -A FORWARD -i enp0s3 -o ztf6kxub6 -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i ztf6kxub6 -o enp0s3 -j ACCEPT

3. Guardar Configuración

sudo apt update
sudo apt install iptables-persistent -y
sudo netfilter-persistent save

4. Verificar Configuración

# Verificar IP forwarding
sudo sysctl net.ipv4.ip_forward
# Debe mostrar: 1

# Verificar reglas NAT
sudo iptables -t nat -L -v -n

# Verificar reglas FORWARD
sudo iptables -L -v -n

# Verificar interfaz ZeroTier
sudo zerotier-cli listnetworks

Pruebas de Conectividad
Pruebas entre nodos (misma subred)

# Desde vm-nodo1
ping -c 4 10.147.20.1  # A router
ping -c 4 10.147.20.3  # A nodo2
ping -c 4 10.147.20.4  # A nodo3

# Desde vm-nodo2
ping -c 4 10.147.20.1  # A router
ping -c 4 10.147.20.2  # A nodo1
ping -c 4 10.147.20.4  # A nodo3

# Desde vm-nodo3
ping -c 4 10.147.20.1  # A router
ping -c 4 10.147.20.2  # A nodo1
ping -c 4 10.147.20.3  # A nodo2

Prueba de enrutamiento a internet

# Desde vm-nodo1 (tráfico debe pasar por vm-router)
ping -c 4 8.8.8.8

# Desde vm-nodo2
ping -c 4 8.8.8.8

# Desde vm-nodo3
ping -c 4 8.8.8.8


