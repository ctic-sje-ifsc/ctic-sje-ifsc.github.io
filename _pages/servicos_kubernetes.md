---
permalink: /servicos_kubernetes.html
layout: default
title: Serviços na Nuvem Privada
---

# Serviços

Foco em aplicações Web na modalidade SaaS.

## Serviços Web

### [Netbox](https://netbox.sj.ifsc.edu.br)

O primeiro serviço publicado foi o [netbox](https://netbox.sj.ifsc.edu.br/) para uso interno da TI (documentação da rede física e lógica). O código está em [srv/netbox](https://github.com/ctic-sje-ifsc/kubernetes/tree/master/srv/netbox).

### Matlab

[![](https://images.microbadger.com/badges/commit/cticsjeifsc/matlab.svg)](https://microbadger.com/images/cticsjeifsc/matlab "Get your own commit badge on microbadger.com")


[MATLAB](https://www.mathworks.com/products/matlab.html) trata-se de um software interativo de alto desempenho voltado para o cálculo numérico.
Utilizada a imagem [matlab](https://github.com/ctic-sje-ifsc/imagens/tree/master/matlab) para execução remota da aplicação via SSH:

```sh
ssh -XC seulogin@matlab.sj.ifsc.edu.br -p 2222
```

### Octave

[![](https://images.microbadger.com/badges/commit/cticsjeifsc/octave.svg)](https://microbadger.com/images/cticsjeifsc/octave "Get your own commit badge on microbadger.com")

[GNU Octave](https://www.gnu.org) é uma linguagem de alto nível, principalmente destinada a computação numérica. Ele fornece uma conveniente interface de linha de comando para resolver problemas numericamente lineares e não-lineares. Utilizada a imagem [octave](https://github.com/ctic-sje-ifsc/imagens/tree/master/octave) para execução remota da aplicação via SSH:

```sh
ssh -XC seulogin@octave.sj.ifsc.edu.br -p 2223
```

### Nyqlab

[![](https://images.microbadger.com/badges/commit/cticsjeifsc/nyqlab.svg)](https://microbadger.com/images/cticsjeifsc/nyqlab "Get your own commit badge on microbadger.com")

[NyqLab](https://github.com/rwnobrega/nyqlab) é um aplicativo educacional que visa ajudar os alunos a aprender e praticar conceitos básicos em sistemas de comunicação analógicos e digitais. Utilizada a imagem [nyqlab](https://github.com/ctic-sje-ifsc/imagens/tree/master/nyqlab) para execução remota da aplicação via SSH:

```sh
ssh -XC seulogin@nyqlab.sj.ifsc.edu.br -p 2225
```

## Outros Serviços

### Mosquitto

[![](https://images.microbadger.com/badges/commit/cticsjeifsc/mosquitto.svg)](https://microbadger.com/images/cticsjeifsc/mosquitto "Get your own commit badge on microbadger.com")

Utilizada a implementação [Mosquitto](https://mosquitto.org/) para _Broker_ MQTT. Para o serviço, que por enquanto opera apenas com protocolos MQTT v3.1 e v3.1.1, foi criada uma imagem [mosquitto](https://github.com/ctic-sje-ifsc/imagens/tree/master/mosquitto).

## Repositório

O repositório é [https://github.com/ctic-sje-ifsc/servicos_kubernetes](https://github.com/ctic-sje-ifsc/servicos_kubernetes).
