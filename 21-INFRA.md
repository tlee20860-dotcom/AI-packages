> 生成時間：2026-10-25 18:39:21 (HKT)
> ⚠️ 給 AI 的指示：本包包含體系唯一腳本與各專案呼叫器。

# 基礎設施總包 (INFRA)

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

def check_file_boundary(path, content):
    if not path.endswith(".md"):
        return None
    lines = [line.strip() for line in content.splitlines() if line.strip()]
    if not lines:
        return "檔案為空"
    
    first_line = lines[0]
    last_line = lines[-1]
    
    # 檢查是否以反引號開頭/結尾（防止整檔被包裹）
    if first_line.startswith("```") or first_line.startswith("````"):
        return "檔案第一行以反引號開頭（F-004）"
    if last_line.endswith("```") or last_line.endswith("````"):
        return "檔案最後一行以反引號結尾（F-004）"
    
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
        ("檔案邊界", lambda c: check_file_boundary(path, c)),
        ("開頭標題", lambda c: check_head(c)),
    ):
        if label == "檔案邊界" and not path.endswith(".md"):
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
# setup-structure.yml — 動態共享包打包腳本 (統一大腦版)
# 用途：手動觸發，所有生成物只寫入 AI-Packages，絕不回寫源檔
# ============================================

name: Build Shared Packages

on:
  workflow_dispatch:

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

      - name: "Step 0: 同步 Repos 名單 (Sync Repos List)"
        env:
          GH_TOKEN: ${{ secrets.CROSS_REPO_PAT }}
        run: |
          mkdir -p config
          
          echo '[' > /tmp/projects_array.json
          
          REPOS=$(gh search repos --owner tlee20860-dotcom --topic ai-systems --json name,owner --jq '.[] | select(.name != "AI-kernel" and .name != "AI-General" and .name != "AI-infra" and .name != "AI-packages" and .name != "old-AI-WA-FAMILY") | .owner.login + "/" + .name')
          
          FIRST=true
          for REPO in $REPOS; do
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
              continue
            fi

            if [ "$FIRST" = true ]; then FIRST=false; else echo ',' >> /tmp/projects_array.json; fi
            echo "    {\"repo\": \"$REPO\", \"domain\": \"$DOMAIN\", \"code\": \"$CODE\", \"display\": \"$DISPLAY\", \"pkg_no\": \"$PKG_NO\"}" >> /tmp/projects_array.json
          done

          OLD_REPO="tlee20860-dotcom/old-AI-WA-FAMILY"
          OLD_META=$(curl -s -H "Authorization: token $GH_TOKEN" "https://raw.githubusercontent.com/$OLD_REPO/main/meta.json")
          if echo "$OLD_META" | grep -q "pkg_no"; then
            if [ "$FIRST" = true ]; then FIRST=false; else echo ',' >> /tmp/projects_array.json; fi
            DOMAIN=$(echo "$OLD_META" | jq -r '.domain')
            PKG_NO=$(echo "$OLD_META" | jq -r '.pkg_no')
            CODE=$(echo "$OLD_META" | jq -r '.code // "-"')
            DISPLAY=$(echo "$OLD_META" | jq -r '.display // "-"')
            echo "    {\"repo\": \"$OLD_REPO\", \"domain\": \"$DOMAIN\", \"code\": \"$CODE\", \"display\": \"$DISPLAY\", \"pkg_no\": \"$PKG_NO\"}" >> /tmp/projects_array.json
          fi

          echo '' >> /tmp/projects_array.json
          echo ']' >> /tmp/projects_array.json

          echo '{' > config/repos.json
          echo '  "kernel": "tlee20860-dotcom/AI-kernel",' >> config/repos.json
          echo '  "general": "tlee20860-dotcom/AI-General",' >> config/repos.json
          echo '  "packages": "tlee20860-dotcom/AI-packages",' >> config/repos.json
          echo -n '  "projects": ' >> config/repos.json
          jq 'sort_by(.pkg_no)' /tmp/projects_array.json >> config/repos.json
          echo '}' >> config/repos.json
          
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          git add config/repos.json
          git diff --cached --quiet || (git commit -m "chore: auto-sync repos.json [skip ci]" && git push)

      - name: Read Repos Config
        id: config
        run: |
          cat config/repos.json
          KERNEL=$(jq -r '.kernel' config/repos.json)
          GENERAL=$(jq -r '.general' config/repos.json)
          PACKAGES=$(jq -r '.packages' config/repos.json)
          echo "KERNEL_REPO=$KERNEL" >> $GITHUB_ENV
          echo "GENERAL_REPO=$GENERAL" >> $GITHUB_ENV
          echo "PACKAGES_REPO=$PACKAGES" >> $GITHUB_ENV
          jq -c '.projects' config/repos.json > /tmp/projects.json

      - name: Checkout AI-kernel (git clone main)
        run: |
          git clone --depth 1 -b main "https://x-access-token:${{ secrets.CROSS_REPO_PAT }}@github.com/${{ env.KERNEL_REPO }}.git" AI-kernel || \
          git clone --depth 1 -b master "https://x-access-token:${{ secrets.CROSS_REPO_PAT }}@github.com/${{ env.KERNEL_REPO }}.git" AI-kernel

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

      - name: "Step 1.5: 生成體系狀態 (20-SP-Sys.md)"
        run: |
          OUTPUT="build-repo/20-SP-Sys.md"
          echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" > "$OUTPUT"
          echo "" >> "$OUTPUT"
          echo "# AI 對話系統 - 體系狀態包" >> "$OUTPUT"
          echo "" >> "$OUTPUT"
          echo "---" >> "$OUTPUT"
          echo "⚠️ **【給 AI 的強制前置檢查】**" >> "$OUTPUT"
          echo "請立刻確認：你是否已經讀取 \`00-IP.md\`、\`01-AP.md\`、\`02-RP.md\`、\`03-UP.md\`（體系四檔）？" >> "$OUTPUT"
          echo "1. 如果「否」或「不確定」，請立刻停止閱讀本檔，先去讀取缺漏的檔案，載入身份、流程、規則與偏好。" >> "$OUTPUT"
          echo "2. 如果「是」，請依據 \`00-IP.md\` 與 \`01-AP.md\` 輸出載入確認。" >> "$OUTPUT"
          echo "---" >> "$OUTPUT"
          echo "" >> "$OUTPUT"

          # 打包體系紀錄
          for f in AI-General/docs/records/SYSTEM-HANDOFF.md AI-General/docs/records/PROGRESS.md AI-General/docs/records/SYSTEM-CONCLUSION.md AI-General/docs/records/SYSTEM-REJECTED.md AI-General/docs/records/SYSTEM-DEBT.md; do
            if [ -f "$f" ]; then bash .github/workflows/append_file.sh "$f" "$OUTPUT" || true; fi
          done

      - name: "Step 1.8: 生成通用決策包 (04-GC.md)"
        run: |
          OUTPUT="build-repo/04-GC.md"
          SOURCE="AI-General/docs/records/GENERAL-CONCLUSION.md"
          
          echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" > "$OUTPUT"
          echo "" >> "$OUTPUT"
          echo "# 通用對話決策總集 (GENERAL CONCLUSION)" >> "$OUTPUT"
          echo "" >> "$OUTPUT"
          
          if [ -f "$SOURCE" ]; then
            bash .github/workflows/append_file.sh "$SOURCE" "$OUTPUT" || true
          else
            echo "⚠️ 找不到 $SOURCE，生成空包。" >> "$OUTPUT"
            echo "<!-- 無通用決策紀錄 -->" >> "$OUTPUT"
          fi

      - name: "Step 1.9: 生成 I 包 (00-IP.md)"
        run: |
          OUTPUT="build-repo/00-IP.md"
          echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" > "$OUTPUT"
          echo "" >> "$OUTPUT"
          echo "# I 包：身份與信仰" >> "$OUTPUT"
          echo "" >> "$OUTPUT"
          if [ -f "AI-kernel/docs/a-guide/I-00-identity.md" ]; then bash .github/workflows/append_file.sh "AI-kernel/docs/a-guide/I-00-identity.md" "$OUTPUT" || true; fi

      - name: "Step 2: 生成 A 包 (01-AP.md)"
        run: |
          OUTPUT="build-repo/01-AP.md"
          echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" > "$OUTPUT"
          echo "" >> "$OUTPUT"
          echo "# A 包：啟動引導與角色定義" >> "$OUTPUT"
          echo "" >> "$OUTPUT"
          if [ -f "AI-kernel/docs/a-guide/A-00-guide.md" ]; then bash .github/workflows/append_file.sh "AI-kernel/docs/a-guide/A-00-guide.md" "$OUTPUT" || true; fi

      - name: "Step 3: 生成 R 包 (02-RP.md)"
        run: |
          OUTPUT="build-repo/02-RP.md"
          echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" > "$OUTPUT"
          echo "" >> "$OUTPUT"
          echo "# R 包：規則總覽" >> "$OUTPUT"
          echo "" >> "$OUTPUT"
          for f in R-03-terms R-00-rules R-02-patterns R-04-learning R-05-dsl-map; do
            FILE_PATH="AI-kernel/docs/r-rules/${f}.md"
            if [ -f "$FILE_PATH" ]; then bash .github/workflows/append_file.sh "$FILE_PATH" "$OUTPUT" || true; fi
          done

      - name: "Step 4: 生成 U 包 (03-UP.md)"
        run: |
          OUTPUT="build-repo/03-UP.md"
          echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" > "$OUTPUT"
          echo "" >> "$OUTPUT"
          echo "# U 包：使用者通用描述" >> "$OUTPUT"
          echo "" >> "$OUTPUT"
          if [ -f "AI-kernel/docs/u-profile/U-00-profile.md" ]; then bash .github/workflows/append_file.sh "AI-kernel/docs/u-profile/U-00-profile.md" "$OUTPUT" || true; fi

      - name: "Step 5: 生成 W 包 (10-WP.md)（若存在）"
        run: |
          OUTPUT="build-repo/10-WP.md"
          if [ -d "AI-kernel/docs/skills/W" ]; then
            echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" > "$OUTPUT"
            echo "" >> "$OUTPUT"
            echo "# W 包：Web App 領域技能" >> "$OUTPUT"
            echo "" >> "$OUTPUT"
            for f in AI-kernel/docs/skills/W/W-*.md; do
              if [ -f "$f" ]; then bash .github/workflows/append_file.sh "$f" "$OUTPUT" || true; fi
            done
          fi

      - name: "Step 6.5: 生成審查包 (23-CHK.md)"
        run: |
          OUTPUT="build-repo/23-CHK.md"
          SRC_DIR="AI-kernel/docs/review"
          SRC_LIST="REVIEW-FORMAT.md ROLE-BOUNDARY.md CHK-INDEX.md CHK-E.md"

          MISSING=""
          for f in $SRC_LIST; do
            if [ ! -f "$SRC_DIR/$f" ]; then MISSING="$MISSING $f"; fi
          done

          if [ -n "$MISSING" ]; then
            echo "[FAIL] CHK.md 生成中止：缺源檔$MISSING"
            rm -f "$OUTPUT"
            exit 1
          fi

          echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" > "$OUTPUT"
          echo "" >> "$OUTPUT"
          echo "# CHK 審查包（合規審判者用）" >> "$OUTPUT"
          echo "" >> "$OUTPUT"

          for f in $SRC_LIST; do
            bash .github/workflows/append_file.sh "$SRC_DIR/$f" "$OUTPUT" || { rm -f "$OUTPUT"; exit 1; }
          done

      - name: "Step 7: 生成專案狀態 ({pkg_no}-SP.md)"
        run: |
          jq -c '.[]' /tmp/projects.json | while read -r row; do
            REPO=$(echo "$row" | jq -r '.repo')
            PKG_NO=$(echo "$row" | jq -r '.pkg_no')
            DOMAIN=$(echo "$row" | jq -r '.domain')
            CODE=$(echo "$row" | jq -r '.code')
            DISPLAY=$(echo "$row" | jq -r '.display // "-"')
            SAFE_NAME=$(echo "$REPO" | awk -F/ '{print $2}')
            
            OUTPUT="build-repo/${PKG_NO}-SP.md"
            PROJ_DIR="AI-General/docs/p${PKG_NO}"

            # 防呆：若源目錄不存在，則不產出空殼
            if [ ! -d "$PROJ_DIR" ]; then
              echo "⏭️ 找不到專案目錄 $PROJ_DIR，跳過 ${PKG_NO}-SP.md 生成。"
              continue
            fi

            echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" > "$OUTPUT"
            echo "" >> "$OUTPUT"
            echo "# 專案狀態包：${SAFE_NAME}" >> "$OUTPUT"
            echo "" >> "$OUTPUT"

            # 打包進度與技術檔 (使用 *P-*.md 匹配)
            for f in "$PROJ_DIR"/WF-*.md "$PROJ_DIR"/*P-*.md; do
              if [ -f "$f" ]; then bash .github/workflows/append_file.sh "$f" "$OUTPUT" || true; fi
            done
          done

      - name: "Step 8: 生成專案技術包與代碼包 ({pkg_no}-P.md, {pkg_no}-Code.md)"
        run: |
          jq -c '.[]' /tmp/projects.json | while read -r row; do
            REPO=$(echo "$row" | jq -r '.repo')
            PKG_NO=$(echo "$row" | jq -r '.pkg_no')
            SAFE_NAME=$(echo "$REPO" | awk -F/ '{print $2}')
            
            OUTPUT_P="build-repo/${PKG_NO}-P.md"
            OUTPUT_CODE="build-repo/${PKG_NO}-Code.md"

            # 防呆：專案技術檔 (P)
            PROJ_DIR="AI-General/docs/p${PKG_NO}"
            if [ -d "$PROJ_DIR" ]; then
              echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" > "$OUTPUT_P"
              echo "" >> "$OUTPUT_P"
              echo "# 專案技術總集：${SAFE_NAME}" >> "$OUTPUT_P"
              echo "" >> "$OUTPUT_P"
              
              # 打包 P 檔至專案技術包 (使用 *P-*.md 匹配)
              for f in "$PROJ_DIR"/*P-*.md; do
                if [ -f "$f" ]; then bash .github/workflows/append_file.sh "$f" "$OUTPUT_P" || true; fi
              done
            else
              echo "⏭️ 找不到專案目錄 $PROJ_DIR，跳過 ${PKG_NO}-P.md 生成。"
            fi

            # 防呆：專案代碼 (Code)
            if [ -d "external/${SAFE_NAME}" ]; then
              echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" > "$OUTPUT_CODE"
              echo "" >> "$OUTPUT_CODE"
              echo "# 專案全程式碼包：${SAFE_NAME}" >> "$OUTPUT_CODE"
              echo "" >> "$OUTPUT_CODE"
              (
                cd "external/${SAFE_NAME}" || exit 0
                find . -type f \( -name "*.js" -o -name "*.html" -o -name "*.css" -o -name "*.json" \) \
                  -not -path "./.git/*" \
                  -not -path "./node_modules/*" \
                  -not -path "./docs/*" \
                  -not -path "./.github/*" | sort | while read -r f; do
                  if [ -f "$f" ]; then bash "../../.github/workflows/append_file.sh" "$f" "../../$OUTPUT_CODE" || true; fi
                done
              ) || true
            else
              echo "⏭️ 找不到專案源碼 external/${SAFE_NAME}，跳過 ${PKG_NO}-Code.md 生成。"
            fi
          done

      - name: "Step 9: 生成基礎設施總包 (21-INFRA.md)"
        run: |
          OUTPUT="build-repo/21-INFRA.md"
          echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" > "$OUTPUT"
          echo "> ⚠️ 給 AI 的指示：本包包含體系唯一腳本與各專案呼叫器。" >> "$OUTPUT"
          echo "" >> "$OUTPUT"
          echo "# 基礎設施總包 (INFRA)" >> "$OUTPUT"
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

          for wf in setup-structure.yml migrate.yml append_file.sh backup.yml copy-backup.yml cleanup-legacy.yml; do
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

      - name: "Step 9.3: 生成歷史檔案包 (24-HISTORY.md)"
        run: |
          OUTPUT="build-repo/24-HISTORY.md"
          SRC_FILES="AI-General/docs/records/SYSTEM-EVOLUTION.md AI-General/docs/records/SYSTEM-REJECTED.md AI-General/docs/records/SYSTEM-DEBT.md AI-kernel/docs/r-rules/R-04-learning.md"

          MISSING=""
          for f in $SRC_FILES; do
            if [ ! -f "$f" ]; then MISSING="$MISSING $f"; fi
          done

          if [ -n "$MISSING" ]; then
            echo "[FAIL] HISTORY.md 生成中止：缺源檔$MISSING"
            rm -f "$OUTPUT"
            exit 1
          fi

          echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" > "$OUTPUT"
          echo "> ⚠️ 給 AI 的指示：本包為歷史台帳與進化史，供查閱與追溯；不含會變動的 STATE-INDEX。" >> "$OUTPUT"
          echo "" >> "$OUTPUT"
          echo "# 歷史檔案包 (HISTORY)" >> "$OUTPUT"
          echo "" >> "$OUTPUT"

          for f in $SRC_FILES; do
            bash .github/workflows/append_file.sh "$f" "$OUTPUT" || { rm -f "$OUTPUT"; exit 1; }
          done

      - name: "Step 9.4: 生成全庫總索引 (25-INDEX.md)"
        run: |
          OUTPUT="build-repo/25-INDEX.md"
          echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" > "$OUTPUT"
          echo "" >> "$OUTPUT"
          echo "# 全庫檔案總索引 (GLOBAL INDEX)" >> "$OUTPUT"
          echo "" >> "$OUTPUT"
          echo "本檔包含所有體系與專案的實體檔案清單，供 AI 掌握全局檔案分佈。" >> "$OUTPUT"
          echo "" >> "$OUTPUT"

          # 1. 體系索引
          echo "## 🏢 體系源檔 (AI-kernel / AI-General)" >> "$OUTPUT"
          echo "| Repo | 檔案 | Version | Writer |" >> "$OUTPUT"
          echo "|---|---|---|---|" >> "$OUTPUT"
          TMP_TABLE="/tmp/state-index-sys.txt"
          > "$TMP_TABLE"
          for REPO_DIR in "AI-General/docs" "AI-kernel/docs"; do
            if [ -d "$REPO_DIR" ]; then
              find "$REPO_DIR" -type f -name "*.md" -not -path "*/.git/*" | while read -r f; do
                V=$(tail -n 15 "$f" | grep -oE 'V: *[0-9]+' | head -1 | sed 's/V: *//' || true)
                W=$(tail -n 15 "$f" | grep -oE 'W: *[^ ]+' | head -1 | sed 's/W: *//' || true)
                REPO_NAME="AI-General"
                CLEAN_F=${f#AI-General/}
                if [[ "$f" == AI-kernel/* ]]; then REPO_NAME="AI-kernel"; CLEAN_F=${f#AI-kernel/}; fi
                if [ -n "$V" ]; then echo "| $REPO_NAME | $CLEAN_F | $V | $W |" >> "$TMP_TABLE"; else echo "| $REPO_NAME | $CLEAN_F | - | - |" >> "$TMP_TABLE"; fi
              done
            fi
          done || true
          if [ -s "$TMP_TABLE" ]; then sort -t'|' -k1,1 -k2 "$TMP_TABLE" >> "$OUTPUT"; fi
          echo "" >> "$OUTPUT"

          # 2. 基建索引 (AI-infra)
          echo "## ⚙️ 基礎設施源檔 (AI-infra)" >> "$OUTPUT"
          echo "| 目錄/路徑 | 檔案名稱 |" >> "$OUTPUT"
          echo "|---|---|" >> "$OUTPUT"
          find . -type f \( -name "*.yml" -o -name "*.py" -o -name "*.sh" -o -name "*.html" -o -name "*.js" -o -name "*.json" \) \
            -not -path "./.git/*" -not -path "./build-repo/*" -not -path "./AI-*/*" -not -path "./external/*" | sort | while read -r f; do
            CLEAN_F=${f#./}
            DIR_NAME=$(dirname "$CLEAN_F")
            FILE_NAME=$(basename "$CLEAN_F")
            echo "| $DIR_NAME | $FILE_NAME |" >> "$OUTPUT"
          done
          echo "" >> "$OUTPUT"

          # 3. 專案源碼索引
          echo "## 📦 專案源碼檔案樹 (external)" >> "$OUTPUT"
          echo "| 專案倉庫 | 檔案路徑 |" >> "$OUTPUT"
          echo "|---|---|" >> "$OUTPUT"
          
          if [ -d "external" ]; then
            find external -type f -not -path "*/.git/*" -not -path "*/node_modules/*" | sort | while read -r f; do
              CLEAN_F=${f#external/}
              REPO_NAME=$(echo "$CLEAN_F" | awk -F/ '{print $1}')
              FILE_PATH=$(echo "$CLEAN_F" | cut -d'/' -f2-)
              echo "| $REPO_NAME | $FILE_PATH |" >> "$OUTPUT"
            done
          fi
          echo "" >> "$OUTPUT"

          # 4. 生成物自身索引 (build-repo)
          echo "## 🎁 本輪生成物清單 (AI-Packages)" >> "$OUTPUT"
          echo "| 生成物檔案 | 大小 (KB) |" >> "$OUTPUT"
          echo "|---|---|" >> "$OUTPUT"
          if [ -d "build-repo" ]; then
            find build-repo -maxdepth 1 -name "*.md" -type f -not -name "25-INDEX.md" | sort | while read -r f; do
              FNAME=$(basename "$f")
              SIZE=$(du -k "$f" | cut -f1)
              echo "| $FNAME | $SIZE |" >> "$OUTPUT"
            done
          fi

      - name: "Step 9.5: 生成控制台共享包 (22-Console.md)"
        run: |
          OUTPUT="build-repo/22-Console.md"
          echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" > "$OUTPUT"
          echo "" >> "$OUTPUT"
          echo "# 92 包：AI 控制台原始碼" >> "$OUTPUT"
          echo "" >> "$OUTPUT"
          
          for f in index.html manifest.json sw.js; do
            if [ -f "$f" ]; then bash .github/workflows/append_file.sh "$f" "$OUTPUT" || true; fi
          done

      - name: "Step 9.8: 交付驗證報告（report 模式，不擋）"
        run: |
          if [ ! -f ".github/scripts/validate_delivery.py" ]; then
            echo "[SKIP] validate_delivery.py 不存在"
            exit 0
          fi
          CHANGED=$(find AI-kernel/docs AI-General/docs -type f -name "*.md" 2>/dev/null || true)
          export CHANGED_FILES="$CHANGED"
          export VALIDATE_MODE=report
          python3 .github/scripts/validate_delivery.py || true

      - name: "Step 10: 上傳共享包 (僅推送到 AI-Packages)"
        run: |
          cd build-repo
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          git add -A
          git diff --cached --quiet || (
            git commit -m "chore: auto-build packages [skip ci]" && 
            (git pull --rebase origin main || true) && 
            git push
          )

# ═══ EOF ═══
# F: AI-infra/.github/workflows/setup-structure.yml
# V: 1792900108
# W: ai-system-20261024-013#8
# ═══ EOF ═══```

<!-- === FILE END: .github/workflows/setup-structure.yml === -->

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

### 📄 檔案：backup.yml (infra)

<!-- === FILE START: .github/workflows/backup.yml === -->

## 📄 .github/workflows/backup.yml

```yml
# ============================================
# backup.yml — 源檔自動備份腳本
# 用途：每日定時為所有源檔倉庫打上 Git Tag，防止手動貼檔覆蓋導致資料遺失
# ============================================

name: Daily Source Backup

on:
  schedule:
    # 每天 UTC 22:00 (即香港時間 HKT 早上 06:00) 執行
    - cron: '0 22 * * *'
  # 允許手動觸發，方便測試
  workflow_dispatch:

jobs:
  backup:
    runs-on: ubuntu-latest
    permissions:
      contents: read

    steps:
      - name: Create Daily Backup Tags
        env:
          GH_TOKEN: ${{ secrets.CROSS_REPO_PAT }}
        run: |
          # 建立 Tag 名稱，例如 backup-20261024
          TAG_NAME="backup-$(date +'%Y%m%d')"
          echo "Preparing to create tag: $TAG_NAME"

          # 獲取所有帶有 ai-systems 標籤的倉庫，並排除生成物倉庫 AI-packages
          REPOS=$(gh search repos --owner tlee20860-dotcom --topic ai-systems --json name,owner --jq '.[] | select(.name != "AI-packages") | .owner.login + "/" + .name')
          
          # 手動加入不在 topic 中但需要備份的舊版倉庫
          OLD_REPO="tlee20860-dotcom/old-AI-WA-FAMILY"
          
          for REPO in $REPOS $OLD_REPO; do
            echo "----------------------------------------"
            echo "Processing repository: $REPO"
            
            # 取得該倉庫的預設分支 (通常是 main 或 master)
            DEFAULT_BRANCH=$(gh repo view "$REPO" --json defaultBranchRef --jq '.defaultBranchRef.name')
            
            # 取得該分支最新 Commit 的 SHA
            SHA=$(gh api "repos/$REPO/commits/$DEFAULT_BRANCH" --jq '.sha')
            
            if [ -z "$SHA" ]; then
              echo "⚠️ 無法取得 $REPO 的 Commit SHA，跳過。"
              continue
            fi

            # 檢查該 Tag 是否已存在
            if gh api "repos/$REPO/git/ref/tags/$TAG_NAME" >/dev/null 2>&1; then
              echo "⏭️ Tag $TAG_NAME 已存在於 $REPO，跳過。"
            else
              # 建立 Tag
              gh api -X POST "repos/$REPO/git/refs" -f ref="refs/tags/$TAG_NAME" -f sha="$SHA" > /dev/null
              echo "✅ 成功為 $REPO 建立 Tag $TAG_NAME (基於 $DEFAULT_BRANCH: $SHA)。"
            fi
          done

          echo "========================================"
          echo "🎉 所有源檔倉庫備份完成。"

# ═══ EOF ═══
# F: AI-infra/.github/workflows/backup.yml
# V: 1792900050
# W: ai-system-20261024-012#69
# ═══ EOF ═══```

<!-- === FILE END: .github/workflows/backup.yml === -->

### 📄 檔案：copy-backup.yml (infra)

<!-- === FILE START: .github/workflows/copy-backup.yml === -->

## 📄 .github/workflows/copy-backup.yml

```yml
# ============================================
# copy-backup.yml — 接收網頁指令的全量快照備份
# 用途：手動觸發或由控制台呼叫，將指定倉庫完整複製到備份倉庫的時間戳資料夾中
# ============================================

name: Copy and Backup Repositories

on:
  workflow_dispatch:
    inputs:
      repos:
        description: '要備份的倉庫名稱 (逗號分隔)'
        required: true
        default: 'AI-kernel,AI-General,old-AI-WA-FAMILY,AI-OS'
      target_repo:
        description: '備份存放的目標倉庫'
        required: true
        default: 'tlee20860-dotcom/AI-Backup'

jobs:
  backup:
    runs-on: ubuntu-latest
    permissions:
      contents: write

    steps:
      - name: Checkout Backup Repository
        uses: actions/checkout@v4
        with:
          repository: ${{ github.event.inputs.target_repo }}
          token: ${{ secrets.CROSS_REPO_PAT }}
          path: backup-repo

      - name: Set Timestamp
        id: timestamp
        run: |
          # 產生例如 20261024-1900 的格式
          FOLDER_NAME="backup-$(TZ=Asia/Hong_Kong date +'%Y%m%d-%H%M')"
          echo "FOLDER_NAME=$FOLDER_NAME" >> $GITHUB_ENV
          echo "備份目錄: $FOLDER_NAME"

      - name: Clone and Copy
        env:
          GH_TOKEN: ${{ secrets.CROSS_REPO_PAT }}
          REPOS: ${{ github.event.inputs.repos }}
        run: |
          mkdir -p "backup-repo/${{ env.FOLDER_NAME }}"
          
          # 將逗號分隔的字串轉為陣列
          IFS=',' read -ra REPO_ARRAY <<< "$REPOS"
          
          for REPO_NAME in "${REPO_ARRAY[@]}"; do
            # 去除前後空白
            REPO_NAME=$(echo "$REPO_NAME" | xargs)
            if [ -z "$REPO_NAME" ]; then continue; fi
            
            echo "========================================"
            echo "正在備份: $REPO_NAME"
            
            # 優先嘗試 main 分支，失敗則嘗試 master
            git clone --depth 1 -b main "https://x-access-token:${GH_TOKEN}@github.com/tlee20860-dotcom/${REPO_NAME}.git" "temp-${REPO_NAME}" 2>/dev/null || \
            git clone --depth 1 -b master "https://x-access-token:${GH_TOKEN}@github.com/tlee20860-dotcom/${REPO_NAME}.git" "temp-${REPO_NAME}" 2>/dev/null
            
            if [ -d "temp-${REPO_NAME}" ]; then
              # 將整個目錄結構完整複製過去
              cp -a "temp-${REPO_NAME}" "backup-repo/${{ env.FOLDER_NAME }}/${REPO_NAME}"
              # 移除 .git 資料夾以節省備份倉庫空間
              rm -rf "backup-repo/${{ env.FOLDER_NAME }}/${REPO_NAME}/.git"
              echo "✅ ${REPO_NAME} 備份完成 (完整保留目錄結構)。"
            else
              echo "⚠️ ${REPO_NAME} clone 失敗，請確認名稱是否正確或權限不足，跳過。"
            fi
            
            # 清理臨時目錄
            rm -rf "temp-${REPO_NAME}"
          done

      - name: Commit and Push
        run: |
          cd backup-repo
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          git add .
          
          # 檢查是否有變更
          if git diff --cached --quiet; then
            echo "沒有新的變更需要備份。"
          else
            git commit -m "chore: backup snapshot ${{ env.FOLDER_NAME }}"
            git push
            echo "✅ 備份已成功推送到目標倉庫。"
          fi

# ═══ EOF ═══
# F: AI-infra/.github/workflows/copy-backup.yml
# V: 1792900065
# W: ai-system-20261024-012#100
# ═══ EOF ═══```

<!-- === FILE END: .github/workflows/copy-backup.yml === -->

### 📄 檔案：cleanup-legacy.yml (infra)

<!-- === FILE START: .github/workflows/cleanup-legacy.yml === -->

## 📄 .github/workflows/cleanup-legacy.yml

```yml
# ============================================
# cleanup-legacy.yml — 一次性清理腳本
# 用途：手動觸發，清理各倉庫殘留的舊檔案與空目錄，為統一大腦鋪路
# ============================================

name: Cleanup Legacy Files

on:
  workflow_dispatch:

jobs:
  cleanup:
    runs-on: ubuntu-latest
    permissions:
      contents: write

    steps:
      - name: Cleanup AI-General
        env:
          GH_TOKEN: ${{ secrets.CROSS_REPO_PAT }}
        run: |
          echo "=== 清理 AI-General 舊檔案 ==="
          git clone --depth 1 "https://x-access-token:${GH_TOKEN}@github.com/tlee20860-dotcom/AI-General.git" AI-General
          cd AI-General
          
          # 刪除 docs/p90/ 下非 90-P-*.md 且非 WF-*.md 的舊 P 檔 (包含 P-01, P-02 等)
          find docs/p90 -type f -name "P-*.md" ! -name "90-P-*.md" -delete || true
          
          # 刪除舊的 STATE-INDEX
          git rm --ignore-unmatch docs/records/STATE-INDEX.md || true
          
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          git add -A
          git diff --cached --quiet || (git commit -m "chore: cleanup legacy P files and STATE-INDEX [skip ci]" && git push)
          cd ..

      - name: Cleanup AI-WEB-APP-Family
        env:
          GH_TOKEN: ${{ secrets.CROSS_REPO_PAT }}
        run: |
          echo "=== 清理 AI-WEB-APP-Family 舊紀錄 ==="
          git clone --depth 1 "https://x-access-token:${GH_TOKEN}@github.com/tlee20860-dotcom/AI-WEB-APP-Family.git" AI-WEB-APP-Family
          cd AI-WEB-APP-Family
          
          # 強制移除專案紀錄目錄 (落實專案倉庫純化)
          git rm -r --ignore-unmatch docs/records || true
          git rm -r --ignore-unmatch docs/projects || true
          
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          git add -A
          git diff --cached --quiet || (git commit -m "chore: cleanup legacy records and projects [skip ci]" && git push)
          cd ..

      - name: Cleanup old-AI-WA-FAMILY
        env:
          GH_TOKEN: ${{ secrets.CROSS_REPO_PAT }}
        run: |
          echo "=== 清理 old-AI-WA-FAMILY 舊紀錄 ==="
          git clone --depth 1 "https://x-access-token:${GH_TOKEN}@github.com/tlee20860-dotcom/old-AI-WA-FAMILY.git" old-AI-WA-FAMILY
          cd old-AI-WA-FAMILY
          
          git rm -r --ignore-unmatch docs/records || true
          git rm -r --ignore-unmatch docs/projects || true
          
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          git add -A
          git diff --cached --quiet || (git commit -m "chore: cleanup legacy records and projects [skip ci]" && git push)
          cd ..

          echo "🎉 所有源檔倉庫清理完成。"

# ═══ EOF ═══
# F: AI-infra/.github/workflows/cleanup-legacy.yml
# V: 1792900081
# W: ai-system-20261024-012#132
# ═══ EOF ═══```

<!-- === FILE END: .github/workflows/cleanup-legacy.yml === -->

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
    "repo": "tlee20860-dotcom/AI-WEB-APP-Family",
    "domain": "W",
    "code": "webapp",
    "display": "家庭 webapp",
    "pkg_no": "90"
  },
  {
    "repo": "tlee20860-dotcom/old-AI-WA-FAMILY",
    "domain": "W",
    "code": "old-webapp",
    "display": "家庭 webapp 舊版",
    "pkg_no": "90-old"
  },
  {
    "repo": "tlee20860-dotcom/AI-OS",
    "domain": "E",
    "code": "ai-os",
    "display": "AI-OS 系統",
    "pkg_no": "91"
  }
]
}
```

<!-- === FILE END: config/repos.json === -->

