# 基礎設施總包 (INFRA)

> 生成時間：2026-10-19 22:42:32 (HKT)
> ⚠️ 給 AI 的指示：本包包含體系唯一腳本與各專案呼叫器。

## 🏢 體系層：AI-infra（唯一腳本）

### 📄 檔案：apply_patch.py

<!-- === FILE START: .github/scripts/apply_patch.py === -->

## 📄 .github/scripts/apply_patch.py

```py
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
print("[RESET] PATCH.md reset")```

<!-- === FILE END: .github/scripts/apply_patch.py === -->

### 📄 檔案：validate_delivery.py

<!-- === FILE START: .github/scripts/validate_delivery.py === -->

## 📄 .github/scripts/validate_delivery.py

```py
#!/usr/bin/env python3
"""交付驗證腳本（機器可驗的規則）"""

import os
import re
import sys

MODE = os.environ.get("VALIDATE_MODE", "report").strip().lower()
CHANGED = os.environ.get("CHANGED_FILES", "").strip()

def iter_targets():
    if CHANGED:
        for line in CHANGED.splitlines():
            path = line.strip()
            if path and path.endswith(".md") and os.path.exists(path):
                yield path
        return
    for root, _dirs, files in os.walk("docs"):
        for name in files:
            if name.endswith(".md"):
                yield os.path.join(root, name)

def check_eof(content):
    tail = content[-800:]
    m = re.search(r"F:\s*(\S+)\s*\nV:\s*(\d+)\s*\nW:\s*(\S+)", tail)
    if not m:
        return "EOF 缺 F/V/W 三行"
    _f, v, _w = m.groups()
    if len(v) != 10:
        return f"V 不是 10 位 Unix epoch（{v}）"
    return None

def check_inner_backticks(path, content):
    if not path.endswith(".md"):
        return None
    if "```" in content:
        return "md 內含三反引號（F-004）"
    return None

def check_head(content):
    for line in content.splitlines()[:5]:
        s = line.strip()
        if not s:
            continue
        if s.startswith("#"):
            return None
        return f"檔案開頭不是 # 標題（首行為：{s[:40]}）"
    return "檔案為空或前 5 行皆空白"

def validate(path, content):
    problems = []
    for label, fn in (
        ("EOF", lambda c: check_eof(c)),
        ("內層反引號", lambda c: check_inner_backticks(path, c)),
        ("開頭標題", lambda c: check_head(c)),
    ):
        if label == "內層反引號" and not path.endswith(".md"):
            continue
        err = fn(content)
        if err:
            problems.append(err)
    return problems

def main():
    total = 0
    bad = 0
    for path in iter_targets():
        total += 1
        try:
            with open(path, "r", encoding="utf-8") as fh:
                content = fh.read()
        except Exception as exc:
            print(f"[ERROR] {path}: 讀取失敗（{exc}）")
            bad += 1
            continue
        for msg in validate(path, content):
            print(f"[ERROR] {path}: {msg}")
            bad += 1

    print(f"[SUMMARY] 檢查 {total} 檔，問題 {bad} 項（模式：{MODE}）")
    if bad and MODE == "strict":
        sys.exit(1)
    sys.exit(0)

if __name__ == "__main__":
    main()```

<!-- === FILE END: .github/scripts/validate_delivery.py === -->

### 📄 檔案：setup-structure.yml (infra)

<!-- === FILE START: .github/workflows/setup-structure.yml === -->

## 📄 .github/workflows/setup-structure.yml

```yml
# ============================================
# setup-structure.yml — 動態共享包打包腳本 (v15.0)
# 用途：修正路徑錯誤，無差別暴力抓取 workflows
# ============================================

name: Build Shared Packages

on:
  workflow_dispatch:
  repository_dispatch:
    types: [project-updated]
  push:
    branches: [main, master]
    paths:
      - '.github/workflows/setup-structure.yml'
      - '.github/scripts/**'
      - 'config/repos.json'

concurrency:
  group: build-packages
  cancel-in-progress: true

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: write

    steps:
      - name: Checkout AI-infra
        uses: actions/checkout@v4
        with:
          token: ${{ secrets.CROSS_REPO_PAT }}

      - name: Read Repos Config
        id: config
        run: |
          echo "Reading repos.json..."
          cat config/repos.json
          
          KERNEL=$(jq -r '.kernel' config/repos.json)
          GENERAL=$(jq -r '.general' config/repos.json)
          PACKAGES=$(jq -r '.packages' config/repos.json)
          
          echo "KERNEL_REPO=$KERNEL" >> $GITHUB_ENV
          echo "GENERAL_REPO=$GENERAL" >> $GITHUB_ENV
          echo "PACKAGES_REPO=$PACKAGES" >> $GITHUB_ENV
          
          jq -c '.projects' config/repos.json > /tmp/projects.json

      - name: Checkout ai-kernel (git clone main)
        run: |
          git clone --depth 1 -b main "https://x-access-token:${{ secrets.CROSS_REPO_PAT }}@github.com/${{ env.KERNEL_REPO }}.git" ai-kernel || \
          git clone --depth 1 -b master "https://x-access-token:${{ secrets.CROSS_REPO_PAT }}@github.com/${{ env.KERNEL_REPO }}.git" ai-kernel

      - name: Checkout AI-General (git clone main)
        run: |
          git clone --depth 1 -b main "https://x-access-token:${{ secrets.CROSS_REPO_PAT }}@github.com/${{ env.GENERAL_REPO }}.git" AI-General || \
          git clone --depth 1 -b master "https://x-access-token:${{ secrets.CROSS_REPO_PAT }}@github.com/${{ env.GENERAL_REPO }}.git" AI-General

      - name: Checkout AI-packages
        uses: actions/checkout@v4
        with:
          repository: ${{ env.PACKAGES_REPO }}
          token: ${{ secrets.CROSS_REPO_PAT }}
          path: build-repo

      - name: Checkout project repos
        run: |
          jq -c '.[]' /tmp/projects.json | while read -r row; do
            REPO=$(echo "$row" | jq -r '.repo')
            SAFE_NAME=$(echo "$REPO" | awk -F/ '{print $2}')
            git clone --depth 1 -b main "https://x-access-token:${{ secrets.CROSS_REPO_PAT }}@github.com/$REPO.git" "external/$SAFE_NAME" 2>/dev/null || \
            git clone --depth 1 -b master "https://x-access-token:${{ secrets.CROSS_REPO_PAT }}@github.com/$REPO.git" "external/$SAFE_NAME" 2>/dev/null || echo "clone fail: $REPO"
          done

      - name: "Step 1: 清理舊共享包 (清道夫)"
        run: |
          find build-repo -maxdepth 1 -name "*.md" -type f -delete
          echo "舊共享包已清空。"

      - name: "Step 1.5: 生成 STATE-INDEX.md"
        run: |
          OUTPUT="AI-General/docs/records/STATE-INDEX.md"
          mkdir -p "AI-General/docs/records"
          TMP_TABLE="/tmp/state-index-files.txt"
          > "$TMP_TABLE"
          
          for REPO_DIR in "AI-General" "ai-kernel"; do
            if [ -d "$REPO_DIR/docs" ]; then
              find "$REPO_DIR/docs" -type f -name "*.md" -not -path "*/.git/*" | while read -r f; do
                V=$(tail -n 15 "$f" | grep -oE 'V: *[0-9]+' | head -1 | sed 's/V: *//' || true)
                if [ -z "$V" ]; then continue; fi
                W=$(tail -n 15 "$f" | grep -oE 'W: *[^ ]+' | head -1 | sed 's/W: *//' || true)
                
                REPO_NAME="AI-General"
                CLEAN_F=${f#AI-General/}
                if [[ "$f" == ai-kernel/* ]]; then 
                  REPO_NAME="ai-kernel"
                  CLEAN_F=${f#ai-kernel/}
                fi
                
                echo "$REPO_NAME|$CLEAN_F|$V|$W" >> "$TMP_TABLE"
              done
            fi
          done || true

          {
            echo "# STATE-INDEX"
            echo ""
            echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)"
            echo ""
            echo "## 一、專案總覽"
            echo "| 專案顯示名稱 | code | domain | package_no | repo |"
            echo "|---|---|---|---|---|"
            jq -r '.[] | "| \(.display // "-") | \(.code // "-") | \(.domain) | \(.pkg_no) | \(.repo) |"' /tmp/projects.json
            echo ""
            echo "## 二、檔案清單"
            echo "| Repo | 檔案 | Version | Writer |"
            echo "|---|---|---|---|"
            if [ -s "$TMP_TABLE" ]; then sort -t'|' -k3 -nr "$TMP_TABLE" | head -50; fi
            echo ""
            echo "/* ═══ EOF ═══"
            echo "F: AI-General/docs/records/STATE-INDEX.md"
            echo "V: $(date +%s)"
            echo "W: github-actions[bot]"
            echo "═══ */"
          } > "$OUTPUT"

      - name: "Step 2: 生成 A 包 (A00.md)"
        run: |
          OUTPUT="build-repo/A00.md"
          echo "# A 包：啟動引導與角色定義" > "$OUTPUT"
          echo "" >> "$OUTPUT"
          echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" >> "$OUTPUT"
          echo "" >> "$OUTPUT"
          if [ -f "ai-kernel/docs/a-guide/A-00-guide.md" ]; then 
            bash .github/workflows/append_file.sh "ai-kernel/docs/a-guide/A-00-guide.md" "$OUTPUT" || true
          fi

      - name: "Step 3: 生成 R 包 (R00.md)"
        run: |
          OUTPUT="build-repo/R00.md"
          echo "# R 包：規則總覽" > "$OUTPUT"
          echo "" >> "$OUTPUT"
          echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" >> "$OUTPUT"
          echo "" >> "$OUTPUT"
          
          for f in R-00-rules R-03-terms R-02-patterns R-04-learning R-01-mapping; do
            FILE_PATH="ai-kernel/docs/r-rules/${f}.md"
            if [ -f "$FILE_PATH" ]; then 
              bash .github/workflows/append_file.sh "$FILE_PATH" "$OUTPUT" || true
            fi
          done

      - name: "Step 4: 生成 U 包 (U00.md)"
        run: |
          OUTPUT="build-repo/U00.md"
          echo "# U 包：使用者通用描述" > "$OUTPUT"
          echo "" >> "$OUTPUT"
          echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" >> "$OUTPUT"
          echo "" >> "$OUTPUT"
          if [ -f "ai-kernel/docs/u-profile/U-00-profile.md" ]; then 
            bash .github/workflows/append_file.sh "ai-kernel/docs/u-profile/U-00-profile.md" "$OUTPUT" || true
          fi

      - name: "Step 5: 生成 W 包 (W00.md)（若存在）"
        run: |
          OUTPUT="build-repo/W00.md"
          if [ -d "ai-kernel/docs/skills/W" ]; then
            echo "# W 包：Web App 領域技能" > "$OUTPUT"
            echo "" >> "$OUTPUT"
            echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" >> "$OUTPUT"
            echo "" >> "$OUTPUT"
            for f in ai-kernel/docs/skills/W/W-0*.md; do
              if [ -f "$f" ]; then bash .github/workflows/append_file.sh "$f" "$OUTPUT" || true; fi
            done
          fi

      - name: "Step 6: 生成 S00 包 (S00-ai-system.md)"
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

          for f in AI-General/docs/records/SYSTEM-HANDOFF.md AI-General/docs/records/PROGRESS.md AI-General/docs/records/STATE-INDEX.md AI-General/docs/records/SYSTEM-CONCLUSION.md; do
            if [ -f "$f" ]; then bash .github/workflows/append_file.sh "$f" "$OUTPUT" || true; fi
          done

      - name: "Step 7: 生成專案包 (動態領域判斷)"
        run: |
          jq -c '.[]' /tmp/projects.json | while read -r row; do
            REPO=$(echo "$row" | jq -r '.repo')
            PKG_NO=$(echo "$row" | jq -r '.pkg_no')
            DOMAIN=$(echo "$row" | jq -r '.domain')
            SAFE_NAME=$(echo "$REPO" | awk -F/ '{print $2}')
            OUTPUT="build-repo/${PKG_NO}-${SAFE_NAME}.md"
            
            echo "# 專案包：${SAFE_NAME}" > "$OUTPUT"
            echo "" >> "$OUTPUT"
            echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" >> "$OUTPUT"
            echo "" >> "$OUTPUT"
            
            proj_docs_dir="external/${SAFE_NAME}/docs/projects/${SAFE_NAME}"
            if [ -d "$proj_docs_dir" ]; then
              for prefix in P-01 P-02 P-05 P-06 P-07 P-08 P-09 P-12 P-13 P-98 P-99; do
                for f in ${proj_docs_dir}/${prefix}-*.md; do
                  if [ -f "$f" ]; then bash .github/workflows/append_file.sh "$f" "$OUTPUT" || true; fi
                done
              done
            fi
            
            if [ -f "external/${SAFE_NAME}/meta.json" ]; then
              echo "" >> "$OUTPUT"
              echo "## meta.json" >> "$OUTPUT"
              bash .github/workflows/append_file.sh "external/${SAFE_NAME}/meta.json" "$OUTPUT" || true
            fi

            if [ "$DOMAIN" == "E" ]; then
              for f in external/${SAFE_NAME}/E-*.md; do
                if [ -f "$f" ]; then bash .github/workflows/append_file.sh "$f" "$OUTPUT" || true; fi
              done
            fi

            if [ -d "external/${SAFE_NAME}" ]; then
              (
                cd "external/${SAFE_NAME}" || exit 0
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
          done

      - name: "Step 8: 生成基礎設施總包 (03-INFRA.md)"
        run: |
          OUTPUT="build-repo/03-INFRA.md"
          echo "# 基礎設施總包 (INFRA)" > "$OUTPUT"
          echo "" >> "$OUTPUT"
          echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" >> "$OUTPUT"
          echo "> ⚠️ 給 AI 的指示：本包包含體系唯一腳本與各專案呼叫器。" >> "$OUTPUT"
          echo "" >> "$OUTPUT"

          echo "## 🏢 體系層：AI-infra（唯一腳本）" >> "$OUTPUT"
          echo "" >> "$OUTPUT"
          
          for py in apply_patch.py validate_delivery.py; do
            if [ -f ".github/scripts/$py" ]; then 
              echo "### 📄 檔案：$py" >> "$OUTPUT"
              bash .github/workflows/append_file.sh ".github/scripts/$py" "$OUTPUT" || true
              echo "" >> "$OUTPUT"
            fi
          done

          for wf in setup-structure.yml sync-repos.yml migrate.yml append_file.sh; do
            if [ -f ".github/workflows/$wf" ]; then 
              echo "### 📄 檔案：$wf (infra)" >> "$OUTPUT"
              bash .github/workflows/append_file.sh ".github/workflows/$wf" "$OUTPUT" || true
              echo "" >> "$OUTPUT"
            fi
          done

          if [ -f "config/repos.json" ]; then 
            echo "### 📄 檔案：config/repos.json (infra)" >> "$OUTPUT"
            bash .github/workflows/append_file.sh "config/repos.json" "$OUTPUT" || true
            echo "" >> "$OUTPUT"
          fi

          if [ -f ".gitignore" ]; then 
            echo "### 📄 檔案：.gitignore (infra)" >> "$OUTPUT"
            bash .github/workflows/append_file.sh ".gitignore" "$OUTPUT" || true
            echo "" >> "$OUTPUT"
          fi

          if [ -d "ai-kernel/.github/workflows" ]; then
            echo "## 🏢 體系層：AI-kernel 的呼叫器" >> "$OUTPUT"
            echo "" >> "$OUTPUT"
            (
              cd ai-kernel || exit 0
              # 暴力抓取所有檔案
              find .github/workflows -type f | sort | while read -r file; do 
                echo "### 📄 檔案：${file}" >> "../../$OUTPUT"
                echo "" >> "../../$OUTPUT"
                bash "../.github/workflows/append_file.sh" "$file" "../../$OUTPUT" || true
                echo "" >> "../../$OUTPUT"
              done
            ) || true
          else
            echo "> ⚠️ 找不到 `ai-kernel/.github/workflows` 資料夾，無法打包。" >> "$OUTPUT"
          fi
          if [ -f "ai-kernel/.gitignore" ]; then 
            echo "### 📄 檔案：.gitignore (ai-kernel)" >> "$OUTPUT"
            bash .github/workflows/append_file.sh "ai-kernel/.gitignore" "$OUTPUT" || true
            echo "" >> "$OUTPUT"
          fi

          if [ -d "AI-General/.github/workflows" ]; then
            echo "## 🏢 體系層：AI-General 的呼叫器" >> "$OUTPUT"
            echo "" >> "$OUTPUT"
            (
              cd AI-General || exit 0
              find .github/workflows -type f | sort | while read -r file; do 
                echo "### 📄 檔案：${file}" >> "../../$OUTPUT"
                echo "" >> "../../$OUTPUT"
                bash "../.github/workflows/append_file.sh" "$file" "../../$OUTPUT" || true
                echo "" >> "../../$OUTPUT"
              done
            ) || true
          else
            echo "> ⚠️ 找不到 `AI-General/.github/workflows` 資料夾，無法打包。" >> "$OUTPUT"
          fi
          if [ -f "AI-General/.gitignore" ]; then 
            echo "### 📄 檔案：.gitignore (AI-General)" >> "$OUTPUT"
            bash .github/workflows/append_file.sh "AI-General/.gitignore" "$OUTPUT" || true
            echo "" >> "$OUTPUT"
          fi

          jq -c '.[]' /tmp/projects.json | while read -r row; do
            REPO=$(echo "$row" | jq -r '.repo')
            SAFE_NAME=$(echo "$REPO" | awk -F/ '{print $2}')
            if [ -d "external/${SAFE_NAME}/.github/workflows" ]; then
              echo "## 🏢 專案層：${REPO} 的呼叫器" >> "$OUTPUT"
              echo "" >> "$OUTPUT"
              (
                cd "external/${SAFE_NAME}" || exit 0
                find .github/workflows -type f | sort | while read -r file; do 
                  echo "### 📄 檔案：${file}" >> "../../$OUTPUT"
                  echo "" >> "../../$OUTPUT"
                  bash "../../.github/workflows/append_file.sh" "$file" "../../$OUTPUT" || true
                  echo "" >> "../../$OUTPUT"
                done
              ) || true
            fi
            if [ -f "external/${SAFE_NAME}/.gitignore" ]; then 
              echo "### 📄 檔案：.gitignore (${SAFE_NAME})" >> "$OUTPUT"
              bash .github/workflows/append_file.sh "external/${SAFE_NAME}/.gitignore" "$OUTPUT" || true
              echo "" >> "$OUTPUT"
            fi
          done

      - name: "Step 9: 上傳共享包"
        run: |
          cd AI-General
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          git add docs/records/STATE-INDEX.md
          git diff --cached --quiet || (git commit -m "chore: auto-update STATE-INDEX [skip ci]" && git push)

          cd ../build-repo
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          git add -A
          git diff --cached --quiet || (git commit -m "chore: auto-build packages [skip ci]" && git push)

# ═══ EOF ═══
# F: AI-infra/.github/workflows/setup-structure.yml
# V: 1792404000
# W: ai-system-20261019-010#191
# ═══ EOF ═══```

<!-- === FILE END: .github/workflows/setup-structure.yml === -->

### 📄 檔案：sync-repos.yml (infra)

<!-- === FILE START: .github/workflows/sync-repos.yml === -->

## 📄 .github/workflows/sync-repos.yml

```yml
name: Sync Repos List

on:
  workflow_dispatch:
  schedule:
    - cron: '0 0 * * *'
  repository_dispatch:
    types: [sync-repos]
  push:
    branches: [main, master]
    paths:
      - '.github/workflows/sync-repos.yml'

# 防止多個同步 Action 同時執行
concurrency:
  group: sync-repos
  cancel-in-progress: true

jobs:
  sync:
    runs-on: ubuntu-latest
    permissions:
      contents: write

    steps:
      - name: Checkout AI-infra
        uses: actions/checkout@v4

      - name: Generate repos.json
        env:
          GH_TOKEN: ${{ secrets.CROSS_REPO_PAT }}
        run: |
          mkdir -p config
          
          echo '{' > config/repos.json
          echo '  "kernel": "tlee20860-dotcom/AI-kernel",' >> config/repos.json
          echo '  "general": "tlee20860-dotcom/AI-General",' >> config/repos.json
          echo '  "packages": "tlee20860-dotcom/AI-packages",' >> config/repos.json
          echo '  "projects": [' >> config/repos.json

          REPOS=$(gh search repos --owner tlee20860-dotcom --topic ai-systems --json name,owner --jq '.[] | select(.name != "AI-kernel" and .name != "AI-General" and .name != "AI-infra" and .name != "AI-packages") | .owner.login + "/" + .name')

          FIRST=true
          for REPO in $REPOS; do
            echo "Checking $REPO..."
            
            META=$(curl -s -H "Authorization: token $GH_TOKEN" "https://raw.githubusercontent.com/$REPO/main/meta.json")
            if ! echo "$META" | grep -q "pkg_no"; then
              META=$(curl -s -H "Authorization: token $GH_TOKEN" "https://raw.githubusercontent.com/$REPO/master/meta.json")
            fi
            
            if echo "$META" | grep -q "pkg_no"; then
              DOMAIN=$(echo "$META" | jq -r '.domain')
              PKG_NO=$(echo "$META" | jq -r '.pkg_no')
              CODE=$(echo "$META" | jq -r '.code // "-"')
              DISPLAY=$(echo "$META" | jq -r '.display // "-"')
            else
              echo "  [WARN] $REPO 缺少有效的 meta.json，跳過。"
              continue
            fi

            if [ "$FIRST" = true ]; then
              FIRST=false
            else
              echo ',' >> config/repos.json
            fi
            
            echo "    {" >> config/repos.json
            echo "      \"repo\": \"$REPO\"," >> config/repos.json
            echo "      \"domain\": \"$DOMAIN\"," >> config/repos.json
            echo "      \"code\": \"$CODE\"," >> config/repos.json
            echo "      \"display\": \"$DISPLAY\"," >> config/repos.json
            echo "      \"pkg_no\": \"$PKG_NO\"" >> config/repos.json
            echo -n "    }" >> config/repos.json
          done

          echo '' >> config/repos.json
          echo '  ]' >> config/repos.json
          echo '}' >> config/repos.json

      - name: Commit repos.json
        id: commit
        run: |
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          git add config/repos.json
          if git diff --cached --quiet; then
            echo "No changes to config/repos.json"
            echo "CHANGED=false" >> $GITHUB_ENV
          else
            git commit -m "chore: auto-sync repos.json [skip ci]"
            git push
            echo "CHANGED=true" >> $GITHUB_ENV
          fi

      - name: Trigger Build Packages
        if: env.CHANGED == 'true'
        run: |
          curl -X POST \
            -H "Accept: application/vnd.github.v3+json" \
            -H "Authorization: token ${{ secrets.CROSS_REPO_PAT }}" \
            https://api.github.com/repos/tlee20860-dotcom/AI-infra/dispatches \
            -d '{"event_type":"project-updated"}'

# ═══ EOF ═══
# F: AI-infra/.github/workflows/sync-repos.yml
# V: 1792382000
# W: ai-system-20261019-010#140
# ═══ EOF ═══```

<!-- === FILE END: .github/workflows/sync-repos.yml === -->

### 📄 檔案：migrate.yml (infra)

<!-- === FILE START: .github/workflows/migrate.yml === -->

## 📄 .github/workflows/migrate.yml

```yml
# ============================================
# migrate.yml — 一次性遷移腳本
# 用途：將 AI-General 的 W 包搬到 ai-kernel，並清理舊檔案
# ============================================

name: Migrate W-Package and Cleanup

on:
  workflow_dispatch: # 僅允許手動觸發

jobs:
  migrate:
    runs-on: ubuntu-latest
    permissions:
      contents: write

    steps:
      - name: Checkout AI-General
        uses: actions/checkout@v4
        with:
          repository: tlee20860-dotcom/AI-General
          token: ${{ secrets.CROSS_REPO_PAT }}
          path: AI-General

      - name: Checkout ai-kernel
        uses: actions/checkout@v4
        with:
          repository: tlee20860-dotcom/AI-kernel
          token: ${{ secrets.CROSS_REPO_PAT }}
          path: ai-kernel

      - name: Move W-Package to ai-kernel
        run: |
          echo "=== 搬移 W 技能包至 AI-kernel ==="
          mkdir -p ai-kernel/docs/skills/W
          
          if [ -d "AI-General/docs/skills/W" ]; then
            cp -r AI-General/docs/skills/W/* ai-kernel/docs/skills/W/ || true
            echo "[OK] W 技能包已複製。"
          else
            echo "[WARNING] 在 AI-General 找不到 docs/skills/W，跳過複製。"
          fi

          cd ai-kernel
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          git add docs/skills/W/
          
          if git diff --cached --quiet; then
            echo "ai-kernel 無變更，不需 commit。"
          else
            git commit -m "chore: migrate W-package from AI-General [skip ci]"
            git push
            echo "[OK] ai-kernel 已更新並 push。"
          fi

      - name: Cleanup AI-General
        run: |
          echo "=== 清理 AI-General 舊檔案 ==="
          cd AI-General
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          
          git rm -r --ignore-unmatch docs/common/ || true
          git rm -r --ignore-unmatch docs/skills/ || true
          
          if git diff --cached --quiet; then
            echo "AI-General 無變更，不需 commit。"
          else
            git commit -m "chore: cleanup old docs [skip ci]"
            git push
            echo "[OK] AI-General 已清理並 push。"
          fi

# ═══ EOF ═══
# F: AI-infra/.github/workflows/migrate.yml
# V: 1792359000
# W: ai-system-20261019-010#095
# ═══ EOF ═══```

<!-- === FILE END: .github/workflows/migrate.yml === -->

### 📄 檔案：append_file.sh (infra)

<!-- === FILE START: .github/workflows/append_file.sh === -->

## 📄 .github/workflows/append_file.sh

```sh
#!/bin/bash
# append_file.sh - 用於將源檔案合併到共享包中

if [ $# -ne 2 ]; then
    echo "Usage: $0 <source_file> <target_file>"
    exit 1
fi

SOURCE=$1
TARGET=$2

if [ ! -f "$SOURCE" ]; then
    echo "[WARN] Source file not found: $SOURCE"
    exit 0
fi

# 寫入標記與檔名
echo "" >> "$TARGET"
echo "<!-- === FILE START: $SOURCE === -->" >> "$TARGET"
echo "" >> "$TARGET"
echo "## 📄 $SOURCE" >> "$TARGET"
echo "" >> "$TARGET"

# 取得副檔名
EXT="${SOURCE##*.}"

# 寫入檔案內容（防呆：如果是 .md 檔且內部有三反引號，跳過包裹；其他副檔名一律強制包裹）
if [ "$EXT" = "md" ] && grep -q '\`\`\`' "$SOURCE"; then
    cat "$SOURCE" >> "$TARGET"
else
    echo "\`\`\`$EXT" >> "$TARGET"
    cat "$SOURCE" >> "$TARGET"
    echo "\`\`\`" >> "$TARGET"
fi

# 寫入結束標記
echo "" >> "$TARGET"
echo "<!-- === FILE END: $SOURCE === -->" >> "$TARGET"

# ═══ EOF ═══
# F: AI-infra/.github/workflows/append_file.sh
# V: 1792383600
# W: ai-system-20261019-011#004
# ═══ EOF ═══```

<!-- === FILE END: .github/workflows/append_file.sh === -->

### 📄 檔案：config/repos.json (infra)

<!-- === FILE START: config/repos.json === -->

## 📄 config/repos.json

```json
{
  "kernel": "tlee20860-dotcom/AI-kernel",
  "general": "tlee20860-dotcom/AI-General",
  "packages": "tlee20860-dotcom/AI-packages",
  "projects": [
    {
      "repo": "tlee20860-dotcom/AI-OS",
      "domain": "E",
      "code": "ai-os",
      "display": "AI-OS 系統",
      "pkg_no": "91"
    },
    {
      "repo": "tlee20860-dotcom/AI-WEB-APP-Family",
      "domain": "W",
      "code": "webapp",
      "display": "家庭 webapp",
      "pkg_no": "90"
    }
  ]
}
```

<!-- === FILE END: config/repos.json === -->

### 📄 檔案：.gitignore (infra)

<!-- === FILE START: .gitignore === -->

## 📄 .gitignore

```gitignore
# ============================================
# 系統與編輯器暫存檔
# ============================================
.DS_Store
Thumbs.db
*.swp

# ============================================
# 本地環境變數與金鑰（絕對不可上傳）
# ============================================
.env
.env.local

# ============================================
# Node.js / Python 依賴包
# ============================================
node_modules/
__pycache__/
*.pyc

# ============================================
# AI-infra 自動化專用：避免把 clone 的專案與打包結果上傳
# ============================================
external/
build-repo/
tmp/```

<!-- === FILE END: .gitignore === -->

## 🏢 體系層：AI-kernel 的呼叫器

### 📄 檔案：.gitignore (ai-kernel)

<!-- === FILE START: ai-kernel/.gitignore === -->

## 📄 ai-kernel/.gitignore

```gitignore
# ============================================
# 系統與編輯器暫存檔
# ============================================
.DS_Store
Thumbs.db
*.swp

# ============================================
# 本地環境變數與金鑰（絕對不可上傳）
# ============================================
.env
.env.local

# ============================================
# Node.js / Python 依賴包
# ============================================
node_modules/
__pycache__/
*.pyc

# ============================================
# AI-infra 自動化專用：避免把 clone 的專案與打包結果上傳
# ============================================
external/
build-repo/
tmp/```

<!-- === FILE END: ai-kernel/.gitignore === -->

## 🏢 體系層：AI-General 的呼叫器

### 📄 檔案：.gitignore (AI-General)

<!-- === FILE START: AI-General/.gitignore === -->

## 📄 AI-General/.gitignore

```gitignore
# 忽略 Actions 臨時 checkout 目錄
external/
build-repo/

# 忽略系統檔案
.DS_Store
*.log
```

<!-- === FILE END: AI-General/.gitignore === -->

## 🏢 專案層：tlee20860-dotcom/AI-OS 的呼叫器

### 📄 檔案：.github/workflows/apply-patch.yml


<!-- === FILE START: .github/workflows/apply-patch.yml === -->

## 📄 .github/workflows/apply-patch.yml

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

<!-- === FILE END: .github/workflows/apply-patch.yml === -->

### 📄 檔案：.github/workflows/notify.yml


<!-- === FILE START: .github/workflows/notify.yml === -->

## 📄 .github/workflows/notify.yml

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

<!-- === FILE END: .github/workflows/notify.yml === -->

## 🏢 專案層：tlee20860-dotcom/AI-WEB-APP-Family 的呼叫器

### 📄 檔案：.github/workflows/apply-patch.yml


<!-- === FILE START: .github/workflows/apply-patch.yml === -->

## 📄 .github/workflows/apply-patch.yml

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
# F: AI-WEB-APP-Family/.github/workflows/apply-patch.yml
# V: 1792358000
# W: ai-system-20261019-010#088
# ═══ EOF ═══```

<!-- === FILE END: .github/workflows/apply-patch.yml === -->

### 📄 檔案：.github/workflows/notify.yml


<!-- === FILE START: .github/workflows/notify.yml === -->

## 📄 .github/workflows/notify.yml

```yml
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
# ═══ EOF ═══```

<!-- === FILE END: .github/workflows/notify.yml === -->

### 📄 檔案：.gitignore (AI-WEB-APP-Family)

<!-- === FILE START: external/AI-WEB-APP-Family/.gitignore === -->

## 📄 external/AI-WEB-APP-Family/.gitignore

```gitignore
# ============================================
# 系統與編輯器暫存檔
# ============================================
.DS_Store
Thumbs.db
*.swp

# ============================================
# 本地環境變數與金鑰（絕對不可上傳）
# ============================================
.env
.env.local

# ============================================
# Node.js / Python 依賴包
# ============================================
node_modules/
__pycache__/
*.pyc

# ============================================
# AI-infra 自動化專用：避免把 clone 的專案與打包結果上傳
# ============================================
external/
build-repo/
tmp/```

<!-- === FILE END: external/AI-WEB-APP-Family/.gitignore === -->

