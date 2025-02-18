# Exercice

## kernel.yml
Écrivez un playbook kernel.yml qui affiche les infos détaillées du noyau sur tous vos Target Hosts. Utilisez la commande uname -a et le module debug avec le paramètre msg.
```yaml
---  # kernel.yml

- hosts: all
  gather_facts: false

  tasks:
    - name: "Afficher les infos du noyau"
      command: uname -a
      change_when: false
      register: kernel_info

    - name: "Afficher les infos du noyau"
      debug:
        msg: "{{ kernel_info.stdout }}"

...
```
### Execution du playbook
```bash
ansible-playbook playbooks/kernel.yml
```
<details>
    <summary>Résultat</summary>

    PLAY [all] ***********************************************************************************************************************************************************************************

    TASK [Gathering Facts] ***********************************************************************************************************************************************************************
    ok: [rocky]
    ok: [debian]
    ok: [suse]

    TASK [Afficher les infos du noyau] ***********************************************************************************************************************************************************
    ok: [debian]
    ok: [suse]
    ok: [rocky]

    TASK [Afficher les infos du noyau] ***********************************************************************************************************************************************************
    ok: [rocky] => {
        "msg": "Linux rocky 5.14.0-362.13.1.el9_3.x86_64 #1 SMP PREEMPT_DYNAMIC Wed Dec 13 14:07:45 UTC 2023 x86_64 x86_64 x86_64 GNU/Linux"
    }
    ok: [debian] => {
        "msg": "Linux debian 6.1.0-17-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.69-1 (2023-12-30) x86_64 GNU/Linux"
    }
    ok: [suse] => {
        "msg": "Linux suse 5.14.21-150500.55.39-default #1 SMP PREEMPT_DYNAMIC Tue Dec 5 10:06:35 UTC 2023 (2e4092e) x86_64 x86_64 x86_64 GNU/Linux"
    }

    PLAY RECAP ***********************************************************************************************************************************************************************************
    debian                     : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
    rocky                      : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
    suse                       : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

</details>

## kernel2.yml
Essayez d’obtenir le même résultat en utilisant le paramètre var du module debug.
```yaml
---  # kernel2.yml

- hosts: all
  gather_facts: false

  tasks:
    - name: "Afficher les infos du noyau"
      command: uname -a
      change_when: false
      register: kernel_info

    - name: "Afficher les infos du noyau"
      debug:
        var: kernel_info.stdout

...
```
### Execution du playbook
```bash
ansible-playbook playbooks/kernel2.yml
```
<details>
    <summary>Résultat</summary>
    
    PLAY [all] ***********************************************************************************************************************************************************************************

    TASK [Gathering Facts] ***********************************************************************************************************************************************************************
    ok: [suse]
    ok: [debian]
    ok: [rocky]

    TASK [Afficher les infos du noyau] ***********************************************************************************************************************************************************
    ok: [debian]
    ok: [suse]
    ok: [rocky]

    TASK [Afficher les infos du noyau] ***********************************************************************************************************************************************************
    ok: [rocky] => {
        "kernel_info.stdout": "Linux rocky 5.14.0-362.13.1.el9_3.x86_64 #1 SMP PREEMPT_DYNAMIC Wed Dec 13 14:07:45 UTC 2023 x86_64 x86_64 x86_64 GNU/Linux"
    }
    ok: [debian] => {
        "kernel_info.stdout": "Linux debian 6.1.0-17-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.69-1 (2023-12-30) x86_64 GNU/Linux"
    }
    ok: [suse] => {
        "kernel_info.stdout": "Linux suse 5.14.21-150500.55.39-default #1 SMP PREEMPT_DYNAMIC Tue Dec 5 10:06:35 UTC 2023 (2e4092e) x86_64 x86_64 x86_64 GNU/Linux"
    }

    PLAY RECAP ***********************************************************************************************************************************************************************************
    debian                     : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
    rocky                      : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
    suse                       : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
</details>

## packages.yml
Écrivez un playbook packages.yml qui affiche le nombre total de paquets RPM installés sur les hôtes rocky et suse (rpm -qa | wc -l).
```yaml
---  # packages.yml

- hosts: rocky, suse
  gather_facts: false

  tasks:
    - name: "Afficher le nombre de paquets RPM installés"
      shell: rpm -qa | wc -l
      change_when: false
      register: rpm_count

    - name: "Afficher le nombre de paquets RPM installés"
      debug:
        msg: "Nombre de paquets RPM installés: {{ rpm_count.stdout }}"

...
```
### Execution du playbook
```bash
ansible-playbook playbooks/packages.yml
```
<details>
    <summary>Résultat</summary>
    
    PLAY [rocky,suse] ****************************************************************************************************************************************************************************

    TASK [Gathering Facts] ***********************************************************************************************************************************************************************
    ok: [suse]
    ok: [rocky]

    TASK [Afficher le nombre de paquets RPM installés] *******************************************************************************************************************************************
    ok: [rocky]
    ok: [suse]

    TASK [Afficher le nombre de paquets RPM installés] *******************************************************************************************************************************************
    ok: [rocky] => {
        "msg": "Nombre de paquets RPM installés: 671"
    }
    ok: [suse] => {
        "msg": "Nombre de paquets RPM installés: 917"
    }

    PLAY RECAP ***********************************************************************************************************************************************************************************
    rocky                      : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
    suse                       : ok=4    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
</details>