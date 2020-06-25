---
permalink: /boas_praticas.html
layout: default
title: Boas Práticas
---

# Boas Práticas

As boas práticas aqui descritas são excertos das fontes pesquisadas, de forma a serem lidas por completo como uma lista de itens a validar.

## Fontes

### Leitura Obrigatória

- [Configuration Best Practices](https://kubernetes.io/docs/concepts/configuration/overview/)
- [The Twelve-Factor App](https://12factor.net/)
- [Semantic Versioning 2.0.0](https://semver.org/)

### Leituras Opcionais

- [Kubernetes Patterns](https://k8spatterns.io/)

## Diretrizes

### Ambiente de Desenvolvimento

1. Usar 2FA/MFA para autenticação nas plataformas de desenvolvimento, sempre que houver: [GitLab](https://gitlab.com), [GitHub](https://github.com) etc.
1. Recomendação de ferramenta: [Visual Studio Code](https://code.visualstudio.com/) ou equivalente como [Gitpod](https://gitpod.io) e outros.
1. Extensões: [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens), [Indented Block HighLighting](https://marketplace.visualstudio.com/items?itemName=byi8220.indented-block-highlighting), [Kubernetes](https://marketplace.visualstudio.com/items?itemName=ms-kubernetes-tools.vscode-kubernetes-tools), [REST Client](https://marketplace.visualstudio.com/items?itemName=humao.rest-client) e [Settings Sync](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync).

### Controle de versão

1. Arquivos em YAML e validados e organizados por alguma ferramenta como [YAML Lint](http://www.yamllint.com/).
1. Todos os arquivos de configuração sob controle de versão.
1. Uso de ramos para cada estágio do código: desenvolvimento (`desenvolvimento`), teste (`testes`), produção (`master`) e código fora de uso (`abandonado`).
1. Os namespaces devem estar alinhados com os estágios de desenvolvimento, com especial cuidado na sequência: quando pronto o código para o próximo estágio, deve-se alterar primeiramente o namespace para depois mesclar (`git  merge`) no ramo da próxima etapa.

### [Definição das imagens Docker](/container_imagens.html)

1. Preferir sempre a menor imagem possível: Alpine, depois Debian Slim, depois os demais…
1. Verificar se a instalação dos pacotes (`apk`, `apt`) irá gerar uma imagem muito grande, e daí considerar outra distribuição mais adequada/pronta.
1. Criar um _webhook_ no [Docker Hub](https://hub.docker.com) para criar/atualizar a imagem  e salvar a URL no repositório privado de _webhooks_.
1. Quando usado código de terceiro, preferir sempre a política de atualização deste. Quando houver dúvidas na política, fixar as imagens pelo número primário (_major_) e acompanhar regularmente (assinatura de _email_, _changelog_ etc.) a publicação de novas versões. A leitura das modificações é importante por dois motivos: compatibilidade com a(s) versão(ões) anterior(es) e possível mudança nos parâmetros padrão de configuração.
1. Quando houver uma atualização de versão, a política mais simples é a de encerrar os atuais pods e esperar o sistema (`ReplicaSet`) convergir para a quantidade de pods definida.
1. Executar com usuário com o mínimo de privilégios, evitando ao máximo root.

### [Definição dos objetos Kubernetes](/servicos_kubernetes.html)

1. _Labels_ da aplicação: nome (`app`), tipo (`type`, como `backend` ou `database`), versão (`vX.X.X`), entre outros específicos por serviço/aplicação.
1. O _namespace_ deve estar incluído na definição do objeto, já que usamos vários repositórios (GitLab e GitHub).
1. KISS: valores padrão são omitidos.
1. Recursos: tanto recursos mínimos (`resources`) quanto limites (`limits`). Sempre. Se houver dúvida dos valores, pode executar uma primeira vez, observar os números no Grafana para executar novamente com os valores próximos da realidade.
1. Sempre que possível, deve haver monitoramento constante da aplicação com livenessProbe. Para aplicações HTTP, obrigatório monitoramento, direto do protocolo. Para os demais serviços, teste TCP ou, no pior caso, comando a ser executado.

#### Ordem dos objetos K8s

1. Volumes: `Secret`, `ConfigMap`, `PersistentVolume`;
1. Associações de volumes: `PersistentVolumeClaim`;
1. Ingresso: `Ingress`;
1. Serviço: `Service`;
1. Pods: `Deployment`, `StatefulSet`, `DaemonSet`.

### Atualização de ambiente

1. Ler _changelog_, sempre. Os valores padrão podem ser alterados, e portanto deve-se ter cuidado.
