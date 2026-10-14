
name: GTA 6 Daily Countdown

on:
  schedule:
    - cron: "17 12 * * *"
  workflow_dispatch:

permissions:
  contents: read

jobs:
  countdown:
    runs-on: ubuntu-latest
    steps:
      - name: Post daily countdown
        env:
          DISCORD_WEBHOOK: ${{ secrets.DISCORD_WEBHOOK }}
        run: |
          python - <<'PY'
          import os, json, urllib.request
          from datetime import datetime, timezone

          release = datetime(2026, 11, 19).date()
          today = datetime.now(timezone.utc).date()
          days = max(0, (release - today).days)

          content = (
              f"🎮 GTA 6 COUNTDOWN\n"
              f"⏳ **{days} days remaining!**\n"
              f"📅 November 19, 2026"
          )

          url = os.environ["DISCORD_WEBHOOK"] + "?wait=true"
          data = json.dumps({"content": content}).encode()
          request = urllib.request.Request(
              url, data=data,
              headers={"Content-Type": "application/json"},
              method="POST"
          )

          with urllib.request.urlopen(request) as response:
              print("Countdown posted:", response.status)
          PY
          
