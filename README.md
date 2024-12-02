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
Aquí configuraremos el nombre de nuestra zona accediendo al fichero `/etc/bind/named.conf.local`.
Aqui dentro usaremos la siguiente estructura:
```ini
zone "Skletin.org" {
    type master;
    file "db.Skletin";
};

```
Deberemos poner el nombre de nuestro dominio junto a "zone" y el nombre del fichero que configuraremos junto a "file" poniendo "db." antes del nombre.
Guardamos y cerramos.
---
## Creación del archivo de zona
Crearemos el archivo que mencionamos anteriormente en `/etc/bind` con el nombre que hemos mencionado antes,
que en mi caso sería `db.Skletin`, con el contenido correspondiente a nuestra zona.
```ini
; zone file for Skletin.org
@       IN      SOA    ns1.Skletin.org. hostname.Skletin.org. (
                3600 ; refresh
                3600 ; retry
                604800 ; expire
                3600 ) ; minimum

; Name servers
@       IN      NS     ns1.Skletin.org
@       IN      NS     ns2.Skletin.org

; A records
www     IN      A      192.168.1.100
mail    IN      A      192.168.1.101

; MX records
@       IN      MX     10 mail.Skletin.org.
```
---
## Configuración de la zona inversa
En el protocolo DNS se dispone de un fichero con configuración inversa para convertir una dirección IP en un nombre de dominio o host asociado a esa dirección.
```ini
zone "1.168.192.in-addr.arpa" {
    type master;
    file "db.192.168";
};
```
Como puedes ver, hemos mencionado otro fichero "db." con nuesta ip. Deberemos crearla y configurarla tambien.
---
## Creación del archivo de zona inversa
Deberemos crear el fichero "db.192.168" y configurarlo.
```ini
; Zone file for 192.168.1.0/24
@       IN      SOA    ns1.Skletin.org. hostname.Skletin.org. (
                3600 ; refresh
                3600 ; retry
                604800 ; expire
                3600 ) ; minimum

; Reverse DNS
1       IN      PTR    Skletin.org.
2       IN      PTR    mail.Skletin.org.
```
---
## Reinicio y prueba del servicio
Despues de configurar nuestro DNS deberemos reiniciar el servicio:
`$systemctl restart bind9`
Y posteriormente usaremos la orden "nslookup" para comprobar que nuestro dominio esta funcionando.
