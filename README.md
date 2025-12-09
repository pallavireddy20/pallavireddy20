name: Daily Streak Commit

on:
  schedule:
    - cron: '0 0 * * *'   # UTC 00:00 daily (adjust if you want different time)
  workflow_dispatch:      # allows manual run from Actions tab

jobs:
  commit:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
        with:
          persist-credentials: true

      - name: Make a timestamp commit
        run: |
          echo "Updated on $(date -u)" >> .github/streak.txt
          git config user.name "github-actions[bot]"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git add .github/streak.txt
          git commit -m "chore(streak): update $(date -u +'%Y-%m-%d %H:%M:%S') UTC" || echo "No changes to commit"
          git push
