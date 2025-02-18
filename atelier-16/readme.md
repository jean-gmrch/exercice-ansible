# Exercice
## pkg-info.yml
`pkg-info.yml` pour afficher le gestionnaire de paquets utilisé
```yaml
---  # pkg-info.yml

- hosts: all

  tasks:
    - name: "Afficher le gestionnaire de paquets"
      debug:
        var: ansible_pkg_mgr

...
```
### Execution du playbook
```bash
ansible-playbook playbooks/pkg-info.yml
```
<details>
    <summary>Résultat</summary>
    
    PLAY [all] ***********************************************************************************************************************************************************************************

    TASK [Gathering Facts] ***********************************************************************************************************************************************************************
    ok: [debian]
    ok: [suse]
    ok: [rocky]

    TASK [Afficher le gestionnaire de paquets] ***************************************************************************************************************************************************
    ok: [rocky] => {
        "ansible_pkg_mgr": "dnf"
    }
    ok: [debian] => {
        "ansible_pkg_mgr": "apt"
    }
    ok: [suse] => {
        "ansible_pkg_mgr": "zypper"
    }

    PLAY RECAP ***********************************************************************************************************************************************************************************
    debian                     : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
    rocky                      : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
    suse                       : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

</details>

## python-info.yml
`python-info.yml` pour afficher la version de Python installée
```yaml
---  # python-info.yml

- hosts: all

  tasks:
    - name: "Afficher la version de Python installée"
      debug:
        var: ansible_python_version

...
```
### Execution du playbook
```bash
ansible-playbook playbooks/python-info.yml
```
<details>
    <summary>Résultat</summary>
    
    PLAY [all] ***********************************************************************************************************************************************************************************

    TASK [Gathering Facts] ***********************************************************************************************************************************************************************
    ok: [debian]
    ok: [suse]
    ok: [rocky]

    TASK [Afficher la version de Python installée] ***********************************************************************************************************************************************
    ok: [rocky] => {
        "ansible_python_version": "3.9.18"
    }
    ok: [debian] => {
        "ansible_python_version": "3.11.2"
    }
    ok: [suse] => {
        "ansible_python_version": "3.6.15"
    }

    PLAY RECAP ***********************************************************************************************************************************************************************************
    debian                     : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
    rocky                      : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
    suse                       : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

</details>

## dns-info.yml
`dns-info.yml` pour afficher le(s) serveur(s) DNS utilisé(s)
```yaml
---  # dns-info.yml

- hosts: all

  tasks:
    - name: "Afficher le(s) serveur(s) DNS utilisé(s)"
      debug:
        var: ansible_dns.nameservers

...
```
### Execution du playbook
```bash
ansible-playbook playbooks/dns-info.yml
```
<details>
    <summary>Résultat</summary>

    PLAY [all] ***********************************************************************************************************************************************************************************

    TASK [Gathering Facts] ***********************************************************************************************************************************************************************
    ok: [debian]
    ok: [suse]
    ok: [rocky]

    TASK [Afficher le(s) serveur(s) DNS utilisé(s)] **********************************************************************************************************************************************
    ok: [rocky] => {
        "ansible_dns.nameservers": [
            "10.0.2.3"
        ]
    }
    ok: [debian] => {
        "ansible_dns.nameservers": [
            "4.2.2.1",
            "4.2.2.2",
            "208.67.220.220"
        ]
    }
    ok: [suse] => {
        "ansible_dns.nameservers": [
            "10.0.2.3"
        ]
    }

    PLAY RECAP ***********************************************************************************************************************************************************************************
    debian                     : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
    rocky                      : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
    suse                       : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
    
</details>