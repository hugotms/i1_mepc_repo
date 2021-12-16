# Rendu packer Hugo TOMASI - I1 ASR

## Dépendances

Pour installer les dépendances liées à la virtualisation sur la machine, il est nécessaire d'exécuter les commandes suivantes :

```bash
dnf module install virt
dnf -y install virt-install virt-viewer libguestfs-tools
systemctl enable --now libvirtd
```

Afin de pouvoir exécuter le playbook (et les rôles) Ansible, les commandes suivantes ont été exécutées :

```bash
dnf -y install python3 python3-pip
pip3 install --upgrade pip
pip3 install ansible
```

## Ansible

 - Toutes les variables utilisées par ansible sont situées dans le fichier `group_vars/all.yml`.
 - Le playbook a été redéfinit pour utiliser la notion de rôles ansible (permettant de réutiliser facilement des configurations). Ces derniers sont disponibles dans le répertoire `roles/`du projet. 
 - 3 rôles ont été créés afin de gérer la configuration réseau de la VM, l'installation de services (avec plusieurs méthodes) et leur démarrage si nécessaires, ainsi que la création des users (et de leurs groupes), l'attribution de leurs droits sudo et l'établissement de leurs clés ssh.
 - Certains rôles utilisent un fichier `defaults/main.yml`, celui-ci a pour but de garantir une configuration minimale dans le cas où l'utilisateur ne surchargerait pas cette variable par ses propres configurations (il n'était pas nécessaire dans le cadre de ce TP d'y recourir mais l'exemple semblait être pertinent).
 - Il s'agit d'un très petit éventail d'actions possibles pour sécuriser une machine virtuelle de type RedHat 8 / Centos (Stream) 8 / Rocky Linux 8. La liste des nombreuses actions à mener est disponible ici : https://paper.bobylive.com/Security/CIS/CIS_Red_Hat_Enterprise_Linux_8_Benchmark_v1_0_0.pdf

# packer-course-bootstrap

Avant d'exécuter le script, s'assurer que l'iso de Rocky Linux 8 est copiée dans le répertoire racine du projet. Il est en effet impossible de l'uploader sur GitHub.

## Kickstart 

#### Générer le mot de passe de root

```bash
openssl passwd -6
```
La suite de caractére obtenu est a spécifier de la maniére suivante :
```ks
rootpw --iscrypted <password>
```


## Debug

#### Augmenter le niveau de log de packer

Avant de lancer Packer définissez la variable d'environnement 'PACKER_LOG'.

```bash
export PACKER_LOG=1
```
#### Augmenter le niveau de log d'Ansible
Dans votre fichier PAcker, ajouter la configuration suivante au provisionner d'Ansible.

```hcl
    extra_arguments = [ "-vv" ]
```
