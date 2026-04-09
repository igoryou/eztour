# APIs, wireframes, roadmap, aceite e testes

## Rotas REST principais

Base path sugerido: `/api/v1`

### Auth e perfis

- `POST /auth/register`
- `POST /auth/login`
- `POST /auth/oauth/google`
- `POST /auth/oauth/apple`
- `POST /auth/refresh`
- `POST /auth/logout`
- `GET /me`
- `PATCH /me`
- `GET /me/career`

### Organizacoes e assinatura

- `POST /organizations`
- `GET /organizations/:organizationId`
- `PATCH /organizations/:organizationId`
- `GET /organizations/:organizationId/members`
- `POST /organizations/:organizationId/members/invite`
- `PATCH /organizations/:organizationId/subscription`
- `GET /organizations/:organizationId/billing/overview`
- `GET /organizations/:organizationId/payments`

### Campeonatos e categorias

- `POST /championships`
- `GET /championships`
- `GET /championships/:championshipId`
- `PATCH /championships/:championshipId`
- `POST /championships/:championshipId/publish`
- `POST /championships/:championshipId/categories`
- `PATCH /categories/:categoryId`
- `POST /categories/:categoryId/rules`
- `POST /categories/:categoryId/schedule/generate`
- `POST /categories/:categoryId/schedule/live-draw`
- `POST /categories/:categoryId/schedule/reseed`

### Inscricoes e times

- `GET /categories/:categoryId/registration-form`
- `POST /categories/:categoryId/registrations`
- `GET /categories/:categoryId/registrations`
- `POST /registrations/:registrationId/approve`
- `POST /registrations/:registrationId/reject`
- `POST /teams`
- `GET /teams/:teamId`
- `PATCH /teams/:teamId`
- `POST /teams/:teamId/members`
- `PATCH /team-members/:teamMemberId`
- `POST /team-entries/:teamEntryId/check-in`

### Locais, agenda e partidas

- `POST /venues`
- `GET /venues`
- `POST /venues/:venueId/areas`
- `POST /venue-areas/:venueAreaId/availability`
- `POST /matches`
- `GET /matches/:matchId`
- `PATCH /matches/:matchId`
- `POST /matches/:matchId/reschedule`
- `POST /matches/:matchId/start`
- `POST /matches/:matchId/finish`
- `POST /matches/:matchId/mvp`

### Sumula digital e operacao ao vivo

- `POST /matches/:matchId/scorecard/open`
- `POST /matches/:matchId/scorecard/events`
- `POST /matches/:matchId/scorecard/sync-batch`
- `POST /matches/:matchId/scorecard/signatures`
- `POST /matches/:matchId/checkins/qr`
- `GET /matches/:matchId/timeline`
- `GET /matches/:matchId/live`
- `POST /matches/:matchId/live/pause`

### Estatisticas e classificacao

- `GET /categories/:categoryId/standings`
- `GET /categories/:categoryId/stats/players`
- `GET /categories/:categoryId/stats/teams`
- `GET /championships/:championshipId/rankings`
- `GET /players/:athleteProfileId/career`
- `POST /matches/:matchId/recalculate-stats`
- `GET /championships/:championshipId/exports`
- `POST /championships/:championshipId/exports`

### Conteudo e engajamento

- `POST /championships/:championshipId/news`
- `GET /championships/:championshipId/news`
- `POST /championships/:championshipId/stories`
- `POST /matches/:matchId/media`
- `POST /comments`
- `POST /reactions`
- `GET /chat/channels`
- `POST /chat/channels/:channelId/messages`

### Patrocinio e publico

- `POST /championships/:championshipId/sponsors`
- `GET /championships/:championshipId/public-page`
- `GET /championships/:championshipId/embed/table`
- `GET /public/championships/:slug`
- `GET /public/matches/:matchId/live`

### Observabilidade e IA

- `POST /categories/:categoryId/ai/schedule-suggestions`
- `GET /matches/:matchId/ai/integrity-flags`
- `POST /matches/:matchId/ai/integrity-flags/:flagId/resolve`

## Consultas GraphQL recomendadas

- `championshipPublicPage(slug)`
- `organizerDashboard(organizationId, range)`
- `playerCareer(publicSlug)`
- `categoryStandings(categoryId, roundId)`
- `liveMatchCenter(matchId)`
- `crossTournamentRanking(filters)`

## Wireframes descritivos

### Organizador - dashboard web

1. Header com seletor de campeonato, busca global, notificacoes e switch light/dark.
2. Hero com tres cards: jogos hoje, inscricoes pendentes, pagamentos pendentes.
3. Calendario semanal em destaque com jogos arrastaveis para reagendamento.
4. Bloco lateral com AI sugerindo melhores datas, conflitos de venue e partidas sem arbitro.
5. Abas inferiores: `Visao geral`, `Times`, `Tabela`, `Ao vivo`, `Financeiro`, `Conteudo`.

### Organizador - criacao em 3 passos

1. Passo 1: nome, esporte, temporada, visibilidade e capa.
2. Passo 2: categorias, formato, regras e quantidade de times.
3. Passo 3: locais, janela de inscricao, taxa e opcao de gerar a tabela.

### Jogador/Capitao - app mobile

1. Home com proximo jogo, status de presenca e atalhos de elenco.
2. Card do time com escudo, forma recente, punicoes e ranking.
3. Aba `Carreira` com historico em linha do tempo, premios e estatisticas totais.
4. Aba `Mensagens` com chat do campeonato e avisos oficiais.
5. Aba `Partidas` com confirmacao de presenca, local no mapa e QR de identificacao.

### Arbitro - modo offline-first

1. Tela inicial com lista de partidas designadas e badge de conectividade.
2. Tela da partida com cronometro central, placar grande e botoes rapidos de evento.
3. Linha do tempo rolavel com undo controlado e marcacao de eventos pendentes de sync.
4. Tela final para assinatura do arbitro e capitaes, resumo automatico e exportacao PDF.

### Torcedor - pagina publica

1. Hero com banner do campeonato, patrocinadores e CTA para acompanhar ao vivo.
2. Abas `Jogos`, `Tabela`, `Artilharia`, `Noticias`, `Midia`.
3. Card de partida ao vivo com ticker de eventos, placar e botao compartilhar.
4. Widget embed compacto com tabela e proxima rodada.

## Roadmap

### Fase 1 - MVP em 3 meses

Objetivo: lancar operacao real para futebol/futsal com organizador, atleta e torcedor.

- Auth com email/senha, Google e RBAC basico
- Organizacoes, planos e trial
- Campeonatos, categorias, times, elencos e inscricoes
- Geracao de tabela para pontos corridos, eliminatoria e grupos + mata-mata
- Partidas, venues e reagendamento
- Sumula digital com gols, cartoes, substituicoes, cronometro e assinaturas
- Placar ao vivo, pagina publica e tabela automatica
- Estatisticas basicas: tabela, artilharia, assistencias, cartoes e forma recente
- Financeiro basico: taxa de inscricao, Pix e painel simples
- Exportacao PDF/CSV da sumula e classificacao

### Fase 2 - v1.0 em 6 meses

- Swiss system
- App mobile mais completo para capitao e arbitro
- Patrocinadores por tier com exibicao automatica
- Chat, noticias, comentarios e reacoes
- Historico unificado do atleta
- Marketplace de campeonatos
- API publica documentada e widget embed
- BI: heatmaps, ranking cross-tournament e relatorios XLSX

### Fase 3 - v2.0 em 12 meses

- Stories, videos e highlights automatizados
- IA de previsao de rodada e deteccao de inconsistencias
- Sorteio ao vivo sincronizado
- QR identity com antifraude reforcada
- Suporte aprofundado a basquete, volei, handebol, beach tennis e padel
- White-label parcial para organizadores Elite

## Planos e monetizacao

### Free

- Ate 2 campeonatos ativos
- Ate 50 jogadores
- Sem patrocinadores
- Com marca d'agua da plataforma

### Starter

- Preco alvo: `R$ 29/mes`
- Ate 5 campeonatos ativos
- Ate 200 jogadores
- Ate 2 patrocinadores
- Sem marca d'agua

### Pro

- Preco alvo: `R$ 59/mes`
- Campeonatos ilimitados
- Ate 600 jogadores
- Ate 10 patrocinadores
- Relatorios avancados
- URL personalizada

### Elite

- Preco alvo: `R$ 99/mes`
- Tudo do Pro
- API publica
- Widget embed
- Suporte prioritario
- Sem limite de jogadores

### Regras comerciais

- Trial de 14 dias em qualquer plano pago sem exigir cartao
- Comissao opcional de 2% sobre pagamentos processados pela plataforma
- Monetizacao exclusiva pelo lado do organizador, sem anuncios para torcedores

## Bibliotecas e dependencias recomendadas

### Monorepo e shared tooling

- `pnpm` para workspaces leves e rapidos
- `turbo` para pipeline incremental e cache
- `typescript` para contratos compartilhados
- `zod` para validacao ponta a ponta

### Web

- `next` para App Router, SSR, RSC e PWA
- `tailwindcss` para velocidade de UI
- `shadcn/ui` para base consistente e customizavel
- `react-hook-form` + `zod` para formularios complexos
- `tanstack-query` para cache e sync de dados cliente
- `socket.io-client` para eventos ao vivo
- `leaflet` ou `mapbox-gl` para mapa
- `framer-motion` para microanimacoes e onboarding
- `next-pwa` para modo offline parcial

### Mobile

- `expo` para acelerar build iOS/Android
- `expo-router` para navegacao com arquivos
- `@tanstack/react-query` para sync offline/online
- `expo-notifications` para push
- `expo-barcode-scanner` para QR code
- `expo-sqlite` ou `watermelondb` para fila offline da sumula
- `socket.io-client` para placar ao vivo

### Backend

- `@nestjs/core`, `@nestjs/websockets`, `@nestjs/swagger`
- `prisma` para produtividade no MVP
- `ioredis` para cache, pub/sub e locks
- `bullmq` para filas de notificacao, export e agregacao
- `passport` + `passport-jwt` + providers OAuth
- `mercadopago` ou `stripe` conforme estrategia comercial
- `pdfkit` ou `puppeteer` para sumulas e relatorios

### Qualidade e observabilidade

- `vitest` ou `jest` para unitarios
- `supertest` para integracao HTTP
- `playwright` para E2E web
- `detox` ou `maestro` para E2E mobile
- `sentry` para erros
- `pino` para logging estruturado

## Criterios de aceitacao por modulo

### Auth e perfis

- Usuario consegue criar conta, entrar e recuperar sessao
- Organizador pode convidar staff com permissao especifica
- Torcedor acessa paginas publicas sem login

### Campeonatos e categorias

- Organizador cria campeonato em no maximo 3 passos
- Cada categoria aceita formato independente e regras proprias de classificacao
- Sistema impede publicacao sem locais, regulamento minimo e janela de inscricao valida

### Inscricoes e times

- Formulario online aceita respostas customizadas
- Limite de vagas e lista de espera funcionam automaticamente
- Time aprovado entra na tabela apenas apos validacao obrigatoria

### Agenda e venues

- Sistema gera tabela automaticamente sem conflitos de local no mesmo horario
- Reagendamento dispara notificacao para envolvidos
- AI sugere ao menos 3 slots validos quando existir disponibilidade

### Sumula digital

- Arbitro consegue operar a partida offline e sincronizar depois
- Todo evento gera timeline consistente e atualiza o placar ao vivo
- Sumula final exige assinatura do arbitro e de ambos os capitaes

### Estatisticas

- Classificacao respeita criterios configurados por categoria
- Dados de artilharia e disciplina atualizam ate 30 segundos apos encerramento
- Perfil do atleta mostra historico consolidado entre campeonatos

### Conteudo e engajamento

- Organizador publica noticia com texto rico e midia
- Torcedor comenta e reage em ambiente moderavel
- Cards de resultado podem ser gerados e compartilhados

### Financeiro e patrocinio

- Organizacao visualiza receitas pagas, pendentes e taxas da plataforma
- Patrocinadores aparecem automaticamente em pagina publica, tabelas e PDFs elegiveis
- Trial e limites por plano sao aplicados sem intervencao manual

### API publica e embeds

- Endpoint publico responde dados do campeonato com documentacao Swagger
- Widget embed pode ser usado em iframe sem quebrar layout
- Rate limiting protege o consumo externo

## Estrategia de testes

### Unitarios

- Regras de standings, criterio de desempate e formatadores de calendario
- Validacao de eventos da sumula e deteccao de inconsistencias
- Calculo de limites de plano, comissao e repasse financeiro

### Integracao

- Auth, convites, criacao de campeonato e geracao de tabela
- Fluxo de inscricao com pagamento e aprovacao
- Persistencia da sumula, agregacao de stats e disparo de notificacoes
- WebSocket com publicacao de placar e sincronizacao de cache

### End-to-end

- Web: organizador cria campeonato, gera tabela, inicia partida e exporta PDF
- Web publico: torcedor acompanha pagina, tabela e placar ao vivo
- Mobile arbitro: abre sumula offline, registra eventos e sincroniza com sucesso
- Mobile capitao: confirma presenca, consulta estatisticas e recebe push

### Gates de qualidade

- Cobertura minima de 80% nos modulos de regras de negocio
- Smoke E2E obrigatorio a cada PR em `web`, `api` e `mobile`
- Carga basica validando 500 conexoes websocket simultaneas no MVP

## Plano de equipe para 90 dias

### Dev 1

- Lidera `apps/api`, banco, auth, campeonatos e billing

### Dev 2

- Lidera `apps/web`, dashboard do organizador e experiencia publica

### Dev 3

- Lidera `apps/mobile`, modo arbitro e sincronizacao offline

### Ritual recomendado

- Sprints semanais com demo operacional toda sexta
- Definicao de pronto exige teste automatizado e telemetria minima
- Entregas orientadas por fluxo completo, nao por tela isolada
