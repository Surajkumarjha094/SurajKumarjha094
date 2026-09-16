# Path: .github/workflows/profile-3d.yml
name: Generate 3D Contribution Graph

on:
  schedule:
    - cron: "0 18 * * *"    # roz ek baar
  workflow_dispatch:
  push:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: write

    steps:
      - uses: actions/checkout@v4

      - name: Generate 3D profile
        uses: yoshi389111/github-profile-3d-contrib@0.7.1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          USERNAME: ${{ github.repository_owner }}

      - name: Commit generated SVGs
        run: |
          git config user.name  "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          git add -A profile-3d-contrib
          git commit -m "chore: update 3D contribution graph" || echo "no changes"
          git push
