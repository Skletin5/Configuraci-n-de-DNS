# Configuración de DNS
Repositorio que iré actualizando con información sobre la configuración de DNS.
Para esto, iré creando enlaces a otros archivos con una guias de como configurar cada archivo y que función tienen.
---
## Instalación
Primero deberemos instalar el servicio de dns, que se llama bind9.

```
$sudo apt update
$sudo apt install bind9
```
---
## Configuración de la zona directa
Aquí configuraremos el nombre de nuestra zona accediendo al siguiente fichero:
```bash
$nano /etc/bind/named.conf.local
```
Aqui dentro usaremos la siguiente estructura:
```bash
zone "Skletin.org" {
    type master;
    file "db.Skletin";
};

```
