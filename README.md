# Đặt file này tại: .github/workflows/snake.yml
# trong repo xuansang2005/xuansang2005 (repo trùng tên username)
#
# Sau khi push:
# 1. Vào tab Actions → chọn "Generate Snake" → Run workflow (chạy tay lần đầu)
# 2. Action sẽ tạo branch `output` chứa snake.svg và snake-dark.svg
# 3. README sẽ tự hiển thị được ảnh

name: Generate Snake

on:
  schedule:
    - cron: "0 0 * * *"   # chạy mỗi ngày lúc 00:00 UTC
  workflow_dispatch:        # cho phép chạy thủ công
  push:
    branches:
      - main

permissions:
  contents: write

jobs:
  generate:
    runs-on: ubuntu-latest
    timeout-minutes: 10
    steps:
      - name: Generate snake SVGs
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/snake.svg
            dist/snake-dark.svg?palette=github-dark

      - name: Push to output branch
        uses: crazy-max/ghaction-github-pages@v4
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
