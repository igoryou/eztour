# EZ TOUR Sports Platform

Blueprint inicial para uma plataforma web e mobile de gerenciamento de campeonatos esportivos com foco em organizadores, atletas e torcedores.

## Objetivo

Entregar um produto lancavel em 90 dias para uma equipe de 3 desenvolvedores, com base preparada para escalar para multiplos esportes, operacao em tempo real e monetizacao B2B2C sem anuncios para o publico final.

## Stack alvo

- Web: Next.js + Tailwind CSS + shadcn/ui + PWA
- Mobile: React Native com Expo
- Backend: NestJS + REST + GraphQL + Socket.io
- Dados: PostgreSQL + Redis
- Midia: Cloudflare R2 ou AWS S3
- Auth: JWT + OAuth Google/Apple + email/senha
- Infra: Docker + CI/CD + Railway ou Fly.io

## Estrutura sugerida

```text
apps/
  api/        Backend NestJS e gateways realtime
  mobile/     App React Native para atleta, arbitro e organizador em campo
  web/        Portal Next.js para organizadores, atletas e torcedores
docs/         Arquitetura, dados, APIs, wireframes e roadmap
infra/        Docker, observabilidade e ambientes
packages/
  ui/         Design system compartilhado
```

## Entregaveis criados neste repositorio

- [Arquitetura](C:/Users/igorl/Desktop/Projects/EZ%20TOUR/docs/architecture.md)
- [Banco de dados relacional](C:/Users/igorl/Desktop/Projects/EZ%20TOUR/docs/database.md)
- [Plano de entrega, APIs, wireframes, aceite e testes](C:/Users/igorl/Desktop/Projects/EZ%20TOUR/docs/delivery-plan.md)

## Principios do produto

- Operacao em tempo real com baixa friccao para arbitros e organizadores
- Experiencia publica excelente para torcedores sem exigir login
- Dados historicos por atleta, time e campeonato
- Regras flexiveis para varios formatos de disputa e varios esportes
- Offline-first para sumula e modo arbitro
- Monetizacao apenas pelo lado do organizador

## Proximo passo recomendado

Usar este blueprint para iniciar o monorepo com:

1. `apps/api` com modulos de auth, championships, matches, stats e billing
2. `apps/web` com dashboard do organizador e pagina publica do campeonato
3. `apps/mobile` com modo arbitro e central do atleta
4. `packages/ui` com tokens, componentes base e identidade visual
