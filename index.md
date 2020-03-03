---
permalink: /
layout: default
---

O gerenciamento de configuração dos servidores mudou ao longo desses últimos anos. Inicialmente, foi usado o [CoreOS/Tectonic](https://github.com/ctic-sje-ifsc/baremetal_cluster_coreos) com `cloud-config` e instalação manual do Kubernetes nas máquinas físicas (repositório arquivado).

Depois, foi adotado o [Rancher](https://github.com/ctic-sje-ifsc/baremetal_rke_kubernetes) para as mesmas máquinas físicas (repositório arquivado):
- RancherOS como sistema operacional e configurado, assim como o CoreOS, com `cloud-config`.
- RKE como ferramenta de implantação do Kubernetes.
- Rancher como interface (_dashboard_) da nuvem privada.

Com o ambiente em produção, a CTIC adotou o Proxmox como ferramenta de virtualização.

Também estabeleceu-se o [Ansible](/ansible.html) como ferramenta de gerenciamento de configuração das estações de trabalho.

Assim, os vários projetos foram integrados, culminando com a automatização das máquinas virtuais com [Ansible, RKE e Rancher](/cluster_k8s_rke.html).
