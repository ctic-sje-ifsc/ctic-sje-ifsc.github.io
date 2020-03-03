---
permalink: /acesso_nuvem_k8s.html
layout: default
---


# acesso_cloud_kubernetes
Nesse repositório são descritas as formas de acessos dos usuários a nuvem CaaS/PaaS (_Containers as a service/Platform as a Service/_) kubernetes (k8s) do IFSC-SJE. Também são descritos seus namespaces e limites.

__Para ter acesso a nuvem CaaS/PaaS k8s (https://tectonic.sj.ifsc.edu.br) entre em contato com a CTIC-SJE pelo email suporte.ti.sje@ifsc.edu.br pedindo a criação de um usuário e relatando brevemente qual será o uso e por quanto tempo necessita do acesso.__

Utilizamos autenticação [RBAC](https://kubernetes.io/docs/admin/authorization/#rbac-mode) do k8s provida pelo [tectonic-identity](https://coreos.com/tectonic/docs/latest/admin/identity-management.html#rbac-in-tectonic) e vinculamos os usuários a determinados namespaces informados a seguir.


## Motivação para usar namespaces([retirado daqui](https://kubernetes.io/docs/tasks/administer-cluster/namespaces/) e traduzido pelo google translate):

Um único cluster deve ser capaz de satisfazer as necessidades de múltiplos usuários ou grupos de usuários (uma "comunidade de usuários").

__Os namespaces k8s ajudam diferentes projetos, equipes ou clientes a compartilhar um cluster k8s.__

__Ele faz isso fornecendo o seguinte:__

* Um escopo para [Nomes](https://kubernetes.io/docs/concepts/overview/working-with-objects/names/).
* Um mecanismo para anexar autorização e política a uma subseção do cluster.

__Cada comunidade de usuários quer ser capaz de trabalhar isoladamente de outras comunidades.__

__Cada comunidade de usuários tem sua própria:__

* Recursos (pods, serviços, controladores de replicação, etc.)
* Políticas (que podem ou não podem realizar ações em sua comunidade)
* Restrições (esta comunidade tem permissão dessa quantidade de cota, etc.)

__O espaço de nomes fornece um escopo exclusivo para:__

* Recursos nomeados (para evitar colisões básicas de nomeação)
* Autoridade de gerenciamento delegada para usuários confiáveis
* Capacidade de limitar o consumo de recursos da comunidade

__Os casos de uso incluem:__

* Como um operador de cluster, eu quero apoiar múltiplas comunidades de usuários em um único cluster.
* Como operador de cluster, eu quero delegar autoridade para partições do cluster para usuários confiáveis nessas comunidades.
* Como operador de cluster, eu quero limitar a quantidade de recursos que cada comunidade pode consumir para limitar o impacto a outras comunidades que usam o cluster.
* Como um usuário de cluster, eu quero interagir com recursos que são pertinentes para minha comunidade de usuários em isolamento do que outras comunidades de usuários estão fazendo no cluster.


## Possuímos os seguintes namespaces:
* producao
* desenvolvimento
* alunos
* professores
* ensino
* pesquisa
* projetos

## Foram configuradas as seguintes [Resource Quotas](https://kubernetes.io/docs/concepts/policy/resource-quotas/) (cotas de recurso) para os seguintes namespace:

```
Name:            resourcequota-alunos
Namespace:       alunos
Resource         Used  Hard
--------         ----  ----
limits.cpu       0     3
limits.memory    0     10Gi
pods             0     20
requests.cpu     0     1
requests.memory  0     500Mi
```

```
Name:            resourcequota-professores
Namespace:       professores
Resource         Used  Hard
--------         ----  ----
limits.cpu       0     3
limits.memory    0     10Gi
pods             0     20
requests.cpu     0     1
requests.memory  0     500Mi
```

```
Name:            resourcequota-ensino
Namespace:       ensino
Resource         Used  Hard
--------         ----  ----
limits.cpu       0     10
limits.memory    0     10Gi
pods             0     15
requests.cpu     0     2
requests.memory  0     2Gi
```
```
Name:            resourcequota-projetos
Namespace:       projetos
Resource         Used  Hard
--------         ----  ----
limits.cpu       0     6
limits.memory    0     10Gi
pods             0     10
requests.cpu     0     2
requests.memory  0     2Gi
```

Para verificar a cota utilizada:

```sh
kubectl describe quota resourcequota-NAMESPACE --namespace=NAMESPACE
```

## Requisições e Limites (Requests and Limits) padrões por Namespace:

Quando um POD for instanciado sem [valores de request e limit](https://kubernetes.io/docs/concepts/configuration/manage-compute-resources-container/) serão atribuidos os seguintes valores:

```yaml
apiVersion: v1
kind: LimitRange
metadata:
  name: mem-limit-range
spec:
  limits:
  - default:
      memory: 256Mi
    defaultRequest:
      memory: 128Mi
    type: Container

---
apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-limit-range
spec:
  limits:
  - default:
      cpu: 300m
    defaultRequest:
      cpu: 200m
    type: Container
```

Para verificar o LimitRange utilize:
```sh
kubectl describe limitrange --namespace=NAMESPACE
```
