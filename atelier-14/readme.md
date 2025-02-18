# Exercice

## myvars1.yml

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

