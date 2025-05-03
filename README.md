# 🚫 Code Freezing para GitLab com Docker

Este projeto implementa um sistema de *code freeze* automatizado usando Python e GitLab CI, com serviços orquestrados por `docker-compose`. O objetivo é impedir deploys ou execuções de pipeline em períodos críticos, como finais de ano ou feriados, exceto para usuários autorizados.

## Stack

A stack é composta por:

- **GitLab CE**: Servidor GitLab Community Edition.
- **Docker-in-Docker (dind)**: Necessário para que o runner execute jobs em containers.
- **GitLab Runner**: Executor de pipelines, com acesso ao Docker e ao script de bloqueio.

##  Como Funciona

1. O pipeline do GitLab executa o script `code-freezing.py` no início.
2. O script carrega as configurações do arquivo `config.yml`.
3. Ele verifica:
   - Se o usuário que iniciou o pipeline está no grupo de exceção (`bypass_group`).
   - Se a data atual está dentro de algum período de congelamento (`freezing_dates`).
4. Se o usuário **não estiver no grupo de exceção** e **a data cair dentro do período de congelamento**, o script **encerra com erro**, impedindo a continuação da pipeline.
