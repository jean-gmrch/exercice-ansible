# Exercice

```
vagrant up
vagrant ssh control
```

- Éditez /etc/hosts de manière à ce que les Target Hosts soient joignables par leur nom d’hôte simple.
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
- Configurez l’authentification par clé SSH avec les trois Target Hosts.
```bash
ssh-keyscan -t rsa target01 target02 target03 >> .ssh/known_hosts
ssh-keygen
ssh-copy-id vagrant@target01
ssh-copy-id vagrant@target02
ssh-copy-id vagrant@target03
```
- Installez Ansible.
```bash
sudo apt update
sudo apt install -y ansible
```
- Envoyez un premier ping Ansible sans configuration.
```bash
ansible all -i target01,target02,target03 -u vagrant -m ping
```
```
target02 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
target03 | SUCCESS => {
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
- Créez un répertoire de projet ~/monprojet.
```bash
mkdir -v ~/monprojet
cd $_
```
- Créez un fichier vide ansible.cfg dans ce répertoire.
```bash
touch ansible.cfg
```
- Vérifiez si ce fichier est bien pris en compte par Ansible.
```bash
ansible --version | head -n 2
```
```
ansible 2.10.8
  config file = /home/vagrant/monprojet/ansible.cfg
```
- Spécifiez un inventaire nommé hosts.
```bash
vim ansible.cfg
```
```
[defaults]
inventory = ./hosts
```
- Activez la journalisation dans ~/journal/ansible.log.
```bash
mkdir -v ~/journal
vim ansible.cfg
```
Ajouter la ligne suivante dans le fichier `ansible.cfg`
```
log_path = ~/journal/ansible.log
```
- Testez la journalisation.
```bash
ansible all -m ping
```
```bash
cat ~/journal/ansible.log
```
```
2025-02-12 10:18:02,295 p=3693 u=vagrant n=ansible | [WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'
```
- Créez un groupe [testlab] avec vos trois Target Hosts.
```bash
vim hosts
```
```
[testlab]
target01
target02
target03
```
- Définissez explicitement l’utilisateur vagrant pour la connexion à vos cibles.
```bash
vim hosts
```
```
[testlab:vars]
ansible_python_interpreter=/usr/bin/python3
ansible_user=vagrant
```
- Envoyez un ping Ansible vers le groupe de machines [all].
```bash
ansible all -m ping
```
```
target01 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
target02 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
target03 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
```
- Définissez l’élévation des droits pour l’utilisateur vagrant sur les Target Hosts.
```bash
vim hosts
```
```
[testlab:vars]
ansible_become=true
```
- Affichez la première ligne du fichier /etc/shadow sur tous les Target Hosts.
```bash
ansible testlab -m shell -a "head -n 1 /etc/shadow"
```
```
target02 | CHANGED | rc=0 >>
root:*:19769:0:99999:7:::
target01 | CHANGED | rc=0 >>
root:*:19769:0:99999:7:::
target03 | CHANGED | rc=0 >>
root:*:19769:0:99999:7:::
```
- Quittez le Control Host et supprimez toutes les VM de l’atelier.
```bash
exit
vagrant destroy -f
```
