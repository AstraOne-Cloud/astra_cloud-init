
<div class="title-block" style="text-align: center;" align="center">

Astra-One Cloud - Cloud-Init

<p><img title="logo" src="assets/logo.png" width="320" height="320"></p>

</div>
## Introduction

> [!WARNING]  
> La solution suivante est encore en développement et est donc expérimentale. Il est donc possible qu'il y ait des changements de configuration ou des bugs.

Astra-One Cloud est une plateforme de cloud privé sécurisé conçue pour héberger, exploiter et administrer des services internes critiques au sein d’une infrastructure totalement maîtrisée. Contrairement à une architecture distribuée dans un cloud public, Astra-One Cloud repose sur un modèle souverain : les ressources physiques, les mécanismes de virtualisation, le système d’exploitation, la segmentation réseau et la gestion des secrets sont intégralement contrôlés en interne.

La plateforme s’appuie sur une architecture virtualisée construite autour d’un hyperviseur centralisé, permettant la création de machines virtuelles isolées. Chaque composant est intégré dans une logique de cloisonnement et de séparation des responsabilités. Les secrets ne sont jamais stockés en clair et sont gérés par HashiCorp Vault. Le système d’exploitation standardisé est Rocky Linux afin d’assurer stabilité, cohérence et support à long terme.

Astra-One Cloud ne doit pas être compris comme un simple cluster de machines virtuelles. Il s’agit d’une architecture structurée selon des principes de “Security by Design”, où la sécurité est une propriété intrinsèque et non une surcouche ajoutée.  

---
## Architecture

> [!IMPORTANT]  
> Par ailleurs, toutes les VM utilisées sont sous Rocky Linux 9, RHEL 10 n’étant pas encore supporté par OpenNebula.

![preview](assets/preview.png)
Astra-One Cloud repose sur une architecture modulaire :

- **Frontend Node**
    - OpenNebula Core
    - Sunstone GUI
    - Base de données SQL
        
- **Hypervisor Nodes**
    - KVM    
    - Virtualisation des VM
        
- **Storage Layer**
    - Stockage SAN
        
- **Bastion**
    - Solution Vault par HashiCorp
## Cloud-init dans Astra-One Cloud

### Utilité
Dans Astra-One Cloud, **cloud-init** permet d’automatiser l’initialisation des machines utilisées en appliquant dès le premier démarrage une configuration de base sécurisée. Il configure le système (hostname, timezone), crée un utilisateur administrateur protégé par clé SSH et installe les outils essentiels comme Vim et Ansible. Son rôle est de préparer proprement chaque nodes afin qu’il puisse ensuite être configuré de manière complète et reproductible par les playbooks Ansible, garantissant cohérence et sécurité dans toute l’infrastructure.

### Utilisation
Il faut changer les lignes suivantes :
- `hostname: node_name`
- `name: admin`
- `passwd: hashpassword`, il faut rentrer un mot de passe hash (`openssl yourpassword -6`)
- `ssh_authorized_keys`

N'oublier pas d'utilisé une image adapté (une image cloud rocky linux 9 [https://rockylinux.org/fr-FR/download](https://rockylinux.org/fr-FR/download))
