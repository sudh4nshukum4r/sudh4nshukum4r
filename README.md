
name: Generate Contribution Snake

on:
  workflow_dispatch:
  schedule:
    - cron: "30 0 * * *"

permissions:
  contents: write

jobs:
  generate:
    runs-on: ubuntu-latest
    timeout-minutes: 10

    steps:
      - name: Generate multicolor snake
        uses: Platane/snk@v3
        with:
          github_user_name: sudh4nshukum4r
          outputs: |
            dist/contribution-snake.svg?color_snake=%2300D9FF&color_dots=%23161B22,%23FF595E,%23FFCA3A,%238AC926,%231982C4

      - name: Publish snake
        uses: peaceiris/actions-gh-pages@v4
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_branch: output
          publish_dir: dist
