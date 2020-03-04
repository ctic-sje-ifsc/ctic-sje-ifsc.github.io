---
permalink: /cluster_k8s_rke.html
layout: default
title: Cluster Kubernetes via RKE
---

# Cluster Kubernetes via RKE

Os nós (_hosts_) são instanciados da seguinte maneira:

Primeiramente, clona-se o [template](https://pve.proxmox.com/wiki/VM_Templates_and_Clones) `CTIC-VM-Nuvem-Template` presente no nosso Proxmox em uma VM no modo `Full Clone`. Após o clone, configuram-se os parâmetros do [`cloud init`](https://pve.proxmox.com/wiki/Cloud-Init_Support). Exemplo:

![CLoudInitParametros](/assets/img/cloud-init-example.png)

Em seguida, adicionar o _host_ no [inventário](https://gitlab.com/ctic-sje-ifsc/inventory_ansible/blob/master/servidores/host) do Ansible no grupo vms_nuvem para aplicar o [playbook](https://github.com/ctic-sje-ifsc/ansible/blob/master/servidores/vms_nuvem.yml) que configura a VM e instala as dependências.

```bash
ansible-playbook -i inventory_ansible/servidores/host ansible/servidores/vms_nuvem.yml -u root
```

Adicionar a entrada no DNS do câmpus:

```
vmnuvemX IN A 191.36.8.X
```

O próximo passo é adicionar o nó no arquivo [cluster.yml](cluster.yml):

```yml
- address: vmnuvemX.sj.ifsc.edu.br
  role: [worker] # opções: [controlplane,worker,etcd]
  hostname_override: vmnuvemX
  user: root
  ssh_key_path: ~/.ssh/id_rsa
```


## Instalando o kubernetes via [rke](https://github.com/rancher/rke):

Com a configuração do cluster.yml gerada utilizamos o seguinte comando para instalar e configurar o kubernetes:

```bash
./rke up --config cluster.yml
```

No DNS também é necessário adicionar o host nas entradas de api-cloud e ingress-cloud dependendo do papel do host. Caso seja [controlplane] deve colocar no api-cloud e sendo [worker] colocar no ingress-cloud.

```bash
api-cloud IN A 191.36.8.X
;
ingress-cloud IN A 191.36.8.X
```

# Instalação do Rancher

Foi seguido o [tutorial oficial](https://rancher.com/docs/rancher/v2.x/en/installation/ha/helm-rancher/).

Em primeiro lugar, criar a chave:
```bash
kubectl -n cattle-system create secret tls tls-rancher-ingress --cert=tls.crt --key=tls.key
```

Depois, iniciar o serviço do Rancher via `helm`:

```bash
helm install rancher-stable/rancher --name rancher --namespace cattle-system --set hostname=projetos.sj.ifsc.edu.br --set ingress.tls.source=tls-rancher-ingress
```

## Repositório

O repositório é [https://github.com/ctic-sje-ifsc/cluster_k8s_rke](https://github.com/ctic-sje-ifsc/cluster_k8s_rke).
