# Modelo relacional

## Convencoes

- Todas as tabelas usam `id UUID PK`
- Auditoria padrao: `created_at`, `updated_at`, `deleted_at`
- Multi-tenant: entidades administrativas carregam `organization_id`
- Campos muito dinamicos podem usar `JSONB` no MVP

## Identidade e acesso

### `users`
- `email`, `password_hash`, `display_name`, `avatar_url`, `phone`, `status`, `last_login_at`

### `user_identities`
- `user_id -> users.id`
- `provider`, `provider_subject`, `provider_email`, `linked_at`

### `organizations`
- `owner_user_id -> users.id`
- `name`, `slug`, `logo_url`, `billing_email`, `default_timezone`, `status`

### `organization_members`
- `organization_id -> organizations.id`
- `user_id -> users.id`
- `role`, `invited_by_user_id -> users.id`, `accepted_at`

### `athlete_profiles`
- `user_id -> users.id NULL`
- `full_name`, `nickname`, `birth_date`, `gender`, `document_number`
- `city`, `state`, `dominant_side`, `preferred_positions JSONB`
- `height_cm`, `weight_kg`, `career_summary`, `public_slug`

## Comercial

### `subscription_plans`
- `code`, `name`, `monthly_price_cents`
- `active_championship_limit`, `player_limit`, `sponsor_limit`
- `watermark_enabled`, `public_api_enabled`, `embed_enabled`

### `organization_subscriptions`
- `organization_id -> organizations.id`
- `plan_id -> subscription_plans.id`
- `status`, `trial_ends_at`, `current_period_start`, `current_period_end`
- `billing_provider`, `external_subscription_id`

### `payment_transactions`
- `organization_id -> organizations.id`
- `championship_id -> championships.id NULL`
- `registration_id -> registrations.id NULL`
- `type`, `method`, `gross_amount_cents`, `fee_amount_cents`
- `platform_commission_cents`, `net_amount_cents`, `status`, `paid_at`
- `provider_payload JSONB`

### `sponsors`
- `organization_id -> organizations.id`
- `championship_id -> championships.id NULL`
- `name`, `tier`, `logo_url`, `website_url`
- `contact_name`, `contact_phone`, `start_at`, `end_at`, `display_order`

## Estrutura esportiva

### `championships`
- `organization_id -> organizations.id`
- `name`, `slug`, `sport_type`, `season_year`, `visibility`, `status`
- `registration_open_at`, `registration_close_at`, `starts_at`, `ends_at`
- `timezone`, `description`, `rules_markdown`, `watermark_enabled`, `custom_domain`

### `championship_categories`
- `championship_id -> championships.id`
- `name`, `slug`, `age_rule`, `gender_rule`, `skill_level`
- `competition_format`, `max_teams`, `min_teams`, `roster_limit`
- `entry_fee_cents`, `standings_rules JSONB`

### `registration_forms`
- `championship_category_id -> championship_categories.id`
- `title`, `fields_schema JSONB`, `terms_markdown`, `requires_payment`, `is_active`

### `registrations`
- `championship_category_id -> championship_categories.id`
- `team_id -> teams.id NULL`
- `responsible_user_id -> users.id NULL`
- `status`, `submitted_at`, `approved_at`, `waitlist_position`
- `answers JSONB`, `payment_status`

### `teams`
- `organization_id -> organizations.id NULL`
- `name`, `slug`, `short_name`, `crest_url`
- `primary_color`, `secondary_color`, `city`, `founded_year`, `is_public`

### `team_members`
- `team_id -> teams.id`
- `athlete_profile_id -> athlete_profiles.id`
- `user_id -> users.id NULL`
- `role`, `shirt_number`, `status`, `joined_at`, `left_at`

### `team_entries`
- `championship_category_id -> championship_categories.id`
- `team_id -> teams.id`
- `seed_number`, `entry_status`, `group_position_hint`, `checked_in_at`

## Locais e agenda

### `venues`
- `organization_id -> organizations.id`
- `name`, `address_line`, `city`, `state`, `postal_code`, `country`
- `latitude`, `longitude`, `maps_url`, `notes`

### `venue_areas`
- `venue_id -> venues.id`
- `name`, `sport_type`, `surface_type`, `capacity`, `indoor`

### `venue_availability`
- `venue_area_id -> venue_areas.id`
- `weekday`, `start_time`, `end_time`, `blocked_dates JSONB`

## Competicao e partidas

### `stages`
- `championship_category_id -> championship_categories.id`
- `name`, `stage_type`, `sort_order`, `status`

### `groups`
- `stage_id -> stages.id`
- `name`, `sort_order`

### `group_members`
- `group_id -> groups.id`
- `team_entry_id -> team_entries.id`

### `rounds`
- `stage_id -> stages.id`
- `name`, `round_number`, `starts_at`, `ends_at`

### `matches`
- `championship_category_id -> championship_categories.id`
- `stage_id -> stages.id`
- `round_id -> rounds.id NULL`
- `group_id -> groups.id NULL`
- `venue_area_id -> venue_areas.id NULL`
- `home_team_entry_id -> team_entries.id`
- `away_team_entry_id -> team_entries.id`
- `scheduled_start_at`, `scheduled_end_at`, `actual_start_at`, `actual_end_at`
- `status`, `home_score`, `away_score`, `overtime_score JSONB`, `shootout_score JSONB`
- `attendance_count`, `mvp_athlete_profile_id -> athlete_profiles.id NULL`
- `live_stream_status`, `reschedule_reason`

### `match_officials`
- `match_id -> matches.id`
- `user_id -> users.id NULL`
- `name_snapshot`, `role`

### `digital_scorecards`
- `match_id -> matches.id`
- `status`, `started_by_user_id -> users.id`, `device_id`
- `started_offline`, `final_hash`, `integrity_status`, `locked_at`

### `scorecard_signatures`
- `digital_scorecard_id -> digital_scorecards.id`
- `signer_type`, `user_id -> users.id NULL`, `signature_name`
- `signature_payload`, `signed_at`

### `match_events`
- `match_id -> matches.id`
- `digital_scorecard_id -> digital_scorecards.id`
- `team_entry_id -> team_entries.id NULL`
- `athlete_profile_id -> athlete_profiles.id NULL`
- `secondary_athlete_profile_id -> athlete_profiles.id NULL`
- `event_type`, `period`, `minute`, `second`, `stoppage_time_seconds`
- `payload JSONB`, `source`, `created_by_user_id -> users.id NULL`

### `match_timeline_assets`
- `match_id -> matches.id`
- `media_asset_id -> media_assets.id`
- `linked_event_id -> match_events.id NULL`
- `asset_role`

## Disciplina, IA e verificacoes

### `penalties`
- `championship_id -> championships.id`
- `team_id -> teams.id NULL`
- `athlete_profile_id -> athlete_profiles.id NULL`
- `match_id -> matches.id NULL`
- `penalty_type`, `reason`, `games_suspended`, `fine_amount_cents`, `status`, `applied_at`

### `qr_checkins`
- `match_id -> matches.id`
- `team_entry_id -> team_entries.id`
- `athlete_profile_id -> athlete_profiles.id`
- `checked_by_user_id -> users.id`
- `verification_method`, `status`, `checked_at`

### `ai_integrity_flags`
- `match_id -> matches.id`
- `digital_scorecard_id -> digital_scorecards.id`
- `flag_type`, `severity`, `description`, `resolution_status`
- `resolved_by_user_id -> users.id NULL`

### `ai_schedule_suggestions`
- `championship_category_id -> championship_categories.id`
- `generated_by_user_id -> users.id`
- `inputs JSONB`, `suggested_slots JSONB`, `confidence_score`, `accepted_at`

## Estatisticas e historico

### `standings_snapshots`
- `championship_category_id -> championship_categories.id`
- `stage_id -> stages.id NULL`
- `round_id -> rounds.id NULL`
- `generated_at`, `table_payload JSONB`

### `player_stats_aggregates`
- `championship_id -> championships.id`
- `championship_category_id -> championship_categories.id`
- `athlete_profile_id -> athlete_profiles.id`
- `team_id -> teams.id`
- `matches_played`, `goals`, `assists`, `own_goals`, `saves`
- `yellow_cards`, `red_cards`, `minutes_played`, `mvp_awards`, `heatmap_payload JSONB`

### `team_stats_aggregates`
- `championship_id -> championships.id`
- `championship_category_id -> championship_categories.id`
- `team_id -> teams.id`
- `matches_played`, `wins`, `draws`, `losses`, `points`
- `goals_for`, `goals_against`, `goal_difference`, `clean_sheets`
- `form_last_five JSONB`

### `career_snapshots`
- `athlete_profile_id -> athlete_profiles.id`
- `generated_at`, `payload JSONB`

## Conteudo e comunicacao

### `news_posts`
- `organization_id -> organizations.id`
- `championship_id -> championships.id NULL`
- `author_user_id -> users.id`
- `title`, `slug`, `excerpt`, `body_richtext JSONB`
- `cover_media_asset_id -> media_assets.id NULL`
- `status`, `published_at`

### `stories`
- `organization_id -> organizations.id`
- `championship_id -> championships.id NULL`
- `author_user_id -> users.id`
- `media_asset_id -> media_assets.id`
- `caption`, `expires_at`

### `media_assets`
- `organization_id -> organizations.id NULL`
- `uploaded_by_user_id -> users.id NULL`
- `storage_provider`, `bucket`, `object_key`, `mime_type`
- `width`, `height`, `duration_seconds`, `size_bytes`, `checksum`

### `chat_channels`
- `organization_id -> organizations.id`
- `championship_id -> championships.id NULL`
- `team_id -> teams.id NULL`
- `name`, `channel_type`

### `chat_messages`
- `chat_channel_id -> chat_channels.id`
- `author_user_id -> users.id`
- `body`, `attachments JSONB`, `edited_at`

### `comments`
- `championship_id -> championships.id NULL`
- `match_id -> matches.id NULL`
- `news_post_id -> news_posts.id NULL`
- `author_user_id -> users.id NULL`
- `guest_name`, `body`, `status`

### `reactions`
- `user_id -> users.id NULL`
- `guest_fingerprint`, `target_type`, `target_id`, `reaction_type`

## Operacao, exportacao e auditoria

### `notifications`
- `user_id -> users.id`
- `organization_id -> organizations.id NULL`
- `channel`, `topic`, `title`, `body`, `payload JSONB`
- `scheduled_at`, `sent_at`, `read_at`

### `exports`
- `organization_id -> organizations.id`
- `championship_id -> championships.id NULL`
- `requested_by_user_id -> users.id`
- `export_type`, `status`, `file_media_asset_id -> media_assets.id NULL`, `filters JSONB`

### `audit_logs`
- `organization_id -> organizations.id NULL`
- `actor_user_id -> users.id NULL`
- `entity_type`, `entity_id`, `action`
- `before_payload JSONB`, `after_payload JSONB`, `ip_address`, `user_agent`

## Relacoes criticas

- Uma `organization` possui varios `championships`, `venues`, `members`, `sponsors` e `subscriptions`
- Um `championship` possui varias `categories`, `news_posts`, `matches`, `penalties` e `standings_snapshots`
- Uma `championship_category` possui `registration_forms`, `registrations`, `team_entries`, `stages`, `rounds` e `matches`
- Um `team` possui muitos `team_members` e participa de varios campeonatos via `team_entries`
- Um `match` concentra `digital_scorecard`, `match_events`, `match_officials`, `qr_checkins` e `match_timeline_assets`
- Um `athlete_profile` conecta carreira, eventos de jogo, membros de time e agregacoes estatisticas

## Indices recomendados

- `users(email)` unico
- `organizations(slug)` unico
- `championships(organization_id, slug)` unico
- `championship_categories(championship_id, slug)` unico
- `matches(championship_category_id, scheduled_start_at)`
- `match_events(match_id, minute, second)`
- `player_stats_aggregates(championship_category_id, athlete_profile_id)`
- `team_stats_aggregates(championship_category_id, team_id)`
- `notifications(user_id, read_at, scheduled_at)`
