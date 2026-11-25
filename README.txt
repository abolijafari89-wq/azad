FOOTBALLCHI - PITCHAPI COMPREHENSIVE AUDIT

Run this script in PowerShell 5.1 or later. It performs sequential GET-only requests.
The API key is entered as a hidden SecureString and is never written to reports, CSV, JSON or raw responses.

Files:
- SUMMARY.txt: concise findings and representative endpoint probes
- RESULTS.csv: request-by-request status, latency, bytes and API error details
- REPORT.json: machine-readable aggregate report
- LEAGUES.csv: full PitchAPI league catalogue; compare against FootballChi's configured 50 leagues
- SEASON_COVERAGE.csv: latest-season fixtures/results/upcoming counts per league
- FIXTURES_UNIQUE.csv: collected match list and IDs
- RAW_RESPONSES/: raw JSON returned by tested endpoints (may be large)

Test scope:
1. GET /v1/leagues
2. Date listings for UTC yesterday through UTC today + configured DateWindowDays (status=all)
3. GET /v1/leagues/{id}/matches?status=all for every catalogue league, unless -SkipAllLeagueSeasons is supplied
4. Representative finished-match endpoints: summary, events, lineups, team stats, player stats, shots, momentum, H2H, advanced analytics, advanced player analytics, pass network, heatmaps, team and player profiles when IDs are available
5. Upcoming lineups (predicted vs confirmed) when an upcoming fixture is found
6. Live snapshot only if a live-status fixture is found during this audit

Safety:
- Requests are sequential with a delay; no parallel load and no automatic retries.
- If HTTP 429 is returned, the script stops sending further requests.
- No GOAL, Supabase, app, database or user data is modified.
- A 404 ANALYTICS_UNAVAILABLE can be normal for matches without event-feed analytics.

Official docs: https://pitchapi.dev/