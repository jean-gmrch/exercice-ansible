# Exercice

Connection à `control`
```bash
vagrant up
vagrant ssh control
```

Ajout des dns pour les machines `target01`, `target02` et `target03`
```bash
sudo vim /etc/hosts
```
```
127.0.0.1       localhost.localdomain   localhost
192.168.56.10   control.sandbox.lan     control
192.168.56.20   target01.sandbox.lan    target01
192.168.56.30   target02.sandbox.lan    target02
192.168.56.40   target03.sandbox.lan    target03
```
Test de ping
```bash
for HOST in target01 target02 target03; do ping -c 1 -q $HOST; done
```
```
PING target01.sandbox.lan (192.168.56.20) 56(84) bytes of data.

--- target01.sandbox.lan ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 3.065/3.065/3.065/0.000 ms
PING target02.sandbox.lan (192.168.56.30) 56(84) bytes of data.

--- target02.sandbox.lan ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 5.497/5.497/5.497/0.000 ms
PING target03.sandbox.lan (192.168.56.40) 56(84) bytes of data.

--- target03.sandbox.lan ping statistics ---
1 packets transmitted, 1 received, 0% packet loss, time 0ms
rtt min/avg/max/mdev = 3.396/3.396/3.396/0.000 ms

```
Collecte des clés ssh des targets
```bash
ssh-keyscan -t rsa target01 target02 target03 >> .ssh/known_hosts
```
```
# target01:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.10
# target02:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.10
# target03:22 SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.10
```
Génération de la clé ssh
```bash
ssh-keygen
```
```
Generating public/private rsa key pair.
Enter file in which to save the key (/home/vagrant/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/vagrant/.ssh/id_rsa
Your public key has been saved in /home/vagrant/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:JBmdLNhkBszbhcn2EZXnoodT0PclufQMFP4HwpXGI3Y vagrant@control
The key's randomart image is:
+---[RSA 3072]----+
|   o.*==.=.. o++ |
|    +oBoB o.=oE .|
|     +o+.o =o*oO |
|    . .o. o ..ooo|
|        S+ .    o|
|        + .     .|
|         o       |
|                 |
|                 |
+----[SHA256]-----+
```
Copie de la clé ssh sur les targets
```bash
ssh-copy-id vagrant@target01
ssh-copy-id vagrant@target02
ssh-copy-id vagrant@target03
```
Test de connexion ssh avec ansible
```bash
ansible all -i target01,target02,target03 -u vagrant -m ping
```
```
target03 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target01 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```