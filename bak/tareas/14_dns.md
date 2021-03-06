# DNS

## Servidor Caché

1. Instalar bind9.
2. Investigue los archivos de configuración por defecto y defina brevemente el propósito de cada uno de los archivos.
3. Establezca un servidor sólo de caché, omita el utilizar un `forwarder` y pruebe su funcionamiento, analice el trafico mediante `wireshark` o `tcpdump` (`tcpdump udp port 53`).
4. ¿que sucede al omitir un forwarder, funciona?, ¿cómo?
5. Agregue el DNS público de Google (`8.8.8.8`), o el de su preferencia como forwarder, y reinicie el servicio bind9.
6. Explique el propósito de agregar un `forwarder`, ¿que es lo que realiza el servidor DNS cuando recibe una consulta?

## Servidor Autoritativo

Para utilizar el servidor DNS por defecto en nuestro host, editar el archivo `/etc/resolv.conf`.

- Ingrese en la configuración para un top level domain `.local` y el dominio `adminf` en el archivo `/etc/bind/named.conf.local`.

  ```cs
  zone "adminf.local" IN {
    type master;
    file "/etc/bind/db.adminf.local";
    allow-update { none; };
  };
  ```

- Agregue el archivo de base de datos para la zona. `db.adminf.local`

`$cat db.adminf.local`

```cs
; BIND archivo para el dominio adminf.local
;
$TTL    86400
@       IN      SOA     adminf.local. root.adminf.local. (
                              1         ; Serial
                         604800         ; Refresh
                          86400         ; Retry
                        2419200         ; Expire
                          86400 )       ; Negative Cache TTL
;
@       IN      NS      ns.adminf.local.
ns      IN      A       172.28.128.4
www     IN      A       172.28.128.4
```

- Reinicie el servicio del servidor e investigue mediante los comandos `nslookup` y `tcpdump` el funcionamiento.

`sudo service bind9 restart`
