# Bfloat Cloud Run Environment Variables

## Full Secrets String

```
E2B_API_KEY=E2B_API_KEY:latest,REDIS_PASSWORD=REDIS_PASSWORD:latest,DATABASE_URL=DATABASE_URL:latest,E2B_TEMPLATE_ID=E2B_TEMPLATE_ID:latest,VITE_POSTHOG_KEY=VITE_POSTHOG_KEY:latest,REDIS_HOST=REDIS_HOST:latest,EXPO_EAS_TOKEN=EXPO_EAS_TOKEN:latest,ANTHROPIC_API_KEY=ANTHROPIC_API_KEY:latest,STRIPE_WEBHOOK_SECRET=STRIPE_WEBHOOK_SECRET:latest,WORKOS_CLIENT_ID=WORKOS_CLIENT_ID:latest,OPENAI_API_KEY=OPENAI_API_KEY:latest,AWS_REGION=AWS_REGION:latest,CONVEX_OAUTH_CLIENT_SECRET=CONVEX_OAUTH_CLIENT_SECRET:latest,EXPO_WEBHOOK_SECRET=EXPO_WEBHOOK_SECRET:latest,REDIS_TLS_ENABLED=REDIS_TLS_ENABLED:latest,STRIPE_SECRET_KEY=STRIPE_SECRET_KEY:latest,SUPABASE_URL=SUPABASE_URL:latest,SUPABASE_OAUTH_CLIENT_ID=SUPABASE_OAUTH_CLIENT_ID:latest,WORKOS_REDIRECT_URI=WORKOS_REDIRECT_URI:latest,SENTRY_DSN=SENTRY_DSN:latest,CREDENTIALS_BUCKET_NAME=CREDENTIALS_BUCKET_NAME:latest,DIRECT_URL=DIRECT_URL:latest,STRIPE_PUBLISHABLE_KEY=STRIPE_PUBLISHABLE_KEY:latest,UPLOADS_BUCKET_NAME=UPLOADS_BUCKET_NAME:latest,STRIPE_METER_ID=STRIPE_METER_ID:latest,AWS_SECRET_ACCESS_KEY=AWS_SECRET_ACCESS_KEY:latest,APP_URL=APP_URL:latest,SUPABASE_OAUTH_CLIENT_SECRET=SUPABASE_OAUTH_CLIENT_SECRET:latest,POSTHOG_API_KEY=POSTHOG_API_KEY:latest,EXPO_WEBHOOK_URL=EXPO_WEBHOOK_URL:latest,CHAT_IMAGES_BUCKET_NAME=CHAT_IMAGES_BUCKET_NAME:latest,CONVEX_OAUTH_REDIRECT_URI=CONVEX_OAUTH_REDIRECT_URI:latest,GOOGLE_GENERATIVE_AI_API_KEY=GOOGLE_GENERATIVE_AI_API_KEY:latest,INTERNAL_VERIFY_TOKEN=INTERNAL_VERIFY_TOKEN:latest,SUPABASE_OAUTH_REDIRECT_URI=SUPABASE_OAUTH_REDIRECT_URI:latest,AWS_ACCESS_KEY_ID=AWS_ACCESS_KEY_ID:latest,CONVEX_OAUTH_CLIENT_ID=CONVEX_OAUTH_CLIENT_ID:latest,SUPABASE_KEY=SUPABASE_KEY:latest,REDIS_PORT=REDIS_PORT:latest,GOOGLE_API_KEY=GOOGLE_API_KEY:latest,USE_OPENROUTER=USE_OPENROUTER:latest,WORKOS_COOKIE_PASSWORD=WORKOS_COOKIE_PASSWORD:latest,WORKOS_API_KEY=WORKOS_API_KEY:latest,OPENROUTER_API_KEY=OPENROUTER_API_KEY:latest,BASE_URL=BASE_URL:latest,CLAUDE_CODE_USE_VERTEX=CLAUDE_CODE_USE_VERTEX:latest,CLOUD_ML_REGION=CLOUD_ML_REGION:latest,ANTHROPIC_VERTEX_PROJECT_ID=ANTHROPIC_VERTEX_PROJECT_ID:latest,GITHUB_APP_ID=GITHUB_APP_ID:latest,GITHUB_APP_PRIVATE_KEY=GITHUB_APP_PRIVATE_KEY:latest,GITHUB_APP_INSTALLATION_ID=GITHUB_APP_INSTALLATION_ID:latest,REVENUECAT_OAUTH_CLIENT_ID=REVENUECAT_OAUTH_CLIENT_ID:latest,REVENUECAT_OAUTH_REDIRECT_URI=REVENUECAT_OAUTH_REDIRECT_URI:latest,STRIPE_CONNECT_CLIENT_ID=STRIPE_CONNECT_CLIENT_ID:latest,STRIPE_CONNECT_REDIRECT_URI=STRIPE_CONNECT_REDIRECT_URI:latest,STRIPE_CONNECT_DESKTOP_REDIRECT_URI=STRIPE_CONNECT_DESKTOP_REDIRECT_URI:latest,STRIPE_LIVE_SECRET_KEY=STRIPE_LIVE_SECRET_KEY:latest,STRIPE_LIVE_PUBLISHABLE_KEY=STRIPE_LIVE_PUBLISHABLE_KEY:latest,GOOGLE_OAUTH_CLIENT_ID=GOOGLE_OAUTH_CLIENT_ID:latest,GOOGLE_OAUTH_CLIENT_SECRET=GOOGLE_OAUTH_CLIENT_SECRET:latest,GOOGLE_OAUTH_REDIRECT_URI=GOOGLE_OAUTH_REDIRECT_URI:latest,GOOGLE_OAUTH_DESKTOP_REDIRECT_URI=GOOGLE_OAUTH_DESKTOP_REDIRECT_URI:latest,GITHUB_OAUTH_CLIENT_ID=GITHUB_OAUTH_CLIENT_ID:latest,GITHUB_OAUTH_CLIENT_SECRET=GITHUB_OAUTH_CLIENT_SECRET:latest,GITHUB_OAUTH_REDIRECT_URI=GITHUB_OAUTH_REDIRECT_URI:latest,GITHUB_OAUTH_DESKTOP_REDIRECT_URI=GITHUB_OAUTH_DESKTOP_REDIRECT_URI:latest,CONVEX_OAUTH_DESKTOP_REDIRECT_URI=CONVEX_OAUTH_DESKTOP_REDIRECT_URI:latest,LAUNCHDARKLY_SDK_KEY=LAUNCHDARKLY_SDK_KEY:latest
```

## Variables Breakdown

### Core Infrastructure
| Variable | Description |
|---|---|
| `DATABASE_URL` | PostgreSQL connection string (via pgbouncer) |
| `DIRECT_URL` | Direct PostgreSQL connection (for migrations) |
| `REDIS_HOST` | Redis host |
| `REDIS_PORT` | Redis port |
| `REDIS_PASSWORD` | Redis password |
| `REDIS_TLS_ENABLED` | Enable TLS for Redis |
| `APP_URL` | Application URL |
| `BASE_URL` | Base URL (defaults to https://bfloat.ai) |

### AI / LLM
| Variable | Description |
|---|---|
| `ANTHROPIC_API_KEY` | Anthropic API key |
| `OPENAI_API_KEY` | OpenAI API key |
| `GOOGLE_API_KEY` | Google AI API key |
| `GOOGLE_GENERATIVE_AI_API_KEY` | Google Generative AI key |
| `OPENROUTER_API_KEY` | OpenRouter API key |
| `USE_OPENROUTER` | Enable OpenRouter |
| `CLAUDE_CODE_USE_VERTEX` | Use Vertex AI for Claude |
| `CLOUD_ML_REGION` | Vertex AI region |
| `ANTHROPIC_VERTEX_PROJECT_ID` | Vertex AI project ID |

### AWS
| Variable | Description |
|---|---|
| `AWS_ACCESS_KEY_ID` | AWS access key |
| `AWS_SECRET_ACCESS_KEY` | AWS secret key |
| `AWS_REGION` | AWS region |
| `CREDENTIALS_BUCKET_NAME` | S3 bucket for credentials |
| `UPLOADS_BUCKET_NAME` | S3 bucket for uploads |
| `CHAT_IMAGES_BUCKET_NAME` | S3 bucket for chat images |

### Stripe
| Variable | Description |
|---|---|
| `STRIPE_SECRET_KEY` | Stripe secret key |
| `STRIPE_PUBLISHABLE_KEY` | Stripe publishable key |
| `STRIPE_WEBHOOK_SECRET` | Stripe webhook secret |
| `STRIPE_METER_ID` | Stripe meter ID |
| `STRIPE_CONNECT_CLIENT_ID` | Stripe Connect OAuth client ID |
| `STRIPE_CONNECT_REDIRECT_URI` | Stripe Connect OAuth redirect URI |
| `STRIPE_CONNECT_DESKTOP_REDIRECT_URI` | Stripe Connect desktop redirect URI |
| `STRIPE_LIVE_SECRET_KEY` | Stripe live secret key (for prod deployments) |
| `STRIPE_LIVE_PUBLISHABLE_KEY` | Stripe live publishable key (for prod deployments) |

### WorkOS (Auth)
| Variable | Description |
|---|---|
| `WORKOS_API_KEY` | WorkOS API key |
| `WORKOS_CLIENT_ID` | WorkOS client ID |
| `WORKOS_REDIRECT_URI` | WorkOS redirect URI |
| `WORKOS_COOKIE_PASSWORD` | WorkOS cookie encryption password |

### GitHub App
| Variable | Description |
|---|---|
| `GITHUB_APP_ID` | GitHub App ID |
| `GITHUB_APP_PRIVATE_KEY` | GitHub App private key (RSA PEM) |
| `GITHUB_APP_INSTALLATION_ID` | GitHub App installation ID |

### GitHub OAuth
| Variable | Description |
|---|---|
| `GITHUB_OAUTH_CLIENT_ID` | GitHub OAuth client ID |
| `GITHUB_OAUTH_CLIENT_SECRET` | GitHub OAuth client secret |
| `GITHUB_OAUTH_REDIRECT_URI` | GitHub OAuth redirect URI |
| `GITHUB_OAUTH_DESKTOP_REDIRECT_URI` | GitHub OAuth desktop redirect URI |

### Google OAuth
| Variable | Description |
|---|---|
| `GOOGLE_OAUTH_CLIENT_ID` | Google OAuth client ID |
| `GOOGLE_OAUTH_CLIENT_SECRET` | Google OAuth client secret |
| `GOOGLE_OAUTH_REDIRECT_URI` | Google OAuth redirect URI |
| `GOOGLE_OAUTH_DESKTOP_REDIRECT_URI` | Google OAuth desktop redirect URI |

### Convex OAuth
| Variable | Description |
|---|---|
| `CONVEX_OAUTH_CLIENT_ID` | Convex OAuth client ID |
| `CONVEX_OAUTH_CLIENT_SECRET` | Convex OAuth client secret |
| `CONVEX_OAUTH_REDIRECT_URI` | Convex OAuth redirect URI |
| `CONVEX_OAUTH_DESKTOP_REDIRECT_URI` | Convex OAuth desktop redirect URI |

### Supabase OAuth
| Variable | Description |
|---|---|
| `SUPABASE_URL` | Supabase project URL |
| `SUPABASE_KEY` | Supabase anon key |
| `SUPABASE_OAUTH_CLIENT_ID` | Supabase OAuth client ID |
| `SUPABASE_OAUTH_CLIENT_SECRET` | Supabase OAuth client secret |
| `SUPABASE_OAUTH_REDIRECT_URI` | Supabase OAuth redirect URI |

### RevenueCat OAuth
| Variable | Description |
|---|---|
| `REVENUECAT_OAUTH_CLIENT_ID` | RevenueCat OAuth client ID |
| `REVENUECAT_OAUTH_REDIRECT_URI` | RevenueCat OAuth redirect URI |

### Expo
| Variable | Description |
|---|---|
| `EXPO_EAS_TOKEN` | Expo EAS token |
| `EXPO_WEBHOOK_SECRET` | Expo webhook secret |
| `EXPO_WEBHOOK_URL` | Expo webhook URL |

### E2B (Sandboxes)
| Variable | Description |
|---|---|
| `E2B_API_KEY` | E2B API key |
| `E2B_TEMPLATE_ID` | E2B template ID |

### Analytics / Monitoring
| Variable | Description |
|---|---|
| `VITE_POSTHOG_KEY` | PostHog key (client-side) |
| `POSTHOG_API_KEY` | PostHog API key (server-side) |
| `SENTRY_DSN` | Sentry DSN |
| `LAUNCHDARKLY_SDK_KEY` | LaunchDarkly SDK key |

### Misc
| Variable | Description |
|---|---|
| `INTERNAL_VERIFY_TOKEN` | Internal verification token |

## Notes

- All `*_REDIRECT_URI` values must point to the prod domain (`https://bfloat.dev/...`), not `localhost:3000`
- `REVENUECAT_OAUTH_CLIENT_SECRET` is NOT needed in Cloud Run -- the OAuth flow uses PKCE (public client)
- Dev-only vars NOT included: `CLERK_*`, `BROWSERBASE_*`, `NORTHFLANK_*`, `VERCEL_*`, `DEEPSEEK_API_KEY`, `X_API_KEY`, `MINIMAX_API_KEY`, `KIMI_API_KEY`, `ZAI_API_KEY`, `MOONSHOT_API_KEY`, `SENTRY_AUTH_TOKEN`
- Total: 67 variables
