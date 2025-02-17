# Exercice

```
vagrant up
vagrant ssh ansible
```

## apache-debian.yml

```yaml
---  # apache-debian.yml

- hosts: debian

  tasks:

    - name: Update package information
      apt:
        update_cache: true
        cache_valid_time: 3600

    - name: Install Apache
      apt:
        name: apache2

    - name: Start & enable Apache
      service:
        name: apache2
        state: started
        enabled: true

    - name: Install custom web page
      copy:
        dest: /var/www/html/index.html
        mode: 0644
        content: |
          <!doctype html>
          <html>
            <head>
              <meta charset="utf-8">
              <title>Test</title>
            </head>
            <body>
              <h1>My first Ansible-managed website</h1>
            </body>
          </html>

...
```

## apache-rocky.yml

```yaml
---  # apache-01.yml

- hosts: rocky

  tasks:

    - name: Install Apache
      dnf:
        name: httpd 

    - name: Start & enable Apache
      service:
        name: httpd
        state: started
        enabled: true

    - name: Install custom web page
      copy:
        dest: /var/www/html/index.html
        mode: 0644
        content: |
          <!doctype html>
          <html>
            <head>
              <meta charset="utf-8">
              <title>Test</title>
            </head>
            <body>
              <h1>My first Ansible-managed website</h1>
            </body>
          </html>

...
```

## apache-suse.yml

```yaml
---  # apache-debian.yml

- hosts: suse 

  tasks:

    - name: Install Apache
      zypper:
        name: apache2

    - name: Start & enable Apache
      service:
        name: apache2
        state: started
        enabled: true

    - name: Install custom web page
      copy:
        dest: /srv/www/htdocs/index.html
        mode: 0644
        content: |
          <!doctype html>
          <html>
            <head>
              <meta charset="utf-8">
              <title>Test</title>
            </head>
            <body>
              <h1>My first Ansible-managed website</h1>
            </body>
          </html>

...
```
