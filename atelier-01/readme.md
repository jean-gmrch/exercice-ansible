# Exercice 1

```
vagrant up ubuntu
vagrant ssh ubuntu
```
```
sudo apt update
apt-cache search --names-only ansible
apt install ansible
```
```
ansible --version
```
> ansible 2.10.8
```
vagrant destroy -f ubuntu
```

# Exercice 2

```
vagrant up ubuntu
vagrant ssh ubuntu
```
```
sudo apt update
sudo apt-add-repository ppa:ansible/ansible
sudo apt install ansible
```
```
ansible --version
```
> ansible 2.17.8

# Exercice 3

```
vagrant up rocky
vagrant ssh rocky
```
```
python -m venv .venv/ansible
source .venv/ansible/bin/activate
```
```
pip install --upgrade pip
pip install ansible
```
```
ansible --version
```
> ansible 2.15.13
