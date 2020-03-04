---
permalink: /container_imagens.html
layout: default
title: Imagens de Contêineres
---

# Imagens de Contêineres

Sempre que possível, são usadas as imagens de contêineres oficiais das aplicações. Entretanto, algumas requerem personalização (pacotes, arquivos etc.) que, na prática, implicam criar imagens derivadas das originais para atender as demandas das aplicações do câmpus São José. Por exemplo, a imagem `mediawiki` adiciona o suporte a LDAP, entre outras ações.

## Repositório

O repositório é [https://github.com/ctic-sje-ifsc/container_imagens](https://github.com/ctic-sje-ifsc/container_imagens). Assim que é feito um `git push`, é disparado um _webhook_ para o Docker Hub, onde são geradas as novas imagens (demandadas na última versão): [https://hub.docker.com/u/cticsjeifsc/](https://hub.docker.com/u/cticsjeifsc/).
