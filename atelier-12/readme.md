# Exercice
```bash
---

- hosts: redhat

  tasks:
    - name: Install Chrony
      dnf:
        name: chrony

    - name: Start service chrony
      service:
        name: chronyd
        state: started
        enabled: true

    - name: Configure Chrony
      copy:
        dest: /etc/chrony.conf
        mode: 0644
        owner: root
        group: root
        content: |
          # /etc/chrony.conf
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
        name: chronyd
        state: reloaded

...
```
