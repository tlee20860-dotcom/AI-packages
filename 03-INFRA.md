# 基礎設施總包 (INFRA)

> ⚠️ 給 AI 的指示：本包包含體系唯一腳本與各專案呼叫器。AI 必須對比版本，並確認當前對話所屬 Repo 進行修改。

## 🏢 體系層：AI-General（唯一腳本）

### 📄 檔案：apply_patch.py


<!-- === FILE START: .github/scripts/apply_patch.py === -->

## 📄 .github/scripts/apply_patch.py

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

<!-- === FILE END: .github/scripts/apply_patch.py === -->


### 📄 檔案：apply-patch.yml (體系觸發器)


<!-- === FILE START: .github/workflows/apply-patch.yml === -->

## 📄 .github/workflows/apply-patch.yml

```
name: Apply Patch
on:
  push:
    paths:
      - '.github/PATCH.md'

jobs:
  apply:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
        with:
          token: ${{ secrets.CROSS_REPO_PAT }}
      - name: Run Patch Script
        run: python3 .github/scripts/apply_patch.py
      - name: Commit changes
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          git add -A
          git diff --cached --quiet || (
            git commit -m "chore: apply patch" && 
            git pull --rebase origin main && 
            git push
          )
```

<!-- === FILE END: .github/workflows/apply-patch.yml === -->


### 📄 檔案：setup-structure.yml (打包腳本自身)


<!-- === FILE START: .github/workflows/setup-structure.yml === -->

## 📄 .github/workflows/setup-structure.yml

```
# ============================================
# setup-structure.yml — 四倉架構 + 動態共享包（v12.6）
# ============================================

name: Build Shared Packages and Deploy

on:
  push:
    branches: [main, master]
    paths-ignore:
      - 'docs/records/STATE-INDEX.md'
      - '**/*.png'
      - '**/*.jpg'
      - '**/*.jpeg'
      - '**/*.gif'
      - '**/*.svg'
      - '**/*.ico'
  repository_dispatch:
    types: [web-app-family-updated, ai-os-updated, ai-kernel-updated]
  workflow_dispatch:

concurrency:
  group: setup-structure
  cancel-in-progress: true

env:
  PROJECT_REPOS: |
    web-app-family|external/web-app-family|tlee20860-dotcom/WEB-APP-Family|90|PACKAGE-web-app-family.md|W|家庭 webapp
    ai-os|external/ai-os|tlee20860-dotcom/AI-OS|91|PACKAGE-ai-os.md|E|AI-OS 輕量版

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: write

    steps:
      - name: Checkout AI-General
        uses: actions/checkout@v4
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          fetch-depth: 2

      - name: Checkout ai-kernel
        uses: actions/checkout@v4
        with:
          repository: tlee20860-dotcom/ai-kernel
          token: ${{ secrets.CROSS_REPO_PAT }}
          path: ai-kernel

      - name: Checkout AI-packages
        uses: actions/checkout@v4
        with:
          repository: tlee20860-dotcom/AI-packages
          token: ${{ secrets.CROSS_REPO_PAT }}
          path: build-repo

      - name: Checkout project repos
        run: |
          if [ -n "$PROJECT_REPOS" ]; then
            echo "$PROJECT_REPOS" | sed 's/\r//g' | while IFS='|' read -r name path repo pkgno pkg domain display; do
              if [ -z "$repo" ]; then continue; fi
              git clone --depth 1 "https://x-access-token:${{ secrets.CROSS_REPO_PAT }}@github.com/$repo.git" "$path" 2>/dev/null || echo "clone fail"
            done
          fi

      - name: "Step 0: 驗證交付格式（report 模式）"
        run: |
          CHANGED=$(git diff --name-only HEAD~1 HEAD -- '*.md' 2>/dev/null || true)
          export CHANGED_FILES="$CHANGED"
          export VALIDATE_MODE="report"
          if [ -f ".github/scripts/validate_delivery.py" ]; then
            python3 .github/scripts/validate_delivery.py || true
          else
            echo "[SKIP] validate_delivery.py 不存在"
          fi

      - name: "Step 1: 清理舊檔"
        run: |
          mkdir -p /tmp/history
          if [ -f "docs/records/R-01-writers.md" ]; then git rm -f docs/records/R-01-writers.md || true; fi
          if [ -f "docs/records/PROJECT-LIST.md" ]; then git rm -f docs/records/PROJECT-LIST.md || true; fi
          if [ -f "docs/records/FILE-INDEX.md" ]; then git rm -f docs/records/FILE-INDEX.md || true; fi
          if [ -f "docs/records/HISTORY-INDEX.md" ]; then git rm -f docs/records/HISTORY-INDEX.md || true; fi

      - name: "Step 2a: 掃描各 P-01 YAML"
        run: |
          > /tmp/p01-list.txt
          if [ -d "external" ]; then
            for p01 in external/*/docs/projects/*/P-01-index.md; do
              if [ ! -f "$p01" ]; then continue; fi
              name=$(awk '/^---$/{c++; next} c==1 && /^name:/{sub(/^name:[ ]*/, ""); print; exit}' "$p01")
              code=$(awk '/^---$/{c++; next} c==1 && /^code:/{sub(/^code:[ ]*/, ""); print; exit}' "$p01")
              domain=$(awk '/^---$/{c++; next} c==1 && /^domain:/{sub(/^domain:[ ]*/, ""); print; exit}' "$p01")
              pkgno=$(awk '/^---$/{c++; next} c==1 && /^package_no:/{sub(/^package_no:[ ]*/, ""); print; exit}' "$p01")
              repo=$(awk '/^---$/{c++; next} c==1 && /^repo:/{sub(/^repo:[ ]*/, ""); print; exit}' "$p01")
              display=$(awk '/^---$/{c++; next} c==1 && /^display:/{sub(/^display:[ ]*/, ""); print; exit}' "$p01")
              name=${name:-$(basename "$(dirname "$p01")")}
              echo "$display|$code|$domain|$pkgno|$repo" >> /tmp/p01-list.txt
            done
          fi

      - name: "Step 2b: 生成 STATE-INDEX.md"
        run: |
          OUTPUT="docs/records/STATE-INDEX.md"
          TMP_TABLE="/tmp/state-index-files.txt"
          > "$TMP_TABLE"
          
          for REPO_DIR in "." "ai-kernel"; do
            if [ -d "$REPO_DIR/docs" ]; then
              find "$REPO_DIR/docs" -type f -name "*.md" -not -path "*/.git/*" -not -path "*/build-repo/*" | while read -r f; do
                V=$(tail -n 15 "$f" | grep -oE '^V: *[0-9]+' | head -1 | sed 's/^V: *//' || true)
                if [ -z "$V" ]; then continue; fi
                W=$(tail -n 15 "$f" | grep -oE '^W: *[^ ]+' | head -1 | sed 's/^W: *//' || true)
                REPO_NAME="AI-General"
                if [[ "$f" == ai-kernel/* ]]; then REPO_NAME="ai-kernel"; fi
                CLEAN_F=${f#ai-kernel/}
                echo "$REPO_NAME|$CLEAN_F|$V|$W" >> "$TMP_TABLE"
              done
            fi
          done || true

          {
            echo "# STATE-INDEX"
            echo ""
            echo "## 一、專案總覽"
            echo "| 專案顯示名稱 | code | domain | package_no | repo |"
            echo "|---|---|---|---|---|"
            if [ -s "/tmp/p01-list.txt" ]; then cat /tmp/p01-list.txt; fi
            echo ""
            echo "## 二、檔案清單"
            echo "| Repo | 檔案 | Version | Writer |"
            echo "|---|---|---|---|"
            if [ -s "$TMP_TABLE" ]; then sort -t'|' -k3 -nr "$TMP_TABLE" | head -50; fi
            echo ""
            echo "## 三、歷史歸屬"
            echo "| 檔案 | 舊版本 | 舊 Writer | 新版本 | 新 Writer | 取代時間 |"
            echo "|---|---|---|---|---|---|"
            echo ""
            echo "## 四、已廢除檔案"
            echo "| 檔案 | 廢除時間 | 內容去向 |"
            echo "|---|---|---|"
            echo "| R-01-writers.md | 2026-10-17 | 內容分散至 C-02、P-01 YAML、STATE-INDEX |"
          } > "$OUTPUT"

      - name: "Step 3: 生成 A 包 (A00.md)"
        run: |
          OUTPUT="build-repo/A00.md"
          echo "# A 包：啟動引導與角色定義" > "$OUTPUT"
          echo "" >> "$OUTPUT"
          echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" >> "$OUTPUT"
          echo "" >> "$OUTPUT"
          if [ -f "ai-kernel/docs/a-guide/A-00-guide.md" ]; then 
            bash .github/workflows/append_file.sh "ai-kernel/docs/a-guide/A-00-guide.md" "$OUTPUT" || true
          fi

      - name: "Step 3.1: 生成 R 包 (R00.md)"
        run: |
          OUTPUT="build-repo/R00.md"
          echo "# R 包：規則總覽" > "$OUTPUT"
          echo "" >> "$OUTPUT"
          echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" >> "$OUTPUT"
          echo "" >> "$OUTPUT"
          
          # 依照訂立的合併順序：R00 -> R03 -> R02 -> R04 -> R01
          for f in R-00-rules R-03-terms R-02-patterns R-04-learning R-01-mapping; do
            FILE_PATH="ai-kernel/docs/r-rules/${f}.md"
            if [ -f "$FILE_PATH" ]; then 
              bash .github/workflows/append_file.sh "$FILE_PATH" "$OUTPUT" || true
            fi
          done

      - name: "Step 3.2: 生成 U 包 (U00.md)"
        run: |
          OUTPUT="build-repo/U00.md"
          echo "# U 包：使用者通用描述" > "$OUTPUT"
          echo "" >> "$OUTPUT"
          echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" >> "$OUTPUT"
          echo "" >> "$OUTPUT"
          if [ -f "ai-kernel/docs/u-profile/U-00-profile.md" ]; then 
            bash .github/workflows/append_file.sh "ai-kernel/docs/u-profile/U-00-profile.md" "$OUTPUT" || true
          fi

      - name: "Step 4: 生成 S00 包 (S00-ai-system.md)"
        run: |
          OUTPUT="build-repo/S00-ai-system.md"
          echo "# AI 對話系統 - 體系狀態包" > "$OUTPUT"
          echo "" >> "$OUTPUT"
          echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" >> "$OUTPUT"
          echo "" >> "$OUTPUT"
          
          echo "---" >> "$OUTPUT"
          echo "⚠️ **【給 AI 的強制前置檢查】**" >> "$OUTPUT"
          echo "請立刻確認：你是否已經讀取 \`A00.md\`（啟動包）？" >> "$OUTPUT"
          echo "1. 如果「否」或「不確定」，請立刻停止閱讀本檔，先去讀取 \`A00.md\`，載入引導協議與防呆規則。" >> "$OUTPUT"
          echo "2. 如果「是」，請依據 \`A00.md\` 輸出載入確認。" >> "$OUTPUT"
          echo "---" >> "$OUTPUT"
          echo "" >> "$OUTPUT"

          for f in docs/records/SYSTEM-HANDOFF.md docs/records/PROGRESS.md docs/records/STATE-INDEX.md docs/records/SYSTEM-CONCLUSION.md; do
            if [ -f "$f" ]; then bash .github/workflows/append_file.sh "$f" "$OUTPUT" || true; fi
          done

      - name: "Step 5: 生成專案包"
        run: |
          > /tmp/projects.txt
          if [ -n "$PROJECT_REPOS" ]; then echo "$PROJECT_REPOS" | sed 's/\r//g' >> /tmp/projects.txt; fi

          while IFS='|' read -r name path repo pkgno pkg domain display; do
            if [ -z "$name" ] || [ -z "$pkg" ]; then continue; fi
            SHORT="${pkg#PACKAGE-}"
            OUTPUT="build-repo/${pkgno}-${SHORT}"

            if [ "$name" = "ai-os" ]; then
              echo "# AI-OS 輕量版引導系統" > "$OUTPUT"
              echo "" >> "$OUTPUT"
              echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" >> "$OUTPUT"
              echo "" >> "$OUTPUT"
              for f in "$path"/E-*.md; do
                if [ -f "$f" ]; then 
                  bash .github/workflows/append_file.sh "$f" "$OUTPUT" || true
                fi
              done
            else
              echo "# 專案包：${name}" > "$OUTPUT"
              echo "" >> "$OUTPUT"
              
              proj_docs_dir="$path/docs/projects/${name}"
              if [ -d "$proj_docs_dir" ]; then
                for prefix in P-01 P-02 P-05 P-06 P-07 P-08 P-09 P-12 P-13 P-98 P-99; do
                  for f in ${proj_docs_dir}/${prefix}-*.md; do
                    if [ -f "$f" ]; then bash .github/workflows/append_file.sh "$f" "$OUTPUT" || true; fi
                  done
                done
              fi

              if [ -d "$path" ]; then
                (
                  cd "$path" || exit 0
                  find . -type f \( -name "*.js" -o -name "*.html" -o -name "*.css" \) -not -path "./.git/*" -not -path "./docs/*" -not -path "./.github/*" | sort > /tmp/code_files.txt || true
                  
                  if [ -s /tmp/code_files.txt ]; then
                    {
                      echo ""
                      echo "## 程式碼檔案樹（僅列路徑，不讀內容）"
                      echo ""
                      echo "總檔數：$(wc -l < /tmp/code_files.txt)"
                      echo ""
                      sed 's|^\./||' /tmp/code_files.txt
                    } >> "../../$OUTPUT"
                  fi
                ) || true
              fi
            fi
          done < /tmp/projects.txt || true

      - name: "Step 6: 生成基礎設施總包 (03-INFRA.md)"
        run: |
          OUTPUT="build-repo/03-INFRA.md"
          echo "# 基礎設施總包 (INFRA)" > "$OUTPUT"
          echo "" >> "$OUTPUT"
          echo "> ⚠️ 給 AI 的指示：本包包含體系唯一腳本與各專案呼叫器。AI 必須對比版本，並確認當前對話所屬 Repo 進行修改。" >> "$OUTPUT"
          echo "" >> "$OUTPUT"

          echo "## 🏢 體系層：AI-General（唯一腳本）" >> "$OUTPUT"
          echo "" >> "$OUTPUT"
          echo "### 📄 檔案：apply_patch.py" >> "$OUTPUT"
          echo "" >> "$OUTPUT"
          if [ -f ".github/scripts/apply_patch.py" ]; then bash .github/workflows/append_file.sh ".github/scripts/apply_patch.py" "$OUTPUT" || true; fi
          
          echo "" >> "$OUTPUT"
          echo "### 📄 檔案：apply-patch.yml (體系觸發器)" >> "$OUTPUT"
          echo "" >> "$OUTPUT"
          if [ -f ".github/workflows/apply-patch.yml" ]; then bash .github/workflows/append_file.sh ".github/workflows/apply-patch.yml" "$OUTPUT" || true; fi

          echo "" >> "$OUTPUT"
          echo "### 📄 檔案：setup-structure.yml (打包腳本自身)" >> "$OUTPUT"
          echo "" >> "$OUTPUT"
          if [ -f ".github/workflows/setup-structure.yml" ]; then bash .github/workflows/append_file.sh ".github/workflows/setup-structure.yml" "$OUTPUT" || true; fi

          while IFS='|' read -r name path repo pkgno pkg domain display; do
            if [ -z "$name" ] || [ ! -d "$path" ]; then continue; fi
            if [ -d "$path/.github/workflows" ]; then
              echo "" >> "$OUTPUT"
              echo "## 🏢 專案層：${repo} (${name}) 的呼叫器" >> "$OUTPUT"
              echo "" >> "$OUTPUT"
              (
                cd "$path" || exit 0
                find .github/workflows -type f -name "*.yml" | sort > /tmp/infra_files.txt || true
                if [ -s /tmp/infra_files.txt ]; then 
                  while read -r file; do 
                    echo "### 📄 檔案：${file}" >> "../../$OUTPUT"
                    echo "" >> "../../$OUTPUT"
                    bash "../../.github/workflows/append_file.sh" "$file" "../../$OUTPUT" || true
                    echo "" >> "../../$OUTPUT"
                  done < /tmp/infra_files.txt
                fi
              ) || true
            fi
          done < /tmp/projects.txt || true

      - name: "Step 7: 上傳共享包"
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          
          git add docs/records/STATE-INDEX.md
          if git diff --cached --quiet; then
            echo "No changes to commit"
          else
            git commit -m "chore: auto-update STATE-INDEX [skip ci]"
            git push || echo "push failed"
          fi

          cd build-repo
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          git add -A
          if git diff --cached --quiet; then
            echo "No changes to push"
            exit 0
          fi
          git commit -m "chore: auto-build packages [skip ci]"
          git push

# ═══ EOF ═══
# F: AI-General/.github/workflows/setup-structure.yml
# V: 1792348500
# W: ai-system-20261019-010#055
# ═══ EOF ═══
```

<!-- === FILE END: .github/workflows/setup-structure.yml === -->


## 🏢 專案層：tlee20860-dotcom/WEB-APP-Family (web-app-family) 的呼叫器

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
```

<!-- === FILE END: .github/workflows/apply-patch.yml === -->


### 📄 檔案：.github/workflows/notify.yml


<!-- === FILE START: .github/workflows/notify.yml === -->

## 📄 .github/workflows/notify.yml

```
# ============================================
# notify.yml — WEB-APP-Family 變更時通知 AI-General
# 位置：WEB-APP-Family/.github/workflows/notify.yml
# ============================================

name: Notify AI-General

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
      - name: Trigger AI-General build
        run: |
          curl -X POST \
            -H "Accept: application/vnd.github.v3+json" \
            -H "Authorization: token ${{ secrets.CROSS_REPO_PAT }}" \
            https://api.github.com/repos/tlee20860-dotcom/AI-General/dispatches \
            -d '{"event_type":"web-app-family-updated"}'
          echo "已通知 AI-General 重新建置"

# ═══ EOF ═══
# F: WEB-APP-Family/.github/workflows/notify.yml
# V: 1791936000
# B: B34-5
# W: ai-system-20261016-003#135
# ═══ EOF ═══
```

<!-- === FILE END: .github/workflows/notify.yml === -->



## 🏢 專案層：tlee20860-dotcom/AI-OS (ai-os) 的呼叫器

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
# notify.yml — 手動通知 AI-General 重新打包
# ============================================

name: Manual Notify AI-General

on:
  workflow_dispatch:

jobs:
  notify:
    runs-on: ubuntu-latest
    steps:
      - name: Trigger AI-General build
        run: |
          curl -X POST \
            -H "Accept: application/vnd.github.v3+json" \
            -H "Authorization: token ${{ secrets.CROSS_REPO_PAT }}" \
            https://api.github.com/repos/tlee20860-dotcom/AI-General/dispatches \
            -d '{"event_type":"ai-os-updated"}'
          echo "已通知 AI-General 重新建置 91-ai-os.md"

# ═══ EOF ═══
# F: AI-OS/.github/workflows/notify.yml
# V: 1792316000
# W: ai-system-20261018-008#092
# ═══ EOF ═══
```

<!-- === FILE END: .github/workflows/notify.yml === -->


