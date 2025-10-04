# Publication automatique des images Docker

Ce workflow GitHub Actions publie automatiquement les images Docker de Firecrawl sur votre registry personnel GitHub Container Registry (`ghcr.io/cmoi936`).

## Images publiées

Le workflow publie cinq images Docker :

- `ghcr.io/cmoi936/firecrawl-api:latest` - L'API principale de Firecrawl
- `ghcr.io/cmoi936/playwright-service:latest` - Le service Playwright pour le scraping
- `ghcr.io/cmoi936/nuq-postgres:latest` - L'image PostgreSQL personnalisée avec NuQ
- `ghcr.io/cmoi936/firecrawl-mcp:latest` - Le service MCP (Model Context Protocol) pour Firecrawl
- `ghcr.io/cmoi936/redis:alpine` - L'image Redis officielle (version Alpine)

**Note**: Les images MCP et Redis sont des images externes officielles qui sont automatiquement récupérées et re-publiées sur votre registry personnel.

## Déclencheurs

Le workflow se déclenche automatiquement sur :

- **Push vers main** : Quand des fichiers dans `apps/api/`, `apps/playwright-service-ts/`, `apps/nuq-postgres/` ou `docker-compose.yaml` sont modifiés
- **Workflow dispatch** : Peut être déclenché manuellement depuis l'onglet Actions de GitHub

## Utilisation manuelle

1. Allez dans l'onglet **Actions** de votre repository GitHub
2. Sélectionnez le workflow **"Publish Docker Images to Personal GHCR"**
3. Cliquez sur **"Run workflow"**
4. (Optionnel) Spécifiez un tag personnalisé (par défaut : `latest`)

## Tags automatiques

Le workflow génère automatiquement plusieurs tags :

- `latest` - Version la plus récente (sur la branche main)
- `main` - Tag basé sur la branche
- `main-<sha>` - Tag avec le hash court du commit
- Tag personnalisé si spécifié manuellement

## Mise à jour automatique du docker-compose.yaml

Après publication, le workflow met automatiquement à jour le fichier `docker-compose.yaml` pour utiliser les nouvelles images publiées au lieu de la construction locale.

## Permissions requises

Le workflow nécessite les permissions suivantes :
- `contents: read` - Pour lire le code source
- `packages: write` - Pour publier sur GHCR

Ces permissions sont automatiquement accordées par GitHub pour les workflows dans le repository.

## Cache de build

Le workflow utilise GitHub Actions Cache pour accélérer les builds successifs en cachant les couches Docker.

## Sécurité

- Utilise `GITHUB_TOKEN` pour l'authentification (pas besoin de PAT)
- Les images sont publiées sur votre registry privé
- Le cache est stocké de manière sécurisée dans GitHub Actions