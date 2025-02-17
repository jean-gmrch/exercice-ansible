# Exercice

```
vagrant up
vagrant ssh control
```

## Installation des paquets `tree`, `git` et `nmap`
```bash
ansible all -m package -a "name=tree,git,nmap"
```
<details>
    <summary>Premiere Execution</summary>
    
    rocky | CHANGED => {
        "changed": true,
        "msg": "",
        "rc": 0,
        "results": [
            "Installed: perl-Git-2.43.5-2.el9_5.noarch",
            "Installed: tree-1.8.0-10.el9.x86_64",
            "Installed: git-2.43.5-2.el9_5.x86_64",
            "Installed: nmap-3:7.92-3.el9.x86_64"
        ]
    }
    debian | CHANGED => {
        "cache_update_time": 1704860215,
        "cache_updated": false,
        "changed": true,
        "stderr": "",
        "stderr_lines": [],
        "stdout": ...,
        "stdout_lines": [...]
    }
    suse | CHANGED => {
        "changed": true,
        "cmd": [
            "/usr/bin/zypper",
            "--quiet",
            "--non-interactive",
            "--xmlout",
            "install",
            "--type",
            "package",
            "--auto-agree-with-licenses",
            "--no-recommends",
            "--",
            "+tree",
            "+git",
            "+nmap"
        ],
        "name": [
            "tree",
            "git",
            "nmap"
        ],
        "rc": 0,
        "state": "present",
        "stderr": "",
        "stderr_lines": [],
        "stdout": ...,
        "stdout_lines": [...],
        "update_cache": false
    }
</details>

<details>
    <summary>Deuxieme Execution</summary>

    ```
    suse | SUCCESS => {
        "changed": false,
        "name": [
            "tree",
            "git",
            "nmap"
        ],
        "rc": 0,
        "state": "present",
        "update_cache": false
    }
    debian | SUCCESS => {
        "cache_update_time": 1704860215,
        "cache_updated": false,
        "changed": false
    }
    rocky | SUCCESS => {
        "changed": false,
        "msg": "Nothing to do",
        "rc": 0,
        "results": []
    }
    ```
</details>

## Desinstallation des paquets `tree`, `git` et `nmap`
```bash
ansible all -m package -a "name=tree,git,nmap state=absent"
```
<details>
    <summary>Premiere Execution</summary>

    suse | CHANGED => {
        "changed": true,
        "cmd": [
            "/usr/bin/zypper",
            "--quiet",
            "--non-interactive",
            "--xmlout",
            "remove",
            "--type",
            "package",
            "tree",
            "git",
            "nmap"
        ],
        "name": [
            "tree",
            "git",
            "nmap"
        ],
        "rc": 0,
        "state": "absent",
        "stderr": "",
        "stderr_lines": [],
        "stdout": ...,
        "stdout_lines": [...],
        "update_cache": false
    }
    rocky | CHANGED => {
        "changed": true,
        "msg": "",
        "rc": 0,
        "results": [
            "Removed: git-2.43.5-2.el9_5.x86_64",
            "Removed: nmap-3:7.92-3.el9.x86_64",
            "Removed: tree-1.8.0-10.el9.x86_64",
            "Removed: perl-Git-2.43.5-2.el9_5.noarch"
        ]
    }
    debian | CHANGED => {
        "changed": true,
        "stderr": "",
        "stderr_lines": [],
        "stdout": ...,
        "stdout_lines": [...]
    }
</details>

<details>
    <summary>Deuxieme Execution</summary>

    debian | SUCCESS => {
        "changed": false
    }
    suse | SUCCESS => {
        "changed": false,
        "name": [
            "tree",
            "git",
            "nmap"
        ],
        "rc": 0,
        "state": "absent",
        "update_cache": false
    }
    rocky | SUCCESS => {
        "changed": false,
        "msg": "Nothing to do",
        "rc": 0,
        "results": []
    }
</details>

## Copier le fichier `/etc/fstab` du Control Host vers tous les Target Hosts sous forme d’un fichier `/tmp/test3.txt`

```bash
ansible all -m copy -a "src=/etc/fstab dest=/tmp/test3.txt"
```
<details>
    <summary>Premiere Execution</summary>

    debian | CHANGED => {
        "changed": true,
        "checksum": "d39263691e31170df199aae3d93f7c556fbb3446",
        "dest": "/tmp/test3.txt",
        "gid": 0,
        "group": "root",
        "md5sum": "b6f1fe0d790a8d2f9b74b95df1c889dc",
        "mode": "0644",
        "owner": "root",
        "size": 743,
        "src": "/home/vagrant/.ansible/tmp/ansible-tmp-1739781197.3512714-6112-123715676331797/source",
        "state": "file",
        "uid": 0
    }
    rocky | CHANGED => {
        "changed": true,
        "checksum": "d39263691e31170df199aae3d93f7c556fbb3446",
        "dest": "/tmp/test3.txt",
        "gid": 0,
        "group": "root",
        "md5sum": "b6f1fe0d790a8d2f9b74b95df1c889dc",
        "mode": "0644",
        "owner": "root",
        "secontext": "unconfined_u:object_r:user_home_t:s0",
        "size": 743,
        "src": "/home/vagrant/.ansible/tmp/ansible-tmp-1739781197.2996871-6111-73128297772411/source",
        "state": "file",
        "uid": 0
    }
    suse | CHANGED => {
        "changed": true,
        "checksum": "d39263691e31170df199aae3d93f7c556fbb3446",
        "dest": "/tmp/test3.txt",
        "gid": 0,
        "group": "root",
        "md5sum": "b6f1fe0d790a8d2f9b74b95df1c889dc",
        "mode": "0644",
        "owner": "root",
        "size": 743,
        "src": "/home/vagrant/.ansible/tmp/ansible-tmp-1739781197.192181-6113-213335000550623/source",
        "state": "file",
        "uid": 0
    }
</details>
<details>
    <summary>Deuxieme Execution</summary>

    debian | SUCCESS => {
        "changed": false,
        "checksum": "d39263691e31170df199aae3d93f7c556fbb3446",
        "dest": "/tmp/test3.txt",
        "gid": 0,
        "group": "root",
        "mode": "0644",
        "owner": "root",
        "path": "/tmp/test3.txt",
        "size": 743,
        "state": "file",
        "uid": 0
    }
    suse | SUCCESS => {
        "changed": false,
        "checksum": "d39263691e31170df199aae3d93f7c556fbb3446",
        "dest": "/tmp/test3.txt",
        "gid": 0,
        "group": "root",
        "mode": "0644",
        "owner": "root",
        "path": "/tmp/test3.txt",
        "size": 743,
        "state": "file",
        "uid": 0
    }
    rocky | SUCCESS => {
        "changed": false,
        "checksum": "d39263691e31170df199aae3d93f7c556fbb3446",
        "dest": "/tmp/test3.txt",
        "gid": 0,
        "group": "root",
        "mode": "0644",
        "owner": "root",
        "path": "/tmp/test3.txt",
        "secontext": "unconfined_u:object_r:user_home_t:s0",
        "size": 743,
        "state": "file",
        "uid": 0
    }
</details>

## Supprimez le fichier `/tmp/test3.txt` sur les Target Hosts en utilisant le module file avec le paramètre `state=absent`
```bash
ansible all -m file -a "path=/tmp/test3.txt state=absent"
```
<details>
    <summary>Premiere Execution</summary>

    suse | CHANGED => {
        "changed": true,
        "path": "/tmp/test3.txt",
        "state": "absent"
    }
    debian | CHANGED => {
        "changed": true,
        "path": "/tmp/test3.txt",
        "state": "absent"
    }
    rocky | CHANGED => {
        "changed": true,
        "path": "/tmp/test3.txt",
        "state": "absent"
    }
</details>
<details>
    <summary>Deuxieme Execution</summary>

    debian | SUCCESS => {
        "changed": false,
        "path": "/tmp/test3.txt",
        "state": "absent"
    }
    suse | SUCCESS => {
        "changed": false,
        "path": "/tmp/test3.txt",
        "state": "absent"
    }
    rocky | SUCCESS => {
        "changed": false,
        "path": "/tmp/test3.txt",
        "state": "absent"
    }
</details>

## Enfin, affichez l’espace utilisé par la partition principale sur tous les Target Hosts. Que remarquez-vous ?
```bash
ansible all -m shell -a "df -h /"
```
<details>
    <summary>Premiere Execution</summary>
    
    debian | CHANGED | rc=0 >>
    Filesystem      Size  Used Avail Use% Mounted on
    /dev/sda3       124G  2.3G  115G   2% /
    suse | CHANGED | rc=0 >>
    Filesystem      Size  Used Avail Use% Mounted on
    /dev/sda3       124G  2.8G  118G   3% /
    rocky | CHANGED | rc=0 >>
    Filesystem                  Size  Used Avail Use% Mounted on
    /dev/mapper/rl_rocky9-root   70G  2.4G   68G   4% /
</details>
<details>
    <summary>Deuxieme Execution</summary>
    debian | CHANGED | rc=0 >>
    Filesystem      Size  Used Avail Use% Mounted on
    /dev/sda3       124G  2.3G  115G   2% /
    suse | CHANGED | rc=0 >>
    Filesystem      Size  Used Avail Use% Mounted on
    /dev/sda3       124G  2.8G  118G   3% /
    rocky | CHANGED | rc=0 >>
    Filesystem                  Size  Used Avail Use% Mounted on
    /dev/mapper/rl_rocky9-root   70G  2.4G   68G   4% /
</details>

> Cette commande ne genere pas d'etat different donc on a toujours un `CHANGED`