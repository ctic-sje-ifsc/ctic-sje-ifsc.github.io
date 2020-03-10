---
permalink: /recuperacao_desastre.html
layout: default
title: Recuperação de Desastre
---

# Recuperação de desastre

A recuperação de desastre dessa estrutura consiste em seguir os passos desse tutorial a partir da criação das máquinas virtuais.

Caso não seja necessário reinstalar o SO, os passos são os seguintes:

0. Remover o cluster kubernetes:

```bash
./rke remove
```

1. Executar os seguintes comandos em cada nó:

```bash
sudo docker stop $(docker ps -a -q)
sudo docker rm $(docker ps -a -q)
sudo docker volume prune
sudo rm -rf /var/lib/rook
```

## Reestabelecer o _cluster_ Kubernetes e os serviços

Quando já possua acesso via ssh (`ssh rancher@nodenuvemX`) a todos os nós, pode-se executar o comando para criar/atualizar o _cluster_ (de posse do arquivo `cluster.yml`):

```bash
./rke up --config cluster.yml
```

Baseado nas configurações oficiais de [Backups and Disaster Recovery](https://rancher.com/docs/rke/v0.1.x/en/etcd-snapshots/) e [Creating Backups—High Availability Installs](https://rancher.com/docs/rancher/v2.x/en/backups/backups/ha-backups/) foi configurado o serviço de instantâneos (_snapshots_) recorrentes do `etcd` no [cluster.yml](cluster.yml). Os instantâneos são sincronizados diariamente no servidor de monitoramento/_backup_ em `/backup/etcd_kubernetes` e gravados em fita.

Para fazer a recuperação do _backup_ deve-se utilizar os passos descritps em [Restoring Backups—High Availability Installs](https://rancher.com/docs/rancher/v2.x/en/backups/restorations/ha-restoration/).

## Caso não seja possível a opção acima

0. Clonar o repositório dos serviços: https://github.com/ctic-sje-ifsc/servicos_kubernetes

```bash
git clone https://github.com/ctic-sje-ifsc/servicos_kubernetes
```

1. Criar todos os _namespaces_:

```bash
cd servicos_kubernetes/namespaces
kubectl create -f namespaces.yaml
```

2. Baixar os arquivos de _secrets_ do [GitLab](https://gitlab.com/ctic-sje-ifsc/secrets_kubernetes) (acesso restrito) e criá-los com o comando:

```bash
make -i create
```

3. Acessar a pasta dos serviços e iniciar tudo:

```bash
cd servicos_kubernetes
make -i create
```
