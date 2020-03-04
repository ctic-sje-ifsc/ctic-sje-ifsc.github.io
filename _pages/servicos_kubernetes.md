---
permalink: /servicos_kubernetes.html
layout: default
title: Serviços na Nuvem Privada
---

# Serviços na Nuvem Privada

Por que migrar para contêiner e Kubernetes?

- Fluxo natural da tecnologia.
- Economia de recurso/_hardware_.
- Facilidade de agregar e manter novos serviços.
- Centralização de gerência, monitoramento, administração etc.
- Serviços encapsulados podem ser movidos mais facilmente
  local <=> nuvem privada <=> nuvem pública.
- Controle de versão + moderação + automação de testes = CI/CD.
- Alta disponibilidade, fácil, fácil.
- Auto escalonamento, fácil, fácil.
- Google usa há 15 anos em todos os seus serviços, bilhões de contêineres por
  semana. Se eles podem, nós também pode(re)mos.

## Estrutura do projeto macro

![Projeto Macro](/assets/img/projeto_macro_ctic.jpg)

## Serviços que podemos/queremos oferecer

![Projeto Macro](/assets/img/servicos_possiveis.png)

Nesse repositório estamos colocando cada implementação desenvolvida.
Estamos utilizando a seguinte estratégia de atuação:

- Migrar/instalar serviços já implementados em `docker compose` ou Kubernetes.
- Migrar serviços não críticos para testar a tecnologia.
- Implementações novas importantes e críticas como fase de testes/migração.
- Implementar serviços internos para testes de estabilidade e desempenho da
  tecnologia.

## Armazenamento de estados e dados

Utilizamos uma abordagem para armazenamento de estados e dados onde esses não
são salvos na mesma estrutura onde roda o Kubernetes, e sim em uma estrutura de
armazenamento centralizada. Os
[pods](https://kubernetes.io/docs/concepts/workloads/pods/pod/) montam um
armazenamento NFS e utilizam.
A implementação do storage pode ser encontrada em
[cticsjeifsc/storage](https://github.com/ctic-sje-ifsc/storage).

Podemos ver um exemplo a seguir de um
[volume persistente](https://kubernetes.io/docs/concepts/storage/persistent-volumes/)
(PV):

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: netbox-postgresql-base
spec:
  capacity:
    storage: 1Gi
  accessModes:
    - ReadWriteMany
  nfs:
    server: storage1
    path: /mnt/storage/storage/kubernetes/ifsc/sje/a/saas/srv/netbox/postgresql/base
```

## Serviços Web

### _Front-end_ Nginx

A proposta básica é oferecer os serviços preferencialmente na modalidade
[SaaS](https://pt.wikipedia.org/wiki/Software_como_servi%C3%A7o) com:

1. Disponibilidade: uso de vários servidores físicos e/ou virtuais para manter
   os serviços operando sem sobressaltos.
1. Segurança: uso exclusivo de transmissão criptografada, como
   [HTTPS](https://www.ssllabs.com/ssltest/analyze.html?d=projetos.sj.ifsc.edu.br&latest),
   WebSocket sobre TLS, SSH e outros.
1. Desempenho: [HTTP/2](https://http2.github.io/), balanceamento de carga, cache
   HTTP etc.

A grande maioria dos serviços aqui listados são baseados em HTTP.
Para garantir que todos esses atenderão aos requisitos anteriores
(independente de serem projetados para tal), existe um _front-end_ distribuído
(e escalável), o
[srv/nginx](https://github.com/ctic-sje-ifsc/kubernetes/tree/master/srv/nginx),
de forma a padronizar o acesso.

### [Netbox](https://netbox.sj.ifsc.edu.br)

O primeiro serviço migrado foi o [netbox](https://netbox.sj.ifsc.edu.br/), que já estava rodando em contêiner em uma VM. Pode ser encontrado em [srv/netbox](https://github.com/ctic-sje-ifsc/kubernetes/tree/master/srv/netbox).

### [Wordpress](https://wordpress.sj.ifsc.edu.br)

[WordPress](https://br.wordpress.org) é um aplicativo de sistema de
gerenciamento de conteúdo para web, escrito em PHP com banco de dados MySQL,
voltado principalmente para a criação de sites e blogs via web. Implementação
do Wordpress
[srv/wordpress](https://github.com/ctic-sje-ifsc/kubernetes/tree/master/srv/wordpress).

## Serviços baseados em SSH

### OpenLDAP

Utilizada a imagem
[openldap](https://github.com/ctic-sje-ifsc/imagens/tree/master/openldap)
para base de usuários.

[![](https://images.microbadger.com/badges/image/cticsjeifsc/openldap.svg)](https://microbadger.com/images/cticsjeifsc/openldap "Get your own image badge on microbadger.com")
[![](https://images.microbadger.com/badges/version/cticsjeifsc/openldap.svg)](https://microbadger.com/images/cticsjeifsc/openldap "Get your own version badge on microbadger.com")
[![](https://images.microbadger.com/badges/commit/cticsjeifsc/openldap.svg)](https://microbadger.com/images/cticsjeifsc/openldap "Get your own commit badge on microbadger.com")
[![](https://images.microbadger.com/badges/license/cticsjeifsc/openldap.svg)](https://microbadger.com/images/cticsjeifsc/openldap "Get your own license badge on microbadger.com")

### Matlab

[MATLAB](https://www.mathworks.com/products/matlab.html) trata-se de um software interativo de alta performance voltado para o cálculo numérico.
Utilizada a imagem
[matlab](https://github.com/ctic-sje-ifsc/imagens/tree/master/matlab)
para execução remota da aplicação via SSH.

```sh
ssh -XC seulogin@matlab.sj.ifsc.edu.br -p 2222
```

[![](https://images.microbadger.com/badges/image/cticsjeifsc/matlab.svg)](https://microbadger.com/images/cticsjeifsc/matlab "Get your own image badge on microbadger.com")
[![](https://images.microbadger.com/badges/version/cticsjeifsc/matlab.svg)](https://microbadger.com/images/cticsjeifsc/matlab "Get your own version badge on microbadger.com")
[![](https://images.microbadger.com/badges/commit/cticsjeifsc/matlab.svg)](https://microbadger.com/images/cticsjeifsc/matlab "Get your own commit badge on microbadger.com")
[![](https://images.microbadger.com/badges/license/cticsjeifsc/matlab.svg)](https://microbadger.com/images/cticsjeifsc/matlab "Get your own license badge on microbadger.com")

### Octave

[GNU Octave](https://www.gnu.org) é uma linguagem de alto nível, principalmente destinada a computação numérica. Ele fornece uma conveniente interface de linha de comando para resolver problemas numericamente lineares e não-lineares.
Utilizada a imagem
[octave](https://github.com/ctic-sje-ifsc/imagens/tree/master/octave)
para execução remota da aplicação via SSH.

```sh
ssh -XC seulogin@octave.sj.ifsc.edu.br -p 2223
```

[![](https://images.microbadger.com/badges/image/cticsjeifsc/octave.svg)](https://microbadger.com/images/cticsjeifsc/octave "Get your own image badge on microbadger.com")
[![](https://images.microbadger.com/badges/version/cticsjeifsc/octave.svg)](https://microbadger.com/images/cticsjeifsc/octave "Get your own version badge on microbadger.com")
[![](https://images.microbadger.com/badges/commit/cticsjeifsc/octave.svg)](https://microbadger.com/images/cticsjeifsc/octave "Get your own commit badge on microbadger.com")
[![](https://images.microbadger.com/badges/license/cticsjeifsc/octave.svg)](https://microbadger.com/images/cticsjeifsc/octave "Get your own license badge on microbadger.com")

### Nyqlab

[NyqLab](https://github.com/rwnobrega/nyqlab) é um software educacional que visa ajudar os alunos a aprender e praticar conceitos básicos em sistemas de comunicação analógicos e digitais.
Utilizada a imagem
[nyqlab](https://github.com/ctic-sje-ifsc/imagens/tree/master/nyqlab)
para execução remota da aplicação via SSH.

```sh
ssh -XC seulogin@nyqlab.sj.ifsc.edu.br -p 2225
```

[![](https://images.microbadger.com/badges/image/cticsjeifsc/nyqlab.svg)](https://microbadger.com/images/cticsjeifsc/nyqlab "Get your own image badge on microbadger.com")
[![](https://images.microbadger.com/badges/version/cticsjeifsc/nyqlab.svg)](https://microbadger.com/images/cticsjeifsc/nyqlab "Get your own version badge on microbadger.com")
[![](https://images.microbadger.com/badges/commit/cticsjeifsc/nyqlab.svg)](https://microbadger.com/images/cticsjeifsc/nyqlab "Get your own commit badge on microbadger.com")
[![](https://images.microbadger.com/badges/license/cticsjeifsc/nyqlab.svg)](https://microbadger.com/images/cticsjeifsc/nyqlab "Get your own license badge on microbadger.com")

## Outros Serviços

### Mosquitto

Utilizada a implementação [Mosquitto](https://mosquitto.org/) para MQTT _Broker_.

Para o serviço, que por enquanto opera apenas com protocolos MQTT v3.1 e v3.1.1,
foi criada uma imagem
[mosquitto](https://github.com/ctic-sje-ifsc/imagens/tree/master/mosquitto).

[![](https://images.microbadger.com/badges/image/cticsjeifsc/mosquitto.svg)](https://microbadger.com/images/cticsjeifsc/mosquitto "Get your own image badge on microbadger.com")
[![](https://images.microbadger.com/badges/version/cticsjeifsc/mosquitto.svg)](https://microbadger.com/images/cticsjeifsc/mosquitto "Get your own version badge on microbadger.com")
[![](https://images.microbadger.com/badges/commit/cticsjeifsc/mosquitto.svg)](https://microbadger.com/images/cticsjeifsc/mosquitto "Get your own commit badge on microbadger.com")
[![](https://images.microbadger.com/badges/license/cticsjeifsc/mosquitto.svg)](https://microbadger.com/images/cticsjeifsc/mosquitto "Get your own license badge on microbadger.com")

## Repositório

O repositório é [https://github.com/ctic-sje-ifsc/servicos_kubernetes](https://github.com/ctic-sje-ifsc/servicos_kubernetes).
