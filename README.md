# NextBank-FastCash LMS — Change Claiming System

Internal financial system for NextBank FastCash loan management and change claiming operations.

**Live URL:** https://nbccs.fcash.com.ph/

## Setup

### Prerequisites
- Supabase project (PostgreSQL backend)
- GitHub Pages (static hosting)
- Cloudflare (DNS + security headers)

### Environment

This app uses Supabase directly from the browser. The Supabase anon key is embedded in `index.html`. Ensure:

1. Supabase Row-Level Security (RLS) is **enabled on all tables**
2. The anon key has minimal permissions (SELECT only where appropriate)
3. The service role key is **never** placed in `index.html` or any client file

### GitHub Secrets Required

| Secret | Description |
|--------|-------------|
| `SUPABASE_URL` | Your Supabase project URL (e.g. `https://xxxx.supabase.co`) |
| `SUPABASE_SERVICE_KEY` | Service role key — for backup workflow only, never in client code |

### Backup System

Backups run daily via GitHub Actions (`.github/workflows/supabase-backup.yml`).

- CSV exports are uploaded to **Supabase Storage** in the private `db-backups` bucket
- Only record counts (no raw data) are committed to this repository as `BACKUP_SUMMARY.txt`
- Backups are retained for 90 days in Supabase Storage

To download a backup: Supabase Dashboard → Storage → db-backups → select date folder.

## Security

- This repository must remain **private**
- Never commit `.env` files, CSV exports, or API keys
- Rotate the Supabase anon key immediately if it is ever exposed publicly
- All user accounts must have MFA enabled (Supabase Auth → TOTP)
