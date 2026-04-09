# Arquitetura do sistema

## Perfis e experiencias

### 1. Organizador

- Cria campeonatos, categorias, formatos e regras de classificacao
- Gerencia times, inscricoes, patrocinadores, arbitragem, financeiro e conteudo
- Acompanha dashboards, calendario semanal, relatorios e alertas operacionais

### 2. Jogador ou capitao

- Visualiza elenco, jogos, confirmacao de presenca e estatisticas pessoais
- Recebe notificacoes de partidas, punicoes, reagendamentos e resultados
- Acessa historico global da carreira dentro da plataforma

### 3. Torcedor

- Navega em campeonatos publicos sem conta
- Acompanha jogos, tabelas, placares ao vivo, noticias, fotos e comentarios
- Compartilha cards e embeds em redes sociais e sites parceiros

## Arquitetura em camadas

```mermaid
graph TD
    subgraph Clients
        WebOrg["Next.js Organizer Portal"]
        WebFan["Next.js Public Experience"]
        Mobile["React Native App"]
        Embed["Embeddable Widget / iframe"]
    end

    subgraph Edge
        CDN["CDN + Image Optimization"]
        PWA["Service Worker / Offline Cache"]
        API["NestJS API Gateway"]
        WS["Socket.io Gateway"]
    end

    subgraph Domain
        Auth["Auth + Identity"]
        Champ["Championships + Scheduling"]
        Match["Match Ops + Digital Scorecard"]
        Stats["Standings + Analytics"]
        Comm["News + Chat + Notifications"]
        Billing["Billing + Sponsors + Plans"]
        AI["AI Scheduling + Integrity Checks"]
        Public["Public API + Widgets"]
    end

    subgraph Data
        PG["PostgreSQL"]
        Redis["Redis Cache + Queues"]
        Blob["R2/S3 Media Storage"]
    end

    WebOrg --> CDN
    WebFan --> CDN
    Mobile --> API
    Mobile --> WS
    Embed --> CDN
    CDN --> PWA
    PWA --> API
    API --> Auth
    API --> Champ
    API --> Match
    API --> Stats
    API --> Comm
    API --> Billing
    API --> AI
    API --> Public
    WS --> Match
    WS --> Stats
    Auth --> PG
    Champ --> PG
    Match --> PG
    Match --> Redis
    Stats --> PG
    Stats --> Redis
    Comm --> PG
    Comm --> Blob
    Billing --> PG
    Billing --> Redis
    AI --> PG
    AI --> Redis
    Public --> PG
    Public --> Redis
```

## Modulos do backend NestJS

- `auth`: JWT, refresh token, OAuth Google/Apple, RBAC e perfis
- `organizations`: ligas, staff, membership e assinaturas
- `championships`: campeonato, categoria, regulamento, formatos e configuracao
- `registrations`: formularios, limite de vagas, lista de espera e pagamento
- `teams`: times, escudos, elencos, capitaes e vinculos historicos
- `venues`: locais, quadras/campos, mapas e janelas de disponibilidade
- `scheduling`: geracao de tabela, sorteio ao vivo, seeding e reagendamento
- `matches`: partidas, sumula, arbitros, lances, assinaturas e arquivos
- `live`: placar ao vivo, cronometro, retransmissao websocket e fallback polling
- `stats`: classificacao, ranking, heatmaps, lideres e historico multi-campeonato
- `content`: noticias, stories, comentarios, reacoes, galerias e cards
- `notifications`: push, email, WhatsApp futuro e filas assicronas
- `billing`: planos, trial, cobranca recorrente, comissao e patrocinadores
- `public-api`: endpoints externos, widgets e documentacao Swagger
- `audit`: trilha de auditoria, deteccao de inconsistencias e eventos de seguranca

## Frontend web

- Dashboard do organizador com calendario semanal, KPIs e centro de acoes
- Central do capitao com elenco, presenca, punicoes e agenda
- Experiencia publica com pagina do campeonato, rodada, tabela e feed
- Painel de operacao da partida para desktop/tablet quando o arbitro estiver online

## App mobile React Native

- Modos dedicados para jogador, capitao, arbitro e staff
- Persistencia local para modo arbitro offline-first
- Scanner QR code para confirmacao de identidade
- Linha do tempo da partida com feedback visual para gol, cartao e substituicao

## Fluxo de dados principal

```mermaid
sequenceDiagram
    participant Arb as Arbitro Mobile
    participant API as NestJS API
    participant WS as Socket Gateway
    participant Redis as Redis
    participant DB as PostgreSQL
    participant Fan as Torcedor Web

    Arb->>API: envia evento da sumula
    API->>DB: persiste match_event e scorecard
    API->>Redis: atualiza cache do placar e fila de agregacao
    API->>WS: publica evento realtime
    WS-->>Fan: atualiza placar e timeline ao vivo
    Redis->>API: dispara recalculo assicrono
    API->>DB: atualiza standings e stats agregadas
    API-->>Arb: confirma sync ou mantem fila offline
```

## Decisoes de arquitetura

- Monorepo com `pnpm` + Turborepo para compartilhar tipos, design system e SDKs
- REST para operacoes de escrita e integracoes externas
- GraphQL para consultas ricas de estatisticas, paginas publicas e dashboards complexos
- PostgreSQL como fonte de verdade; Redis para cache, pub/sub, rate limiting e jobs curtos
- WebSocket para placar ao vivo; fallback por polling de 30 segundos para ambientes restritos
- Storage de objetos para escudos, midia, PDFs e cards de resultados
- Multi-tenant por `organization_id`, com isolamento logico e trilha de auditoria

## Escalabilidade e seguranca

- Agregacoes assicronas para ranking e historico
- CDN para midia e paginas publicas cacheaveis
- RBAC por perfil e papel contextual por campeonato
- Assinatura digital da sumula com hash do documento final
- Logs de auditoria para toda alteracao sensivel
- LGPD: consentimento, exclusao logica e retention policy
