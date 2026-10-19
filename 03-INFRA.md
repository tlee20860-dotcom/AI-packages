# 基礎設施總包 (INFRA)

> ⚠️ 給 AI 的指示：本包包含體系唯一腳本與各專案呼叫器。

## 🏢 體系層：AI-General（唯一腳本）

### 📄 檔案：apply_patch.py

<!-- === FILE START: AI-General/.github/scripts/apply_patch.py === -->

## 📄 AI-General/.github/scripts/apply_patch.py

```
import sys
import os
import re

patch_file = ".github/PATCH.md"

if not os.path.exists(patch_file):
    print("PATCH.md not found")
    sys.exit(0)

with open(patch_file, "r", encoding="utf-8") as f:
    text = f.read()

text = text.replace('\r\n', '\n')

if not re.search(r"## (?:FILE|修改目標|修改目标):", text):
    print("PATCH.md is empty or invalid format. Exiting safely.")
    sys.exit(0)

pattern = re.compile(r"## (?:FILE|修改目標|修改目标):\s*`?([^`\n]+)`?\n(.*?)(?=## (?:FILE|修改目標|修改目标):|\Z)", re.S)
matches = pattern.findall(text)

planned = []
errors = []

for rel_path, content_block in matches:
    rel_path = rel_path.strip()
    if not os.path.exists(rel_path):
        errors.append(f"{rel_path}: File not found")
        continue
        
    with open(rel_path, "r", encoding="utf-8") as f:
        content = f.read()
        
    content = content.replace('\r\n', '\n')
    new_content = content
    
    pairs = re.findall(r"<<<<<<< (OLD|REGEX|REPLACE_ALL)\s*\n(.*?)\n=======\s*\n(.*?)\n>>>>>>> NEW", content_block, re.S)
    if not pairs:
        errors.append(f"{rel_path}: No OLD/REGEX/REPLACE_ALL pairs found")
        continue
        
    for mode, old, new in pairs:
        if mode == "REGEX":
            new_content, count = re.subn(old, new, new_content, count=1)
            if count == 1:
                print(f"[OK] {rel_path}: Regex replaced 1 instance")
            elif count == 0:
                if re.search(new, new_content):
                    print(f"[SKIP] {rel_path}: Already applied")
                else:
                    errors.append(f"{rel_path}: Regex OLD not found, NEW not present")
        elif mode == "REPLACE_ALL":
            count = new_content.count(old)
            if count > 0:
                new_content = new_content.replace(old, new)
                print(f"[OK] {rel_path}: Replaced ALL {count} instances")
            elif count == 0:
                if new in new_content:
                    print(f"[SKIP] {rel_path}: Already applied")
                else:
                    errors.append(f"{rel_path}: REPLACE_ALL OLD not found, NEW not present")
        else:
            count = new_content.count(old)
            if count == 1:
                new_content = new_content.replace(old, new, 1)
                print(f"[OK] {rel_path}: Replaced 1 instance")
            elif count == 0:
                if new in new_content:
                    print(f"[SKIP] {rel_path}: Already applied")
                else:
                    errors.append(f"{rel_path}: OLD not found, NEW not present")
            else:
                errors.append(f"{rel_path}: OLD found {count} times (not unique)")
            
    if new_content != content:
        planned.append((rel_path, new_content))

if errors:
    for e in errors:
        print(f"[ERROR] {e}")
    sys.exit(1)

for rel_path, new_content in planned:
    with open(rel_path, "w", encoding="utf-8", newline='\n') as f:
        f.write(new_content)
    print(f"[WRITE] {rel_path}")

with open(patch_file, "w", encoding="utf-8") as f:
    f.write("# 請在此貼上新的替換指令\n")
print("[RESET] PATCH.md reset")
```

<!-- === FILE END: AI-General/.github/scripts/apply_patch.py === -->


## 🏢 專案層：tlee20860-dotcom/AI-WEB-APP-Family 的呼叫器

### 📄 檔案：.github/workflows/apply-patch.yml


<!-- === FILE START: .github/workflows/apply-patch.yml === -->

## 📄 .github/workflows/apply-patch.yml

```
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
# F: AI-WEB-APP-Family/.github/workflows/apply-patch.yml
# V: 1792358000
# W: ai-system-20261019-010#088
# ═══ EOF ═══
```

<!-- === FILE END: .github/workflows/apply-patch.yml === -->


### 📄 檔案：.github/workflows/notify.yml


<!-- === FILE START: .github/workflows/notify.yml === -->

## 📄 .github/workflows/notify.yml

```
# ============================================
# notify.yml — 專案變更時通知 AI-infra
# ============================================
name: Notify AI-infra

on:
  push:
    branches: [main, master]
    paths-ignore:
      - '**/*.png'
      - '**/*.jpg'
      - '**/*.jpeg'
      - '**/*.gif'
      - '**/*.svg'
      - '**/*.ico'
      - '.gitignore'
      - 'LICENSE'

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
# F: AI-WEB-APP-Family/.github/workflows/notify.yml
# V: 1792357000
# W: ai-system-20261019-010#085
# ═══ EOF ═══
```

<!-- === FILE END: .github/workflows/notify.yml === -->



## 🏢 專案層：tlee20860-dotcom/AI-OS 的呼叫器

### 📄 檔案：.github/workflows/apply-patch.yml


<!-- === FILE START: .github/workflows/apply-patch.yml === -->

## 📄 .github/workflows/apply-patch.yml

```
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
          curl -sL https://raw.githubusercontent.com/tlee20860-dotcom/AI-General/main/.github/scripts/apply_patch.py -o /tmp/apply_patch.py

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
# V: 1792315000
# W: ai-system-20261018-008#091
# ═══ EOF ═══
```

<!-- === FILE END: .github/workflows/apply-patch.yml === -->


### 📄 檔案：.github/workflows/notify.yml


<!-- === FILE START: .github/workflows/notify.yml === -->

## 📄 .github/workflows/notify.yml

```
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
# ═══ EOF ═══
```

<!-- === FILE END: .github/workflows/notify.yml === -->


