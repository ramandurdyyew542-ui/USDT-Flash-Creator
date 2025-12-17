# USDT-Flash-Creator AI Agent Instructions

## Project Overview
This is a Python-based automation tool for managing API keys (Twitter, OpenAI) and generating/tweeting ETF-related content. It fetches exposed keys from GitHub, validates them, and uses them for automated tweeting with AI-generated content.

## Architecture
- **Key Management**: SQLite database (`keys.db`) stores encrypted API credentials with validation timestamps and rate limits
- **Data Flow**: GitHub search → Key extraction/validation → Encryption → Database storage → Tweet generation via OpenAI → Posting
- **Async Processing**: Uses aiohttp for concurrent Twitter API validation with rate limiting (180 calls/15min)
- **Security**: PBKDF2-derived Fernet encryption for sensitive data, environment variables for runtime secrets

## Core Components
- `fetch_keys.py`: GitHub code search for exposed keys using PyGithub
- `extract_keys.py`: File-based key extraction with Twitter API validation and encryption
- `tweet.py`: Basic tweeting utility using Tweepy
- `log/etf_tweet.py`: AI-powered tweet generation and posting workflow

## Developer Workflows
- **Run Key Fetcher**: `python fetch_keys.py` (requires `GITCLASIC_TOKEN` or `GITFINEPAT_TOKEN`)
- **Extract & Validate Keys**: `python extract_keys.py` (set `INPUT_FILE`, `ENCRYPTION_KEY`, `OUTPUT_FILE` env vars)
- **Generate Tweet**: `python log/etf_tweet.py` (needs OpenAI API key and DB with valid Twitter keys)
- **CI/CD**: GitHub Actions workflow runs every 6 hours: key fetching → tweeting → monitoring

## Conventions & Patterns
- **Database Schema**: `twitter_keys` table with validation metadata; `api_keys` for mixed types
- **Logging**: Multi-handler setup (file, stream, rotating) with process/thread context
- **Error Handling**: Context managers for DB ops, try/except with detailed logging
- **Key Validation**: OAuth 1.0a signature generation for Twitter API verification
- **Environment Config**: All secrets via `os.getenv()`, no hardcoded values
- **File Structure**: Root-level scripts, `log/` for specialized modules, `data/` for DB in CI

## Dependencies & Setup
- Install: `pip install -r requirements.txt`
- Key libraries: `tweepy` (Twitter), `openai` (AI generation), `cryptography` (encryption), `ratelimit` (API throttling)
- Database: Auto-created SQLite files, no migrations needed

## Integration Points
- **GitHub API**: Code search for credential leaks
- **Twitter API**: Key validation and posting with OAuth
- **OpenAI API**: Tweet content generation
- **GitHub Actions**: Scheduled automation with artifact logging

## Common Patterns
- Regex patterns for key extraction: `TWITTER_CONSUMER_KEY="([^"]+)"` etc.
- Dataclass for structured key objects with validation
- Async gather for batch API validations
- JSON output with encrypted payloads for persistence</content>
<filePath>c:\Users\RZF\Documents\GitHub\USDT-Flash-Creator\.github\copilot-instructions.md