> 體系版本：SV09
> 生成時間：2026-10-29 23:39:48 (HKT)

# 專案全程式碼包：AI-OS


<!-- === FILE START: ./.github/workflows/apply-patch.yml === -->

## 📄 ./.github/workflows/apply-patch.yml

```yml
name: Apply Patch (Project)
on:
  workflow_dispatch:
  push:
    paths:
      - '.github/PATCH.md'

permissions:
  contents: write

jobs:
  apply:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Python
        uses: actions/setup-python@v5
        with:
          python-version: "3.x"

      - name: Download central script
        run: |
          curl -sL https://raw.githubusercontent.com/tlee20860-dotcom/AI-infra/main/.github/scripts/apply_patch.py -o /tmp/apply_patch.py

      - name: Run Patch
        run: python3 /tmp/apply_patch.py

      - name: Commit and push
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "41898282+github-actions[bot]@users.noreply.github.com"
          git add -A
          git diff --cached --quiet && exit 0
          git commit -m "Apply PATCH.md [skip ci]"
          git push

# ═══ EOF ═══
# F: AI-OS/.github/workflows/apply-patch.yml
# V: 1792358000
# W: ai-system-20261019-010#088
# ═══ EOF ═══```

<!-- === FILE END: ./.github/workflows/apply-patch.yml === -->

<!-- === FILE START: ./.github/workflows/notify.yml === -->

## 📄 ./.github/workflows/notify.yml

```yml
# ============================================
# notify.yml — 專案變更時通知 AI-infra
# ============================================
name: Notify AI-infra

on:
  push:
    branches: [main, master]
  workflow_dispatch: # 保留手動觸發功能

jobs:
  notify:
    runs-on: ubuntu-latest
    steps:
      - name: Trigger AI-infra build
        run: |
          curl -X POST \
            -H "Accept: application/vnd.github.v3+json" \
            -H "Authorization: token ${{ secrets.CROSS_REPO_PAT }}" \
            https://api.github.com/repos/tlee20860-dotcom/AI-infra/dispatches \
            -d '{"event_type":"project-updated"}'
          echo "已通知 AI-infra 重新建置"

# ═══ EOF ═══
# F: AI-OS/.github/workflows/notify.yml
# V: 1792357000
# W: ai-system-20261019-010#085
# ═══ EOF ═══```

<!-- === FILE END: ./.github/workflows/notify.yml === -->

<!-- === FILE START: ./meta.json === -->

## 📄 ./meta.json

```json
{
  "domain": "E",
  "pkg_no": "91",
  "code": "ai-os",
  "display": "AI-OS 系統"
}```

<!-- === FILE END: ./meta.json === -->
