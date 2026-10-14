
name: GTA 6 Daily Countdown

on:
  schedule:
    - cron: "0 12 * * *"
  workflow_dispatch:

permissions:
  contents: read

jobs:
  countdown:
    runs-on: ubuntu-latest
    steps:
      - name: Update countdown
        env:
          DISCORD_WEBHOOK: ${{ secrets.DISCORD_WEBHOOK }}
        run: |
          python - <<'PY'
          import os, json, urllib.request
          from datetime import datetime, timezone

          release = datetime(2026, 11, 19, tzinfo=timezone.utc)
          now = datetime.now(timezone.utc)
          days = max(0, (release.date() - now.date()).days)

          message = {
              "content": f"🎮 **GTA 6 COUNTDOWN**\n⏳ **{days} days remaining!**\n📅 November 19, 2026"
          }

          req = urllib.request.Request(
              os.environ["DISCORD_WEBHOOK"],
              data=json.dumps(message).encode(),
              headers={"Content-Type": "application/json"},
              method="POST"
          )
          with urllib.request.urlopen(req) as response:
              print("Discord message posted:", response.status)
          PY
          
