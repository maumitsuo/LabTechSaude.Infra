# Instruções para o Copilot

## Build e validação

Este repositório não tem suíte de testes automatizados nem configuração de lint. Valide a configuração do Compose e compile a imagem de cada stack a partir do diretório correspondente:

```powershell
Set-Location Development\PgVector
docker compose config --quiet
docker compose build db-pgvector
```

```powershell
Set-Location Development\Redis
docker compose config --quiet
docker compose build redis
```

Para iniciar uma stack localmente, execute `docker compose up -d --build` no diretório dela. O Compose carrega o arquivo `.env` desse diretório.

## Arquitetura

Este repositório contém infraestrutura para o projeto de estudos práticos LabTechSaude, não código da aplicação. O README descreve o foco como investigação de saúde, diário alimentar e acompanhamento de alergias.

Cada serviço de desenvolvimento tem seu próprio Dockerfile e arquivo Compose no mesmo diretório, e é compilado a partir dele:

- `Development\PgVector` cria uma imagem baseada em PostgreSQL 18 com pgvector e instala pacotes de localização. A stack usa um volume nomeado para os dados, configura o fuso de São Paulo e a localidade pt_BR, e recebe a porta do host pela configuração.
- `Development\Redis` cria uma imagem baseada em Redis 8.10.2, habilita persistência append-only e armazena os dados em um volume nomeado. A porta publicada é vinculada a `127.0.0.1`.

## Convenções do repositório

- Mantenha a definição da imagem e a configuração Compose de cada serviço juntas; o contexto de build do Compose é o diretório do serviço.
- Use os arquivos `.env` ignorados para configuração e credenciais dos serviços, nunca arquivos rastreados. As definições Compose usam `PGVECTOR_PASSWORD` e `PGVECTOR_PORT`; o Redis inicia com `REDIS_PASSWORD`.
- Preserve as configurações atuais de localidade e fuso horário ao alterar os serviços. Mantenha os dados nos volumes nomeados (`pgdata` e `redisdata`), em vez de depender do armazenamento interno do contêiner.
- Preserve o vínculo da porta do Redis somente ao loopback, a menos que a mudança de acesso seja intencional.
