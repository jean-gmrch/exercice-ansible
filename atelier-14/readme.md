# Exercice

## myvars1.yml
Écrivez un playbook myvars1.yml qui affiche respectivement votre voiture et votre moto préférée en utilisant le module debug et deux variables mycar et mybike définies en tant que play vars.
```yaml
---  # myvars1.yml

- hosts: localhost

  vars:
    mycar: "Audi"
    mybike: "Harley Davidson"

  tasks:
    - name: "Afficher ma voiture et ma moto préférée"
      debug:
        msg: "Ma voiture préférée est {{ mycar }} et ma moto préférée est {{ mybike }}"
...
```

### Execution du playbook
```bash
ansible-playbook myvars1.yml
```
<details>
    <summary>Résultat</summary>

    PLAY [localhost] *****************************************************************************************************************************************************************************

    TASK [Gathering Facts] ***********************************************************************************************************************************************************************
    ok: [localhost]

    TASK [Afficher ma voiture et ma moto préférée] ***********************************************************************************************************************************************
    ok: [localhost] => {
        "msg": "Ma voiture préférée est Audi et ma moto préférée est Harley Davidson"
    }

    PLAY RECAP ***********************************************************************************************************************************************************************************
    localhost                  : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

</details>

## utilisation des extra vars
En utilisant les extra vars, remplacez successivement l’une et l’autre marque – puis les deux à la fois – avant d’exécuter le play.
```bash
ansible-playbook myvars1.yml -e mycar=Renault -e mybike=Kawasaki
```
<details>
    <summary>Résultat</summary>

    PLAY [localhost] *****************************************************************************************************************************************************************************

    TASK [Gathering Facts] ***********************************************************************************************************************************************************************
    ok: [localhost]

    TASK [Afficher ma voiture et ma moto préférée] ***********************************************************************************************************************************************
    ok: [localhost] => {
        "msg": "Ma voiture préférée est Renault et ma moto préférée est Kawasaki"
    }

    PLAY RECAP ***********************************************************************************************************************************************************************************
    localhost                  : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
</details>

## myvars2.yml
Écrivez un playbook myvars2.yml qui fait essentiellement la même chose que myvars1.yml, mais en utilisant une tâche avec set_fact pour définir les deux variables.
```yaml
---  # myvars2yml

- hosts: localhost

  tasks:
    - name: Define variables---  # myvars2yml

- hosts: localhost

  tasks:
    - name: Define variables
      set_fact:
        mycar: "Audi"
        mybike: "Harley Davidson"

    - name: "Afficher ma voiture et ma moto préférée"
      debug:
        msg: "Ma voiture préférée est {{ mycar }} et ma moto préférée est {{ mybike }}"
...
      set_fact:
        mycar: "Audi"
        mybike: "Harley Davidson"

    - name: "Afficher ma voiture et ma moto préférée"
      debug:
        msg: "Ma voiture préférée est {{ mycar }} et ma moto préférée est {{ mybike }}"
...
```
### Execution du playbook
```bash
ansible-playbook myvars2.yml -e mycar=Renault
```
<details>
    <summary>Résultat</summary>

    PLAY [localhost] *****************************************************************************************************************************************************************************

    TASK [Gathering Facts] ***********************************************************************************************************************************************************************
    ok: [localhost]

    TASK [Define variables] **********************************************************************************************************************************************************************
    ok: [localhost]

    TASK [Afficher ma voiture et ma moto préférée] ***********************************************************************************************************************************************
    ok: [localhost] => {
        "msg": "Ma voiture préférée est Renault et ma moto préférée est Harley Davidson"
    }

    PLAY RECAP ***********************************************************************************************************************************************************************************
    localhost                  : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

</details>

## myvars3.yml
Écrivez un playbook myvars3.yml qui affiche le contenu des deux variables mycar et mybike mais sans les définir. Avant d’exécuter le playbook, définissez VW et BMW comme valeurs par défaut pour mycar et mybike pour tous les hôtes, en utilisant l’endroit approprié.
```yaml
---  # myvars3.yml

- hosts: localhost

  tasks:

    - name: "Afficher ma voiture et ma moto préférée"
      debug:
        msg: "Ma voiture préférée est {{ mycar }} et ma moto préférée est {{ mybike }}"
...
```
```bash
mkdir group_vars
vim group_vars/all.yml
```
```yaml
---  # all.yml

mycar: VW
mybike: BMW

...
```
### Execution du playbook
```bash
ansible-playbook playbooks/myvars3.yml
```
<details>
    <summary>Résultat</summary>

    PLAY [localhost] *****************************************************************************************************************************************************************************

    TASK [Gathering Facts] ***********************************************************************************************************************************************************************
    ok: [localhost]

    TASK [Afficher ma voiture et ma moto préférée] ***********************************************************************************************************************************************
    ok: [localhost] => {
        "msg": "Ma voiture préférée est VW et ma moto préférée est BMW"
    }

    PLAY RECAP ***********************************************************************************************************************************************************************************
    localhost                  : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
</details>

## target02
Effectuez le nécessaire pour remplacer VW et BMW par Mercedes et Honda sur l’hôte target02.
```bash
mkdir host_vars
vim host_vars/target02.yml
```
```yaml
---  # target02.yml

mycar: Mercedes
mybike: Honda

...
```
### Execution du playbook
```bash
ansible-playbook playbooks/myvars3.yml
```
<details>
    <summary>Résultat</summary>

    PLAY [localhost] *****************************************************************************************************************************************************************************

    TASK [Gathering Facts] ***********************************************************************************************************************************************************************
    ok: [localhost]

    TASK [Afficher ma voiture et ma moto préférée] ***********************************************************************************************************************************************
    ok: [localhost] => {
        "msg": "Ma voiture préférée est Mercedes et ma moto préférée est Honda"
    }

    PLAY RECAP ***********************************************************************************************************************************************************************************
    localhost                  : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
</details>

## display_user.yml
Écrivez un playbook display_user.yml qui affiche un utilisateur et son mot de passe correspondant à l’aide des variables user et password. Ces deux variables devront être saisies de manière interactive pendant l’exécution du playbook. Les valeurs par défaut seront microlinux pour user et yatahongaga pour password. Le mot de passe ne devra pas s’afficher pendant la saisie.
```yaml
---  # diplay_user.yml

- hosts: localhost
  
  vars_prompt:
    - name: user
      prompt: Please enter an user
      default: microlinux
      private: false
        
    - name: password
      prompt: Please enter a password
      default: yatahongaga
      private: true
        
  tasks:
    - debug:
        msg: "User: {{ user }}, Password: {{ password }}"
        
...
```
### Execution du playbook
```bash
ansible-playbook playbooks/display_user.yml
```
<details>
    <summary>Résultat</summary>

    Please enter an user [microlinux]: user
    Please enter a password [yatahongaga]: 

    PLAY [localhost] *****************************************************************************************************************************************************************************

    TASK [Gathering Facts] ***********************************************************************************************************************************************************************
    ok: [localhost]

    TASK [debug] *********************************************************************************************************************************************************************************
    ok: [localhost] => {
        "msg": "User: user, Password:  azer"
    }

    PLAY RECAP ***********************************************************************************************************************************************************************************
    localhost                  : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

</details>

