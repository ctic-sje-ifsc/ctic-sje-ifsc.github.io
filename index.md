---
permalink: /
layout: default
---

# CTIC - SJE - IFSC

A Coordenadoria de Tecnologia da Informação e Comunicação (CTIC) do Instituto Federal de Santa Catarina (IFSC) câmpus São José (SJE) entende como boa prática publicar todo o código fonte da infraestrutura (Infraestrutura como Código - IaC) - desde que não afete a segurança da informação ou legislação vigente. O principal objetivo é compartilhar a experiência com as outras CTICs do Instituto, além de dar transparência do serviço prestado ao público em geral: alunos, professores, servidores etc.

## Infraestrutura como Código (IaC)

Nos últimos anos, a [infraestrutura](/infraestrutura.html) do câmpus mudou significativamente, migrando do modelo tradicional para IaC. Nos parque de servidores, foi usado inicialmente o [CoreOS/Tectonic](https://github.com/ctic-sje-ifsc/baremetal_cluster_coreos) com `cloud-config` e instalação manual do Kubernetes nas máquinas físicas. Depois, foi adotado o [Rancher](https://github.com/ctic-sje-ifsc/baremetal_rke_kubernetes) para as mesmas máquinas:
- [RancherOS](https://github.com/rancher/os) como sistema operacional e configurado, assim como o CoreOS, com `cloud-config`.
- [RKE](https://github.com/rancher/rke) como ferramenta de implantação do Kubernetes.
- [Rancher](https://github.com/rancher/rancher) como interface (_dashboard_) da nuvem privada.

Com o ambiente em produção como Rancher, a CTIC adotou o Proxmox como ferramenta de virtualização. Também estabeleceu-se o [Ansible](/ansible.html) como ferramenta de gerenciamento de configuração das estações de trabalho. Ao longo do tempo, esses vários projetos foram integrados, culminando com a automatização das máquinas virtuais com [Ansible, RKE e Rancher](/cluster_k8s_rke.html).

## Serviços
Em relação ao serviços, há as [imagens personalizadas](/container_imagens.html) dos [serviços na nuvem privada](/servicos_kubernetes.html). Assim como as máquinas virtuais, os serviços foram projetados para rápida [recuperação de desastre](/recuperacao_desastre.html).
