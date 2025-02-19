# Exercice

# chrony.yml
Écrivez un playbook chrony.yml qui installe un fichier de configuration personnalisé sur vos cibles. La première ligne de commentaire devra indiquer le chemin complet vers le fichier :
* Dans certains cas ce sera /etc/chrony/chrony.conf.
* Dans d’autres cas ce sera simplement /etc/chrony.conf.

```yaml
---  # chrony.yml

- hosts: all

  vars:
    chrony:
      Debian:
        package: chrony
        service: chrony
        confdir: /etc/chrony
      Ubuntu:
        package: chrony
        service: chrony
        confdir: /etc/chrony
      Rocky:
        package: chrony
        service: chronyd
        confdir: /etc
      openSUSE Leap:
        package: chrony
        service: chronyd
        confdir: /etc

  tasks:
    - name: Update package information on Debian/Ubuntu
      apt:
        update_cache: true
        cache_valid_time: 3600
      when: ansible_os_family == "Debian"
    
    - name: Install Chrony
      package:
        name: "{{ chrony[ansible_distribution]['package'] }}"
        state: present

    - name: Start service Chrony
      service:
        name: "{{ chrony[ansible_distribution]['service'] }}"
        state: started
        enabled: true
    
    - name: Configure Chrony
      template:
        dest: "{{ chrony[ansible_distribution]['confdir'] }}/chrony.conf"
        mode: 0644
        owner: root
        group: root
        src: chrony.conf.j2
      notify: Reload Chrony
  
  handlers:
    - name: Reload Chrony
      service:
        name: "{{ chrony[ansible_distribution]['service'] }}"
        state: restarted

...
```
* Create a file named chrony.conf.j2 in templates directory.
```bash
vim templates/chrony.conf.j2
```
```jinja
# {{chrony[ansible_distribution]['confdir']}}/chrony.conf
server 0.fr.pool.ntp.org iburst
server 1.fr.pool.ntp.org iburst
server 2.fr.pool.ntp.org iburst
server 3.fr.pool.ntp.org iburst
driftfile /var/lib/chrony/drift
makestep 1.0 3
rtcsync
logdir /var/log/chrony
```
### Execution du playbook
```bash
ansible-playbook playbooks/chrony.yml
```
<details>
  <summary>Résultat</summary>

    PLAY [all] ***********************************************************************************************************************************************************************************

    TASK [Gathering Facts] ***********************************************************************************************************************************************************************
    ok: [debian]
    ok: [rocky]
    ok: [suse]
    ok: [ubuntu]

    TASK [Update package information on Debian/Ubuntu] *******************************************************************************************************************************************
    skipping: [rocky]
    skipping: [suse]
    changed: [debian]
    ok: [ubuntu]

    TASK [Install Chrony] ************************************************************************************************************************************************************************
    changed: [debian]
    changed: [ubuntu]
    ok: [rocky]        
    changed: [suse]

    TASK [Start service Chrony] ******************************************************************************************************************************************************************
    ok: [debian]
    ok: [rocky]
    ok: [ubuntu]
    changed: [suse]

    TASK [Configure Chrony] **********************************************************************************************************************************************************************
    changed: [ubuntu]
    changed: [debian]
    changed: [suse]
    changed: [rocky]

    RUNNING HANDLER [Reload Chrony] **************************************************************************************************************************************************************
    changed: [debian]
    changed: [ubuntu]
    changed: [rocky]
    changed: [suse]

    PLAY RECAP ***********************************************************************************************************************************************************************************
    debian                     : ok=6    changed=4    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
    rocky                      : ok=5    changed=2    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0   
    suse                       : ok=5    changed=4    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0   
    ubuntu                     : ok=6    changed=3    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

</details>
