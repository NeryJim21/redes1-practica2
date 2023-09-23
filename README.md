# UNIVERSIDAD DE SAN CARLOS DE GUATEMALA
# REDES DE COMPUTADORAS 1 - PRACTICA 2 

| *Nombre* | *Carnet* |
| - | - |
| Nery Oswaldo Jiménez Contreras | 201700381 |

# TOPOLOGÍA DE LA PRÁCTICA 2
* Para la práctica 2 se necesitan 3 routers, tres switches y seis vpc's con lo que formaremos la siguiente topología:

![](/img/topologia.PNG)

# Distribución `IPV4` para la topología

* Para cada `VPC` tendremos la siguiente distribución de direcciones ip, junto con su máscara y el gateway.

| *VPC* | *DIRECCIÓN DE RED* | *GATEWAY* |
| - | - | - |
| 1 | 192.168.181.0/24 | 192.168.181.1 |
| 2 | 192.168.181.0/24 | 192.168.181.1 |
| 3 | 192.168.182.0/24 | 192.168.182.1 |
| 4 | 192.168.182.0/24 | 192.168.182.1 |
| 5 | 192.168.183.0/24 | 192.168.183.1 |
| 6 | 192.168.183.0/24 | 192.168.183.1 |

# Distribución `IPV4` para los routers
* Así como para los routers, tendremos las siguientes ip's asignadas:

| *Conexión* | *DIRECCIÓN DE RED* | *PRIMERA DIRECCIÓN ASIGNABLE* | *GATEWAY* |
| - | - | - | - |
| R1-R2 | 172.181.0.0/16 | 172.181.0.1 | N.A. |
| R1-R3 | 172.182.0.0/16 | 172.182.0.1 | N.A. |
| R2-R3 | 172.183.0.0/16 | 172.183.0.1 | N.A. |

# Configuración de las `VPC's`

* Asignamos la `ip` correspondiente a cada `vpc` según la distribución realizada:

### PC1

```
ip 192.168.181.10 255.255.255.0 192.168.181.1
save
```

### PC2

```
ip 192.168.181.20 255.255.255.0 192.168.181.1
save
```

### PC3

```
ip 192.168.182.30 255.255.255.0 192.168.182.1
save
```

### PC4

```
ip 192.168.182.40 255.255.255.0 192.168.182.1
save
```

### PC5

```
ip 192.168.183.50 255.255.255.0 192.168.183.1
save
```

### PC6

```
ip 192.168.183.60 255.255.255.0 192.168.183.1
save
```

# Configurando los `Switches`

* Una vez que tenemos las vpc's configuradas, procedemos a configurar cada uno de los switches para permitir la conexión entre las redes:

### Switch1
```
config t
int range f1/0 - 1
switchport mode access
switchport acces vlan 1
no shutdown
exit

int f1/2
switchport mode trunk
switchport trunk allowed  vlan all
no shutdown
exit

do wri
```

### Switch2
```
config t
int range f1/0 - 1
switchport mode access
switchport acces vlan 1
no shutdown
exit

int f1/2
switchport mode trunk
switchport trunk allowed  vlan all
no shutdown
exit

do wri
```

### Switch3
```
config t
int range f1/0 - 1
switchport mode access
switchport acces vlan 1
no shutdown
exit

int f1/2
switchport mode trunk
switchport trunk allowed  vlan all
no shutdown
exit

do wri
```

# Configurando conexiones entre `Router` y `Switch`

* Ya que tenemos los switches configurados, realizamos las conexiones a sus respectivos routers:

### R1 - Switch2
```
config t
int f0/1
ip address 192.168.182.1 255.255.255.0
no shutdown
exit

do wri
```

### R2 - Switch3
```
config t
int f0/1
ip address 192.168.183.1 255.255.255.0
no shutdown
exit

do wri
```

### R3 - Switch1
```
config t
int f0/1
ip address 192.168.181.1 255.255.255.0
no shutdown
exit

do wri
```

# Configurando `Interfaces` de comunicación entre routers

* Es necesario configurar la `ip` y el `puerto ` por donde se estarán comunicando los `router's`, eso lo hacemos con los siguientes comandos:

### R1 - R2
```
conf t
int s1/0
ip address 172.181.0.1 255.255.0.0
no shutdown
exit
```

### R1 - R3
```
conf t
int s1/1
ip address 172.182.0.1 255.255.0.0
no shutdown
exit
```

### R2 - R1
```
conf t
int s1/0
ip address 172.181.0.2 255.255.0.0
no shutdown
exit
```

### R2 - R3
```
conf t
int s1/2
ip address 172.183.0.1 255.255.0.0
no shutdown
exit
```

### R3 - R1
```
conf t
int s1/1
ip address 172.182.0.2 255.255.0.0
no shutdown
exit
```

### R3 - R2
```
conf t
int s1/2
ip address 172.183.0.2 255.255.0.0
no shutdown
exit
```

# Configurando `Enrutamiento Estático`
* Por último, configuramos el enrutamiento estático para cada router. Esto lo hacemos con el comando `ip route` seguido de la `red destino` con su `máscara de subred` y el `puente` por donde se estarán comunicando.

### R1 - R2
```
config t
ip route 192.168.183.0 255.255.255.0 172.181.0.2
exit
```

### R1 - R3
```
config t
ip route 192.168.181.0 255.255.255.0 172.182.0.2
exit
```

### R2 - R1
```
config t
ip route 192.168.182.0 255.255.255.0 172.181.0.1
exit
```

### R2 - R3
```
config t
ip route 192.168.181.0 255.255.255.0 172.183.0.2
exit
```

### R3 - R1
```
config t
ip route 192.168.182.0 255.255.255.0 172.182.0.1
exit
```

### R3 - R2
```
config t
ip route 192.168.183.0 255.255.255.0 172.183.0.1
exit
```

# Comprobando configuración
* Se comprueba que las configuraciones hayan sido correctas con los siguientes comandos:

```
sh ip int br

sh ip ro
```

* Estos son los resultados que nos han devuelto los comandos en los tres routers:

### R1

![](/img/r1.PNG)

### R2

![](/img/r2.PNG)

### R3

![](/img/r3.PNG)

# Pruebas `PING`

* A continuación se muestran los `ping` realizados dentro de la topología

### Desde `VPC1` hacia `VPC4` y `VPC6`

![](/img/pc1_pc2.PNG)
![](/img/pc1_pc4.PNG)
![](/img/pc1_pc6.PNG)


### Desde `VPC3` hacia `VPC1` y `VPC5`

![](/img/pc3_pc1_pc5.PNG)

### Desde `VPC6` hacia `VPC1`

![](/img/pc6_pc1.PNG)
