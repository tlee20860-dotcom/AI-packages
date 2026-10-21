# 基礎設施總包 (INFRA)

> 生成時間：2026-10-21 20:44:25 (HKT)
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
# setup-structure.yml — 動態共享包打包腳本 (手動觸發版)
# 用途：僅限手動觸發，並加入 pull --rebase 防撞機制
# ============================================

name: Build Shared Packages

on:
  workflow_dispatch: # 僅保留手動觸發，避免自動競爭衝突

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
          REPOS=$(gh search repos --owner tlee20860-dotcom --topic ai-systems --json name,owner --jq '.[] | select(.name != "AI-kernel" and .name != "AI-General" and .name != "AI-infra" and .name != "AI-packages") | .owner.login + "/" + .name')
          
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
          echo '' >> /tmp/projects_array.json
          echo ']' >> /tmp/projects_array.json

          # 使用 jq 進行排序（按 pkg_no）
          echo '{' > config/repos.json
          echo '  "kernel": "tlee20860-dotcom/AI-kernel",' >> config/repos.json
          echo '  "general": "tlee20860-dotcom/AI-General",' >> config/repos.json
          echo '  "packages": "tlee20860-dotcom/AI-packages",' >> config/repos.json
          echo -n '  "projects": ' >> config/repos.json
          jq 'sort_by(.pkg_no)' /tmp/projects_array.json >> config/repos.json
          echo '}' >> config/repos.json
          
          # 提交 repos.json 至倉庫備份
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

      - name: "Step 1.5: 生成體系 STATE-INDEX.md"
        run: |
          OUTPUT="AI-General/docs/records/STATE-INDEX.md"
          mkdir -p "AI-General/docs/records"
          TMP_TABLE="/tmp/state-index-files.txt"
          > "$TMP_TABLE"
          
          for REPO_DIR in "AI-General" "AI-kernel"; do
            if [ -d "$REPO_DIR/docs" ]; then
              find "$REPO_DIR/docs" -type f -name "*.md" -not -path "*/.git/*" | while read -r f; do
                V=$(tail -n 15 "$f" | grep -oE 'V: *[0-9]+' | head -1 | sed 's/V: *//' || true)
                if [ -z "$V" ]; then continue; fi
                W=$(tail -n 15 "$f" | grep -oE 'W: *[^ ]+' | head -1 | sed 's/W: *//' || true)
                
                REPO_NAME="AI-General"
                CLEAN_F=${f#AI-General/}
                if [[ "$f" == AI-kernel/* ]]; then 
                  REPO_NAME="AI-kernel"
                  CLEAN_F=${f#AI-kernel/}
                fi
                
                echo "$REPO_NAME|$CLEAN_F|$V|$W" >> "$TMP_TABLE"
              done
            fi
          done || true

          TIMESTAMP=$(date +%s)

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
            echo "V: $TIMESTAMP"
            echo "W: github-actions[bot]"
            echo "═══ */"
          } > "$OUTPUT"

      - name: "Step 1.9: 生成 I 包 (IP.md)"
        run: |
          OUTPUT="build-repo/IP.md"
          echo "# I 包：身份與信仰" > "$OUTPUT"
          echo "" >> "$OUTPUT"
          echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" >> "$OUTPUT"
          echo "" >> "$OUTPUT"
          if [ -f "AI-kernel/docs/a-guide/I-00-identity.md" ]; then 
            bash .github/workflows/append_file.sh "AI-kernel/docs/a-guide/I-00-identity.md" "$OUTPUT" || true
          fi

      - name: "Step 2: 生成 A 包 (AP.md)"
        run: |
          OUTPUT="build-repo/AP.md"
          echo "# A 包：啟動引導與角色定義" > "$OUTPUT"
          echo "" >> "$OUTPUT"
          echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" >> "$OUTPUT"
          echo "" >> "$OUTPUT"
          if [ -f "AI-kernel/docs/a-guide/A-00-guide.md" ]; then 
            bash .github/workflows/append_file.sh "AI-kernel/docs/a-guide/A-00-guide.md" "$OUTPUT" || true
          fi

      - name: "Step 3: 生成 R 包 (RP.md)"
        run: |
          OUTPUT="build-repo/RP.md"
          echo "# R 包：規則總覽" > "$OUTPUT"
          echo "" >> "$OUTPUT"
          echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" >> "$OUTPUT"
          echo "" >> "$OUTPUT"
          
          for f in R-03-terms R-00-rules R-02-patterns R-04-learning; do
            FILE_PATH="AI-kernel/docs/r-rules/${f}.md"
            if [ -f "$FILE_PATH" ]; then 
              bash .github/workflows/append_file.sh "$FILE_PATH" "$OUTPUT" || true
            fi
          done

      - name: "Step 4: 生成 U 包 (UP.md)"
        run: |
          OUTPUT="build-repo/UP.md"
          echo "# U 包：使用者通用描述" > "$OUTPUT"
          echo "" >> "$OUTPUT"
          echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" >> "$OUTPUT"
          echo "" >> "$OUTPUT"
          if [ -f "AI-kernel/docs/u-profile/U-00-profile.md" ]; then 
            bash .github/workflows/append_file.sh "AI-kernel/docs/u-profile/U-00-profile.md" "$OUTPUT" || true
          fi

      - name: "Step 5: 生成 W 包 (WP.md)（若存在）"
        run: |
          OUTPUT="build-repo/WP.md"
          if [ -d "AI-kernel/docs/skills/W" ]; then
            echo "# W 包：Web App 領域技能" > "$OUTPUT"
            echo "" >> "$OUTPUT"
            echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" >> "$OUTPUT"
            echo "" >> "$OUTPUT"
            # [N-02 修正] 使用 W-*.md 取代 W-0*.md
            for f in AI-kernel/docs/skills/W/W-*.md; do
              if [ -f "$f" ]; then bash .github/workflows/append_file.sh "$f" "$OUTPUT" || true; fi
            done
          fi

      - name: "Step 6: 生成 S 包 (SP.md)"
        run: |
          OUTPUT="build-repo/SP.md"
          echo "# AI 對話系統 - 體系狀態包" > "$OUTPUT"
          echo "" >> "$OUTPUT"
          echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" >> "$OUTPUT"
          echo "" >> "$OUTPUT"
          
          echo "---" >> "$OUTPUT"
          echo "⚠️ **【給 AI 的強制前置檢查】**" >> "$OUTPUT"
          echo "請立刻確認：你是否已經讀取 \`IP.md\`、\`AP.md\`、\`RP.md\`、\`UP.md\`（體系四檔）？" >> "$OUTPUT"
          echo "1. 如果「否」或「不確定」，請立刻停止閱讀本檔，先去讀取缺漏的檔案，載入身份、流程、規則與偏好。" >> "$OUTPUT"
          echo "2. 如果「是」，請依據 \`IP.md\` 與 \`AP.md\` 輸出載入確認。" >> "$OUTPUT"
          echo "---" >> "$OUTPUT"
          echo "" >> "$OUTPUT"

          for f in AI-General/docs/records/SYSTEM-HANDOFF.md AI-General/docs/records/PROGRESS.md AI-General/docs/records/SYSTEM-CONCLUSION.md AI-General/docs/records/STATE-INDEX.md; do
            if [ -f "$f" ]; then bash .github/workflows/append_file.sh "$f" "$OUTPUT" || true; fi
          done

      - name: "Step 7: 生成專案包 (Map / P / All-code 拆分)"
        run: |
          jq -c '.[]' /tmp/projects.json | while read -r row; do
            REPO=$(echo "$row" | jq -r '.repo')
            PKG_NO=$(echo "$row" | jq -r '.pkg_no')
            DOMAIN=$(echo "$row" | jq -r '.domain')
            CODE=$(echo "$row" | jq -r '.code')
            SAFE_NAME=$(echo "$REPO" | awk -F/ '{print $2}')
            
            # 定義三個檔案輸出路徑
            OUTPUT_MAP="build-repo/${PKG_NO}-${SAFE_NAME}-Map.md"
            OUTPUT_P="build-repo/${PKG_NO}-${SAFE_NAME}-P.md"
            OUTPUT_CODE="build-repo/${PKG_NO}-${SAFE_NAME}-All-code.md"

            # 初始化標頭
            echo "# 專案地圖包：${SAFE_NAME}" > "$OUTPUT_MAP"
            echo "" >> "$OUTPUT_MAP"
            echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" >> "$OUTPUT_MAP"
            echo "" >> "$OUTPUT_MAP"

            echo "# 專案 P 檔總集：${SAFE_NAME}" > "$OUTPUT_P"
            echo "" >> "$OUTPUT_P"
            echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" >> "$OUTPUT_P"
            echo "" >> "$OUTPUT_P"

            echo "# 專案全程式碼包：${SAFE_NAME}" > "$OUTPUT_CODE"
            echo "" >> "$OUTPUT_CODE"
            echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" >> "$OUTPUT_CODE"
            echo "" >> "$OUTPUT_CODE"

            # 1. 填充 Map 包 (meta.json + 檔案樹)
            if [ -f "external/${SAFE_NAME}/meta.json" ]; then
              echo "## meta.json" >> "$OUTPUT_MAP"
              bash .github/workflows/append_file.sh "external/${SAFE_NAME}/meta.json" "$OUTPUT_MAP" || true
              echo "" >> "$OUTPUT_MAP"
            fi

            if [ -d "external/${SAFE_NAME}" ]; then
              (
                cd "external/${SAFE_NAME}" || exit 0
                find . -type f \( -name "*.js" -o -name "*.html" -o -name "*.css" -o -name "*.json" -o -name "*.yml" -o -name "*.md" \) -not -path "./.git/*" -not -path "./node_modules/*" | sort > /tmp/all_files.txt || true
                if [ -s /tmp/all_files.txt ]; then
                  {
                    echo "## 程式碼檔案樹（僅列路徑，不讀內容）"
                    echo ""
                    echo "總檔數：$(wc -l < /tmp/all_files.txt)"
                    echo ""
                    sed 's|^\./||' /tmp/all_files.txt
                  } >> "../../$OUTPUT_MAP"
                fi
              ) || true
            fi

            # 2. 填充 P 包 (智慧抓取所有 P-*.md 與 WF-*.md)
            P_FOUND=false
            for P_DIR_NAME in "web-app-family" "webapp" "${SAFE_NAME}" "AI-WEB-APP-Family"; do
              P_DIR="external/${SAFE_NAME}/docs/projects/${P_DIR_NAME}"
              if [ -d "$P_DIR" ]; then
                P_FOUND=true
                for f in ${P_DIR}/*.md; do
                  if [ -f "$f" ]; then bash .github/workflows/append_file.sh "$f" "$OUTPUT_P" || true; fi
                done
                break
              fi
            done
            
            if [ "$P_FOUND" = false ] && [ -d "external/${SAFE_NAME}/docs/projects" ]; then
               (
                 cd "external/${SAFE_NAME}" || exit 0
                 find docs/projects -type f \( -name "P-*.md" -o -name "WF-*.md" \) -not -path "*/.git/*" | sort | while read -r f; do
                   bash "../../.github/workflows/append_file.sh" "$f" "../../$OUTPUT_P" || true
                 done
               ) || true
            fi

            # 3. 填充 All-code 包
            if [ -d "external/${SAFE_NAME}" ]; then
              (
                cd "external/${SAFE_NAME}" || exit 0
                find . -type f \( -name "*.js" -o -name "*.html" -o -name "*.css" -o -name "*.json" \) \
                  -not -path "./.git/*" \
                  -not -path "./node_modules/*" \
                  -not -path "./docs/*" \
                  -not -path "./.github/*" | sort | while read -r f; do
                  if [ -f "$f" ]; then 
                    bash "../../.github/workflows/append_file.sh" "$f" "../../$OUTPUT_CODE" || true
                  fi
                done
              ) || true
            fi
          done

      - name: "Step 7.5: 生成並更新專案 WF-STATE-INDEX"
        run: |
          jq -c '.[]' /tmp/projects.json | while read -r row; do
            REPO=$(echo "$row" | jq -r '.repo')
            PKG_NO=$(echo "$row" | jq -r '.pkg_no')
            DOMAIN=$(echo "$row" | jq -r '.domain')
            CODE=$(echo "$row" | jq -r '.code')
            DISPLAY=$(echo "$row" | jq -r '.display // "-"')
            SAFE_NAME=$(echo "$REPO" | awk -F/ '{print $2}')
            
            PROJ_DIR="external/${SAFE_NAME}/docs/projects"
            if [ ! -d "$PROJ_DIR" ]; then continue; fi

            TARGET_DIR=$(find "$PROJ_DIR" -maxdepth 1 -mindepth 1 -type d | head -n 1)
            if [ -z "$TARGET_DIR" ]; then continue; fi

            TMP_TABLE="/tmp/state-index-${SAFE_NAME}.txt"
            > "$TMP_TABLE"
            
            find "$TARGET_DIR" -type f -name "*.md" -not -name "WF-STATE-INDEX.md" | sort | while read -r f; do
              V=$(tail -n 15 "$f" | grep -oE 'V: *[0-9]+' | head -1 | sed 's/V: *//' || true)
              if [ -z "$V" ]; then continue; fi
              W=$(tail -n 15 "$f" | grep -oE 'W: *[^ ]+' | head -1 | sed 's/W: *//' || true)
              FNAME=$(basename "$f")
              echo "| $FNAME | $V | $W |" >> "$TMP_TABLE"
            done

            OUTPUT="$TARGET_DIR/WF-STATE-INDEX.md"
            TIMESTAMP=$(date +%s)
            
            {
              echo "# WF-STATE-INDEX（專案檔案狀態索引）"
              echo ""
              echo "最後更新：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)"
              echo "Version: $TIMESTAMP"
              echo "Writer: github-actions[bot]"
              echo "用途：記錄所有已交付檔案的版本與清單（由體系自動生成）"
              echo ""
              echo "---"
              echo ""
              echo "## 一、專案總覽"
              echo ""
              echo "| 專案顯示名稱 | code | domain | package_no | repo |"
              echo "|---|---|---|---|---|"
              echo "| $DISPLAY | $CODE | $DOMAIN | $PKG_NO | $REPO |"
              echo ""
              echo "## 二、檔案清單（自動生成）"
              echo ""
              echo "| 檔案 | 版本 (V) | 編寫者 (W) |"
              echo "|---|---|---|"
              if [ -s "$TMP_TABLE" ]; then sort -t'|' -k2 -nr "$TMP_TABLE"; fi
              echo ""
              echo "/* ═══ EOF ═══"
              echo "F: WF-STATE-INDEX.md"
              echo "V: $TIMESTAMP"
              echo "W: github-actions[bot]"
              echo "═══ */"
            } > "$OUTPUT"
            
            echo "Generated $OUTPUT"
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

          for wf in setup-structure.yml migrate.yml append_file.sh; do
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

          # === AI-kernel 區塊 ===
          echo "## 🏢 體系層：AI-kernel 的呼叫器" >> "$OUTPUT"
          echo "" >> "$OUTPUT"
          mkdir -p "AI-kernel/.github/workflows"
          
          curl -s -f -L -H "Authorization: token ${{ secrets.CROSS_REPO_PAT }}" "https://raw.githubusercontent.com/${{ env.KERNEL_REPO }}/main/.github/workflows/apply-patch.yml" -o "AI-kernel/.github/workflows/apply-patch.yml" || true
          if [ -s "AI-kernel/.github/workflows/apply-patch.yml" ]; then
            echo "### 📄 檔案：.github/workflows/apply-patch.yml" >> "$OUTPUT"
            echo "" >> "$OUTPUT"
            bash .github/workflows/append_file.sh "AI-kernel/.github/workflows/apply-patch.yml" "$OUTPUT" || true
            echo "" >> "$OUTPUT"
          else
            echo "> ⚠️ 找不到 \`AI-kernel/.github/workflows/apply-patch.yml\`" >> "$OUTPUT"
          fi

          curl -s -f -L -H "Authorization: token ${{ secrets.CROSS_REPO_PAT }}" "https://raw.githubusercontent.com/${{ env.KERNEL_REPO }}/main/.github/workflows/notify.yml" -o "AI-kernel/.github/workflows/notify.yml" || true
          if [ -s "AI-kernel/.github/workflows/notify.yml" ]; then
            echo "### 📄 檔案：.github/workflows/notify.yml" >> "$OUTPUT"
            echo "" >> "$OUTPUT"
            bash .github/workflows/append_file.sh "AI-kernel/.github/workflows/notify.yml" "$OUTPUT" || true
            echo "" >> "$OUTPUT"
          else
            echo "> ⚠️ 找不到 \`AI-kernel/.github/workflows/notify.yml\`" >> "$OUTPUT"
          fi
          
          if [ -f "AI-kernel/.gitignore" ]; then 
            echo "### 📄 檔案：.gitignore (AI-kernel)" >> "$OUTPUT"
            bash .github/workflows/append_file.sh "AI-kernel/.gitignore" "$OUTPUT" || true
            echo "" >> "$OUTPUT"
          fi

          # === AI-General 區塊 ===
          echo "## 🏢 體系層：AI-General 的呼叫器" >> "$OUTPUT"
          echo "" >> "$OUTPUT"
          mkdir -p "AI-General/.github/workflows"
          
          curl -s -f -L -H "Authorization: token ${{ secrets.CROSS_REPO_PAT }}" "https://raw.githubusercontent.com/${{ env.GENERAL_REPO }}/main/.github/workflows/apply-patch.yml" -o "AI-General/.github/workflows/apply-patch.yml" || true
          if [ -s "AI-General/.github/workflows/apply-patch.yml" ]; then
            echo "### 📄 檔案：.github/workflows/apply-patch.yml" >> "$OUTPUT"
            echo "" >> "$OUTPUT"
            bash .github/workflows/append_file.sh "AI-General/.github/workflows/apply-patch.yml" "$OUTPUT" || true
            echo "" >> "$OUTPUT"
          else
            echo "> ⚠️ 找不到 \`AI-General/.github/workflows/apply-patch.yml\`" >> "$OUTPUT"
          fi

          curl -s -f -L -H "Authorization: token ${{ secrets.CROSS_REPO_PAT }}" "https://raw.githubusercontent.com/${{ env.GENERAL_REPO }}/main/.github/workflows/notify.yml" -o "AI-General/.github/workflows/notify.yml" || true
          if [ -s "AI-General/.github/workflows/notify.yml" ]; then
            echo "### 📄 檔案：.github/workflows/notify.yml" >> "$OUTPUT"
            echo "" >> "$OUTPUT"
            bash .github/workflows/append_file.sh "AI-General/.github/workflows/notify.yml" "$OUTPUT" || true
            echo "" >> "$OUTPUT"
          else
            echo "> ⚠️ 找不到 \`AI-General/.github/workflows/notify.yml\`" >> "$OUTPUT"
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

      - name: "Step 8.5: 生成控制台共享包 (92-Ai-github.md)"
        run: |
          OUTPUT="build-repo/92-Ai-github.md"
          echo "# 92 包：AI 控制台原始碼" > "$OUTPUT"
          echo "" >> "$OUTPUT"
          echo "> 生成時間：$(TZ=Asia/Hong_Kong date '+%Y-%m-%d %H:%M:%S') (HKT)" >> "$OUTPUT"
          echo "" >> "$OUTPUT"
          
          for f in index.html manifest.json sw.js; do
            if [ -f "$f" ]; then 
              bash .github/workflows/append_file.sh "$f" "$OUTPUT" || true
            fi
          done

      - name: "Step 9: 上傳共享包與專案索引"
        run: |
          # 1. 提交體系 STATE-INDEX
          cd AI-General
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          git add docs/records/STATE-INDEX.md
          git diff --cached --quiet || (
            git commit -m "chore: auto-update STATE-INDEX [skip ci]" && 
            (git pull --rebase origin main || true) && 
            git push
          )
          cd ..

          # 2. 提交專案 WF-STATE-INDEX
          jq -c '.[]' /tmp/projects.json | while read -r row; do
            REPO=$(echo "$row" | jq -r '.repo')
            SAFE_NAME=$(echo "$REPO" | awk -F/ '{print $2}')
            
            if [ -d "external/${SAFE_NAME}" ]; then
              cd "external/${SAFE_NAME}"
              git config user.name "github-actions[bot]"
              git config user.email "github-actions[bot]@users.noreply.github.com"
              
              if [ -d "docs/projects" ]; then
                TARGET_FILES=$(find docs/projects -name "WF-STATE-INDEX.md")
              else
                TARGET_FILES=""
              fi

              if [ -n "$TARGET_FILES" ]; then
                git add $TARGET_FILES
                if ! git diff --cached --quiet; then
                  git commit -m "chore: auto-update WF-STATE-INDEX [skip ci]"
                  git pull --rebase origin main || true
                  git push
                  echo "Pushed WF-STATE-INDEX for $SAFE_NAME"
                else
                  echo "No changes in WF-STATE-INDEX for $SAFE_NAME"
                fi
              else
                echo "No WF-STATE-INDEX generated for $SAFE_NAME"
              fi
              cd ../..
            fi
          done

          # 3. 提交 build-repo 共享包
          cd build-repo
          git config user.name "github-actions[bot]"
          git config user.email "github-actions[bot]@users.noreply.github.com"
          git add -A
          git diff --cached --quiet || (
            git commit -m "chore: auto-build packages [skip ci]" && 
            (git pull --rebase origin main || true) && 
            git push
          )```

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

### 📄 檔案：.github/workflows/apply-patch.yml


<!-- === FILE START: AI-kernel/.github/workflows/apply-patch.yml === -->

## 📄 AI-kernel/.github/workflows/apply-patch.yml

```yml
name: Apply Patch (AI-kernel)
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
# F: AI-kernel/.github/workflows/apply-patch.yml
# V: 1792401000
# W: ai-system-20261019-010#187
# ═══ EOF ═══```

<!-- === FILE END: AI-kernel/.github/workflows/apply-patch.yml === -->

### 📄 檔案：.github/workflows/notify.yml


<!-- === FILE START: AI-kernel/.github/workflows/notify.yml === -->

## 📄 AI-kernel/.github/workflows/notify.yml

```yml
name: Notify AI-infra

on:
  push:
    branches: [main, master]
    paths:
      - '**.md'
      - '.github/workflows/notify.yml'

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
# F: AI-kernel/.github/workflows/notify.yml
# V: 1792401000
# W: ai-system-20261019-010#187
# ═══ EOF ═══```

<!-- === FILE END: AI-kernel/.github/workflows/notify.yml === -->

### 📄 檔案：.gitignore (AI-kernel)

<!-- === FILE START: AI-kernel/.gitignore === -->

## 📄 AI-kernel/.gitignore

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

<!-- === FILE END: AI-kernel/.gitignore === -->

## 🏢 體系層：AI-General 的呼叫器

### 📄 檔案：.github/workflows/apply-patch.yml


<!-- === FILE START: AI-General/.github/workflows/apply-patch.yml === -->

## 📄 AI-General/.github/workflows/apply-patch.yml

```yml
name: Apply Patch (AI-General)
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
# F: AI-General/.github/workflows/apply-patch.yml
# V: 1792401000
# W: ai-system-20261019-010#187
# ═══ EOF ═══```

<!-- === FILE END: AI-General/.github/workflows/apply-patch.yml === -->

### 📄 檔案：.github/workflows/notify.yml


<!-- === FILE START: AI-General/.github/workflows/notify.yml === -->

## 📄 AI-General/.github/workflows/notify.yml

```yml
name: Notify AI-infra

on:
  push:
    branches: [main, master]
    paths:
      - '**.md'
      - '.github/workflows/notify.yml'
    paths-ignore:
      - 'docs/records/STATE-INDEX.md'

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
# F: AI-General/.github/workflows/notify.yml
# V: 1792401000
# W: ai-system-20261019-010#187
# ═══ EOF ═══```

<!-- === FILE END: AI-General/.github/workflows/notify.yml === -->

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
      - 'docs/projects/**/WF-STATE-INDEX.md' # 👈 新增這行，避免自動生成的索引檔觸發通知

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
          echo "已通知 AI-infra 重新建置"```

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

