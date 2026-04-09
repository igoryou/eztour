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

Arquivos direto na raiz do projeto:

- [architecture.md](C:/Users/igorl/Desktop/Projects/EZ%20TOUR/architecture.md)
- [database.md](C:/Users/igorl/Desktop/Projects/EZ%20TOUR/database.md)
- [delivery-plan.md](C:/Users/igorl/Desktop/Projects/EZ%20TOUR/delivery-plan.md)
- [app-api.md](C:/Users/igorl/Desktop/Projects/EZ%20TOUR/app-api.md)
- [app-mobile.md](C:/Users/igorl/Desktop/Projects/EZ%20TOUR/app-mobile.md)
- [app-web.md](C:/Users/igorl/Desktop/Projects/EZ%20TOUR/app-web.md)
- [packages-ui.md](C:/Users/igorl/Desktop/Projects/EZ%20TOUR/packages-ui.md)
- [infra.md](C:/Users/igorl/Desktop/Projects/EZ%20TOUR/infra.md)

## Principios do produto

- Operacao em tempo real com baixa friccao para arbitros e organizadores
- Experiencia publica excelente para torcedores sem exigir login
- Dados historicos por atleta, time e campeonato
- Regras flexiveis para varios formatos de disputa e varios esportes
- Offline-first para sumula e modo arbitro
- Monetizacao apenas pelo lado do organizador

## Proximo passo recomendado

Usar este blueprint para iniciar o monorepo com:

1. `app-api.md` com o backend NestJS planejado
2. `app-web.md` com o portal Next.js planejado
3. `app-mobile.md` com o app React Native planejado
4. `packages-ui.md` com o design system compartilhado
