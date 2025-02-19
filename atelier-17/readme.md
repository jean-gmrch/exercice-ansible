# Exercice

Écrivez successivement deux playbooks pour mettre en place la synchronisation NTP avec Chrony sur vos quatre Target Hosts sous Debian, Rocky Linux, SUSE Linux et Ubuntu. Il vous faudra identifier le nom du paquet, le service correspondant et le chemin vers le fichier de configuration par défaut pour chacune des distributions.

## chrony-01.yml
Le premier playbook `chrony-01.yml` utilisera les modules de gestion de paquets natifs `apt`, `dnf` et `zypper` et s’inspirera de la méthode « gros sabots » utilisée plus haut dans cet article.

```yaml
---  # chrony-01.yml

- hosts: all

  tasks:
    - name: Update package information on Debian/Ubutnu
      apt:
        update_cache: yes
        cache_valid_time: 3600
      when: ansible_os_family == 'Debian'
    
    - name: Install Chrony on Debian/Ubuntu
      apt:
        name: chrony
      when: ansible_os_family == 'Debian'

    - name: Install Chrony on Rocky
      dnf:
        name: chrony
      when: ansible_distribution == "Rocky"
    
    - name: Install Chrony on SUSE
      zypper:
        name: chrony
      when: ansible_distribution == "openSUSE Leap"
    
    - name: Start service Chrony
      service:
        name: chronyd
        state: started
        enabled: true
    
    - name: Configure Chrony on Debian/Ubuntu
      copy:
        dest: /etc/chrony/chrony.conf
        mode: 0644
        owner: root
        group: root
        content: |
          # chrony.conf
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
      notify: Reload Chrony
      when: ansible_os_family == 'Debian'
    
    - name: Configure Chrony on Rocky/SUSE
      copy:
        dest: /etc/chrony.conf
        mode: 0644
        owner: root
        group: root
        content: |
          # chrony.conf
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
      notify: Reload Chrony
      when: ansible_os_family == 'RedHat' or ansible_os_family == 'openSUSE Leap'

  handlers:
    - name: Reload Chrony
      service:
        name: chronyd
        state: restarted

...
```
### Execution du playbook
```bash
ansible-playbook playbooks/chrony-01.yml
```
<details>
  <summary>Résultat</summary>

    PLAY [all] ***********************************************************************************************************************************************************************************

    TASK [Gathering Facts] ***********************************************************************************************************************************************************************
    ok: [debian]
    ok: [rocky]
    ok: [ubuntu]
    ok: [suse]

    TASK [Update package information on Debian/Ubutnu] *******************************************************************************************************************************************
    skipping: [rocky]
    skipping: [suse]
    ok: [debian]
    ok: [ubuntu]

    TASK [Install Chrony on Debian/Ubuntu] *******************************************************************************************************************************************************
    skipping: [rocky]
    skipping: [suse]
    ok: [debian]
    ok: [ubuntu]

    TASK [Install Chrony on Rocky] ***************************************************************************************************************************************************************
    skipping: [debian]
    skipping: [suse]
    skipping: [ubuntu]
    ok: [rocky]

    TASK [Install Chrony on SUSE] ****************************************************************************************************************************************************************
    skipping: [rocky]
    skipping: [debian]
    skipping: [ubuntu]
    ok: [suse]

    TASK [Start service Chrony] ******************************************************************************************************************************************************************
    ok: [ubuntu]
    ok: [debian]
    ok: [suse]
    ok: [rocky]

    TASK [Configure Chrony on Debian/Ubuntu] *****************************************************************************************************************************************************
    skipping: [rocky]
    skipping: [suse]
    ok: [debian]
    ok: [ubuntu]

    TASK [Configure Chrony on Rocky/SUSE] ********************************************************************************************************************************************************
    skipping: [debian]
    skipping: [suse]
    skipping: [ubuntu]
    ok: [rocky]

    PLAY RECAP ***********************************************************************************************************************************************************************************
    debian                     : ok=5    changed=0    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0   
    rocky                      : ok=4    changed=0    unreachable=0    failed=0    skipped=4    rescued=0    ignored=0   
    suse                       : ok=3    changed=0    unreachable=0    failed=0    skipped=5    rescued=0    ignored=0   
    ubuntu                     : ok=5    changed=0    unreachable=0    failed=0    skipped=3    rescued=0    ignored=0
</details>

```bash
ansible all -m command -a "chronyc sources"
```
```
suse | CHANGED | rc=0 >>
MS Name/IP address         Stratum Poll Reach LastRx Last sample               
===============================================================================
^* 45.132.96.81                  2   6   377    62  -6956us[-8915us] +/-   49ms
^+ ntp.tuxfamily.net             2   6   377    63  -7901us[-9853us] +/-   82ms
^+ x.ns.gin.ntt.net              2   6   377    63   -881us[-2830us] +/-  114ms
^+ 51-15-12-120.rev.poneyte>     2   6   377    62  -9086us[  -11ms] +/-   65ms
rocky | CHANGED | rc=0 >>
MS Name/IP address         Stratum Poll Reach LastRx Last sample               
===============================================================================
^- 82-64-81-218.subs.proxad>     2   6   377    15    -15ms[  -11ms] +/-   86ms
^* free.saclay.org               1   6   377    15  -5360us[-1226us] +/-   27ms
^+ 82-64-42-185.subs.proxad>     2   6   377    15  -1031us[+3102us] +/-   44ms
^+ time.cloudflare.com           3   6   377    15    -12ms[  -12ms] +/-   81ms
ubuntu | CHANGED | rc=0 >>
MS Name/IP address         Stratum Poll Reach LastRx Last sample               
===============================================================================
^- 95.179.212.126.vultruser>     2   6   377    26  -6779us[-6779us] +/-  133ms
^+ silas.dioptre.fr              2   6   377   221    -22ms[  -12ms] +/-   77ms
^* mwc339.ftth.cust.milkywa>     2   6   377    27  -6959us[-7068us] +/-   35ms
^+ kuro-home.net                 1   6   377    28    -15ms[  -15ms] +/-   41ms
debian | CHANGED | rc=0 >>
MS Name/IP address         Stratum Poll Reach LastRx Last sample               
===============================================================================
^* 82-64-42-185.subs.proxad>     2   6   377    26    -15ms[  -15ms] +/-   47ms
^+ router-ix5.servme.network     3   6   377    24  -7377us[-7377us] +/-   75ms
^+ 82-64-81-218.subs.proxad>     2   6   377    26  -8746us[-8746us] +/-  105ms
^+ likho.oneyed.monster          2   6   377    27  -9305us[-9305us] +/-   67ms
```

## chrony-02.yml
Le deuxième playbook `chrony-02.yml` définira trois variables `chrony_package`, `chrony_service` et `chrony_confdir` et utilisera le module de gestion de paquets générique package.

```yaml
---  # chrony-02.yml

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
      copy:
        dest: "{{ chrony[ansible_distribution]['confdir'] }}/chrony.conf"
        mode: 0644
        owner: root
        group: root
        content: |
          # chrony.conf
          server 0.fr.pool.ntp.org iburst
          server 1.fr.pool.ntp.org iburst
          server 2.fr.pool.ntp.org iburst
          server 3.fr.pool.ntp.org iburst
          driftfile /var/lib/chrony/drift
          makestep 1.0 3
          rtcsync
          logdir /var/log/chrony
      notify: Reload Chrony
  
  handlers:
    - name: Reload Chrony
      service:
        name: "{{ chrony[ansible_distribution]['service'] }}"
        state: restarted

...
```
### Execution du playbook
```bash
ansible-playbook playbooks/chrony-02.yml
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
  ok: [debian]
  ok: [ubuntu]

  TASK [Install Chrony] ************************************************************************************************************************************************************************
  ok: [debian]
  ok: [suse]
  ok: [ubuntu]
  ok: [rocky]

  TASK [Start service Chrony] ******************************************************************************************************************************************************************
  ok: [ubuntu]
  ok: [debian]
  ok: [suse]
  ok: [rocky]

  TASK [Configure Chrony] **********************************************************************************************************************************************************************
  ok: [ubuntu]
  ok: [debian]
  ok: [suse]
  ok: [rocky]

  PLAY RECAP ***********************************************************************************************************************************************************************************
  debian                     : ok=5    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   
  rocky                      : ok=4    changed=0    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0   
  suse                       : ok=4    changed=0    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0   
  ubuntu                     : ok=5    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0     
</details>

```bash
ansible all -m command -a "chronyc sources"
```
```
ubuntu | CHANGED | rc=0 >>
MS Name/IP address         Stratum Poll Reach LastRx Last sample               
===============================================================================
^- 95.179.212.126.vultruser>     2   7   377    20    -16ms[  -16ms] +/-  131ms
^+ silas.dioptre.fr              2   7   377    21    -10ms[  -10ms] +/-   65ms
^* mwc339.ftth.cust.milkywa>     2   7   377    87    -35ms[  -34ms] +/-   63ms
^+ kuro-home.net                 1   6   377    21  -1262us[-1262us] +/-   26ms
rocky | CHANGED | rc=0 >>
MS Name/IP address         Stratum Poll Reach LastRx Last sample               
===============================================================================
^- 82-64-81-218.subs.proxad>     2   7   377    71    -17ms[  -18ms] +/-  103ms
^* free.saclay.org               1   7   377     6  -2311us[-3137us] +/-   28ms
^+ 82-64-42-185.subs.proxad>     2   6   377     7  -4154us[-4980us] +/-   50ms
^+ time.cloudflare.com           3   7   377     7    -17ms[  -18ms] +/-   49ms
suse | CHANGED | rc=0 >>
MS Name/IP address         Stratum Poll Reach LastRx Last sample               
===============================================================================
^- ns3166978.ip-51-178-79.eu     3   6    36    72    -17ms[  -19ms] +/-   68ms
^? serveur-sauvegarde.easys>     0   6     0     -     +0ns[   +0ns] +/-    0ns
^- freebox-server.groumpf.o>     2   6    37     7    -16ms[  -16ms] +/-   43ms
^* 82-65-248-56.subs.proxad>     1   6    37     8    +59us[-1579us] +/-   37ms
debian | CHANGED | rc=0 >>
MS Name/IP address         Stratum Poll Reach LastRx Last sample               
===============================================================================
^* 82-64-42-185.subs.proxad>     2   7   377    85  -4699us[-4258us] +/-   43ms
^+ router-ix5.servme.network     3   7   377    88  -5817us[-5376us] +/-   87ms
^+ 82-64-81-218.subs.proxad>     2   7   377    21  -7471us[-7471us] +/-   87ms
^+ likho.oneyed.monster          2   6   377    21  -6950us[-6950us] +/-   59ms
```