# Examen-1av-IvanPortela

1. Explica métodos para 'abrir' unha consola/shell a un contenedor en execución.

**Para poder abrir ou acceder ao contenedor que esta execución nunha consola o no shell deberemos utilizar o seguinte comando:**
```
docker exec -it (nome do contenedor) /bin/bash
```
**O -it e opcional**
**No meu caso para explicalo crearei un novo contenedor chamado "exame" e a ruta bash e para que nos abra o contenidor na consola.Unha vez feito isto estaremos dentro do contenedor**

**Outra maneira dentro do shell de visual code apareceranos a carpeta onde se atopa e contenedor e podemos facer click dereito e pondranos varias opciones e unha e para abrilo e entrar nel.**

2. No contenedor anterior (en execución), qué opciones tes que ter usado ó arrincalo para poder interactuar coas súas entradas e salidas

**Para poder interectuar coas entradas e salidas do contenedor en execuciónd deberemos de utilizar -d para que o contenedor non se detenga cando entramos en segundo plano e -it para que nos libere a consolo ou o shell.**

3. Cómo sería un ficheiro docker-compose para que dous contenedores se comuniquen entre si nunha rede só deles?

**Para que dous contenedores podan comunicarse podemos establecer nun arquivo yml unha relación co network entre os contenedores.**
```
version: '3'
services:
  contedor1:
    image: [exame]
    networks:
      - mi_rede

  contedor2:
    image: [exame2]
    networks:
      - mi_rede

networks:
  mi_rede:
    driver: bridge

```
4. Qué tes que engadir ó ficheiro anterior para que un contenedor teña unha IP fixa?

**Para isto engadiremos unha subnet no apartado de networks a cal funcionara como unha IP fixa e o arquivo tras modificalo quedaría da seguinte maneira:**

```
version: '3'
services:
  contedor1:
    image: [imaxe1]
    networks:
      mi_rede:
        ipv4_address: 172.16.0.2

  contedor2:
    image: [imaxe2]
    networks:
      mi_rede:
        ipv4_address: 172.16.0.3

networks:
  mi_rede:
    driver: bridge
    ipam:
      config:
        - subnet: 172.16.0.0/24

```

5. Qué comando de consola podo usar para sabe-las ips dos contenedores anteriores?

```
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' [nome_do_contedor]
```


6. Cál é a funcionalidade do apartado "ports" en docker compose?

**O apartado ports en docker-compose.yml mapea os portos do contedor aos portos da máquina host, permitindo o acceso á aplicación ou servizo dentro do contedor desde fóra.**

Como puede ser:
	55:53/upd
	esto es para que a maquina local escolle no porte 55 e a virtual no 53

Unha das ventaxas é que docker permite aos contenedores escoller o seu respectivo porto.	

7. Para qué serve o rexistro CNAME? Pon un exemplo

**O rexistro CNAME é un tipo de rexistro DNS que permite crear un alias para outro dominio ou subdominio. Ao utilizar un CNAME, o alias resolve cara ao dominio orixinal.**

8. Cómo podo facer para que a configuración dun contenedor DNS non se borre se creo outro contenedor?

**Para que a configuración dun contedor DNS non se perda ao crear outro contedor, podes usar volumes en Docker. Isto permite que a configuración se almacene de forma persistente no host ou nun volume externo.**

9. Engade unha zoa tendaelectronica.int no teu docker DNS que teña:
- www á IP 172.16.0.1
- owncloud sexa un CNAME de www
- un rexistro de texto có contido "1234ASDF"
Comproba que todo funciona có comando "dig"
Mostra nos logs que o servicio funciona ben usando a saída da terminal ó levantar o compose ou có comando "docker logs [nomeContenedorOuID]"
(o apartado 9 realízase na máquina virtual)
**Ao facer o dig apararece o seguinte:**
```
dig ns.tendaelectronica.int

; <<>> DiG 9.18.27 <<>> ns.tendaelectronica.int
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 13170
;; flags: qr aa rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1232
; COOKIE: cbc58f839edd52900100000067379837112acd43d807a2a7 (good)
;; QUESTION SECTION:
;ns.tendaelectronica.int.	IN	A

;; ANSWER SECTION:
ns.tendaelectronica.int. 38400	IN	A	172.16.0.2

;; Query time: 1 msec
;; SERVER: 127.0.0.11#53(127.0.0.11) (UDP)
;; WHEN: Fri Nov 15 18:51:35 UTC 2024
;; MSG SIZE  rcvd: 96
```

**Ao facer o docker logs aparece o seguinte:**

```
15-Nov-2024 19:56:33.343 BIND 9 is maintained by Internet Systems Consortium,
15-Nov-2024 19:56:33.343 Inc. (ISC), a non-profit 501(c)(3) public-benefit 
15-Nov-2024 19:56:33.343 corporation.  Support and training for BIND 9 are 
15-Nov-2024 19:56:33.343 available at https://www.isc.org/support
15-Nov-2024 19:56:33.343 ----------------------------------------------------
15-Nov-2024 19:56:33.343 found 1 CPU, using 1 worker thread
15-Nov-2024 19:56:33.343 using 1 UDP listener per interface
15-Nov-2024 19:56:33.343DNSSEC algorithms: RSASHA1 NSEC3RSASHA1 RSASHA256 RSASHA512 ECDSAP256SHA256 ECDSAP384SHA384 ED25519 ED448
15-Nov-2024 19:56:33.343 DS algorithms: SHA-1 SHA-256 SHA-384
15-Nov-2024 19:56:33.343 HMAC algorithms: HMAC-MD5 HMAC-SHA1 HMAC-SHA224 HMAC-SHA256 HMAC-SHA384 HMAC-SHA512
15-Nov-2024 19:56:33.343 TKEY mode 2 support (Diffie-Hellman): yes
15-Nov-2024 19:56:33.343 TKEY mode 3 support (GSS-API): yes
15-Nov-2024 19:56:33.343 loading configuration from '/etc/bind/named.conf'
15-Nov-2024 19:56:33.343 unable to open '/etc/bind/bind.keys'; using built-in keys instead
15-Nov-2024 19:56:33.343 looking for GeoIP2 databases in '/usr/share/GeoIP'
15-Nov-2024 19:56:33.343 using default UDP/IPv4 port range: [32768, 60999]
15-Nov-2024 19:56:33.343 using default UDP/IPv6 port range: [32768, 60999]
15-Nov-2024 19:56:33.343 listening on IPv4 interface lo, 127.0.0.1#53
15-Nov-2024 19:56:33.343 listening on IPv4 interface eth0, 172.16.0.2#53
15-Nov-2024 19:56:33.343 IPv6 socket API is incomplete; explicitly binding to each IPv6 address separately
15-Nov-2024 19:56:33.343 listening on IPv6 interface lo, ::1#53
15-Nov-2024 19:56:33.343 generating session key for dynamic DNS
15-Nov-2024 19:56:33.343 sizing zone task pool based on 1 zones
15-Nov-2024 19:56:33.343 none:99: 'max-cache-size 90%' - setting to 7148MB (out of 7942MB)
15-Nov-2024 19:56:33.343 set up managed keys zone for view _default, file 'managed-keys.bind'
15-Nov-2024 19:56:33.343 configuring command channel from '/etc/bind/rndc.key'
15-Nov-2024 19:56:33.343 command channel listening on 127.0.0.1#953
15-Nov-2024 19:56:33.343 configuring command channel from '/etc/bind/rndc.key'
15-Nov-2024 19:56:33.343 command channel listening on ::1#953
15-Nov-2024 19:56:33.343 not using config file logging statement for logging due to -g option
15-Nov-2024 19:56:33.343 managed-keys-zone: loaded serial 0
15-Nov-2024 19:56:33.343 zone tendaelectronica.int/IN: loaded serial 10000002
15-Nov-2024 19:56:33.343 all zones loaded
15-Nov-2024 19:56:33.343 running
15-Nov-2024 19:56:33.343 missing expected cookie from 1.1.1.1#53
```
