---
permalink: /recuperacao_desastre.html
layout: default
title: Recuperação de Desastre
---

Documentar como se recuperar de possíveis desastres na infraestrutura ou servicos.

# Problema no 3850

# Restaurar backup de VM

# Restaurar nuvem

# Restaurar


# Recuperação de desastre

A recuperação de desastre dessa estrutura consiste em seguir os passos desse tutorial a partir da instanciação dos _hosts_.

Caso não seja necessário reinstalar o SO, os passos são os seguintes:

0. Remover o cluster kubernetes:

```
./rke remove
```

1. Executar os seguintes comandos em cada nó:

```bash
sudo docker stop $(docker ps -a -q)
sudo docker rm $(docker ps -a -q)
sudo docker volume prune
sudo rm -rf /var/lib/rook
```

## Reestabelecendo o cluster kubernetes e os serviços:

Quando você já possui acesso via ssh (ssh rancher@nodenuvemX) a todos os hosts pode executar o comando para criar/atualizar o cluster (possuindo o arquivo `cluster.yml`):

```bash
./rke up --config cluster.yml
```

Baseado nas configurações oficiais de [Backups and Disaster Recovery](https://rancher.com/docs/rke/v0.1.x/en/etcd-snapshots/) e [Creating Backups—High Availability Installs](https://rancher.com/docs/rancher/v2.x/en/backups/backups/ha-backups/) configuramos o serviço de snapshots recorrentes do etcd no [cluster.yml](cluster.yml). Os snapshots estão sendo sincronizados diariamente no servidor de monitoramento/backup em /backup/etcd_kubernetes e são gravados em fita.

Para fazer o restore do backup utilize os passos descritps em [Restoring Backups—High Availability Installs](https://rancher.com/docs/rancher/v2.x/en/backups/restorations/ha-restoration/).

## Caso não seja possível a opção acima, é possivel recolocar os serviços no ar seguindo os passos abaixo:

0. Clonar o repositório dos serviços: https://github.com/ctic-sje-ifsc/servicos_kubernetes

```bash
git clone https://github.com/ctic-sje-ifsc/servicos_kubernetes
```

1. Criar todos os namespaces:

```bash
cd servicos_kubernetes/namespaces
kubectl create -f namespaces.yaml
```

2. Baixar as chaves do [GDrive](https://drive.google.com/drive/folders/0B_KFdN7OB_xwZ1J0SVk2QWNnU3M?usp=sharing) e criar todas com o comando:

```bash
make -i create
```

3. Acessar a pasta dos serviços e iniciar tudo:

```bash
cd servicos_kubernetes
make -i create
```
