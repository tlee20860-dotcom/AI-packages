> 體系版本：SV08
> 生成時間：2026-10-27 20:12:14 (HKT)

# 專案全程式碼包：AI-WA-UPWO


<!-- === FILE START: ./index.html === -->

## 📄 ./index.html

```html
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>AI 控制台 - 管理中心</title>
    <link rel="manifest" href="manifest.json">
    <meta name="theme-color" content="#6366f1">
    <style>
        :root {
            --primary: #6366f1; --primary-hover: #4f46e5; --bg: #f8fafc;
            --card-bg: #ffffff; --text: #1e293b; --text-muted: #64748b;
            --success: #10b981; --danger: #ef4444; --border: #e2e8f0;
            --warning: #f59e0b;
        }
        * { box-sizing: border-box; margin: 0; padding: 0; font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif; }
        body { background-color: var(--bg); color: var(--text); padding: 15px; line-height: 1.5; -webkit-tap-highlight-color: transparent; padding-bottom: 80px; }
        .container { max-width: 600px; margin: 0 auto; }
        
        .card { background: var(--card-bg); border-radius: 12px; padding: 15px; margin-bottom: 15px; box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1); border: 1px solid var(--border); }
        .card-title { font-size: 1.1rem; font-weight: bold; margin-bottom: 4px; display: flex; align-items: center; gap: 8px; color: var(--text); }
        .subtitle { color: var(--text-muted); font-size: 0.8rem; margin-bottom: 12px; }
        
        .sticky-header { 
            position: sticky; 
            top: 0; 
            background: var(--bg); 
            z-index: 100; 
            padding-top: 15px; 
            margin-top: -15px;
            margin-bottom: 15px; 
        }
        
        input[type="text"], input[type="password"], textarea { width: 100%; padding: 12px; border: 1px solid var(--border); border-radius: 8px; font-size: 0.95rem; margin-bottom: 10px; font-family: inherit; }
        textarea { resize: vertical; min-height: 120px; font-family: monospace; }
        button { width: 100%; padding: 12px; border: none; border-radius: 8px; font-size: 1rem; font-weight: bold; cursor: pointer; transition: background-color 0.2s, opacity 0.2s; display: flex; justify-content: center; align-items: center; gap: 8px; }
        button:disabled { opacity: 0.6; cursor: not-allowed; }
        .btn-primary { background-color: var(--primary); color: white; }
        .btn-success { background-color: var(--success); color: white; }
        .btn-danger { background-color: var(--danger); color: white; }
        .btn-small { width: auto; padding: 6px 10px; font-size: 0.8rem; border-radius: 6px; }
        
        .status-box { padding: 10px; border-radius: 8px; background-color: #f1f5f9; font-size: 0.85rem; display: flex; align-items: center; justify-content: center; gap: 8px; margin-top: 10px; width: 100%; }
        .status-dot { width: 10px; height: 10px; border-radius: 50%; background-color: var(--text-muted); flex-shrink: 0; }
        .status-dot.running { background-color: var(--warning); animation: pulse 1.5s infinite; }
        .status-dot.success { background-color: var(--success); }
        .status-dot.failure { background-color: var(--danger); }
        @keyframes pulse { 0% { transform: scale(0.95); opacity: 0.7; } 50% { transform: scale(1.1); opacity: 1; } 100% { transform: scale(0.95); opacity: 0.7; } }

        .error-panel, .success-panel { display: none; padding: 15px; border-radius: 8px; margin-bottom: 15px; }
        .error-panel { background-color: #fef2f2; border: 1px solid #fecaca; color: #991b1b; }
        .success-panel { background-color: #ecfdf5; border: 1px solid #a7f3d0; color: #065f46; }
        .error-list { font-size: 0.85rem; margin-bottom: 10px; white-space: pre-wrap; font-family: monospace; }
        
        details { margin-bottom: 15px; }
        summary { cursor: pointer; font-weight: bold; padding: 12px 15px; background: #f8fafc; border-radius: 8px; border: 1px solid var(--border); outline: none; display: flex; align-items: center; gap: 8px; }
    </style>
    
    <script>
        const OWNER = 'tlee20860-dotcom';
        const INFRA_REPO = 'AI-infra';
        const WORKFLOW_ID = 'setup-structure.yml';
        const CONFIG_URL = `https://raw.githubusercontent.com/${OWNER}/AI-kernel/main/docs/r-rules/validate-config.json`;
        const FORBIDDEN_URL = `https://raw.githubusercontent.com/${OWNER}/AI-kernel/main/docs/r-rules/forbidden_words.txt`;
        const REPOS_URL = `https://raw.githubusercontent.com/${OWNER}/AI-infra/main/config/repos.json`;

        let currentToken = '';
        let tokenList = [];
        let triggerTime = 0;
        let pollInterval = null;
        let hasUnpackedChanges = false;

        let globalForbiddenWords = [];
        let globalConfig = { console_rules: {}, file_specific_rules: {} };
        let allowedRepos = [];

        window.onload = async () => {
            loadTokens(); 
            await loadDictionaries();
            if (currentToken) checkBuildStatus();
            else updateStatus('build-status', '', '等待操作');

            if ('serviceWorker' in navigator) {
                navigator.serviceWorker.register('sw.js').catch(err => console.log('SW fail:', err));
            }
        };

        async function loadDictionaries() {
            try {
                const txtRes = await fetch(`${FORBIDDEN_URL}?t=${Date.now()}`);
                if (txtRes.ok) {
                    const text = await txtRes.text();
                    globalForbiddenWords = text.split('\n').map(w => w.trim()).filter(w => w && !w.startsWith('#'));
                }
                
                const jsonRes = await fetch(`${CONFIG_URL}?t=${Date.now()}`);
                if (jsonRes.ok) {
                    globalConfig = await jsonRes.json();
                    if (globalConfig.console_rules && globalConfig.console_rules.base_system_repos) {
                        allowedRepos = [...globalConfig.console_rules.base_system_repos];
                    }
                }

                const reposRes = await fetch(`${REPOS_URL}?t=${Date.now()}`);
                if (reposRes.ok) {
                    const reposData = await reposRes.json();
                    if (reposData.projects) {
                        reposData.projects.forEach(p => {
                            const repoName = p.repo.split('/')[1];
                            if (repoName && !allowedRepos.includes(repoName)) {
                                allowedRepos.push(repoName);
                            }
                        });
                    }
                }
            } catch (e) {
                console.warn("⚠️ 無法載入遠端配置，將使用預設值。", e);
            }
        }

        function loadTokens() {
            const stored = localStorage.getItem('gh_token_list');
            if (stored) {
                try {
                    tokenList = JSON.parse(stored);
                    if (tokenList.length > 0) {
                        const active = tokenList.find(t => t.isActive);
                        currentToken = active ? active.token.trim() : tokenList[0].token.trim();
                    }
                } catch (e) { console.error(e); }
            }
            renderTokens();
        }
        function saveTokensToLocal() { localStorage.setItem('gh_token_list', JSON.stringify(tokenList)); }
        function addToken() {
            const name = document.getElementById('new-token-name').value.trim();
            const tokenStr = document.getElementById('new-token-value').value.trim();
            if (!name || !tokenStr) return alert('請填寫名稱與 Token');
            const newToken = { id: Date.now().toString(), name, token: tokenStr, isActive: tokenList.length === 0 };
            tokenList.push(newToken); saveTokensToLocal();
            if (newToken.isActive) currentToken = newToken.token;
            document.getElementById('new-token-name').value = ''; document.getElementById('new-token-value').value = '';
            renderTokens(); checkBuildStatus();
        }
        function setActive(id) {
            tokenList.forEach(t => { t.isActive = (t.id === id); if (t.isActive) currentToken = t.token.trim(); });
            saveTokensToLocal(); renderTokens(); checkBuildStatus();
        }
        function deleteToken(id) {
            if (!confirm('確定刪除？')) return;
            tokenList = tokenList.filter(t => t.id !== id);
            if (tokenList.length > 0) { setActive(tokenList[0].id); } else { currentToken = ''; saveTokensToLocal(); renderTokens(); updateStatus('build-status', '', '等待操作'); }
        }
        function copyToken(tokenStr) { navigator.clipboard.writeText(tokenStr).then(() => alert('已複製 Token')); }
        function renderTokens() {
            const listEl = document.getElementById('token-list');
            listEl.innerHTML = tokenList.length === 0 ? '<div style="text-align:center;color:#999;font-size:0.8rem; padding:10px;">無 Token</div>' : '';
            tokenList.forEach(t => {
                const item = document.createElement('div');
                item.style = "display:flex; justify-content:space-between; align-items:center; padding:8px 0; border-bottom:1px solid #eee;";
                const masked = t.token.substring(0, 4) + '****' + t.token.substring(t.token.length - 4);
                item.innerHTML = `
                    <div><span style="font-weight:bold; font-size:0.9rem;">${t.name} ${t.isActive ? '✅' : ''}</span><br><span style="font-size:0.75rem;color:#666;">${masked}</span></div>
                    <div style="display:flex; gap:5px;">
                        ${!t.isActive ? `<button class="btn-primary btn-small" onclick="setActive('${t.id}')">啟用</button>` : ''}
                        <button class="btn-primary btn-small" style="background:#64748b;" onclick="copyToken('${t.token}')">複製</button>
                        <button class="btn-danger btn-small" onclick="deleteToken('${t.id}')">刪除</button>
                    </div>`;
                listEl.appendChild(item);
            });
        }

        async function githubAPI(url, options = {}) {
            if (!currentToken) throw new Error("未啟用 GitHub Token");
            const headers = { 'Accept': 'application/vnd.github.v3+json', 'Authorization': `token ${currentToken.trim()}`, ...options.headers };
            
            const controller = new AbortController();
            const timeoutId = setTimeout(() => controller.abort(), 15000);

            let res;
            try {
                res = await fetch(`https://api.github.com${url}?t=${Date.now()}`, { ...options, headers, signal: controller.signal });
            } catch (fetchError) {
                if (fetchError.name === 'AbortError') {
                    throw new Error("連線超時（15秒），請檢查手機網路或稍後重試。");
                }
                throw new Error("無法連線至 GitHub（請確認 Token 無多餘空白，或關閉廣告攔截器）");
            } finally {
                clearTimeout(timeoutId);
            }

            if (!res.ok) {
                let errMsg = `API 錯誤: ${res.status}`;
                try { const errData = await res.json(); if (errData.message) errMsg += ` - ${errData.message}`; } catch(e) {}
                if (res.status === 401) throw new Error('Token 無效或過期');
                if (res.status === 403) throw new Error('Token 權限不足');
                if (res.status === 404) throw new Error('找不到資源');
                throw new Error(errMsg);
            }
            return res.status === 204 ? null : await res.json();
        }

        async function triggerBuild() {
            if (!currentToken) return alert('請先設定 Token');
            const btn = document.getElementById('btn-trigger');
            btn.disabled = true; triggerTime = Date.now();
            updateStatus('build-status', 'running', '發送指令...');
            
            hasUnpackedChanges = false;

            try {
                await githubAPI(`/repos/${OWNER}/${INFRA_REPO}/actions/workflows/${WORKFLOW_ID}/dispatches`, {
                    method: 'POST', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify({ ref: 'main' })
                });
                updateStatus('build-status', 'running', '等待排隊...');
                if (pollInterval) clearInterval(pollInterval);
                setTimeout(checkBuildStatus, 3000);
                pollInterval = setInterval(checkBuildStatus, 8000);
            } catch (error) { updateStatus('build-status', 'failure', error.message); btn.disabled = false; }
        }

        async function checkBuildStatus() {
            if (!currentToken) return;
            try {
                const data = await githubAPI(`/repos/${OWNER}/${INFRA_REPO}/actions/workflows/${WORKFLOW_ID}/runs?per_page=1`);
                if (data.workflow_runs && data.workflow_runs.length > 0) {
                    const run = data.workflow_runs[0];
                    const runTime = new Date(run.created_at).getTime();
                    const createdAt = new Date(run.created_at).toLocaleString('zh-TW', { timeZone: 'Asia/Hong_Kong' });
                    if (triggerTime > 0 && runTime < triggerTime - 10000) {
                        updateStatus('build-status', 'running', '排隊中...'); return;
                    }
                    if (run.status === 'in_progress' || run.status === 'queued') {
                        updateStatus('build-status', 'running', `打包中...`);
                    } else if (run.conclusion === 'success') {
                        updateStatus('build-status', 'success', `上次成功 (${createdAt})`);
                        document.getElementById('btn-trigger').disabled = false; triggerTime = 0;
                        if (pollInterval) { clearInterval(pollInterval); pollInterval = null; }
                    } else {
                        updateStatus('build-status', 'failure', `上次失敗 (${createdAt})`);
                        document.getElementById('btn-trigger').disabled = false; triggerTime = 0;
                        if (pollInterval) { clearInterval(pollInterval); pollInterval = null; }
                    }
                } else { updateStatus('build-status', '', '尚無紀錄'); document.getElementById('btn-trigger').disabled = false; }
            } catch (error) { updateStatus('build-status', 'failure', error.message); document.getElementById('btn-trigger').disabled = false; }
        }

        function updateStatus(prefix, status, msg) {
            const dot = document.getElementById(`${prefix}-dot`); const text = document.getElementById(`${prefix}-text`);
            if(dot) dot.className = `status-dot ${status}`; if(text) text.textContent = msg;
        }

        function processFile() {
            const content = document.getElementById('file-input').value;
            const errorPanel = document.getElementById('error-panel');
            const successPanel = document.getElementById('success-panel');
            const inputArea = document.getElementById('input-area');
            const forceBtn = document.getElementById('btn-force-publish');
            
            errorPanel.style.display = 'none';
            successPanel.style.display = 'none';
            forceBtn.style.display = 'none';

            if (!content.trim()) return alert('請先貼上內容');

            let errors = [];
            let repo = "", filePath = "", fullPath = "未知路徑";
            const rules = globalConfig.console_rules || {};

            // 優先匹配標準的 EOF 結構 (F, V, W)
            let eofMatch = content.match(/F:\s*([^\s\n]+)\s*\nV:\s*\d+\s*\nW:\s*[^\s\n]+/);
            let pathMatch = null;
            
            if (eofMatch) {
                pathMatch = eofMatch[0].match(/F:\s*([^\s\n]+)/);
            } else {
                pathMatch = content.match(/(?:F:|"F":)\s*"?([^\s"\n]+)"?/);
            }
            
            if (!pathMatch) {
                errors.push("找不到 F: 路徑標記。請確認 AI 是否漏寫 EOF。");
            } else {
                fullPath = pathMatch[1];
                const pathParts = fullPath.split('/');
                if (pathParts.length < 2) {
                    errors.push(`路徑格式錯誤：${fullPath}。必須包含倉庫名。`);
                } else {
                    repo = pathParts[0];
                    filePath = pathParts.slice(1).join('/');
                    
                    if (!allowedRepos.includes(repo)) {
                        if (repo === 'src' || repo === 'public' || repo === 'functions') {
                            errors.push(`路徑缺少倉庫前綴：${fullPath}。請確認 AI 是否漏寫了專案倉庫名（如 AI-WA-UPWO/src/...）。`);
                        } else {
                            errors.push(`未知的倉庫目標：${repo}。請確認是否已在 config/repos.json 中註冊。`);
                        }
                    }
                    document.getElementById('target-path-display').textContent = `目標：${repo} / ${filePath}`;
                    document.getElementById('target-repo').value = repo;
                    document.getElementById('target-file-path').value = filePath;
                }
            }

            let specificRule = (globalConfig.file_specific_rules && globalConfig.file_specific_rules[`${repo}/${filePath}`]) || {};

            let isRecordFile = false;
            if (rules.record_auto_exempt !== false && repo === rules.record_exempt_repo) {
                const exemptPaths = rules.record_exempt_paths || [];
                isRecordFile = exemptPaths.some(p => filePath.includes(p));
            }

            const isCodeFile = filePath.endsWith('.js') || filePath.endsWith('.css') || filePath.endsWith('.html') || filePath.endsWith('.py') || filePath.endsWith('.sh') || filePath.endsWith('.json');

            let cleanContent = content.replace(/```[\s\S]*?```/g, '').replace(/~~~[\s\S]*?~~~/g, '');
            cleanContent = cleanContent.replace(/「[^」]*」/g, '').replace(/`[^`]*`/g, '');

            if (isCodeFile) {
                const lazyPatterns = [
                    { regex: /\/\/\s*\.\.\./, msg: "發現偷懶省略：// ..." },
                    { regex: /\/\*\s*\.\.\./, msg: "發現偷懶省略：/* ..." },
                    { regex: /<!--\s*\.\.\./, msg: "發現偷懶省略：<!-- ..." },
                    { regex: /\/\/\s*(其餘|以下|中間|其他).{0,5}(不變|省略|同上)/, msg: "發現偷懶省略：// 其餘不變" },
                    { regex: /<!--\s*(其餘|以下|中間|其他).{0,5}(不變|省略|同上)/, msg: "發現偷懶省略：<!-- 其餘不變" }
                ];

                lazyPatterns.forEach(p => {
                    if (p.regex.test(cleanContent)) errors.push(p.msg);
                });

                // 安全檢查「省略」或「略」（替代 Lookbehind，避免舊手機 JS 崩潰）
                let hasLue = false;
                let matchIdx = cleanContent.indexOf('略');
                while (matchIdx !== -1) {
                    let prefix = cleanContent[matchIdx - 1];
                    if (!['忽', '省', '策', '戰', '大', '方', '概'].includes(prefix)) {
                        hasLue = true;
                        break;
                    }
                    matchIdx = cleanContent.indexOf('略', matchIdx + 1);
                }
                
                if (cleanContent.includes("省略") || hasLue) {
                    errors.push("發現禁用詞彙：程式碼中不允許出現「省略」或「略」字");
                }

            } else {
                if (specificRule.check_forbidden_words !== false && !isRecordFile) {
                    globalForbiddenWords.forEach(word => {
                        let isViolation = false;
                        if (word === '略') {
                            let matchIdx = cleanContent.indexOf('略');
                            while (matchIdx !== -1) {
                                let prefix = cleanContent[matchIdx - 1];
                                if (!['忽', '省', '策', '戰', '大', '方', '概'].includes(prefix)) {
                                    isViolation = true;
                                    break;
                                }
                                matchIdx = cleanContent.indexOf('略', matchIdx + 1);
                            }
                        } else {
                            if (cleanContent.includes(word)) {
                                isViolation = true;
                            }
                        }
                        if (isViolation) {
                            errors.push(`發現禁用詞彙：「${word}」`);
                        }
                    });
                }

                if (specificRule.check_simplified !== false) {
                    const simplified = globalConfig.simplified_chars || [];
                    simplified.forEach(word => {
                        if (cleanContent.includes(word)) errors.push(`發現簡體字：「${word}」`);
                    });
                }
            }

            if (errors.length > 0) {
                errorPanel.style.display = 'block';
                inputArea.style.display = 'none';
                
                let header = "";
                if (rules.show_file_path_in_error !== false) {
                    header = (rules.error_header_format || "📄 檔案：{path}\n------------------------\n").replace("{path}", fullPath);
                }
                document.getElementById('error-list').textContent = header + errors.join('\n');

                if (repo && filePath) {
                    forceBtn.style.display = 'block';
                }
            } else {
                successPanel.style.display = 'block';
                document.getElementById('success-message').textContent = '✅ 體檢通過！請點擊下方按鈕發布。';
                document.getElementById('btn-publish').style.display = 'block';
                forceBtn.style.display = 'none';
            }
        }

        function resetUI() {
            document.getElementById('file-input').value = '';
            document.getElementById('error-panel').style.display = 'none';
            document.getElementById('success-panel').style.display = 'none';
            document.getElementById('input-area').style.display = 'block';
            document.getElementById('btn-publish').style.display = 'none';
            document.getElementById('btn-reset-next').style.display = 'none';
            
            const btn = document.getElementById('btn-publish');
            btn.disabled = false; 
            btn.textContent = '🚀 確認發布到倉庫';

            const forceBtn = document.getElementById('btn-force-publish');
            forceBtn.style.display = 'none';
            forceBtn.disabled = false;
            forceBtn.textContent = '🚨 強制發布（忽略錯誤）';
        }

        function copyError() {
            const errorText = document.getElementById('error-list').textContent;
            navigator.clipboard.writeText(`網頁體檢報錯：\n${errorText}`).then(() => alert('已複製錯誤訊息，請貼回對話框。'));
        }

        async function publishFile(isForce = false) {
            if (!currentToken) return alert('請先設定 Token');
            
            let targetBtn = isForce ? document.getElementById('btn-force-publish') : document.getElementById('btn-publish');
            const originalText = targetBtn.textContent;
            targetBtn.disabled = true; targetBtn.textContent = '處理中...';
            
            let content = document.getElementById('file-input').value;
            const repo = document.getElementById('target-repo').value;
            const filePath = document.getElementById('target-file-path').value;

            try {
                let sha = null;
                let originalContent = "";
                try {
                    const existing = await githubAPI(`/repos/${OWNER}/${repo}/contents/${filePath}`);
                    if (existing && existing.sha) {
                        sha = existing.sha;
                        originalContent = decodeURIComponent(escape(atob(existing.content.replace(/\n/g, ''))));
                    }
                } catch (e) {
                    if (!e.message.includes('404')) throw e;
                }

                let finalContent = content;

                const patchRegex = /<<<<<<< OLD\n([\s\S]*?)\n=======\n([\s\S]*?)\n>>>>>>> NEW/;
                const patchMatch = content.match(patchRegex);
                
                if (patchMatch && originalContent) {
                    const oldStr = patchMatch[1];
                    const newStr = patchMatch[2];
                    if (!originalContent.includes(oldStr)) {
                        throw new Error(`找不到要替換的目標字串。`);
                    }
                    finalContent = originalContent.replace(oldStr, newStr);
                } 
                else if (content.includes("[APPEND]") && originalContent) {
                    let base = originalContent;
                    const eofIndex = base.lastIndexOf("/* ═══ EOF ═══");
                    const mdEofIndex = base.lastIndexOf("F:");
                    
                    if (eofIndex > 0) base = base.substring(0, eofIndex).trim();
                    else if (mdEofIndex > 0) {
                        const lineStart = base.lastIndexOf("\n", mdEofIndex);
                        if (lineStart > 0) base = base.substring(0, lineStart).trim();
                    }

                    let appendContent = content.replace("[APPEND]", "").trim();
                    finalContent = base + "\n\n" + appendContent;
                }

                const base64Content = btoa(unescape(encodeURIComponent(finalContent)));

                const body = { message: `Update ${filePath} via Console`, content: base64Content };
                if (sha) body.sha = sha;

                await githubAPI(`/repos/${OWNER}/${repo}/contents/${filePath}`, {
                    method: 'PUT', headers: { 'Content-Type': 'application/json' }, body: JSON.stringify(body)
                });

                const timeStr = new Date().toLocaleTimeString('zh-TW', { hour12: false });
                
                document.getElementById('success-message').innerHTML = `🎉 已成功寫入：<br><b>${repo}/${filePath}</b><br><span style="font-size:0.85rem; color:#047857; font-weight:normal;">⏱️ 寫入時間：${timeStr}</span>`;
                targetBtn.style.display = 'none';
                document.getElementById('error-panel').style.display = 'none';
                document.getElementById('success-panel').style.display = 'block';
                document.getElementById('btn-reset-next').style.display = 'block';

                hasUnpackedChanges = true;
                updateStatus('build-status', 'running', '有新檔案！請點擊打包');

            } catch (error) {
                alert(`發布失敗：${error.message}`);
                targetBtn.disabled = false; 
                targetBtn.textContent = originalText;
            }
        }

        async function createNewProject() {
            if (!currentToken) return alert('請先設定 Token');
            const name = document.getElementById('new-repo-name').value.trim();
            const domain = document.getElementById('new-repo-domain').value;
            const pkgNo = document.getElementById('new-repo-pkg').value.trim();
            const code = document.getElementById('new-repo-code').value.trim();
            const display = document.getElementById('new-repo-display').value.trim();

            if (!name || !pkgNo || !code) return alert('請填寫完整資訊');
            
            const btn = document.getElementById('btn-create-repo');
            btn.disabled = true; btn.textContent = '建立中...';

            try {
                await githubAPI(`/user/repos`, {
                    method: 'POST', body: JSON.stringify({ name: name, private: false, auto_init: true })
                });

                await new Promise(r => setTimeout(r, 2000));
                const v = Math.floor(Date.now() / 1000).toString();

                const metaContent = JSON.stringify({
                    domain: domain, pkg_no: pkgNo, code: code, display: display,
                    _EOF: { F: `${name}/meta.json`, V: v, W: `init-${pkgNo}#1` }
                }, null, 2);
                
                await githubAPI(`/repos/${OWNER}/${name}/contents/meta.json`, {
                    method: 'PUT', body: JSON.stringify({ message: 'Init meta.json', content: btoa(unescape(encodeURIComponent(metaContent))) })
                });

                const wfContent = `# WF-HANDOFF (${display})\n\n## 專案基礎資訊\n\n- 專案編號：${pkgNo}\n- 領域代碼：${domain}\n\n/* ═══ EOF ═══\nF: AI-General/docs/p${pkgNo}/WF-HANDOFF.md\nV: ${v}\nW: init-${pkgNo}#1\n═══ */`;
                
                let wfSha = null;
                try {
                    const existWf = await githubAPI(`/repos/${OWNER}/AI-General/contents/docs/p${pkgNo}/WF-HANDOFF.md`);
                    wfSha = existWf.sha;
                } catch(e) {}

                const wfBody = { message: 'Init WF-HANDOFF', content: btoa(unescape(encodeURIComponent(wfContent))) };
                if (wfSha) wfBody.sha = wfSha;

                await githubAPI(`/repos/${OWNER}/AI-General/contents/docs/p${pkgNo}/WF-HANDOFF.md`, {
                    method: 'PUT', body: JSON.stringify(wfBody)
                });

                alert(`✅ 專案 ${name} 建立成功！`);
                document.getElementById('new-repo-name').value = '';
                document.getElementById('new-repo-pkg').value = '';
                document.getElementById('new-repo-code').value = '';
                document.getElementById('new-repo-display').value = '';
            } catch (error) {
                alert(`建庫失敗：${error.message}`);
            } finally {
                btn.disabled = false; btn.textContent = '🚀 一鍵建立專案';
            }
        }
    </script>
</head>
<body>
<div class="container">
    
    <div class="sticky-header">
        <div class="card">
            <div class="card-title">🚀 AI 控制台</div>
            <div class="subtitle">極簡發布與打包中心</div>
            <button class="btn-primary" id="btn-trigger" onclick="triggerBuild()">▶️ 打包</button>
            <div class="status-box">
                <div class="status-dot" id="build-status-dot"></div>
                <div id="build-status-text">等待操作</div>
            </div>
        </div>
    </div>

    <div class="card">
        <div class="card-title">📦 檔案體檢與發布</div>
        
        <div id="input-area">
            <textarea id="file-input" placeholder="請在此貼上 AI 交付的完整內容..."></textarea>
            <button class="btn-primary" onclick="processFile()">🔍 檢查並準備發布</button>
        </div>

        <div class="error-panel" id="error-panel">
            <div style="font-weight:bold; margin-bottom:10px;">❌ 檢查未通過</div>
            <div class="error-list" id="error-list"></div>
            <button class="btn-primary" style="background:#ef4444; margin-bottom:10px;" onclick="copyError()">📋 一鍵複製錯誤訊息</button>
            <button class="btn-primary" style="background:#64748b;" onclick="resetUI()">🔄 清除並重新貼上</button>
            <button class="btn-danger" id="btn-force-publish" style="display:none; margin-top:10px; background:#991b1b;" onclick="publishFile(true)">🚨 強制發布（忽略錯誤）</button>
        </div>

        <div class="success-panel" id="success-panel">
            <div style="font-weight:bold; margin-bottom:10px;" id="success-message">✅ 體檢通過！</div>
            <div style="font-size:0.85rem; margin-bottom:15px; font-family:monospace; background:#fff; padding:8px; border-radius:5px;" id="target-path-display"></div>
            <input type="hidden" id="target-repo">
            <input type="hidden" id="target-file-path">
            
            <button class="btn-success" id="btn-publish" onclick="publishFile(false)">🚀 確認發布到倉庫</button>
            <button class="btn-primary" id="btn-reset-next" style="display:none; margin-top:10px;" onclick="resetUI()">📝 繼續貼上下一個檔案</button>
            <button class="btn-primary" style="background:#64748b; margin-top:10px;" onclick="resetUI()">取消</button>
        </div>
    </div>

    <div class="card">
        <div class="card-title">🏗️ 建立新專案</div>
        <input type="text" id="new-repo-name" placeholder="Repo 名稱 (如 AI-WA-NEW)">
        <div style="display:flex; gap:10px;">
            <select id="new-repo-domain" style="width:30%; padding:12px; border:1px solid #ccc; border-radius:8px;">
                <option value="W">W (Web)</option>
                <option value="V">V (VBA)</option>
                <option value="D">D (Data)</option>
                <option value="M">M (Mobile)</option>
                <option value="E">E (Engine)</option>
            </select>
            <input type="text" id="new-repo-pkg" placeholder="編號 (如 92)" style="width:30%; margin-bottom:0;">
            <input type="text" id="new-repo-code" placeholder="代碼 (如 UPGRADE)" style="width:40%; margin-bottom:0;">
        </div>
        <input type="text" id="new-repo-display" placeholder="顯示名稱 (如 升級系統)" style="margin-top:10px;">
        <button class="btn-primary" id="btn-create-repo" onclick="createNewProject()">🚀 一鍵建立專案</button>
    </div>

    <details>
        <summary>🔑 憑證管理中心 (Token)</summary>
        <div class="card" style="box-shadow:none; border:none; padding:0; margin-top:10px;">
            <input type="text" id="new-token-name" placeholder="名稱 (如：主要 GitHub)">
            <input type="password" id="new-token-value" placeholder="Token 字串">
            <button class="btn-primary" onclick="addToken()">➕ 新增 Token</button>
            <div style="margin-top: 15px; border-top: 1px solid var(--border);" id="token-list"></div>
        </div>
    </details>

</div>
</body>
</html>
<!-- ═══ EOF ═══
F: AI-infra/index.html
V: 1792910200
W: ai-system-004
═══ -->```

<!-- === FILE END: ./index.html === -->

<!-- === FILE START: ./meta.json === -->

## 📄 ./meta.json

```json
{
  "domain": "W",
  "pkg_no": "92",
  "code": "UPGRADE",
  "display": "UPGRADE-WO"
}```

<!-- === FILE END: ./meta.json === -->

<!-- === FILE START: ./package.json === -->

## 📄 ./package.json

```json
{
  "name": "upgrade-wo",
  "private": true,
  "version": "1.0.0",
  "type": "module",
  "scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
  },
  "dependencies": {
    "firebase": "^10.12.2",
    "jspdf": "^2.5.1",
    "lucide": "^0.395.0",
    "xlsx": "^0.18.5"
  },
  "devDependencies": {
    "autoprefixer": "^10.4.19",
    "postcss": "^8.4.38",
    "tailwindcss": "^3.4.4",
    "vite": "^5.3.1"
  }
}```

<!-- === FILE END: ./package.json === -->

<!-- === FILE START: ./postcss.config.js === -->

## 📄 ./postcss.config.js

```js
export default {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
};

/* ═══ EOF ═══
F: postcss.config.js
V: 1792903400
W: webapp-20261026-001#10
═══ */```

<!-- === FILE END: ./postcss.config.js === -->

<!-- === FILE START: ./src/config/constants.js === -->

## 📄 ./src/config/constants.js

```js
// 系統常數
export const SYSTEM_NAME = 'UPGRADE-WO';

// 預設狀態（當資料庫無資料時使用）
export const DEFAULT_STATUSES = ['排產中', '印刷', '裁切', '品檢', '出貨'];

// 路由名稱常數
export const ROUTES = {
  LOGIN: 'login',
  KANBAN: 'kanban',
  TABLE: 'table',
  OVERVIEW: 'overview',
  SALESPERSON: 'salesperson',
  SETTINGS: 'settings'
};

/* ═══ EOF ═══
F: src/config/constants.js
V: 1792903522
W: webapp-20261026-001#34
═══ */```

<!-- === FILE END: ./src/config/constants.js === -->

<!-- === FILE START: ./src/config/firebase-keys.js === -->

## 📄 ./src/config/firebase-keys.js

```js
export const FIREBASE_CONFIG = {
  apiKey: "AIzaSyCOuxzJXxwmPEDYLxg_VHAwPLYikNq8e1c",
  authDomain: "up-wo-87f60.firebaseapp.com",
  projectId: "up-wo-87f60",
  storageBucket: "up-wo-87f60.firebasestorage.app",
  messagingSenderId: "180994034713",
  appId: "1:180994034713:web:fc143f987d14df99102e88"
};

/* ═══ EOF ═══
F: src/config/firebase-keys.js
V: 1792903501
W: webapp-20261026-001#13
═══ */```

<!-- === FILE END: ./src/config/firebase-keys.js === -->

<!-- === FILE START: ./src/config/firebase.js === -->

## 📄 ./src/config/firebase.js

```js
import { initializeApp } from "firebase/app";
import { getFirestore } from "firebase/firestore";
import { FIREBASE_CONFIG } from "./firebase-keys.js";

// 初始化 Firebase App
const app = initializeApp(FIREBASE_CONFIG);

// 初始化並匯出 Firestore 實例
export const db = getFirestore(app);

/* ═══ EOF ═══
F: src/config/firebase.js
V: 1792903502
W: webapp-20261026-001#14
═══ */```

<!-- === FILE END: ./src/config/firebase.js === -->

<!-- === FILE START: ./src/config/state.js === -->

## 📄 ./src/config/state.js

```js
class State {
  constructor() {
    this.user = null;
    this.statuses = ['排產中', '印刷', '裁切', '品檢', '出貨'];
  }

  // ═══ 使用者相關 ═══
  setUser(user) {
    this.user = user;
    // 存入 sessionStorage 以便重新整理時保持登入狀態
    if (user) {
      sessionStorage.setItem('upgrade_user', JSON.stringify(user));
    } else {
      sessionStorage.removeItem('upgrade_user');
    }
  }

  getUser() {
    if (!this.user) {
      const stored = sessionStorage.getItem('upgrade_user');
      if (stored) {
        try {
          this.user = JSON.parse(stored);
        } catch (e) {
          this.user = null;
        }
      }
    }
    return this.user;
  }

  clearUser() {
    this.user = null;
    sessionStorage.removeItem('upgrade_user');
  }

  // ═══ 狀態相關 ═══
  setStatuses(statuses) {
    if (Array.isArray(statuses) && statuses.length > 0) {
      this.statuses = statuses;
    }
  }

  getStatuses() {
    return this.statuses;
  }
}

// 匯出單一實例 (Singleton)
export const AppState = new State();

/* ═══ EOF ═══
F: src/config/state.js
V: 1792903521
W: webapp-20261026-001#33
═══ */```

<!-- === FILE END: ./src/config/state.js === -->

<!-- === FILE START: ./src/db/read.js === -->

## 📄 ./src/db/read.js

```js
import { db } from "../config/firebase.js";
import { 
  collection, 
  getDocs, 
  onSnapshot, 
  doc, 
  getDoc 
} from "firebase/firestore";

/**
 * 一次性取得整個集合的資料
 * @param {string} collectionName 集合名稱
 * @returns {Promise<Array>} 包含 id 與資料的陣列
 */
export async function getCollection(collectionName) {
  try {
    const colRef = collection(db, collectionName);
    const snapshot = await getDocs(colRef);
    return snapshot.docs.map(doc => ({
      id: doc.id,
      ...doc.data()
    }));
  } catch (error) {
    console.error(`[DB Read Error] getCollection(${collectionName}):`, error);
    throw error;
  }
}

/**
 * 監聽整個集合的即時變更
 * @param {string} collectionName 集合名稱
 * @param {function} callback 回呼函式，接收包含 id 與資料的陣列
 * @returns {function} 取消監聽的函式
 */
export function listenCollection(collectionName, callback) {
  const colRef = collection(db, collectionName);
  
  const unsubscribe = onSnapshot(colRef, (snapshot) => {
    const data = snapshot.docs.map(doc => ({
      id: doc.id,
      ...doc.data()
    }));
    callback(data);
  }, (error) => {
    console.error(`[DB Read Error] listenCollection(${collectionName}):`, error);
  });

  return unsubscribe;
}

/**
 * 一次性取得單一文檔資料
 * @param {string} collectionName 集合名稱
 * @param {string} docId 文檔 ID
 * @returns {Promise<Object|null>} 資料物件或 null
 */
export async function getDocument(collectionName, docId) {
  try {
    const docRef = doc(db, collectionName, docId);
    const docSnap = await getDoc(docRef);
    
    if (docSnap.exists()) {
      return { id: docSnap.id, ...docSnap.data() };
    } else {
      return null;
    }
  } catch (error) {
    console.error(`[DB Read Error] getDocument(${collectionName}/${docId}):`, error);
    throw error;
  }
}

/**
 * 監聽單一文檔的即時變更
 * @param {string} collectionName 集合名稱
 * @param {string} docId 文檔 ID
 * @param {function} callback 回呼函式，接收資料物件或 null
 * @returns {function} 取消監聽的函式
 */
export function listenDocument(collectionName, docId, callback) {
  const docRef = doc(db, collectionName, docId);

  const unsubscribe = onSnapshot(docRef, (docSnap) => {
    if (docSnap.exists()) {
      callback({ id: docSnap.id, ...docSnap.data() });
    } else {
      callback(null);
    }
  }, (error) => {
    console.error(`[DB Read Error] listenDocument(${collectionName}/${docId}):`, error);
  });

  return unsubscribe;
}

/* ═══ EOF ═══
F: src/db/read.js
V: 1792903503
W: webapp-20261026-001#15
═══ */```

<!-- === FILE END: ./src/db/read.js === -->

<!-- === FILE START: ./src/db/write.js === -->

## 📄 ./src/db/write.js

```js
import { db } from "../config/firebase.js";
import { 
  doc, 
  setDoc, 
  updateDoc, 
  deleteDoc, 
  addDoc,
  collection 
} from "firebase/firestore";

/**
 * 指定 ID 寫入文檔（覆寫或建立）
 * 註：若需「防覆寫」邏輯，應在上層（如 parser.js）先使用 getDocument 檢查
 * @param {string} collectionName 集合名稱
 * @param {string} docId 文檔 ID
 * @param {Object} data 要寫入的資料
 */
export async function setDocument(collectionName, docId, data) {
  try {
    const docRef = doc(db, collectionName, docId);
    await setDoc(docRef, data);
    return true;
  } catch (error) {
    console.error(`[DB Write Error] setDocument(${collectionName}/${docId}):`, error);
    throw error;
  }
}

/**
 * 自動生成 ID 新增文檔
 * @param {string} collectionName 集合名稱
 * @param {Object} data 要寫入的資料
 * @returns {Promise<string>} 新增的文檔 ID
 */
export async function addDocument(collectionName, data) {
  try {
    const colRef = collection(db, collectionName);
    const docRef = await addDoc(colRef, data);
    return docRef.id;
  } catch (error) {
    console.error(`[DB Write Error] addDocument(${collectionName}):`, error);
    throw error;
  }
}

/**
 * 更新指定文檔的部分欄位
 * @param {string} collectionName 集合名稱
 * @param {string} docId 文檔 ID
 * @param {Object} data 要更新的欄位（僅更新傳入的 key）
 */
export async function updateDocument(collectionName, docId, data) {
  try {
    if (!docId) throw new Error("docId is required for update.");
    if (!data || Object.keys(data).length === 0) throw new Error("data is empty.");
    
    const docRef = doc(db, collectionName, docId);
    await updateDoc(docRef, data);
    return true;
  } catch (error) {
    console.error(`[DB Write Error] updateDocument(${collectionName}/${docId}):`, error);
    throw error;
  }
}

/**
 * 刪除指定文檔
 * @param {string} collectionName 集合名稱
 * @param {string} docId 文檔 ID
 */
export async function deleteDocument(collectionName, docId) {
  try {
    if (!docId) throw new Error("docId is required for delete.");
    
    const docRef = doc(db, collectionName, docId);
    await deleteDoc(docRef);
    return true;
  } catch (error) {
    console.error(`[DB Write Error] deleteDocument(${collectionName}/${docId}):`, error);
    throw error;
  }
}

/* ═══ EOF ═══
F: src/db/write.js
V: 1792903504
W: webapp-20261026-001#16
═══ */```

<!-- === FILE END: ./src/db/write.js === -->

<!-- === FILE START: ./src/domain/parser.js === -->

## 📄 ./src/domain/parser.js

```js
/**
 * 將銷售員字串依據對照表進行標準化
 * @param {string} repString 原始字串 (例如: "王小明 / 123")
 * @param {Array} salesMapping 對照表 (例如: [{ name: "王小明", aliases: ["123"] }])
 * @returns {Array<string>} 標準化後的銷售員名稱陣列
 */
export function normalizeSalesRep(repString, salesMapping) {
  if (!repString || String(repString).trim() === "") return ["未填寫"];
  
  // 依 "/" 分割並去除空白
  const rawNames = String(repString).split('/').map(s => s.trim()).filter(Boolean);
  if (rawNames.length === 0) return ["未填寫"];

  const resultSet = new Set();

  for (const rawName of rawNames) {
    let matched = false;
    
    // 尋找對照表中符合的 name 或 aliases
    for (const mapItem of salesMapping) {
      const isNameMatch = mapItem.name === rawName;
      const isAliasMatch = mapItem.aliases && mapItem.aliases.includes(rawName);
      
      if (isNameMatch || isAliasMatch) {
        resultSet.add(mapItem.name);
        matched = true;
        break;
      }
    }

    // 若無對應，則直接使用原名
    if (!matched) {
      resultSet.add(rawName);
    }
  }

  return Array.from(resultSet);
}

/**
 * 將 Excel 日期序列號轉換為 YYYY-MM-DD 格式
 * @param {any} val 原始數值或字串
 * @returns {string} 格式化後的日期字串
 */
function formatExcelDate(val) {
  if (!val) return "";
  
  // 如果已經是正常的日期字串 (包含 - 或 /)
  if (typeof val === 'string' && (val.includes('-') || val.includes('/'))) {
    return val.trim();
  }

  // 處理 Excel 序列號 (通常介於 20000 到 60000 之間)
  const num = Number(val);
  if (!isNaN(num) && num > 20000 && num < 80000) {
    // Excel 起始日為 1899-12-30
    const date = new Date((num - 25569) * 86400 * 1000);
    // 檢查是否為有效日期
    if (!isNaN(date.getTime())) {
      return date.toISOString().split('T')[0];
    }
  }

  // 其他無法解析的，直接轉字串回傳
  return String(val).trim();
}

/**
 * 解析 Excel 匯入的資料列（動態 Schema 版）
 * @param {Array<string>} headers Excel 第一列的標題
 * @param {Array<Array<any>>} rows Excel 的資料列（不含標題）
 * @param {Array} salesMapping 銷售員對照表
 * @param {string} defaultStatus 預設狀態 (如 "排產中")
 * @param {Array} schema 系統欄位定義
 * @returns {Array<Object>} 標準化後的工單物件陣列
 */
export function parseExcelData(headers, rows, salesMapping, defaultStatus = "排產中", schema = []) {
  const nowStr = new Date().toISOString();
  const parsedOrders = [];

  // 1. 建立表頭索引映射：強制清洗表頭的空白、換行與全形空白
  const cleanStr = (s) => String(s || '').replace(/[\s\u3000]+/g, '').trim();
  const cleanHeaders = headers.map(cleanStr);

  const columnMap = {};
  schema.forEach(field => {
    const targetLabel = cleanStr(field.label);
    const idx = cleanHeaders.findIndex(h => h === targetLabel);
    if (idx !== -1) {
      columnMap[field.key] = idx;
    }
  });

  // 2. 檢查必要的系統核心欄位是否存在
  if (columnMap['id'] === undefined) {
    throw new Error('找不到「工單號碼 (發票)」欄位，請確認 Excel 表頭或欄位設定。');
  }

  // 3. 逐列解析
  for (const row of rows) {
    // 跳過完全空白的列（防呆）
    if (!row || row.length === 0) continue;

    const rawIdVal = row[columnMap['id']];
    const orderId = rawIdVal !== undefined && rawIdVal !== null ? String(rawIdVal).trim() : "";
    
    // 💡 核心邏輯：遇到 A 欄（工單號碼）為空，直接中斷迴圈（完美剔除尾部的總計與備註）
    if (!orderId) {
      break;
    }

    // 解析日期（轉換 Excel 序列號）
    let uploadDate = nowStr;
    if (columnMap['uploadDate'] !== undefined && row[columnMap['uploadDate']] !== undefined) {
      uploadDate = formatExcelDate(row[columnMap['uploadDate']]);
    }

    // 解析銷售員
    let rawSalesRep = "";
    if (columnMap['salesRep'] !== undefined && row[columnMap['salesRep']] !== undefined) {
      rawSalesRep = String(row[columnMap['salesRep']]);
    }
    const normalizedReps = normalizeSalesRep(rawSalesRep, salesMapping);

    // 收集動態欄位資料 (非系統核心欄位)
    const data = {};
    
    // 💡 自動擷取工單號前兩碼英文作為類別
    const categoryMatch = orderId.match(/^[a-zA-Z]+/);
    data['category'] = categoryMatch ? categoryMatch[0].toUpperCase() : '其他';

    schema.forEach(field => {
      // 排除系統核心欄位 (id, uploadDate, salesRep 等)
      if (!field.isSystem && columnMap[field.key] !== undefined) {
        const val = row[columnMap[field.key]];
        data[field.key] = val !== undefined && val !== null ? val : "";
      }
    });

    parsedOrders.push({
      id: orderId,
      status: defaultStatus,
      salesReps: normalizedReps,
      uploadDate: uploadDate,
      statusUpdateDate: nowStr.split('T')[0], // 狀態更新日預設為今天
      dueDate: "", // 交期依需求為匯入後手動輸入
      data: data   // 動態欄位資料
    });
  }

  return parsedOrders;
}

/* ═══ EOF ═══
F: AI-WA-UPWO/src/domain/parser.js
V: 1792911500
W: UPGRADE-002
═══ */```

<!-- === FILE END: ./src/domain/parser.js === -->

<!-- === FILE START: ./src/domain/stats.js === -->

## 📄 ./src/domain/stats.js

```js
/**
 * 將工單陣列依「狀態」分組
 * @param {Array<Object>} orders 工單陣列
 * @param {Array<string>} allStatuses 所有可能的狀態清單（確保空狀態也有空陣列）
 * @returns {Object} 以狀態為 key 的物件，value 為該狀態的工單陣列
 */
export function groupByStatus(orders, allStatuses = []) {
  const grouped = {};
  
  // 先初始化所有狀態為空陣列
  allStatuses.forEach(status => {
    grouped[status] = [];
  });

  // 遍歷工單進行分組
  orders.forEach(order => {
    const status = order.status || '未知狀態';
    if (!grouped[status]) {
      grouped[status] = [];
    }
    grouped[status].push(order);
  });

  return grouped;
}

/**
 * 將工單陣列依「銷售員」分組
 * @param {Array<Object>} orders 工單陣列
 * @returns {Object} 以銷售員為 key 的物件，value 為該銷售員的工單陣列
 */
export function groupBySalesRep(orders) {
  const grouped = {};

  orders.forEach(order => {
    const reps = order.salesReps || ['未填寫'];
    reps.forEach(rep => {
      if (!grouped[rep]) {
        grouped[rep] = [];
      }
      grouped[rep].push(order);
    });
  });

  return grouped;
}

/**
 * 將工單陣列依「類別 (發票前兩碼)」分組
 * @param {Array<Object>} orders 工單陣列
 * @returns {Object} 以類別為 key 的物件，value 為該類別的工單陣列
 */
export function groupByCategory(orders) {
  const grouped = {};

  orders.forEach(order => {
    // 優先從 data.category 抓取，若無則嘗試從 id 擷取
    let category = order.data && order.data['category'];
    
    if (!category) {
      const match = String(order.id).match(/^[a-zA-Z]+/);
      category = match ? match[0].toUpperCase() : '其他';
    }

    if (!grouped[category]) {
      grouped[category] = [];
    }
    grouped[category].push(order);
  });

  return grouped;
}

/**
 * 計算總體統計數據（用於 Overview 視圖或 Stats 卡片）
 * @param {Array<Object>} orders 工單陣列
 * @param {Array<string>} allStatuses 所有狀態清單
 * @returns {Object} 包含總數與各狀態數量的物件
 */
export function calculateStats(orders, allStatuses = []) {
  const stats = {
    total: orders.length,
    byStatus: {}
  };

  allStatuses.forEach(status => {
    stats.byStatus[status] = 0;
  });

  orders.forEach(order => {
    const status = order.status || '未知狀態';
    if (!stats.byStatus[status]) {
      stats.byStatus[status] = 0;
    }
    stats.byStatus[status]++;
  });

  return stats;
}

/**
 * 生成樞紐交叉表資料（用於 Overview 視圖上半部）
 * @param {Array<Object>} orders 工單陣列
 * @param {Array<string>} statuses 狀態清單（列）
 * @param {Array<string>} salesReps 銷售員清單（欄）
 * @returns {Object} 包含表頭與二維資料的物件
 */
export function generatePivotTable(orders, statuses = [], salesReps = []) {
  // 建立二維陣列結構
  const matrix = {};
  
  // 初始化矩陣
  statuses.forEach(status => {
    matrix[status] = {};
    salesReps.forEach(rep => {
      matrix[status][rep] = 0;
    });
  });

  // 填充數據
  orders.forEach(order => {
    const status = order.status || '未知狀態';
    const reps = order.salesReps || ['未填寫'];
    
    reps.forEach(rep => {
      // 如果該銷售員不在表頭清單中，動態加入
      if (!matrix[status]) {
        matrix[status] = {};
      }
      if (matrix[status][rep] === undefined) {
        matrix[status][rep] = 0;
      }
      matrix[status][rep]++;
    });
  });

  return matrix;
}

/* ═══ EOF ═══
F: AI-WA-UPWO/src/domain/stats.js
V: 1792911600
W: UPGRADE-002
═══ */```

<!-- === FILE END: ./src/domain/stats.js === -->

<!-- === FILE START: ./src/engine/main.js === -->

## 📄 ./src/engine/main.js

```js
import { initRouter } from './router.js';

/**
 * 系統啟動入口
 * 負責等待 DOM 載入完成後，初始化路由
 */
document.addEventListener('DOMContentLoaded', () => {
  const root = document.getElementById('app-root');
  
  if (!root) {
    console.error('[System Error] 找不到 #app-root 容器，無法啟動系統。');
    return;
  }

  // 隱藏初始載入動畫
  const loadingEl = document.getElementById('app-loading');
  if (loadingEl) {
    loadingEl.classList.add('hidden');
  }

  try {
    // 啟動路由系統
    // 註：全域 createIcons 已移除，圖示由各頁面或佈局元件自行呼叫 createIcons 處理
    initRouter();
    
    console.log('[System] UPGRADE-WO 系統啟動成功。');
  } catch (error) {
    console.error('[System Error] 系統啟動失敗:', error);
    root.innerHTML = `
      <div class="min-h-screen flex items-center justify-center bg-bg text-slate-600">
        <div class="glass-card p-8 text-center max-w-md">
          <h2 class="text-xl font-bold text-rose-500 mb-2">系統啟動失敗</h2>
          <p class="text-sm">請檢查 Console 錯誤訊息，或重新整理頁面。</p>
        </div>
      </div>
    `;
  }
});

/* ═══ EOF ═══
F: AI-WA-UPWO/src/engine/main.js
V: 1792909601
W: UPGRADE-002
═══ */```

<!-- === FILE END: ./src/engine/main.js === -->

<!-- === FILE START: ./src/engine/router.js === -->

## 📄 ./src/engine/router.js

```js
import { AppState } from '../config/state.js';
import { renderAppShell, updateNavbar } from '../ui/layout.js';
import { showToast } from '../ui/toast.js';

// 引入各頁面模組
import LoginPage from '../pages/login.js';
import KanbanPage from '../pages/kanban.js';
import TablePage from '../pages/table.js';
import OverviewPage from '../pages/overview.js';
import SalespersonPage from '../pages/salesperson.js';
import SettingsPage from '../pages/settings.js';
import CategoryPage from '../pages/category.js';

const routes = {
  login: LoginPage,
  kanban: KanbanPage,
  table: TablePage,
  overview: OverviewPage,
  salesperson: SalespersonPage,
  settings: SettingsPage,
  category: CategoryPage
};

let currentPageInstance = null;
let isShellRendered = false;

/**
 * 處理路由變更與守衛
 */
function handleRoute() {
  let hash = window.location.hash.replace('#', '') || 'kanban';
  const user = AppState.getUser();

  // 1. 登入守衛
  if (!user) {
    if (hash !== 'login') {
      window.location.hash = '#login';
      return;
    }
  } else {
    // 已登入卻試圖去登入頁
    if (hash === 'login') {
      window.location.hash = '#kanban';
      return;
    }
  }

  // 2. 權限守衛 (Settings 僅限 admin)
  if (hash === 'settings' && user && user.role !== 'admin') {
    showToast('您沒有權限訪問系統設定', 'error');
    window.location.hash = '#kanban';
    return;
  }

  // 3. 渲染頁面
  const PageClass = routes[hash];
  if (!PageClass) {
    window.location.hash = user ? '#kanban' : '#login';
    return;
  }

  // 4. 處理 App Shell 與頁面切換
  if (hash === 'login') {
    isShellRendered = false; // 登出後重置狀態
    if (currentPageInstance && typeof currentPageInstance.destroy === 'function') {
      currentPageInstance.destroy();
    }
    currentPageInstance = PageClass;
    currentPageInstance.render();
  } else {
    // 需要 App Shell 的頁面
    if (!isShellRendered) {
      renderAppShell({
        user: user,
        onLogout: handleLogout,
        onNavigate: (route) => { window.location.hash = `#${route}`; }
      });
      isShellRendered = true;
    }

    // 更新導覽列高亮
    updateNavbar(hash);

    // 銷毀舊頁面
    if (currentPageInstance && typeof currentPageInstance.destroy === 'function') {
      currentPageInstance.destroy();
    }

    // 渲染新頁面
    currentPageInstance = PageClass;
    currentPageInstance.render();
  }
}

/**
 * 處理登出
 */
function handleLogout() {
  AppState.clearUser(); // 假設 state.js 有這個方法
  isShellRendered = false;
  window.location.hash = '#login';
  showToast('已登出系統', 'info');
}

/**
 * 初始化路由
 */
export function initRouter() {
  // 監聽 Hash 變更
  window.addEventListener('hashchange', handleRoute);

  // 監聽登入事件 (由 login.js 觸發)
  window.addEventListener('auth:login', () => {
    // 登入成功後，強制導向首頁
    window.location.hash = '#kanban';
  });

  // 首次載入
  handleRoute();
}

/* ═══ EOF ═══
F: AI-WA-UPWO/src/engine/router.js
V: 1792912200
W: UPGRADE-002
═══ */```

<!-- === FILE END: ./src/engine/router.js === -->

<!-- === FILE START: ./src/lib/io.js === -->

## 📄 ./src/lib/io.js

```js
import * as XLSX from 'xlsx';
import { jsPDF } from 'jspdf';
import { groupByStatus } from '../domain/stats.js';

/**
 * 將工單資料匯出為 Excel 檔案 (依狀態分 Sheet，包含動態欄位)
 * @param {Array<Object>} orders 工單陣列
 * @param {Array<string>} statuses 狀態清單
 * @param {string} filename 檔案名稱 (不含副檔名)
 * @param {Array} schema 系統欄位設定 (用於抓取自訂欄位)
 */
export function exportToExcel(orders, statuses, filename = '工單清單', schema = []) {
  if (!orders || orders.length === 0) {
    throw new Error('無資料可匯出');
  }

  const groupedData = groupByStatus(orders, statuses);
  const wb = XLSX.utils.book_new();

  // 過濾出非系統的自訂欄位
  const customFields = schema.filter(f => !f.isSystem);

  for (const status of statuses) {
    const statusOrders = groupedData[status] || [];
    if (statusOrders.length === 0) continue;

    // 整理要輸出的欄位
    const dataToExport = statusOrders.map(order => {
      // 基礎固定欄位
      const row = {
        '工單號碼': order.id,
        '類別': order.data?.category || '其他',
        '狀態': order.status,
        '銷售員': (order.salesReps || []).join(', '),
        '上傳日期': order.uploadDate || '',
        '狀態更新日': order.statusUpdateDate || '',
        '交貨日期': order.dueDate || ''
      };

      // 動態加入自訂欄位
      customFields.forEach(field => {
        row[field.label] = order.data && order.data[field.key] !== undefined ? order.data[field.key] : '';
      });

      return row;
    });

    // 建立工作表
    const ws = XLSX.utils.json_to_sheet(dataToExport);
    // 將工作表加入活頁簿，並以狀態名稱命名（Excel sheet 名稱限制 31 字元）
    XLSX.utils.book_append_sheet(wb, ws, status.substring(0, 31));
  }

  // 觸發下載
  XLSX.writeFile(wb, `${filename}_${new Date().toISOString().slice(0,10)}.xlsx`);
}

/**
 * 將工單資料匯出為 PDF 檔案 (依狀態分頁，包含動態欄位)
 * @param {Array<Object>} orders 工單陣列
 * @param {Array<string>} statuses 狀態清單
 * @param {string} filename 檔案名稱 (不含副檔名)
 * @param {Array} schema 系統欄位設定
 */
export function exportToPDF(orders, statuses, filename = '工單清單', schema = []) {
  if (!orders || orders.length === 0) {
    throw new Error('無資料可匯出');
  }

  const doc = new jsPDF();
  const groupedData = groupByStatus(orders, statuses);
  
  let isFirstPage = true;

  for (const status of statuses) {
    const statusOrders = groupedData[status] || [];
    if (statusOrders.length === 0) continue;

    if (!isFirstPage) {
      doc.addPage();
    }
    isFirstPage = false;

    // 標題
    doc.setFontSize(16);
    doc.text(`工序狀態：${status}`, 14, 20);
    doc.setFontSize(10);
    doc.setTextColor(100);

    let yPos = 30;
    const lineHeight = 10;
    const pageHeight = doc.internal.pageSize.height;

    // 表頭
    doc.text('工單號碼', 14, yPos);
    doc.text('類別', 50, yPos);
    doc.text('銷售員', 70, yPos);
    doc.text('交貨日', 120, yPos);
    
    yPos += 5;
    doc.setLineWidth(0.5);
    doc.line(14, yPos, 196, yPos);
    yPos += 5;

    // 內容
    doc.setTextColor(0);
    for (const order of statusOrders) {
      if (yPos > pageHeight - 20) {
        doc.addPage();
        yPos = 20;
        // 重新繪製表頭
        doc.text('工單號碼', 14, yPos);
        doc.text('類別', 50, yPos);
        doc.text('銷售員', 70, yPos);
        doc.text('交貨日', 120, yPos);
        yPos += 5;
        doc.line(14, yPos, 196, yPos);
        yPos += 5;
      }

      const orderId = String(order.id).substring(0, 20);
      const cat = String(order.data?.category || '其他').substring(0, 10);
      const salesRep = (order.salesReps || []).join(', ').substring(0, 25);
      const dueDate = String(order.dueDate || '-').substring(0, 15);

      doc.text(orderId, 14, yPos);
      doc.text(cat, 50, yPos);
      doc.text(salesRep, 70, yPos);
      doc.text(dueDate, 120, yPos);

      // 如果有自訂欄位，可以在下方印出（避免版面太擠）
      const customFields = schema.filter(f => !f.isSystem);
      if (customFields.length > 0) {
        yPos += 5;
        const extraInfo = customFields.map(f => {
          const val = order.data && order.data[f.key] !== undefined ? order.data[f.key] : '-';
          return `${f.label}: ${val}`;
        }).join(' | ');
        doc.setFontSize(8);
        doc.setTextColor(100);
        doc.text(extraInfo.substring(0, 100), 14, yPos);
        doc.setFontSize(10);
        doc.setTextColor(0);
        yPos += 5; // 多佔用一行
      }

      yPos += lineHeight;
    }
  }

  // 觸發下載
  doc.save(`${filename}_${new Date().toISOString().slice(0,10)}.pdf`);
}

/* ═══ EOF ═══
F: AI-WA-UPWO/src/lib/io.js
V: 1792912500
W: UPGRADE-002
═══ */```

<!-- === FILE END: ./src/lib/io.js === -->

<!-- === FILE START: ./src/pages/category.js === -->

## 📄 ./src/pages/category.js

```js
import { AppState } from '../config/state.js';
import { listenCollection } from '../db/read.js';
import { updateDocument } from '../db/write.js';
import { renderPageLayout } from '../ui/layout.js';
import { showToast } from '../ui/toast.js';
import { groupByStatus, groupByCategory } from '../domain/stats.js';
import { createIcons, Layers, Inbox, ChevronDown, User, Calendar } from 'lucide';

let unsubscribe = null;
let currentOrders = [];
let allCategories = [];
let currentCategory = null;

export default {
  async render() {
    const user = AppState.getUser();
    if (!user) return;

    // 1. 渲染頁面骨架
    renderPageLayout({
      title: '類別視圖',
      subtitle: '依發票前兩碼檢視工單狀態',
      headerActions: '',
      stats: [],
      controls: '',
      listContainerId: 'category-container'
    });

    // 2. 初始化資料監聽
    this.initDataListener();
  },

  initDataListener() {
    unsubscribe = listenCollection('work_orders', (orders) => {
      currentOrders = orders;
      this.extractCategoriesAndRender();
    });
  },

  extractCategoriesAndRender() {
    const grouped = groupByCategory(currentOrders);
    allCategories = Object.keys(grouped).sort();

    // 預設選中第一個類別（若還沒選過或選中的被刪除了）
    if (allCategories.length > 0 && (!currentCategory || !allCategories.includes(currentCategory))) {
      currentCategory = allCategories[0];
    }

    this.renderLayout();
  },

  renderLayout() {
    const container = document.getElementById('category-container');
    if (!container) return;

    if (allCategories.length === 0) {
      container.innerHTML = `
        <div class="flex flex-col items-center justify-center h-64 text-slate-400 gap-3 bg-white rounded-2xl border border-slate-100 shadow-soft">
          <i data-lucide="inbox" class="w-12 h-12 opacity-30"></i>
          <span class="text-base font-medium">目前尚無任何類別或工單</span>
        </div>
      `;
      createIcons({ icons: { Inbox }, attrs: { class: 'w-12 h-12' } });
      return;
    }

    container.innerHTML = `
      <div class="flex flex-col md:flex-row gap-6 h-[calc(100vh-200px)]">
        
        <!-- 左側：類別清單 -->
        <div class="w-full md:w-64 flex-shrink-0 flex flex-col bg-white rounded-[20px] border border-slate-200/60 shadow-soft overflow-hidden">
          <div class="px-5 py-4 border-b border-slate-100 bg-slate-50/50 flex items-center gap-2">
            <i data-lucide="layers" class="w-5 h-5 text-brand"></i>
            <h2 class="font-bold text-slate-800">類別清單</h2>
          </div>
          <div class="flex-1 overflow-y-auto p-2 flex flex-col gap-1 kanban-scroll">
            ${allCategories.map(cat => `
              <button class="cat-item text-left px-4 py-3 rounded-xl transition-colors text-sm font-medium flex items-center justify-between ${cat === currentCategory ? 'bg-brand/10 text-brand' : 'text-slate-600 hover:bg-slate-50'}" data-cat="${cat}">
                <span>${cat}</span>
                ${cat === currentCategory ? '<span class="w-1.5 h-1.5 rounded-full bg-brand"></span>' : ''}
              </button>
            `).join('')}
          </div>
        </div>

        <!-- 右側：該類別的工單看板 -->
        <div class="flex-1 flex flex-col min-w-0 bg-white rounded-[20px] border border-slate-200/60 shadow-soft overflow-hidden">
          <div class="px-5 py-4 border-b border-slate-100 bg-slate-50/50 flex justify-between items-center">
            <h2 class="font-bold text-slate-800 flex items-center gap-2">
              <i data-lucide="layers" class="w-5 h-5 text-brand"></i>
              類別 ${currentCategory} 的工單
            </h2>
          </div>
          <div class="flex-1 p-4 overflow-x-auto kanban-scroll flex gap-4 bg-slate-50/30" id="cat-board">
            <!-- 由 renderCatBoard 注入 -->
          </div>
        </div>
      </div>
    `;

    createIcons({
      icons: { Layers },
      attrs: { class: 'w-5 h-5' }
    });

    this.bindEvents();
    this.renderCatBoard();
  },

  renderCatBoard() {
    const boardContainer = document.getElementById('cat-board');
    if (!boardContainer) return;

    const statuses = AppState.getStatuses() || ['排產中', '印刷', '裁切', '品檢', '出貨'];
    const user = AppState.getUser();
    const isAdmin = user && user.role === 'admin';
    
    // 過濾出當前選中類別的工單
    const catOrders = currentOrders.filter(o => {
      const cat = o.data && o.data.category ? o.data.category : '其他';
      return cat === currentCategory;
    });
    
    // 依狀態分組
    const grouped = groupByStatus(catOrders, statuses);

    boardContainer.innerHTML = statuses.map(status => {
      const statusOrders = grouped[status] || [];
      
      return `
        <div class="w-[280px] flex-shrink-0 flex flex-col gap-2">
          <div class="flex items-center justify-between px-2">
            <h3 class="font-semibold text-slate-700 text-sm flex items-center gap-2">
              <span class="w-2 h-2 rounded-full ${statusOrders.length > 0 ? 'bg-brand' : 'bg-slate-300'}"></span>
              ${status}
            </h3>
            <span class="text-xs font-medium bg-slate-200/80 text-slate-600 px-2 py-0.5 rounded-full">${statusOrders.length}</span>
          </div>
          <div class="inset-panel p-2 flex flex-col gap-2 min-h-[150px] flex-1">
            ${statusOrders.length === 0 ? `
              <div class="flex items-center justify-center h-full text-slate-300 text-xs py-4">無</div>
            ` : statusOrders.map(order => {
              const reps = (order.salesReps || []).join(', ');
              const otherStatuses = statuses.filter(s => s !== order.status);
              return `
                <div class="glass-card p-3 rounded-xl border border-slate-100 shadow-sm text-sm relative !overflow-visible">
                  <div class="flex justify-between items-start mb-1">
                    <span class="font-bold text-slate-800">${order.id}</span>
                    ${order.dueDate ? `<span class="text-[10px] text-amber-600">交期:${order.dueDate}</span>` : ''}
                  </div>
                  <div class="text-slate-500 text-xs mb-2 flex items-center gap-1">
                    <i data-lucide="user" class="w-3 h-3"></i> ${reps}
                  </div>
                  
                  ${isAdmin ? `
                    <button class="w-full flex items-center justify-between text-xs font-medium text-slate-500 hover:text-brand transition-colors btn-dropdown border-t border-slate-50 pt-2 mt-1">
                      <span>流轉工序</span>
                      <i data-lucide="chevron-down" class="w-3 h-3 transition-transform"></i>
                    </button>
                    <div class="dropdown-menu hidden absolute left-0 right-0 bottom-full mb-1 bg-white border border-slate-100 shadow-xl rounded-xl p-1 z-[999]">
                      ${otherStatuses.map(s => `
                        <button class="w-full text-left px-3 py-2 text-sm text-slate-700 hover:bg-slate-50 rounded-lg transition-colors btn-change-status" data-id="${order.id}" data-new-status="${s}">
                          移至 ${s}
                        </button>
                      `).join('')}
                    </div>
                  ` : ''}
                </div>
              `;
            }).join('')}
          </div>
        </div>
      `;
    }).join('');

    createIcons({ icons: { ChevronDown, User }, attrs: { class: 'w-4 h-4' } });
    this.bindCardEvents();
  },

  bindEvents() {
    document.querySelectorAll('.cat-item').forEach(btn => {
      btn.addEventListener('click', (e) => {
        const newCat = e.currentTarget.dataset.cat;
        if (newCat !== currentCategory) {
          currentCategory = newCat;
          this.renderLayout();
        }
      });
    });
  },

  bindCardEvents() {
    document.querySelectorAll('#cat-board .btn-dropdown').forEach(btnDropdown => {
      const card = btnDropdown.closest('.glass-card');
      const dropdownMenu = card.querySelector('.dropdown-menu');
      
      btnDropdown.addEventListener('click', (e) => {
        e.stopPropagation();
        document.querySelectorAll('#cat-board .dropdown-menu').forEach(m => {
          if (m !== dropdownMenu) m.classList.add('hidden');
        });
        dropdownMenu.classList.toggle('hidden');
        const icon = btnDropdown.querySelector('i');
        if (dropdownMenu.classList.contains('hidden')) {
          icon.style.transform = 'rotate(0deg)';
        } else {
          icon.style.transform = 'rotate(180deg)';
        }
      });
    });

    document.querySelectorAll('#cat-board .btn-change-status').forEach(btn => {
      btn.addEventListener('click', async (e) => {
        e.stopPropagation();
        const newStatus = btn.dataset.newStatus;
        const orderId = btn.dataset.id;
        
        btn.disabled = true;
        btn.textContent = '處理中...';

        try {
          const nowStr = new Date().toISOString();
          await updateDocument('work_orders', orderId, {
            status: newStatus,
            statusUpdateDate: nowStr
          });
          showToast(`工單 ${orderId} 已流轉至【${newStatus}】`, 'success');
        } catch (error) {
          console.error('Update status failed:', error);
          showToast('更新狀態失敗', 'error');
          btn.disabled = false;
          btn.textContent = `移至 ${newStatus}`;
        }
      });
    });

    document.addEventListener('click', () => {
      document.querySelectorAll('#cat-board .dropdown-menu').forEach(m => {
        m.classList.add('hidden');
        const icon = m.parentElement.querySelector('.btn-dropdown i');
        if (icon) icon.style.transform = 'rotate(0deg)';
      });
    }, { once: true });
  },

  destroy() {
    if (unsubscribe) {
      unsubscribe();
      unsubscribe = null;
    }
    currentOrders = [];
    allCategories = [];
    currentCategory = null;
  }
};

/* ═══ EOF ═══
F: AI-WA-UPWO/src/pages/category.js
V: 1792911900
W: UPGRADE-002
═══ */```

<!-- === FILE END: ./src/pages/category.js === -->

<!-- === FILE START: ./src/pages/kanban.js === -->

## 📄 ./src/pages/kanban.js

```js
import { AppState } from '../config/state.js';
import { listenCollection, getDocument } from '../db/read.js';
import { updateDocument, setDocument } from '../db/write.js';
import { renderPageLayout } from '../ui/layout.js';
import { showToast } from '../ui/toast.js';
import { groupByStatus, groupByCategory, calculateStats } from '../domain/stats.js';
import { parseExcelData } from '../domain/parser.js';
import * as XLSX from 'xlsx';
import { createIcons, MoreVertical, ChevronDown, User, Calendar, Upload, CheckCircle2, AlertCircle, Activity, ClipboardList, LayoutGrid, List } from 'lucide';

let unsubscribe = null;
let currentOrders = [];
let currentStatus = '';
let currentCategory = 'ALL';
let currentView = 'card'; // 'card' or 'table'

export default {
  async render() {
    const user = AppState.getUser();
    if (!user) return;

    // 1. 渲染頁面骨架
    renderPageLayout({
      title: '流水線進度控制台',
      subtitle: '即時追蹤工單狀態與生產進度',
      headerActions: `
        <div class="flex items-center gap-2">
          <button id="btn-import-excel" class="bg-white text-slate-700 border border-slate-200 px-4 py-2 rounded-[10px] text-sm font-medium hover:bg-slate-50 transition-colors active:scale-95 flex items-center gap-2 shadow-sm">
            <i data-lucide="upload" class="w-4 h-4"></i> 匯入 Excel
          </button>
          <input type="file" id="excel-import-input" accept=".xlsx, .xls" class="hidden">
          
          <!-- 視圖切換按鈕 -->
          <div class="segmented-control hidden sm:inline-flex">
            <button id="btn-view-card" class="segment-btn ${currentView === 'card' ? 'active' : ''} flex items-center gap-1">
              <i data-lucide="layout-grid" class="w-4 h-4"></i> 卡片
            </button>
            <button id="btn-view-table" class="segment-btn ${currentView === 'table' ? 'active' : ''} flex items-center gap-1">
              <i data-lucide="list" class="w-4 h-4"></i> 表格
            </button>
          </div>
        </div>
      `,
      stats: [],
      controls: `
        <div class="flex flex-col sm:flex-row items-start sm:items-center gap-4 mb-4">
          <div class="flex items-center gap-2">
            <span class="text-sm font-medium text-slate-600">狀態：</span>
            <select id="filter-status" class="px-3 py-2 bg-white border border-slate-200 rounded-xl text-sm focus:outline-none focus:border-brand shadow-sm min-w-[120px]">
              <!-- 由 renderControls 注入 -->
            </select>
          </div>
          <div class="flex items-center gap-2">
            <span class="text-sm font-medium text-slate-600">類別：</span>
            <select id="filter-category" class="px-3 py-2 bg-white border border-slate-200 rounded-xl text-sm focus:outline-none focus:border-brand shadow-sm min-w-[120px]">
              <option value="ALL">全部類別</option>
              <!-- 由 renderControls 注入 -->
            </select>
          </div>
        </div>
      `,
      listContainerId: 'kanban-container'
    });

    // 2. 優先綁定事件
    this.bindEvents();

    // 3. 渲染初始圖示
    createIcons({
      icons: { Upload, LayoutGrid, List },
      attrs: { class: 'w-4 h-4' }
    });

    // 4. 初始化資料監聽
    this.initDataListener();
  },

  bindEvents() {
    const btnImport = document.getElementById('btn-import-excel');
    const fileInput = document.getElementById('excel-import-input');
    if (btnImport && fileInput) {
      btnImport.addEventListener('click', () => fileInput.click());
      fileInput.addEventListener('change', (e) => this.handleFileSelect(e));
    }

    // 視圖切換
    document.getElementById('btn-view-card')?.addEventListener('click', () => {
      currentView = 'card';
      document.getElementById('btn-view-card').classList.add('active');
      document.getElementById('btn-view-table').classList.remove('active');
      this.renderContent();
    });
    document.getElementById('btn-view-table')?.addEventListener('click', () => {
      currentView = 'table';
      document.getElementById('btn-view-table').classList.add('active');
      document.getElementById('btn-view-card').classList.remove('active');
      this.renderContent();
    });

    // 篩選器
    document.getElementById('filter-status')?.addEventListener('change', (e) => {
      currentStatus = e.target.value;
      this.renderContent();
    });
    document.getElementById('filter-category')?.addEventListener('change', (e) => {
      currentCategory = e.target.value;
      this.renderContent();
    });
  },

  initDataListener() {
    let statuses = AppState.getStatuses();
    if (!statuses || statuses.length === 0) {
      statuses = ['排產中', '印刷', '裁切', '品檢', '出貨'];
    }
    if (!currentStatus) currentStatus = statuses[0];

    unsubscribe = listenCollection('work_orders', (orders) => {
      currentOrders = orders;
      this.updateFiltersAndRender(statuses);
    });
  },

  updateFiltersAndRender(statuses) {
    // 更新狀態下拉選單
    const statusSelect = document.getElementById('filter-status');
    if (statusSelect) {
      statusSelect.innerHTML = statuses.map(s => 
        `<option value="${s}" ${s === currentStatus ? 'selected' : ''}>${s} (${currentOrders.filter(o => o.status === s).length})</option>`
      ).join('');
    }

    // 更新類別下拉選單
    const categorySelect = document.getElementById('filter-category');
    if (categorySelect) {
      const grouped = groupByCategory(currentOrders);
      const categories = Object.keys(grouped).sort();
      
      if (currentCategory !== 'ALL' && !categories.includes(currentCategory)) {
        currentCategory = 'ALL';
      }

      categorySelect.innerHTML = `<option value="ALL">全部類別</option>` + 
        categories.map(c => 
          `<option value="${c}" ${c === currentCategory ? 'selected' : ''}>${c} (${grouped[c].length})</option>`
        ).join('');
    }

    // 更新統計卡片
    const stats = calculateStats(currentOrders, statuses);
    const statsContainer = document.querySelector('#app-content > .grid');
    if (statsContainer) {
      statsContainer.innerHTML = `
        <div class="glass-card p-5 flex flex-col gap-2">
          <span class="text-sm text-slate-500 font-medium flex items-center gap-2">
            <i data-lucide="clipboard-list" class="w-4 h-4 text-brand"></i> 總工單數
          </span>
          <span class="text-3xl font-bold text-slate-900">${stats.total}</span>
        </div>
        ${statuses.slice(0, 3).map(s => `
          <div class="glass-card p-5 flex flex-col gap-2">
            <span class="text-sm text-slate-500 font-medium flex items-center gap-2">
              <i data-lucide="activity" class="w-4 h-4 text-blue-500"></i> ${s}
            </span>
            <span class="text-3xl font-bold text-slate-900">${stats.byStatus[s] || 0}</span>
          </div>
        `).join('')}
      `;
      createIcons({ icons: { Activity, ClipboardList }, attrs: { class: 'w-4 h-4' } });
    }

    this.renderContent();
  },

  renderContent() {
    const container = document.getElementById('kanban-container');
    if (!container) return;

    // 過濾資料
    let filtered = currentOrders;
    if (currentStatus) {
      filtered = filtered.filter(o => o.status === currentStatus);
    }
    if (currentCategory && currentCategory !== 'ALL') {
      filtered = filtered.filter(o => {
        const cat = o.data && o.data.category ? o.data.category : '其他';
        return cat === currentCategory;
      });
    }

    if (currentView === 'card') {
      this.renderCardView(container, filtered);
    } else {
      this.renderTableView(container, filtered);
    }
  },

  renderCardView(container, orders) {
    const user = AppState.getUser();
    const isAdmin = user && user.role === 'admin';

    if (orders.length === 0) {
      container.innerHTML = `
        <div class="flex flex-col items-center justify-center h-64 text-slate-400 gap-3 bg-white rounded-2xl border border-slate-100 shadow-soft">
          <i data-lucide="inbox" class="w-12 h-12 opacity-30"></i>
          <span class="text-base font-medium">此狀態或類別下無工單</span>
        </div>
      `;
      createIcons({ attrs: { class: 'w-12 h-12' } });
      return;
    }

    const boardHtml = `
      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-4 pb-4">
        ${orders.map(order => this.renderCard(order, isAdmin)).join('')}
      </div>
    `;

    container.innerHTML = boardHtml;
    createIcons({
      icons: { MoreVertical, ChevronDown, User, Calendar },
      attrs: { class: 'w-4 h-4' }
    });

    this.bindCardEvents();
  },

  renderTableView(container, orders) {
    if (orders.length === 0) {
      container.innerHTML = `
        <div class="flex flex-col items-center justify-center h-64 text-slate-400 gap-3 bg-white rounded-2xl border border-slate-100 shadow-soft">
          <i data-lucide="inbox" class="w-12 h-12 opacity-30"></i>
          <span class="text-base font-medium">無工單資料</span>
        </div>
      `;
      createIcons({ attrs: { class: 'w-12 h-12' } });
      return;
    }

    const html = `
      <div class="bg-white rounded-[20px] border border-slate-200/60 shadow-soft overflow-hidden flex flex-col w-full">
        <div class="overflow-x-auto">
          <table class="w-full text-left border-collapse min-w-[800px]">
            <thead>
              <tr class="bg-slate-50/80 border-b border-slate-200/60 text-sm font-semibold text-slate-600">
                <th class="px-6 py-4 rounded-tl-[20px]">工單號碼</th>
                <th class="px-6 py-4">類別</th>
                <th class="px-6 py-4">銷售員</th>
                <th class="px-6 py-4">上傳日期</th>
                <th class="px-6 py-4">交期</th>
                <th class="px-6 py-4 rounded-tr-[20px]">操作</th>
              </tr>
            </thead>
            <tbody class="divide-y divide-slate-100 text-sm text-slate-700">
              ${orders.map(order => {
                const cat = order.data && order.data.category ? order.data.category : '-';
                const reps = (order.salesReps || []).join(', ');
                return `
                  <tr class="hover:bg-slate-50/50 transition-colors">
                    <td class="px-6 py-4 font-bold text-slate-900">${order.id}</td>
                    <td class="px-6 py-4"><span class="px-2 py-1 bg-slate-100 rounded-md text-xs">${cat}</span></td>
                    <td class="px-6 py-4 text-slate-500">${reps}</td>
                    <td class="px-6 py-4 text-slate-500">${order.uploadDate || '-'}</td>
                    <td class="px-6 py-4 text-slate-500">${order.dueDate || '-'}</td>
                    <td class="px-6 py-4 text-brand font-medium cursor-pointer">流轉</td>
                  </tr>
                `;
              }).join('')}
            </tbody>
          </table>
        </div>
      </div>
    `;
    container.innerHTML = html;
  },

  renderCard(order, isAdmin) {
    const statuses = AppState.getStatuses() || [];
    const otherStatuses = statuses.filter(s => s !== order.status);
    const uploadDate = order.uploadDate ? order.uploadDate.split('T')[0] : '';
    const salesReps = order.salesReps ? order.salesReps.join(', ') : '未填寫';
    const category = order.data && order.data.category ? order.data.category : '其他';
    
    let customerName = order.data ? (order.data['customerNameZh'] || order.data['customerNameEn'] || '') : '';
    if (customerName.includes('***')) customerName = '';
    
    return `
      <div class="glass-card p-4 relative card-item !overflow-visible" data-id="${order.id}" data-current-status="${order.status}">
        <div class="flex justify-between items-start mb-2">
          <div class="flex items-center gap-2">
            <span class="font-bold text-slate-900">${order.id}</span>
            <span class="text-[10px] font-bold bg-brand/10 text-brand px-1.5 py-0.5 rounded">${category}</span>
          </div>
          <span class="text-[10px] font-medium bg-slate-100 text-slate-500 px-2 py-1 rounded-md">${uploadDate}</span>
        </div>
        
        <div class="flex flex-col gap-1.5 mb-3">
          ${customerName ? `<div class="text-sm text-slate-600 truncate">客戶：${customerName}</div>` : ''}
          <div class="flex items-center gap-1.5 text-xs text-slate-500">
            <i data-lucide="user" class="w-3.5 h-3.5"></i> ${salesReps}
          </div>
          ${order.dueDate ? `<div class="text-xs text-amber-600 flex items-center gap-1.5"><i data-lucide="calendar" class="w-3.5 h-3.5"></i> 交期：${order.dueDate}</div>` : ''}
        </div>

        ${isAdmin ? `
          <div class="mt-3 pt-3 border-t border-slate-100 relative">
            <button class="w-full flex items-center justify-between text-xs font-medium text-slate-500 hover:text-brand transition-colors btn-dropdown">
              <span>流轉工序</span>
              <i data-lucide="chevron-down" class="w-4 h-4 transition-transform"></i>
            </button>
            
            <!-- 修復：提高 z-index 並加上絕對定位 -->
            <div class="dropdown-menu hidden absolute left-0 right-0 bottom-full mb-1 bg-white border border-slate-100 shadow-xl rounded-xl p-1 z-[999]">
              ${otherStatuses.map(s => `
                <button class="w-full text-left px-3 py-2 text-sm text-slate-700 hover:bg-slate-50 rounded-lg transition-colors btn-change-status" data-new-status="${s}">
                  移至 ${s}
                </button>
              `).join('')}
            </div>
          </div>
        ` : ''}
      </div>
    `;
  },

  bindCardEvents() {
    document.querySelectorAll('.card-item').forEach(card => {
      const btnDropdown = card.querySelector('.btn-dropdown');
      const dropdownMenu = card.querySelector('.dropdown-menu');
      const btnChangeStatuses = card.querySelectorAll('.btn-change-status');

      if (btnDropdown && dropdownMenu) {
        btnDropdown.addEventListener('click', (e) => {
          e.stopPropagation();
          
          document.querySelectorAll('.dropdown-menu').forEach(m => {
            if (m !== dropdownMenu) m.classList.add('hidden');
          });

          dropdownMenu.classList.toggle('hidden');
          const icon = btnDropdown.querySelector('i');
          if (dropdownMenu.classList.contains('hidden')) {
            icon.style.transform = 'rotate(0deg)';
          } else {
            icon.style.transform = 'rotate(180deg)';
          }
        });

        btnChangeStatuses.forEach(btn => {
          btn.addEventListener('click', async (e) => {
            e.stopPropagation();
            const newStatus = btn.dataset.newStatus;
            const orderId = card.dataset.id;
            
            btn.disabled = true;
            btn.textContent = '處理中...';

            try {
              const nowStr = new Date().toISOString();
              await updateDocument('work_orders', orderId, {
                status: newStatus,
                statusUpdateDate: nowStr
              });
              
              showToast(`工單 ${orderId} 已流轉至【${newStatus}】`, 'success');
            } catch (error) {
              console.error('Update status failed:', error);
              showToast('更新狀態失敗', 'error');
              btn.disabled = false;
              btn.textContent = `移至 ${newStatus}`;
            }
          });
        });
      }
    });

    document.addEventListener('click', () => {
      document.querySelectorAll('.dropdown-menu').forEach(m => {
        m.classList.add('hidden');
        const icon = m.parentElement.querySelector('.btn-dropdown i');
        if (icon) icon.style.transform = 'rotate(0deg)';
      });
    }, { once: true });
  },

  async handleFileSelect(e) {
    const file = e.target.files[0];
    if (!file) return;

    const reader = new FileReader();
    reader.onload = async (evt) => {
      try {
        const data = new Uint8Array(evt.target.result);
        const workbook = XLSX.read(data, { type: 'array' });
        const firstSheetName = workbook.SheetNames[0];
        const worksheet = workbook.Sheets[firstSheetName];
        
        const jsonData = XLSX.utils.sheet_to_json(worksheet, { header: 1 });
        if (jsonData.length < 2) {
          showToast('Excel 檔案沒有資料', 'error');
          return;
        }

        const headers = jsonData[0];
        const rows = jsonData.slice(1);

        showToast('解析中，請稍候...', 'info');

        let salesMapping = [];
        let currentStatuses = AppState.getStatuses() || ['排產中'];
        let currentSchema = [];
        
        try {
          const settingsDoc = await getDocument('settings', 'main');
          if (settingsDoc) {
            salesMapping = settingsDoc.sales_mapping || [];
            if (settingsDoc.statuses && settingsDoc.statuses.length > 0) {
              currentStatuses = settingsDoc.statuses;
            }
            currentSchema = settingsDoc.schema || [];
          }
        } catch (err) {
          console.warn('無法讀取設定檔，將使用預設值', err);
        }

        const parsedOrders = parseExcelData(headers, rows, salesMapping, currentStatuses[0], currentSchema);

        let addedCount = 0;
        let updatedCount = 0;

        for (const order of parsedOrders) {
          const existing = await getDocument('work_orders', order.id);
          if (existing) {
            await updateDocument('work_orders', order.id, {
              salesReps: order.salesReps,
              uploadDate: order.uploadDate,
              data: order.data
            });
            updatedCount++;
          } else {
            await setDocument('work_orders', order.id, order);
            addedCount++;
          }
        }

        showToast(`匯入完成！新增 ${addedCount} 筆，更新 ${updatedCount} 筆。`, 'success');
        e.target.value = '';

      } catch (error) {
        console.error('Import Error:', error);
        showToast(`匯入失敗：${error.message || '請檢查檔案格式'}`, 'error');
        e.target.value = '';
      }
    };
    reader.readAsArrayBuffer(file);
  },

  destroy() {
    if (unsubscribe) {
      unsubscribe();
      unsubscribe = null;
    }
    currentOrders = [];
  }
};

/* ═══ EOF ═══
F: AI-WA-UPWO/src/pages/kanban.js
V: 1792911700
W: UPGRADE-002
═══ */```

<!-- === FILE END: ./src/pages/kanban.js === -->

<!-- === FILE START: ./src/pages/login.js === -->

## 📄 ./src/pages/login.js

```js
import { AppState } from '../config/state.js';
import { getCollection } from '../db/read.js';
import { showToast } from '../ui/toast.js';
import { createIcons, Layers, Lock, User } from 'lucide';

export default {
  async render() {
    // 登入頁不經過 App Shell，因此直接渲染到最外層的 app-root
    const content = document.getElementById('app-root');
    if (!content) return;

    content.innerHTML = `
      <div class="min-h-screen flex items-center justify-center bg-bg">
        <div class="glass-card w-full max-w-md p-8 sm:p-10 relative overflow-hidden">
          
          <!-- 裝飾背景 -->
          <div class="absolute top-0 left-0 w-full h-2 bg-gradient-to-r from-brand to-purple-500"></div>

          <div class="flex flex-col items-center mb-8">
            <div class="w-12 h-12 rounded-2xl bg-brand flex items-center justify-center text-white shadow-lg shadow-brand/30 mb-4">
              <i data-lucide="layers" class="w-7 h-7"></i>
            </div>
            <h1 class="text-2xl font-bold text-slate-900">UPGRADE-WO</h1>
            <p class="text-sm text-slate-500 mt-1">流水線進度控制台</p>
          </div>

          <form id="login-form" class="flex flex-col gap-5">
            <div class="flex flex-col gap-1.5">
              <label class="text-sm font-medium text-slate-700 ml-1">帳號</label>
              <div class="relative">
                <div class="absolute inset-y-0 left-0 pl-3.5 flex items-center pointer-events-none text-slate-400">
                  <i data-lucide="user" class="w-5 h-5"></i>
                </div>
                <input type="text" id="username" required
                  class="w-full pl-11 pr-4 py-3 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-brand/20 focus:border-brand transition-all"
                  placeholder="請輸入帳號">
              </div>
            </div>

            <div class="flex flex-col gap-1.5">
              <label class="text-sm font-medium text-slate-700 ml-1">密碼</label>
              <div class="relative">
                <div class="absolute inset-y-0 left-0 pl-3.5 flex items-center pointer-events-none text-slate-400">
                  <i data-lucide="lock" class="w-5 h-5"></i>
                </div>
                <input type="password" id="password" required
                  class="w-full pl-11 pr-4 py-3 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-brand/20 focus:border-brand transition-all"
                  placeholder="請輸入密碼">
              </div>
            </div>

            <button type="submit" id="login-btn" class="btn-primary w-full py-3 mt-2 flex justify-center items-center gap-2">
              <span>登入系統</span>
            </button>
          </form>
        </div>
      </div>
    `;

    createIcons({
      icons: { Layers, User, Lock },
      attrs: { class: 'w-5 h-5' }
    });

    this.bindEvents();
  },

  bindEvents() {
    const form = document.getElementById('login-form');
    const btn = document.getElementById('login-btn');

    form.addEventListener('submit', async (e) => {
      e.preventDefault();
      
      const usernameInput = document.getElementById('username').value.trim();
      const passwordInput = document.getElementById('password').value;
      
      if (!usernameInput || !passwordInput) {
        showToast('請填寫帳號與密碼', 'error');
        return;
      }

      // 變更按鈕狀態
      const originalBtnHtml = btn.innerHTML;
      btn.disabled = true;
      btn.innerHTML = `<span class="animate-pulse">驗證中...</span>`;

      try {
        // 從 settings 集合獲取使用者清單
        let usersList = [];
        try {
          const settingsData = await getCollection('settings');
          const userSetting = settingsData.find(s => s.id === 'users' || (s.users && Array.isArray(s.users)));
          if (userSetting) {
            usersList = userSetting.users || [];
          }
        } catch (err) {
          console.warn('無法讀取 settings，將使用預設帳密', err);
        }

        // 若資料庫完全沒有資料（全新專案），提供預設的 admin 帳號作為 Fallback
        if (usersList.length === 0) {
          usersList = [{ username: "admin", password: "123456", role: "admin" }];
        }

        // 比對帳密
        const user = usersList.find(u => u.username === usernameInput && u.password === passwordInput);

        if (user) {
          // 登入成功
          showToast(`歡迎回來，${user.username}`, 'success');
          AppState.setUser({ username: user.username, role: user.role });
          
          // 觸發自訂事件，讓 router 接手後續畫面切換
          window.dispatchEvent(new CustomEvent('auth:login', { detail: user }));
        } else {
          // 登入失敗
          showToast('帳號或密碼錯誤', 'error');
          btn.disabled = false;
          btn.innerHTML = originalBtnHtml;
        }

      } catch (error) {
        console.error('Login error:', error);
        showToast('登入過程發生錯誤', 'error');
        btn.disabled = false;
        btn.innerHTML = originalBtnHtml;
      }
    });
  },

  destroy() {
    // 表單會隨 innerHTML 被清空，無需特別清理事件
  }
};

/* ═══ EOF ═══
F: AI-WA-UPWO/src/pages/login.js
V: 1792909602
W: UPGRADE-002
═══ */```

<!-- === FILE END: ./src/pages/login.js === -->

<!-- === FILE START: ./src/pages/overview.js === -->

## 📄 ./src/pages/overview.js

```js
import { AppState } from '../config/state.js';
import { listenCollection } from '../db/read.js';
import { renderPageLayout } from '../ui/layout.js';
import { groupByStatus, generatePivotTable } from '../domain/stats.js';
import { createIcons, Inbox, PieChart, LayoutDashboard } from 'lucide';

let unsubscribe = null;
let currentOrders = [];

export default {
  async render() {
    const user = AppState.getUser();
    if (!user) return;

    // 1. 渲染頁面骨架
    renderPageLayout({
      title: '總狀態視圖',
      subtitle: '全廠工單狀態分佈與銷售員樞紐分析',
      headerActions: '',
      stats: [],
      controls: '',
      listContainerId: 'overview-container'
    });

    // 2. 初始化資料監聽
    this.initDataListener();
  },

  initDataListener() {
    unsubscribe = listenCollection('work_orders', (orders) => {
      currentOrders = orders;
      this.renderOverview(orders);
    });
  },

  renderOverview(orders) {
    const container = document.getElementById('overview-container');
    if (!container) return;

    const statuses = AppState.getStatuses() || ['排產中', '印刷', '裁切', '品檢', '出貨'];

    if (orders.length === 0) {
      container.innerHTML = `
        <div class="flex flex-col items-center justify-center h-64 text-slate-400 gap-3 bg-white rounded-2xl border border-slate-100 shadow-soft">
          <i data-lucide="inbox" class="w-12 h-12 opacity-30"></i>
          <span class="text-base font-medium">目前尚無工單資料</span>
        </div>
      `;
      createIcons({ icons: { Inbox }, attrs: { class: 'w-12 h-12' } });
      return;
    }

    // 取得所有出現過的銷售員（作為樞紐表的欄）
    const allRepsSet = new Set();
    orders.forEach(o => {
      (o.salesReps || ['未知銷售員']).forEach(r => allRepsSet.add(r));
    });
    const allReps = Array.from(allRepsSet).sort();

    // 生成樞紐表數據
    const pivotData = generatePivotTable(orders, statuses, allReps);
    const grouped = groupByStatus(orders, statuses);

    // 上半部：樞紐交叉表
    let pivotHtml = `
      <div class="bg-white rounded-[20px] border border-slate-200/60 shadow-soft overflow-hidden mb-6">
        <div class="px-6 py-4 border-b border-slate-100 bg-slate-50/50 flex items-center gap-2">
          <i data-lucide="pie-chart" class="w-5 h-5 text-brand"></i>
          <h2 class="font-bold text-slate-800">狀態 / 銷售員 樞紐分析</h2>
        </div>
        <div class="overflow-x-auto">
          <table class="w-full text-left border-collapse min-w-[600px]">
            <thead>
              <tr class="border-b border-slate-200/60 text-sm font-semibold text-slate-600 bg-slate-50/30">
                <th class="px-6 py-3 sticky left-0 bg-white/90 backdrop-blur z-10">工序狀態</th>
                ${allReps.map(rep => `<th class="px-4 py-3 text-center whitespace-nowrap">${rep}</th>`).join('')}
                <th class="px-6 py-3 text-center text-brand">總計</th>
              </tr>
            </thead>
            <tbody class="divide-y divide-slate-100 text-sm text-slate-700">
              ${statuses.map(status => {
                const rowData = pivotData[status] || {};
                let rowTotal = 0;
                
                const cellsHtml = allReps.map(rep => {
                  const count = rowData[rep] || 0;
                  rowTotal += count;
                  return `<td class="px-4 py-3 text-center ${count > 0 ? 'font-bold text-slate-800' : 'text-slate-300'}">${count || '-'}</td>`;
                }).join('');

                return `
                  <tr class="hover:bg-slate-50/50 transition-colors">
                    <td class="px-6 py-3 font-medium text-slate-700 sticky left-0 bg-white/90 backdrop-blur z-10">${status}</td>
                    ${cellsHtml}
                    <td class="px-6 py-3 text-center font-bold text-brand">${rowTotal}</td>
                  </tr>
                `;
              }).join('')}
            </tbody>
          </table>
        </div>
      </div>
    `;

    // 下半部：各狀態工單看板 (簡化版)
    let boardHtml = `
      <div class="flex items-center gap-2 mb-4">
        <i data-lucide="layout-dashboard" class="w-5 h-5 text-brand"></i>
        <h2 class="font-bold text-slate-800">各狀態工單看板</h2>
      </div>
      <div class="grid grid-cols-1 md:grid-cols-2 xl:grid-cols-3 gap-5 pb-6">
        ${statuses.map(status => {
          const statusOrders = grouped[status] || [];
          if (statusOrders.length === 0) return ''; // 若該狀態無工單則不顯示區塊

          return `
            <div class="bg-white rounded-[20px] border border-slate-200/60 shadow-soft overflow-hidden flex flex-col h-[300px]">
              <div class="px-5 py-3 border-b border-slate-100 bg-slate-50/50 flex justify-between items-center">
                <h3 class="font-bold text-slate-700 flex items-center gap-2">
                  <span class="w-2 h-2 rounded-full bg-brand"></span> ${status}
                </h3>
                <span class="text-xs font-medium bg-slate-200 text-slate-600 px-2 py-0.5 rounded-full">${statusOrders.length}</span>
              </div>
              <div class="p-3 flex flex-col gap-2 flex-1 overflow-y-auto kanban-scroll inset-panel m-3">
                ${statusOrders.map(order => `
                  <div class="bg-white p-3 rounded-xl border border-slate-100 shadow-sm hover:shadow-md transition-shadow cursor-default flex justify-between items-center">
                    <div class="flex flex-col">
                      <span class="font-bold text-slate-800 text-sm">${order.id}</span>
                      <span class="text-xs text-slate-500 mt-1">${(order.salesReps || []).join(', ')}</span>
                    </div>
                    ${order.dueDate ? `<span class="text-[10px] font-medium bg-amber-50 text-amber-600 px-2 py-1 rounded-md">${order.dueDate}</span>` : ''}
                  </div>
                `).join('')}
              </div>
            </div>
          `;
        }).join('')}
      </div>
    `;

    container.innerHTML = pivotHtml + boardHtml;
    createIcons({
      icons: { PieChart, LayoutDashboard },
      attrs: { class: 'w-5 h-5' }
    });
  },

  destroy() {
    if (unsubscribe) {
      unsubscribe();
      unsubscribe = null;
    }
    currentOrders = [];
  }
};

/* ═══ EOF ═══
F: AI-WA-UPWO/src/pages/overview.js
V: 1792909600
W: UPGRADE-002
═══ */```

<!-- === FILE END: ./src/pages/overview.js === -->

<!-- === FILE START: ./src/pages/salesperson.js === -->

## 📄 ./src/pages/salesperson.js

```js
import { AppState } from '../config/state.js';
import { listenCollection } from '../db/read.js';
import { renderPageLayout } from '../ui/layout.js';
import { groupByStatus } from '../domain/stats.js';
import { createIcons, Inbox, Users, UserCircle, LayoutGrid, List, User, Calendar } from 'lucide';

let unsubscribe = null;
let currentOrders = [];
let allReps = [];
let currentRep = null;
let currentView = 'card'; // 'card' or 'table'

export default {
  async render() {
    const user = AppState.getUser();
    if (!user) return;

    // 1. 渲染頁面骨架
    renderPageLayout({
      title: '銷售員視圖',
      subtitle: '依銷售員檢視名下工單與狀態分佈',
      headerActions: `
        <!-- 視圖切換按鈕 -->
        <div class="segmented-control">
          <button id="btn-view-card" class="segment-btn ${currentView === 'card' ? 'active' : ''} flex items-center gap-1">
            <i data-lucide="layout-grid" class="w-4 h-4"></i> 卡片
          </button>
          <button id="btn-view-table" class="segment-btn ${currentView === 'table' ? 'active' : ''} flex items-center gap-1">
            <i data-lucide="list" class="w-4 h-4"></i> 表格
          </button>
        </div>
      `,
      stats: [],
      controls: '',
      listContainerId: 'salesperson-container'
    });

    // 2. 綁定視圖切換事件
    document.getElementById('btn-view-card')?.addEventListener('click', () => {
      currentView = 'card';
      document.getElementById('btn-view-card').classList.add('active');
      document.getElementById('btn-view-table').classList.remove('active');
      this.renderRepBoard();
    });
    document.getElementById('btn-view-table')?.addEventListener('click', () => {
      currentView = 'table';
      document.getElementById('btn-view-table').classList.add('active');
      document.getElementById('btn-view-card').classList.remove('active');
      this.renderRepBoard();
    });

    createIcons({
      icons: { LayoutGrid, List },
      attrs: { class: 'w-4 h-4' }
    });

    // 3. 初始化資料監聽
    this.initDataListener();
  },

  initDataListener() {
    unsubscribe = listenCollection('work_orders', (orders) => {
      currentOrders = orders;
      this.extractRepsAndRender();
    });
  },

  extractRepsAndRender() {
    // 提取所有不重複的銷售員
    const repsSet = new Set();
    currentOrders.forEach(o => {
      (o.salesReps || ['未填寫']).forEach(r => repsSet.add(r));
    });
    allReps = Array.from(repsSet).sort();

    // 預設選中第一個銷售員
    if (allReps.length > 0 && (!currentRep || !allReps.includes(currentRep))) {
      currentRep = allReps[0];
    }

    this.renderLayout();
  },

  renderLayout() {
    const container = document.getElementById('salesperson-container');
    if (!container) return;

    if (allReps.length === 0) {
      container.innerHTML = `
        <div class="flex flex-col items-center justify-center h-64 text-slate-400 gap-3 bg-white rounded-2xl border border-slate-100 shadow-soft">
          <i data-lucide="inbox" class="w-12 h-12 opacity-30"></i>
          <span class="text-base font-medium">目前尚無銷售員資料或工單</span>
        </div>
      `;
      createIcons({ icons: { Inbox }, attrs: { class: 'w-12 h-12' } });
      return;
    }

    container.innerHTML = `
      <div class="flex flex-col md:flex-row gap-6 h-[calc(100vh-220px)]">
        
        <!-- 左側：銷售員清單 -->
        <div class="w-full md:w-64 flex-shrink-0 flex flex-col bg-white rounded-[20px] border border-slate-200/60 shadow-soft overflow-hidden">
          <div class="px-5 py-4 border-b border-slate-100 bg-slate-50/50 flex items-center gap-2">
            <i data-lucide="users" class="w-5 h-5 text-brand"></i>
            <h2 class="font-bold text-slate-800">銷售員清單</h2>
          </div>
          <div class="flex-1 overflow-y-auto p-2 flex flex-col gap-1 kanban-scroll">
            ${allReps.map(rep => `
              <button class="rep-item text-left px-4 py-3 rounded-xl transition-colors text-sm font-medium flex items-center justify-between ${rep === currentRep ? 'bg-brand/10 text-brand' : 'text-slate-600 hover:bg-slate-50'}" data-rep="${rep}">
                <span>${rep}</span>
                ${rep === currentRep ? '<span class="w-1.5 h-1.5 rounded-full bg-brand"></span>' : ''}
              </button>
            `).join('')}
          </div>
        </div>

        <!-- 右側：該銷售員的工單 -->
        <div class="flex-1 flex flex-col min-w-0 bg-white rounded-[20px] border border-slate-200/60 shadow-soft overflow-hidden">
          <div class="px-5 py-4 border-b border-slate-100 bg-slate-50/50 flex justify-between items-center">
            <h2 class="font-bold text-slate-800 flex items-center gap-2">
              <i data-lucide="user-circle" class="w-5 h-5 text-brand"></i>
              ${currentRep} 的工單
            </h2>
          </div>
          <div class="flex-1 p-4 overflow-hidden bg-slate-50/30 flex flex-col" id="rep-board">
            <!-- 由 renderRepBoard 注入 -->
          </div>
        </div>
      </div>
    `;

    createIcons({
      icons: { Users, UserCircle },
      attrs: { class: 'w-5 h-5' }
    });

    this.bindEvents();
    this.renderRepBoard();
  },

  renderRepBoard() {
    const boardContainer = document.getElementById('rep-board');
    if (!boardContainer) return;

    const statuses = AppState.getStatuses() || ['排產中', '印刷', '裁切', '品檢', '出貨'];
    
    // 過濾出當前選中銷售員的工單
    const repOrders = currentOrders.filter(o => (o.salesReps || []).includes(currentRep));
    
    if (currentView === 'card') {
      // 卡片視圖
      const grouped = groupByStatus(repOrders, statuses);
      
      boardContainer.innerHTML = `
        <div class="flex gap-4 overflow-x-auto kanban-scroll h-full pb-2">
          ${statuses.map(status => {
            const statusOrders = grouped[status] || [];
            return `
              <div class="w-[280px] flex-shrink-0 flex flex-col gap-2">
                <div class="flex items-center justify-between px-2">
                  <h3 class="font-semibold text-slate-700 text-sm flex items-center gap-2">
                    <span class="w-2 h-2 rounded-full ${statusOrders.length > 0 ? 'bg-brand' : 'bg-slate-300'}"></span>
                    ${status}
                  </h3>
                  <span class="text-xs font-medium bg-slate-200/80 text-slate-600 px-2 py-0.5 rounded-full">${statusOrders.length}</span>
                </div>
                <div class="inset-panel p-2 flex flex-col gap-2 min-h-[150px] flex-1 overflow-y-auto kanban-scroll">
                  ${statusOrders.length === 0 ? `
                    <div class="flex items-center justify-center h-full text-slate-300 text-xs py-4">無</div>
                  ` : statusOrders.map(order => {
                    const cat = order.data && order.data.category ? order.data.category : '其他';
                    return `
                      <div class="bg-white p-3 rounded-xl border border-slate-100 shadow-sm text-sm">
                        <div class="flex justify-between items-start mb-1">
                          <span class="font-bold text-slate-800">${order.id}</span>
                          <span class="text-[10px] bg-slate-100 px-1.5 py-0.5 rounded text-slate-500">${cat}</span>
                        </div>
                        <div class="text-[10px] text-slate-400 flex justify-between items-center mt-2 pt-2 border-t border-slate-50">
                          <span>交期：${order.dueDate || '-'}</span>
                        </div>
                      </div>
                    `;
                  }).join('')}
                </div>
              </div>
            `;
          }).join('')}
        </div>
      `;
    } else {
      // 表格視圖
      if (repOrders.length === 0) {
        boardContainer.innerHTML = `<div class="flex items-center justify-center h-full text-slate-400">此銷售員目前無工單</div>`;
        return;
      }
      
      boardContainer.innerHTML = `
        <div class="bg-white rounded-xl border border-slate-200/60 shadow-sm overflow-hidden flex flex-col w-full h-full">
          <div class="overflow-auto flex-1 kanban-scroll">
            <table class="w-full text-left border-collapse min-w-[600px]">
              <thead class="sticky top-0 bg-slate-50 z-10 shadow-sm">
                <tr class="border-b border-slate-200/60 text-sm font-semibold text-slate-600">
                  <th class="px-6 py-3">工單號碼</th>
                  <th class="px-6 py-3">類別</th>
                  <th class="px-6 py-3">當前狀態</th>
                  <th class="px-6 py-3">交貨日期</th>
                </tr>
              </thead>
              <tbody class="divide-y divide-slate-100 text-sm text-slate-700">
                ${repOrders.map(order => {
                  const cat = order.data && order.data.category ? order.data.category : '-';
                  return `
                    <tr class="hover:bg-slate-50/50 transition-colors">
                      <td class="px-6 py-3 font-bold text-slate-800">${order.id}</td>
                      <td class="px-6 py-3"><span class="px-2 py-1 bg-slate-100 rounded text-xs">${cat}</span></td>
                      <td class="px-6 py-3"><span class="px-2 py-1 bg-brand/10 text-brand rounded text-xs font-medium">${order.status}</span></td>
                      <td class="px-6 py-3 ${order.dueDate ? 'text-amber-600 font-medium' : 'text-slate-400'}">${order.dueDate || '-'}</td>
                    </tr>
                  `;
                }).join('')}
              </tbody>
            </table>
          </div>
        </div>
      `;
    }
  },

  bindEvents() {
    // 點擊左側銷售員切換
    document.querySelectorAll('.rep-item').forEach(btn => {
      btn.addEventListener('click', (e) => {
        const newRep = e.currentTarget.dataset.rep;
        if (newRep !== currentRep) {
          currentRep = newRep;
          this.renderLayout();
        }
      });
    });
  },

  destroy() {
    if (unsubscribe) {
      unsubscribe();
      unsubscribe = null;
    }
    currentOrders = [];
    allReps = [];
    currentRep = null;
  }
};

/* ═══ EOF ═══
F: AI-WA-UPWO/src/pages/salesperson.js
V: 1792912400
W: UPGRADE-002
═══ */```

<!-- === FILE END: ./src/pages/salesperson.js === -->

<!-- === FILE START: ./src/pages/settings.js === -->

## 📄 ./src/pages/settings.js

```js
import * as XLSX from 'xlsx';
import { AppState } from '../config/state.js';
import { listenDocument, getDocument, getCollection } from '../db/read.js';
import { setDocument, updateDocument, deleteDocument } from '../db/write.js';
import { renderPageLayout } from '../ui/layout.js';
import { showToast } from '../ui/toast.js';
import { openConfirm } from '../ui/modal.js';
import { parseExcelData } from '../domain/parser.js';
import { createIcons, Upload, Plus, Trash2, GripVertical, FileSpreadsheet, Users, Tag, Shield, X, List, Database, Edit2 } from 'lucide';

let unsubscribe = null;
let currentSettings = { statuses: [], sales_mapping: [], users: [], schema: [] };
let activeTab = 'statuses'; // 'statuses', 'mapping', 'schema', 'users', 'data', 'import'

export default {
  async render() {
    const user = AppState.getUser();
    if (!user || user.role !== 'admin') {
      document.getElementById('app-content').innerHTML = '<div class="p-8 text-center text-rose-500 font-bold">您沒有權限訪問此頁面</div>';
      return;
    }

    // 1. 渲染頁面骨架
    renderPageLayout({
      title: '系統設定',
      subtitle: '管理工序狀態、銷售員對照表、系統欄位與帳號',
      headerActions: '',
      stats: [],
      controls: '',
      listContainerId: 'settings-container'
    });

    // 2. 優先渲染 UI 骨架
    this.renderTabs();

    // 3. 初始化資料監聽
    this.initDataListener();
  },

  initDataListener() {
    unsubscribe = listenDocument('settings', 'main', async (data) => {
      if (data) {
        currentSettings = {
          statuses: data.statuses || ['排產中'],
          sales_mapping: data.sales_mapping || [],
          users: data.users || [{ username: 'admin', password: '123456', role: 'admin' }],
          schema: data.schema || this.getDefaultSchema()
        };
      } else {
        currentSettings = {
          statuses: ['排產中', '印刷', '裁切', '品檢', '出貨'],
          sales_mapping: [],
          users: [{ username: 'admin', password: '123456', role: 'admin' }],
          schema: this.getDefaultSchema()
        };
        await setDocument('settings', 'main', currentSettings);
      }
      
      AppState.setStatuses(currentSettings.statuses);
      this.renderTabs();
    });
  },

  getDefaultSchema() {
    return [
      { key: 'id', label: '發票', type: 'string', isSystem: true },
      { key: 'uploadDate', label: '日期', type: 'date', isSystem: true },
      { key: 'salesRep', label: '我方備考', type: 'string', isSystem: true },
      { key: 'amount', label: '金額', type: 'number', isSystem: false },
      { key: 'category', label: '類別', type: 'string', isSystem: false },
      { key: 'customerNameZh', label: '中文名稱', type: 'string', isSystem: false },
      { key: 'customerNameEn', label: '名稱', type: 'string', isSystem: false }
    ];
  },

  renderTabs() {
    const container = document.getElementById('settings-container');
    if (!container) return;

    container.innerHTML = `
      <div class="bg-white rounded-[20px] border border-slate-200/60 shadow-soft overflow-hidden min-h-[500px] flex flex-col md:flex-row">
        
        <!-- 左側設定選單 -->
        <div class="w-full md:w-64 border-r border-slate-100 bg-slate-50/50 p-4 flex flex-col gap-1 flex-shrink-0 overflow-y-auto">
          <button class="tab-btn ${activeTab === 'statuses' ? 'bg-white text-brand shadow-sm border border-slate-200/50' : 'text-slate-600 hover:bg-slate-100 border border-transparent'} text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3" data-tab="statuses">
            <i data-lucide="tag" class="w-4 h-4"></i> 狀態管理
          </button>
          <button class="tab-btn ${activeTab === 'mapping' ? 'bg-white text-brand shadow-sm border border-slate-200/50' : 'text-slate-600 hover:bg-slate-100 border border-transparent'} text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3" data-tab="mapping">
            <i data-lucide="users" class="w-4 h-4"></i> 對照表管理
          </button>
          <button class="tab-btn ${activeTab === 'schema' ? 'bg-white text-brand shadow-sm border border-slate-200/50' : 'text-slate-600 hover:bg-slate-100 border border-transparent'} text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3" data-tab="schema">
            <i data-lucide="list" class="w-4 h-4"></i> 欄位管理
          </button>
          <button class="tab-btn ${activeTab === 'users' ? 'bg-white text-brand shadow-sm border border-slate-200/50' : 'text-slate-600 hover:bg-slate-100 border border-transparent'} text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3" data-tab="users">
            <i data-lucide="shield" class="w-4 h-4"></i> 帳號管理
          </button>
          <div class="border-t border-slate-200/60 my-2"></div>
          <button class="tab-btn ${activeTab === 'data' ? 'bg-white text-brand shadow-sm border border-slate-200/50' : 'text-slate-600 hover:bg-slate-100 border border-transparent'} text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3" data-tab="data">
            <i data-lucide="database" class="w-4 h-4"></i> 資料管理
          </button>
          <button class="tab-btn ${activeTab === 'import' ? 'bg-white text-brand shadow-sm border border-slate-200/50' : 'text-slate-600 hover:bg-slate-100 border border-transparent'} text-left px-4 py-3 rounded-xl font-medium text-sm transition-all flex items-center gap-3" data-tab="import">
            <i data-lucide="file-spreadsheet" class="w-4 h-4"></i> 匯入 Excel
          </button>
        </div>

        <!-- 右側內容區 -->
        <div class="flex-1 p-6 md:p-8" id="settings-content">
          <!-- 由 renderContent 注入 -->
        </div>
      </div>
    `;

    createIcons({
      icons: { Tag, Users, Shield, FileSpreadsheet, List, Database },
      attrs: { class: 'w-4 h-4' }
    });

    this.bindTabEvents();
    this.renderContent();
  },

  bindTabEvents() {
    document.querySelectorAll('.tab-btn').forEach(btn => {
      btn.addEventListener('click', (e) => {
        activeTab = e.currentTarget.dataset.tab;
        this.renderTabs();
      });
    });
  },

  renderContent() {
    const content = document.getElementById('settings-content');
    if (!content) return;

    if (activeTab === 'statuses') this.renderStatuses(content);
    else if (activeTab === 'mapping') this.renderMapping(content);
    else if (activeTab === 'schema') this.renderSchema(content);
    else if (activeTab === 'users') this.renderUsers(content);
    else if (activeTab === 'data') this.renderDataManagement(content);
    else if (activeTab === 'import') this.renderImport(content);
  },

  // ═══════════ 狀態管理（含同步更新） ═══════════
  renderStatuses(container) {
    container.innerHTML = `
      <div class="flex justify-between items-center mb-6">
        <div>
          <h2 class="text-lg font-bold text-slate-800">工序狀態管理</h2>
          <p class="text-sm text-slate-500 mt-1">可拖曳排序。修改名稱時會同步更新歷史工單。</p>
        </div>
      </div>
      
      <div class="flex gap-2 mb-6">
        <input type="text" id="new-status-input" class="flex-1 px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-brand/20 focus:border-brand" placeholder="輸入新狀態名稱...">
        <button id="btn-add-status" class="btn-primary px-4 py-2.5 text-sm flex items-center gap-2">
          <i data-lucide="plus" class="w-4 h-4"></i> 新增
        </button>
      </div>

      <div class="inset-panel p-4 flex flex-col gap-2" id="status-list">
        ${currentSettings.statuses.map((status, index) => `
          <div class="flex items-center gap-3 bg-white p-3 rounded-xl border border-slate-100 shadow-sm status-item" draggable="true" data-index="${index}">
            <i data-lucide="grip-vertical" class="w-5 h-5 text-slate-300 cursor-grab"></i>
            <span class="flex-1 font-medium text-slate-700 status-name">${status}</span>
            <button class="p-1.5 text-slate-400 hover:text-brand hover:bg-slate-50 rounded-lg transition-colors btn-edit-status" data-status="${status}">
              <i data-lucide="edit-2" class="w-4 h-4"></i>
            </button>
            <button class="p-1.5 text-slate-400 hover:text-rose-500 hover:bg-rose-50 rounded-lg transition-colors btn-del-status" data-status="${status}">
              <i data-lucide="trash-2" class="w-4 h-4"></i>
            </button>
          </div>
        `).join('')}
      </div>
    `;

    createIcons({ icons: { Plus, GripVertical, Trash2, Edit2 }, attrs: { class: 'w-4 h-4' } });

    document.getElementById('btn-add-status')?.addEventListener('click', async () => {
      const input = document.getElementById('new-status-input');
      const val = input.value.trim();
      if (!val) return;
      if (currentSettings.statuses.includes(val)) {
        showToast('狀態已存在', 'error');
        return;
      }
      currentSettings.statuses.push(val);
      await this.saveSettings();
      showToast('新增成功', 'success');
    });

    // 編輯狀態（同步更新歷史工單）
    document.querySelectorAll('.btn-edit-status').forEach(btn => {
      btn.addEventListener('click', async (e) => {
        const oldStatus = e.currentTarget.dataset.status;
        const newStatus = window.prompt(`修改狀態名稱（原為「${oldStatus}」）：`, oldStatus);
        
        if (!newStatus || newStatus.trim() === "" || newStatus === oldStatus) return;
        const trimmedNewStatus = newStatus.trim();

        showToast('正在更新狀態與歷史資料...', 'info');

        try {
          // 1. 更新記憶體中的設定
          const idx = currentSettings.statuses.indexOf(oldStatus);
          if (idx !== -1) currentSettings.statuses[idx] = trimmedNewStatus;
          await this.saveSettings();

          // 2. 掃描並更新歷史工單
          const orders = await getCollection('work_orders');
          let updateCount = 0;
          for (const order of orders) {
            if (order.status === oldStatus) {
              await updateDocument('work_orders', order.id, { status: trimmedNewStatus });
              updateCount++;
            }
          }
          showToast(`已修改狀態，並同步更新 ${updateCount} 筆工單`, 'success');
        } catch (err) {
          console.error(err);
          showToast('修改失敗，請稍後再試', 'error');
        }
      });
    });

    document.querySelectorAll('.btn-del-status').forEach(btn => {
      btn.addEventListener('click', async (e) => {
        const status = e.currentTarget.dataset.status;
        const confirmed = await openConfirm({
          title: '刪除狀態',
          message: `確定要刪除「${status}」嗎？系統將檢查是否有工單處於此狀態。`,
          isDanger: true,
          confirmText: '檢查並刪除'
        });

        if (!confirmed) return;

        try {
          const orders = await getCollection('work_orders');
          const hasOrders = orders.some(o => o.status === status);
          
          if (hasOrders) {
            showToast(`無法刪除：仍有工單處於「${status}」狀態`, 'error');
            return;
          }

          currentSettings.statuses = currentSettings.statuses.filter(s => s !== status);
          await this.saveSettings();
          showToast('刪除成功', 'success');
        } catch (err) {
          showToast('檢查工單失敗', 'error');
        }
      });
    });

    this.initDragAndDrop();
  },

  initDragAndDrop() {
    const list = document.getElementById('status-list');
    if (!list) return;
    let draggedItem = null;

    list.querySelectorAll('.status-item').forEach(item => {
      item.addEventListener('dragstart', function() {
        draggedItem = this;
        setTimeout(() => this.classList.add('opacity-50'), 0);
      });
      item.addEventListener('dragend', function() {
        this.classList.remove('opacity-50');
        draggedItem = null;
        const newOrder = Array.from(list.querySelectorAll('.status-item')).map(el => el.querySelector('.status-name').textContent);
        currentSettings.statuses = newOrder;
        const saveHandler = async () => {
          await this.saveSettings();
          showToast('排序已更新', 'success');
        };
        saveHandler.call(this);
      });
      item.addEventListener('dragover', function(e) {
        e.preventDefault();
        if (this === draggedItem) return;
        const rect = this.getBoundingClientRect();
        const next = (e.clientY - rect.top) / (rect.bottom - rect.top) > 0.5;
        list.insertBefore(draggedItem, next ? this.nextSibling : this);
      });
    });
  },

  // ═══════════ 對照表管理（含同步更新） ═══════════
  renderMapping(container) {
    container.innerHTML = `
      <div class="flex justify-between items-center mb-6">
        <div>
          <h2 class="text-lg font-bold text-slate-800">銷售員對照表</h2>
          <p class="text-sm text-slate-500 mt-1">設定別名，匯入時自動對應。新增後會自動同步更新歷史工單。</p>
        </div>
      </div>

      <div class="flex gap-2 mb-6">
        <input type="text" id="new-rep-name" class="w-1/3 px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none" placeholder="正式名稱 (例：王小明)">
        <input type="text" id="new-rep-alias" class="flex-1 px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none" placeholder="別名，用逗號隔開 (例：123, S-123)">
        <button id="btn-add-rep" class="btn-primary px-4 py-2.5 text-sm flex items-center gap-2">
          <i data-lucide="plus" class="w-4 h-4"></i> 新增
        </button>
      </div>

      <div class="flex flex-col gap-2">
        ${currentSettings.sales_mapping.length === 0 ? '<div class="text-slate-400 text-sm py-4 text-center">尚無對照表資料</div>' : ''}
        ${currentSettings.sales_mapping.map((rep, index) => `
          <div class="flex items-center gap-4 bg-white p-4 rounded-xl border border-slate-100 shadow-sm">
            <div class="w-1/3 font-bold text-slate-800">${rep.name}</div>
            <div class="flex-1 flex flex-wrap gap-1">
              ${(rep.aliases || []).map(a => `<span class="px-2 py-0.5 bg-slate-100 text-slate-600 rounded text-xs">${a}</span>`).join('')}
            </div>
            <button class="p-1.5 text-slate-400 hover:text-rose-500 hover:bg-rose-50 rounded-lg transition-colors btn-del-rep" data-index="${index}">
              <i data-lucide="trash-2" class="w-4 h-4"></i>
            </button>
          </div>
        `).join('')}
      </div>
    `;

    createIcons({ icons: { Plus, Trash2 }, attrs: { class: 'w-4 h-4' } });

    document.getElementById('btn-add-rep')?.addEventListener('click', async () => {
      const name = document.getElementById('new-rep-name').value.trim();
      const aliasStr = document.getElementById('new-rep-alias').value.trim();
      if (!name || !aliasStr) {
        showToast('請輸入正式名稱與至少一個別名', 'error');
        return;
      }
      
      const aliases = aliasStr.split(',').map(s => s.trim()).filter(Boolean);
      
      // 儲存對照表
      currentSettings.sales_mapping.push({ name, aliases });
      await this.saveSettings();

      showToast('正在同步歷史工單...', 'info');

      // 同步更新歷史資料
      try {
        const orders = await getCollection('work_orders');
        let updateCount = 0;
        
        for (const order of orders) {
          let reps = order.salesReps || [];
          let changed = false;
          
          aliases.forEach(alias => {
            if (reps.includes(alias)) {
              reps = reps.filter(r => r !== alias); // 移除別名
              reps.push(name); // 加入正式名稱
              changed = true;
            }
          });

          if (changed) {
            await updateDocument('work_orders', order.id, { salesReps: [...new Set(reps)] });
            updateCount++;
          }
        }
        
        showToast(`新增成功，同步更新了 ${updateCount} 筆工單`, 'success');
      } catch (err) {
        console.error(err);
        showToast('對照表已新增，但同步歷史資料失敗', 'error');
      }
    });

    document.querySelectorAll('.btn-del-rep').forEach(btn => {
      btn.addEventListener('click', async (e) => {
        const idx = e.currentTarget.dataset.index;
        currentSettings.sales_mapping.splice(idx, 1);
        await this.saveSettings();
        showToast('刪除成功（歷史資料保留原名稱）', 'success');
      });
    });
  },

  // ═══════════ 欄位管理 ═══════════
  renderSchema(container) {
    const sysFields = currentSettings.schema.filter(f => f.isSystem);
    const customFields = currentSettings.schema.filter(f => !f.isSystem);

    container.innerHTML = `
      <div class="flex justify-between items-center mb-6">
        <div>
          <h2 class="text-lg font-bold text-slate-800">動態欄位管理</h2>
          <p class="text-sm text-slate-500 mt-1">定義系統欄位，並對應 Excel 表頭。系統核心欄位不可刪除。</p>
        </div>
      </div>

      <div class="flex gap-2 mb-6">
        <input type="text" id="new-field-label" class="flex-1 px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none" placeholder="新增欄位名稱 (例如：利潤)">
        <button id="btn-add-field" class="btn-primary px-4 py-2.5 text-sm flex items-center gap-2">
          <i data-lucide="plus" class="w-4 h-4"></i> 新增
        </button>
      </div>

      <div class="mb-4 text-sm font-semibold text-slate-600">系統核心欄位（不可刪除）</div>
      <div class="flex flex-col gap-2 mb-8">
        ${sysFields.map(f => `
          <div class="flex items-center gap-4 bg-slate-50 p-4 rounded-xl border border-slate-200">
            <div class="w-1/3 font-bold text-slate-700">${f.label}</div>
            <div class="w-1/3 text-slate-400 font-mono text-xs">${f.key}</div>
            <div class="w-1/3 text-right text-slate-400 text-xs">系統保護</div>
          </div>
        `).join('')}
      </div>

      <div class="mb-4 text-sm font-semibold text-slate-600">自訂欄位</div>
      <div class="flex flex-col gap-2">
        ${customFields.length === 0 ? '<div class="text-slate-400 text-sm py-2">目前尚無自訂欄位</div>' : ''}
        ${customFields.map((f, index) => `
          <div class="flex items-center gap-4 bg-white p-4 rounded-xl border border-slate-100 shadow-sm">
            <div class="w-1/3 font-bold text-slate-800">${f.label}</div>
            <div class="w-1/3 text-slate-500 font-mono text-xs">${f.key}</div>
            <div class="w-1/3 text-right">
              <button class="p-1.5 text-slate-400 hover:text-rose-500 hover:bg-rose-50 rounded-lg transition-colors btn-del-field" data-key="${f.key}">
                <i data-lucide="trash-2" class="w-4 h-4"></i>
              </button>
            </div>
          </div>
        `).join('')}
      </div>
    `;

    createIcons({ icons: { Plus, Trash2 }, attrs: { class: 'w-4 h-4' } });

    document.getElementById('btn-add-field')?.addEventListener('click', async () => {
      const input = document.getElementById('new-field-label');
      const val = input.value.trim();
      if (!val) {
        showToast('請輸入欄位名稱', 'error');
        return;
      }
      const key = 'field_' + Date.now().toString(36);
      currentSettings.schema.push({ key, label: val, type: 'string', isSystem: false });
      await this.saveSettings();
      showToast('新增成功', 'success');
    });

    document.querySelectorAll('.btn-del-field').forEach(btn => {
      btn.addEventListener('click', async (e) => {
        const key = e.currentTarget.dataset.key;
        const confirmed = await openConfirm({
          title: '刪除自訂欄位',
          message: `確定要刪除該欄位嗎？(歷史資料將保留，僅不再顯示)`,
          isDanger: true
        });
        if (!confirmed) return;
        currentSettings.schema = currentSettings.schema.filter(f => f.key !== key);
        await this.saveSettings();
        showToast('已刪除欄位', 'success');
      });
    });
  },

  // ═══════════ 帳號管理 ═══════════
  renderUsers(container) {
    container.innerHTML = `
      <div class="flex justify-between items-center mb-6">
        <div>
          <h2 class="text-lg font-bold text-slate-800">帳號管理</h2>
          <p class="text-sm text-slate-500 mt-1">設定登入系統的帳號與角色。</p>
        </div>
      </div>

      <div class="flex gap-2 mb-6">
        <input type="text" id="new-user-name" class="w-1/3 px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none" placeholder="帳號">
        <input type="text" id="new-user-pass" class="w-1/3 px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none" placeholder="密碼">
        <select id="new-user-role" class="w-1/6 px-4 py-2.5 bg-slate-50 border border-slate-200 rounded-xl text-sm focus:outline-none">
          <option value="user">一般使用者</option>
          <option value="admin">管理員</option>
        </select>
        <button id="btn-add-user" class="btn-primary px-4 py-2.5 text-sm flex items-center gap-2">
          <i data-lucide="plus" class="w-4 h-4"></i> 新增
        </button>
      </div>

      <div class="flex flex-col gap-2">
        ${currentSettings.users.map((u, index) => `
          <div class="flex items-center gap-4 bg-white p-4 rounded-xl border border-slate-100 shadow-sm">
            <div class="w-1/3 font-bold text-slate-800">${u.username}</div>
            <div class="w-1/3 text-slate-500 font-mono text-sm">••••••••</div>
            <div class="w-1/6">
              <span class="px-2 py-1 text-xs font-medium rounded-md ${u.role === 'admin' ? 'bg-brand/10 text-brand' : 'bg-slate-100 text-slate-600'}">${u.role}</span>
            </div>
            <button class="p-1.5 text-slate-400 hover:text-rose-500 hover:bg-rose-50 rounded-lg transition-colors btn-del-user" data-index="${index}" ${u.username === 'admin' ? 'disabled title="無法刪除預設 admin"' : ''}>
              <i data-lucide="trash-2" class="w-4 h-4"></i>
            </button>
          </div>
        `).join('')}
      </div>
    `;

    createIcons({ icons: { Plus, Trash2 }, attrs: { class: 'w-4 h-4' } });

    document.getElementById('btn-add-user')?.addEventListener('click', async () => {
      const name = document.getElementById('new-user-name').value.trim();
      const pass = document.getElementById('new-user-pass').value.trim();
      const role = document.getElementById('new-user-role').value;
      
      if (!name || !pass) {
        showToast('請填寫帳號與密碼', 'error');
        return;
      }
      if (currentSettings.users.some(u => u.username === name)) {
        showToast('帳號已存在', 'error');
        return;
      }
      currentSettings.users.push({ username: name, password: pass, role });
      await this.saveSettings();
      showToast('新增成功', 'success');
    });

    document.querySelectorAll('.btn-del-user').forEach(btn => {
      btn.addEventListener('click', async (e) => {
        if (e.currentTarget.disabled) return;
        const idx = e.currentTarget.dataset.index;
        currentSettings.users.splice(idx, 1);
        await this.saveSettings();
        showToast('刪除成功', 'success');
      });
    });
  },

  // ═══════════ 資料管理 ═══════════
  renderDataManagement(container) {
    container.innerHTML = `
      <div class="flex justify-between items-center mb-6">
        <div>
          <h2 class="text-lg font-bold text-slate-800">資料管理</h2>
          <p class="text-sm text-slate-500 mt-1">清除由 Excel 匯入的資料，保留您手動輸入的狀態與交期。</p>
        </div>
      </div>

      <div class="bg-rose-50 border border-rose-200 rounded-2xl p-6">
        <h3 class="text-rose-700 font-bold mb-2 flex items-center gap-2">
          <i data-lucide="alert-circle" class="w-5 h-5"></i> 危險操作區
        </h3>
        <p class="text-sm text-rose-600 mb-4">
          點擊下方按鈕將會清空所有工單中「由 Excel 匯入」的資料（如金額、類別、客戶名稱等），但<b>絕對不會</b>刪除工單號碼、當前狀態、交期與狀態更新日。清空後，您可以重新匯入新的 Excel，系統會自動將新資料與您手動填寫的狀態合併對齊。
        </p>
        <button id="btn-clear-excel-data" class="bg-rose-500 hover:bg-rose-600 text-white px-5 py-2.5 rounded-xl text-sm font-medium transition-colors shadow-sm flex items-center gap-2">
          <i data-lucide="trash-2" class="w-4 h-4"></i> 清空 Excel 匯入資料
        </button>
      </div>
    `;

    createIcons({ icons: { Trash2, AlertCircle }, attrs: { class: 'w-4 h-4' } });

    document.getElementById('btn-clear-excel-data')?.addEventListener('click', async () => {
      const confirmed = await openConfirm({
        title: '清空 Excel 資料',
        message: '確定要清空所有工單的 Excel 資料嗎？此操作無法復原，但您手動填寫的狀態與交期會保留。',
        isDanger: true,
        confirmText: '確認清空'
      });

      if (!confirmed) return;

      showToast('正在清空資料...', 'info');

      try {
        const orders = await getCollection('work_orders');
        let count = 0;
        
        for (const order of orders) {
          await updateDocument('work_orders', order.id, {
            salesReps: ["未填寫"],
            uploadDate: "",
            data: { category: order.data?.category || '' } // 保留類別，或視情況清空
          });
          count++;
        }

        showToast(`成功清空 ${count} 筆工單的 Excel 資料`, 'success');
      } catch (error) {
        console.error('Clear data error:', error);
        showToast('清空失敗，請稍後再試', 'error');
      }
    });
  },

  // ═══════════ 匯入 Excel ═══════════
  renderImport(container) {
    container.innerHTML = `
      <div class="flex flex-col items-center justify-center h-full min-h-[300px] border-2 border-dashed border-slate-200 rounded-2xl bg-slate-50/50 p-8 text-center transition-colors hover:bg-slate-50 relative" id="drop-zone">
        <input type="file" id="excel-input" accept=".xlsx, .xls" class="absolute inset-0 w-full h-full opacity-0 cursor-pointer">
        <div class="w-16 h-16 bg-white rounded-2xl shadow-sm flex items-center justify-center mb-4 text-brand">
          <i data-lucide="file-spreadsheet" class="w-8 h-8"></i>
        </div>
        <h3 class="text-lg font-bold text-slate-800 mb-2">點擊或拖曳 Excel 檔案至此</h3>
        <p class="text-sm text-slate-500 max-w-sm">系統將會依據「欄位管理」中的設定，自動解析 Excel 標題並存入資料庫。若單號已存在，將會更新 Excel 資料並保留手動狀態。</p>
      </div>
    `;

    createIcons({ icons: { FileSpreadsheet }, attrs: { class: 'w-8 h-8' } });

    const fileInput = document.getElementById('excel-input');
    
    fileInput?.addEventListener('change', async (e) => {
      const file = e.target.files[0];
      if (!file) return;
      
      const reader = new FileReader();
      reader.onload = async (evt) => {
        try {
          const data = new Uint8Array(evt.target.result);
          const workbook = XLSX.read(data, { type: 'array' });
          const firstSheetName = workbook.SheetNames[0];
          const worksheet = workbook.Sheets[firstSheetName];
          
          const jsonData = XLSX.utils.sheet_to_json(worksheet, { header: 1 });
          if (jsonData.length < 2) {
            showToast('Excel 檔案沒有資料', 'error');
            return;
          }

          const headers = jsonData[0];
          const rows = jsonData.slice(1);

          showToast('解析中，請稍候...', 'info');

          const parsedOrders = parseExcelData(headers, rows, currentSettings.sales_mapping, currentSettings.statuses[0] || '排產中', currentSettings.schema);

          let addedCount = 0;
          let updatedCount = 0;

          for (const order of parsedOrders) {
            const existing = await getDocument('work_orders', order.id);
            if (existing) {
              await updateDocument('work_orders', order.id, {
                salesReps: order.salesReps,
                uploadDate: order.uploadDate,
                data: order.data
              });
              updatedCount++;
            } else {
              await setDocument('work_orders', order.id, order);
              addedCount++;
            }
          }

          showToast(`匯入完成！新增 ${addedCount} 筆，更新 ${updatedCount} 筆。`, 'success');
          fileInput.value = ''; 

        } catch (error) {
          console.error('Import Error:', error);
          showToast(`匯入失敗：${error.message || '請檢查檔案格式'}`, 'error');
        }
      };
      reader.readAsArrayBuffer(file);
    });
  },

  async saveSettings() {
    await setDocument('settings', 'main', currentSettings);
    this.renderTabs();
  },

  destroy() {
    if (unsubscribe) {
      unsubscribe();
      unsubscribe = null;
    }
  }
};

/* ═══ EOF ═══
F: AI-WA-UPWO/src/pages/settings.js
V: 1792912300
W: UPGRADE-002
═══ */```

<!-- === FILE END: ./src/pages/settings.js === -->

<!-- === FILE START: ./src/pages/table.js === -->

## 📄 ./src/pages/table.js

```js
import { AppState } from '../config/state.js';
import { listenCollection, getDocument } from '../db/read.js';
import { deleteDocument } from '../db/write.js';
import { renderPageLayout } from '../ui/layout.js';
import { exportToExcel, exportToPDF } from '../lib/io.js';
import { showToast } from '../ui/toast.js';
import { openConfirm } from '../ui/modal.js';
import { createIcons, Download, FileText, Inbox, Trash2 } from 'lucide';

let unsubscribe = null;
let currentOrders = [];
let currentSchema = [];
let currentCategory = 'ALL';

export default {
  async render() {
    const user = AppState.getUser();
    if (!user) return;

    // 1. 讀取系統設定（取得動態 Schema）
    try {
      const settingsDoc = await getDocument('settings', 'main');
      if (settingsDoc && settingsDoc.schema) {
        currentSchema = settingsDoc.schema.filter(f => !f.isSystem); // 只取自訂欄位
      }
    } catch (err) {
      console.warn('無法讀取欄位設定，將以預設模式顯示', err);
    }

    // 2. 渲染頁面骨架
    renderPageLayout({
      title: '管理總表',
      subtitle: '所有工單的狀態與時間追蹤列表',
      headerActions: `
        <div class="flex items-center gap-2">
          <button id="btn-export-excel" class="bg-white text-slate-700 border border-slate-200 px-3 py-2 rounded-[10px] text-sm font-medium hover:bg-slate-50 transition-colors active:scale-95 flex items-center gap-2 shadow-sm">
            <i data-lucide="download" class="w-4 h-4"></i> Excel
          </button>
          <button id="btn-export-pdf" class="bg-white text-slate-700 border border-slate-200 px-3 py-2 rounded-[10px] text-sm font-medium hover:bg-slate-50 transition-colors active:scale-95 flex items-center gap-2 shadow-sm">
            <i data-lucide="file-text" class="w-4 h-4"></i> PDF
          </button>
        </div>
      `,
      stats: [],
      controls: `
        <div class="flex flex-col sm:flex-row items-start sm:items-center gap-4 mb-4">
          <div class="flex items-center gap-2">
            <span class="text-sm font-medium text-slate-600">類別篩選：</span>
            <select id="filter-category" class="px-3 py-2 bg-white border border-slate-200 rounded-xl text-sm focus:outline-none focus:border-brand shadow-sm min-w-[120px]">
              <option value="ALL">全部類別</option>
              <!-- 由 updateCategoryFilter 注入 -->
            </select>
          </div>
        </div>
      `,
      listContainerId: 'table-container'
    });

    // 3. 綁定事件
    this.bindHeaderEvents();

    createIcons({
      icons: { Download, FileText },
      attrs: { class: 'w-4 h-4' }
    });

    // 4. 初始化資料監聽
    this.initDataListener();
  },

  initDataListener() {
    unsubscribe = listenCollection('work_orders', (orders) => {
      currentOrders = orders;
      this.updateCategoryFilter();
      this.renderTable();
    });
  },

  updateCategoryFilter() {
    const categorySelect = document.getElementById('filter-category');
    if (!categorySelect) return;

    // 提取不重複的類別
    const categoriesSet = new Set();
    currentOrders.forEach(o => {
      let cat = o.data && o.data.category;
      if (!cat) {
        const match = String(o.id).match(/^[a-zA-Z]+/);
        cat = match ? match[0].toUpperCase() : '其他';
      }
      categoriesSet.add(cat);
    });

    const categories = Array.from(categoriesSet).sort();
    
    if (currentCategory !== 'ALL' && !categories.includes(currentCategory)) {
      currentCategory = 'ALL';
    }

    categorySelect.innerHTML = `<option value="ALL">全部類別</option>` + 
      categories.map(c => `<option value="${c}" ${c === currentCategory ? 'selected' : ''}>${c}</option>`).join('');

    // 避免重複綁定，先移除再綁定
    const newSelect = categorySelect.cloneNode(true);
    categorySelect.parentNode.replaceChild(newSelect, categorySelect);
    newSelect.addEventListener('change', (e) => {
      currentCategory = e.target.value;
      this.renderTable();
    });
  },

  renderTable() {
    const container = document.getElementById('table-container');
    if (!container) return;

    // 過濾資料
    let filtered = currentOrders;
    if (currentCategory !== 'ALL') {
      filtered = filtered.filter(o => {
        const cat = o.data && o.data.category ? o.data.category : '其他';
        return cat === currentCategory;
      });
    }

    if (filtered.length === 0) {
      container.innerHTML = `
        <div class="flex flex-col items-center justify-center h-64 text-slate-400 gap-3 bg-white rounded-2xl border border-slate-100 shadow-soft">
          <i data-lucide="inbox" class="w-12 h-12 opacity-30"></i>
          <span class="text-base font-medium">目前無工單資料</span>
        </div>
      `;
      createIcons({ icons: { Inbox }, attrs: { class: 'w-12 h-12' } });
      return;
    }

    // 將工單依據上傳日期排序
    const sortedOrders = [...filtered].sort((a, b) => {
      const dateA = a.uploadDate || '';
      const dateB = b.uploadDate || '';
      return dateB.localeCompare(dateA);
    });

    const tableHtml = `
      <div class="bg-white rounded-[20px] border border-slate-200/60 shadow-soft overflow-hidden flex flex-col w-full">
        <div class="overflow-x-auto">
          <table class="w-full text-left border-collapse min-w-[800px]">
            <thead>
              <tr class="bg-slate-50/80 border-b border-slate-200/60 text-sm font-semibold text-slate-600">
                <th class="px-6 py-4 rounded-tl-[20px]">工單號碼</th>
                <th class="px-6 py-4">類別</th>
                <th class="px-6 py-4">當前狀態</th>
                <th class="px-6 py-4">上傳日期</th>
                <th class="px-6 py-4">狀態更新日</th>
                <th class="px-6 py-4">交貨日期</th>
                <!-- 動態自訂欄位表頭 -->
                ${currentSchema.map(field => `<th class="px-6 py-4">${field.label}</th>`).join('')}
                <th class="px-6 py-4 text-center rounded-tr-[20px]">操作</th>
              </tr>
            </thead>
            <tbody class="divide-y divide-slate-100 text-sm text-slate-700">
              ${sortedOrders.map(order => {
                const uploadDate = order.uploadDate ? order.uploadDate.split('T')[0] : '-';
                const statusUpdateDate = order.statusUpdateDate ? order.statusUpdateDate.split('T')[0] : '-';
                const dueDate = order.dueDate || '-';
                const cat = order.data && order.data.category ? order.data.category : '其他';
                
                // 依據狀態給予不同顏色的 Badge
                let badgeClass = 'bg-slate-100 text-slate-600';
                if (order.status === '排產中') badgeClass = 'bg-amber-100 text-amber-700';
                else if (order.status === '印刷') badgeClass = 'bg-blue-100 text-blue-700';
                else if (order.status === '裁切') badgeClass = 'bg-purple-100 text-purple-700';
                else if (order.status === '品檢') badgeClass = 'bg-orange-100 text-orange-700';
                else if (order.status === '出貨') badgeClass = 'bg-emerald-100 text-emerald-700';

                return `
                  <tr class="hover:bg-slate-50/50 transition-colors">
                    <td class="px-6 py-4 font-bold text-slate-900">${order.id}</td>
                    <td class="px-6 py-4"><span class="px-2 py-1 bg-slate-100 rounded-md text-xs">${cat}</span></td>
                    <td class="px-6 py-4">
                      <span class="px-2.5 py-1 rounded-md text-xs font-semibold ${badgeClass}">
                        ${order.status || '未知'}
                      </span>
                    </td>
                    <td class="px-6 py-4 text-slate-500">${uploadDate}</td>
                    <td class="px-6 py-4 text-slate-500">${statusUpdateDate}</td>
                    <td class="px-6 py-4 font-medium ${order.dueDate ? 'text-brand' : 'text-slate-400'}">${dueDate}</td>
                    <!-- 動態自訂欄位資料 -->
                    ${currentSchema.map(field => {
                      const val = order.data && order.data[field.key] !== undefined ? order.data[field.key] : '-';
                      return `<td class="px-6 py-4 text-slate-500">${val}</td>`;
                    }).join('')}
                    <td class="px-6 py-4 text-center">
                      <button class="btn-delete-order p-2 text-slate-400 hover:text-rose-500 hover:bg-rose-50 rounded-lg transition-colors" data-id="${order.id}" title="刪除工單">
                        <i data-lucide="trash-2" class="w-4 h-4"></i>
                      </button>
                    </td>
                  </tr>
                `;
              }).join('')}
            </tbody>
          </table>
        </div>
        <div class="bg-slate-50/50 px-6 py-3 border-t border-slate-100 text-xs text-slate-400 flex justify-between items-center">
          <span>共計 ${filtered.length} 筆工單</span>
        </div>
      </div>
    `;

    container.innerHTML = tableHtml;
    
    // 渲染表格內的垃圾桶圖示
    createIcons({ icons: { Trash2 }, attrs: { class: 'w-4 h-4' } });

    // 綁定刪除按鈕事件
    this.bindDeleteEvents();
  },

  bindDeleteEvents() {
    document.querySelectorAll('.btn-delete-order').forEach(btn => {
      btn.addEventListener('click', async (e) => {
        const orderId = e.currentTarget.dataset.id;
        
        const confirmed = await openConfirm({
          title: '刪除工單',
          message: `確定要徹底刪除工單「${orderId}」嗎？此操作無法復原，包含手動填寫的狀態與交期都會被刪除。`,
          isDanger: true,
          confirmText: '確認刪除'
        });

        if (!confirmed) return;

        try {
          await deleteDocument('work_orders', orderId);
          showToast(`工單 ${orderId} 已刪除`, 'success');
        } catch (error) {
          console.error('Delete order failed:', error);
          showToast('刪除失敗，請稍後再試', 'error');
        }
      });
    });
  },

  bindHeaderEvents() {
    const statuses = AppState.getStatuses() || ['排產中', '印刷', '裁切', '品檢', '出貨'];

    document.getElementById('btn-export-excel')?.addEventListener('click', () => {
      if (currentOrders.length === 0) {
        showToast('無資料可匯出', 'info');
        return;
      }
      try {
        exportToExcel(currentOrders, statuses, 'UPGRADE_工單總表', currentSchema);
        showToast('Excel 匯出成功', 'success');
      } catch (err) {
        console.error(err);
        showToast('Excel 匯出失敗', 'error');
      }
    });

    document.getElementById('btn-export-pdf')?.addEventListener('click', () => {
      if (currentOrders.length === 0) {
        showToast('無資料可匯出', 'info');
        return;
      }
      try {
        exportToPDF(currentOrders, statuses, 'UPGRADE_工單總表', currentSchema);
        showToast('PDF 匯出成功', 'success');
      } catch (err) {
        console.error(err);
        showToast('PDF 匯出失敗', 'error');
      }
    });
  },

  destroy() {
    if (unsubscribe) {
      unsubscribe();
      unsubscribe = null;
    }
    currentOrders = [];
  }
};

/* ═══ EOF ═══
F: AI-WA-UPWO/src/pages/table.js
V: 1792911800
W: UPGRADE-002
═══ */```

<!-- === FILE END: ./src/pages/table.js === -->

<!-- === FILE START: ./src/style.css === -->

## 📄 ./src/style.css

```css
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer components {
  /* 實體感卡片 */
  .glass-card {
    @apply bg-white rounded-[20px] border border-black/[0.04] shadow-soft transition-all duration-300;
  }
  
  .glass-card:hover {
    @apply -translate-y-0.5 shadow-lg;
  }

  /* 內凹區塊 */
  .inset-panel {
    @apply bg-[#F8FAFC] rounded-xl shadow-inset-soft border border-black/[0.02];
  }

  /* 主按鈕 */
  .btn-primary {
    @apply bg-brand text-white font-medium rounded-[10px] shadow-md shadow-brand/20 transition-all duration-150;
  }
  
  .btn-primary:hover {
    @apply bg-brand-hover;
  }
  
  .btn-primary:active {
    @apply translate-y-0.5 scale-[0.98] shadow-sm;
  }

  /* 仿 iOS Segmented Control */
  .segmented-control {
    @apply bg-slate-200 p-1 rounded-xl inline-flex relative;
  }
  
  .segment-btn {
    @apply px-4 py-1.5 rounded-lg text-sm font-medium text-slate-500 transition-all duration-300 z-10 cursor-pointer select-none;
  }
  
  .segment-btn.active {
    @apply text-slate-900 bg-white shadow-sm;
  }
}

/* 隱藏滾動條 (Kanban 區域) */
.kanban-scroll::-webkit-scrollbar {
  display: none;
}

.kanban-scroll {
  -ms-overflow-style: none;
  scrollbar-width: none;
}

/* 基礎全域設定 */
body {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

/* ═══ EOF ═══
F: src/style.css
V: 1792903520
W: webapp-20261026-001#32
═══ */```

<!-- === FILE END: ./src/style.css === -->

<!-- === FILE START: ./src/ui/layout.js === -->

## 📄 ./src/ui/layout.js

```js
import { createIcons, Layers, Bell, LogOut, LayoutDashboard, Table, PieChart, Users, Settings, Activity, ClipboardList } from 'lucide';

/**
 * 繪製 App Shell 骨架（側邊欄、頂部導覽列）
 * @param {Object} options
 * @param {Object} options.user 當前登入使用者 (需含 username, role)
 * @param {function} options.onLogout 登出回呼
 * @param {function} options.onNavigate 切換視圖回呼 (接收 route)
 */
export function renderAppShell({ user, onLogout, onNavigate }) {
  const root = document.getElementById('app-root');
  if (!root) return;

  // 清空並建立新的骨架
  root.innerHTML = `
    <div class="flex h-screen w-full bg-bg overflow-hidden">
      <!-- 側邊欄 (Desktop) -->
      <aside class="w-64 bg-white border-r border-slate-200/50 flex-col hidden md:flex z-20">
        <div class="h-16 flex items-center px-6 border-b border-slate-100">
          <div class="flex items-center gap-3">
            <div class="w-8 h-8 rounded-lg bg-brand flex items-center justify-center text-white shadow-sm shadow-brand/30">
              <i data-lucide="layers" class="w-5 h-5"></i>
            </div>
            <span class="font-bold text-lg tracking-tight text-slate-900">UPGRADE-WO</span>
          </div>
        </div>
        
        <nav class="flex-1 py-4 px-3 flex flex-col gap-1 overflow-y-auto" id="sidebar-nav">
          <!-- 導覽按鈕由 renderNavItems 注入 -->
        </nav>

        <div class="p-4 border-t border-slate-100">
          <button id="btn-logout" class="flex items-center gap-3 w-full px-3 py-2.5 rounded-xl text-sm font-medium text-slate-600 hover:bg-rose-50 hover:text-rose-600 transition-colors">
            <i data-lucide="log-out" class="w-5 h-5"></i>
            登出系統
          </button>
        </div>
      </aside>

      <!-- 主內容區 -->
      <div class="flex-1 flex flex-col h-full overflow-hidden relative">
        <!-- 頂部導覽 (Mobile & Desktop) -->
        <header class="h-16 bg-white/80 backdrop-blur-md border-b border-slate-200/50 flex items-center justify-between px-4 sm:px-6 z-10">
          <!-- Mobile Logo & Menu Button -->
          <div class="flex items-center gap-3 md:hidden">
            <button id="btn-mobile-menu" class="p-2 -ml-2 text-slate-600 hover:bg-slate-100 rounded-lg transition-colors">
              <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-menu"><line x1="4" x2="20" y1="12" y2="12"/><line x1="4" x2="20" y1="6" y2="6"/><line x1="4" x2="20" y1="18" y2="18"/></svg>
            </button>
            <div class="w-8 h-8 rounded-lg bg-brand flex items-center justify-center text-white shadow-sm">
              <i data-lucide="layers" class="w-5 h-5"></i>
            </div>
            <span class="font-bold text-slate-900">UPGRADE</span>
          </div>

          <!-- 中間：iOS Segmented Control (視圖切換) - Desktop -->
          <div class="hidden md:flex segmented-control" id="top-segmented-control">
            <!-- 由 renderNavItems 動態注入 -->
          </div>

          <!-- 右側：使用者資訊 -->
          <div class="flex items-center gap-3">
            <div class="text-right hidden sm:block">
              <div class="text-sm font-bold text-slate-700">${user.username}</div>
              <div class="text-[10px] text-slate-400 uppercase tracking-wider">${user.role}</div>
            </div>
            <div class="w-9 h-9 rounded-full bg-gradient-to-tr from-indigo-100 to-purple-100 border border-indigo-200 flex items-center justify-center text-indigo-700 font-bold text-sm shadow-sm cursor-pointer">
              ${user.username.charAt(0).toUpperCase()}
            </div>
          </div>
        </header>

        <!-- 頁面渲染容器 -->
        <main id="app-content" class="flex-1 overflow-y-auto p-4 sm:p-6 lg:p-8 flex flex-col gap-6">
          <!-- 各頁面內容會注入這裡 -->
        </main>
      </div>
    </div>

    <!-- 手機版側邊選單 (Drawer) -->
    <div id="mobile-drawer" class="fixed inset-0 z-50 hidden">
      <div class="absolute inset-0 bg-black/40 backdrop-blur-sm transition-opacity" id="drawer-overlay"></div>
      <aside class="absolute left-0 top-0 bottom-0 w-64 bg-white shadow-2xl transform -translate-x-full transition-transform duration-300 flex flex-col" id="drawer-panel">
        <div class="h-16 flex items-center px-6 border-b border-slate-100 justify-between">
          <div class="flex items-center gap-3">
            <div class="w-8 h-8 rounded-lg bg-brand flex items-center justify-center text-white shadow-sm">
              <i data-lucide="layers" class="w-5 h-5"></i>
            </div>
            <span class="font-bold text-lg text-slate-900">選單</span>
          </div>
          <button id="btn-close-drawer" class="p-2 text-slate-400 hover:bg-slate-100 rounded-lg">
            <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-x"><path d="M18 6 6 18"/><path d="m6 6 12 12"/></svg>
          </button>
        </div>
        <nav class="flex-1 py-4 px-3 flex flex-col gap-1 overflow-y-auto" id="mobile-sidebar-nav">
          <!-- 由 renderNavItems 注入 -->
        </nav>
      </aside>
    </div>
  `;

  // 渲染圖示
  createIcons({
    icons: { Layers, Bell, LogOut, LayoutDashboard, Table, PieChart, Users, Settings },
    attrs: { class: 'w-5 h-5' }
  });

  // 綁定事件
  document.getElementById('btn-logout')?.addEventListener('click', onLogout);

  // 手機版抽屜選單事件
  const drawer = document.getElementById('mobile-drawer');
  const panel = document.getElementById('drawer-panel');
  const overlay = document.getElementById('drawer-overlay');
  
  const openDrawer = () => {
    drawer.classList.remove('hidden');
    setTimeout(() => {
      overlay.classList.add('opacity-100');
      panel.classList.remove('-translate-x-full');
    }, 10);
  };
  
  const closeDrawer = () => {
    overlay.classList.remove('opacity-100');
    panel.classList.add('-translate-x-full');
    setTimeout(() => drawer.classList.add('hidden'), 300);
  };

  document.getElementById('btn-mobile-menu')?.addEventListener('click', openDrawer);
  document.getElementById('btn-close-drawer')?.addEventListener('click', closeDrawer);
  overlay?.addEventListener('click', closeDrawer);

  // 渲染導覽選單
  renderNavItems(user.role, onNavigate, closeDrawer);
}

/**
 * 繪製導覽選單項目
 */
function renderNavItems(role, onNavigate, closeDrawer) {
  const routes = [
    { id: 'kanban', label: '流水線', icon: 'layout-dashboard', roles: ['admin', 'user'] },
    { id: 'table', label: '管理總表', icon: 'table', roles: ['admin', 'user'] },
    { id: 'overview', label: '總狀態', icon: 'pie-chart', roles: ['admin', 'user'] },
    { id: 'category', label: '類別視圖', icon: 'layers', roles: ['admin', 'user'] },
    { id: 'salesperson', label: '銷售員', icon: 'users', roles: ['admin', 'user'] },
    { id: 'settings', label: '系統設定', icon: 'settings', roles: ['admin'] }
  ];

  const sidebarNav = document.getElementById('sidebar-nav');
  const topControl = document.getElementById('top-segmented-control');
  const mobileNav = document.getElementById('mobile-sidebar-nav');

  routes.forEach(route => {
    if (!route.roles.includes(role)) return;

    // 側邊欄項目
    if (sidebarNav) {
      const btn = document.createElement('button');
      btn.className = 'nav-item flex items-center gap-3 w-full px-3 py-2.5 rounded-xl text-sm font-medium text-slate-600 hover:bg-slate-50 hover:text-brand transition-colors';
      btn.dataset.route = route.id;
      btn.innerHTML = `
        <i data-lucide="${route.icon}" class="w-5 h-5"></i>
        ${route.label}
      `;
      btn.addEventListener('click', () => onNavigate(route.id));
      sidebarNav.appendChild(btn);
    }

    // 手機版側邊欄項目
    if (mobileNav) {
      const btn = document.createElement('button');
      btn.className = 'nav-item flex items-center gap-3 w-full px-3 py-2.5 rounded-xl text-sm font-medium text-slate-600 hover:bg-slate-50 hover:text-brand transition-colors';
      btn.dataset.route = route.id;
      btn.innerHTML = `
        <i data-lucide="${route.icon}" class="w-5 h-5"></i>
        ${route.label}
      `;
      btn.addEventListener('click', () => {
        onNavigate(route.id);
        if (typeof closeDrawer === 'function') closeDrawer();
      });
      mobileNav.appendChild(btn);
    }

    // 頂部 Segmented Control 項目
    if (topControl) {
      const segment = document.createElement('div');
      segment.className = 'segment-btn';
      segment.dataset.route = route.id;
      segment.textContent = route.label;
      segment.addEventListener('click', () => onNavigate(route.id));
      topControl.appendChild(segment);
    }
  });

  createIcons({
    icons: { LayoutDashboard, Table, PieChart, Users, Settings, Layers },
    attrs: { class: 'w-5 h-5' }
  });
}

/**
 * 更新當前選中的導覽狀態
 * @param {string} activeRoute 
 */
export function updateNavbar(activeRoute) {
  // 更新側邊欄
  document.querySelectorAll('.nav-item').forEach(item => {
    if (item.dataset.route === activeRoute) {
      item.classList.add('bg-brand/5', 'text-brand', 'font-bold');
      item.classList.remove('text-slate-600');
    } else {
      item.classList.remove('bg-brand/5', 'text-brand', 'font-bold');
      item.classList.add('text-slate-600');
    }
  });

  // 更新頂部 Segmented Control
  document.querySelectorAll('#top-segmented-control .segment-btn').forEach(item => {
    if (item.dataset.route === activeRoute) {
      item.classList.add('active');
    } else {
      item.classList.remove('active');
    }
  });
}

/**
 * 統一的頁面佈局渲染
 * @param {Object} options
 * @param {string} options.title 頁面標題
 * @param {string} options.subtitle 副標題
 * @param {string} options.headerActions 右上角操作按鈕 HTML
 * @param {Array} options.stats 統計卡片資料
 * @param {string} options.controls 控制列 HTML
 * @param {string} options.listContainerId 內容容器 ID
 */
export function renderPageLayout({ title, subtitle = '', headerActions = '', stats = [], controls = '', listContainerId = 'page-content' }) {
  const container = document.getElementById('app-content');
  if (!container) return;

  let statsHtml = '';
  if (stats && stats.length > 0) {
    statsHtml = `
      <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-4 gap-4 mb-2">
        ${stats.map(s => `
          <div class="glass-card p-5 flex flex-col gap-2">
            <span class="text-sm text-slate-500 font-medium flex items-center gap-2">
              <i data-lucide="${s.icon || 'activity'}" class="w-4 h-4 text-brand"></i> ${s.label}
            </span>
            <span class="text-3xl font-bold text-slate-900">${s.value}</span>
          </div>
        `).join('')}
      </div>
    `;
  }

  container.innerHTML = `
    <!-- 標題與操作區 -->
    <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4 mb-2">
      <div>
        <h1 class="text-2xl font-bold text-slate-900">${title}</h1>
        ${subtitle ? `<p class="text-sm text-slate-500 mt-1">${subtitle}</p>` : ''}
      </div>
      <div class="flex items-center gap-3">
        ${headerActions}
      </div>
    </div>

    <!-- 統計卡片 -->
    ${statsHtml}

    <!-- 控制列 -->
    ${controls ? `<div class="w-full">${controls}</div>` : ''}

    <!-- 主內容容器 -->
    <div id="${listContainerId}" class="flex-1 flex flex-col relative w-full">
      <!-- 內容由各頁面自行渲染 -->
    </div>
  `;

  // 重新渲染圖示（補上缺少的參數）
  createIcons({
    icons: { Activity, ClipboardList },
    attrs: { class: 'w-4 h-4' }
  });
}

/* ═══ EOF ═══
F: AI-WA-UPWO/src/ui/layout.js
V: 1792912100
W: UPGRADE-002
═══ */```

<!-- === FILE END: ./src/ui/layout.js === -->

<!-- === FILE START: ./src/ui/modal.js === -->

## 📄 ./src/ui/modal.js

```js
import { createIcons, X } from 'lucide';

/**
 * 建立並顯示一個 Modal
 * @param {Object} options 設定選項
 * @param {string} options.title 標題
 * @param {string} options.content 內容 HTML
 * @param {function} options.onClose 關閉時的回呼
 * @returns {Object} 包含 close 方法的物件，可手動關閉
 */
export function openModal({ title, content, onClose }) {
  // 建立遮罩與容器
  const overlay = document.createElement('div');
  overlay.className = 'fixed inset-0 bg-black/40 backdrop-blur-sm z-50 flex items-center justify-center transition-opacity duration-300 opacity-0';
  
  const modal = document.createElement('div');
  // 使用 tailwind 的 glass-card 風格 (純白背景、圓角20px、細微邊框、柔和陰影)
  modal.className = 'bg-white border border-slate-100 shadow-soft rounded-[20px] w-full max-w-md mx-4 overflow-hidden transform transition-transform duration-300 scale-95';
  
  modal.innerHTML = `
    <div class="flex justify-between items-center p-5 border-b border-slate-100">
      <h3 class="text-lg font-bold text-slate-900">${title}</h3>
      <button id="modal-close-btn" class="text-slate-400 hover:text-slate-600 transition-colors bg-slate-50 hover:bg-slate-100 p-1.5 rounded-full">
        <i data-lucide="x" class="w-5 h-5"></i>
      </button>
    </div>
    <div class="p-5 text-slate-600">
      ${content}
    </div>
  `;

  overlay.appendChild(modal);
  document.body.appendChild(overlay);

  createIcons({
    icons: { X },
    attrs: { class: 'w-5 h-5' }
  });

  // 進場動畫
  requestAnimationFrame(() => {
    overlay.classList.remove('opacity-0');
    modal.classList.remove('scale-95');
    modal.classList.add('scale-100');
  });

  // 關閉邏輯
  const close = () => {
    overlay.classList.add('opacity-0');
    modal.classList.remove('scale-100');
    modal.classList.add('scale-95');
    
    setTimeout(() => {
      overlay.remove();
      if (typeof onClose === 'function') onClose();
    }, 300); // 等待動畫結束
  };

  // 綁定事件
  modal.querySelector('#modal-close-btn').addEventListener('click', close);
  overlay.addEventListener('click', (e) => {
    if (e.target === overlay) close(); // 點擊遮罩關閉
  });

  return { close, element: modal };
}

/**
 * 顯示確認對話框 (Promise 包裝)
 * @param {Object} options 設定選項
 * @param {string} options.title 標題
 * @param {string} options.message 訊息內容
 * @param {string} options.confirmText 確認按鈕文字 (預設 '確認')
 * @param {string} options.cancelText 取消按鈕文字 (預設 '取消')
 * @param {boolean} options.isDanger 是否為危險操作 (紅色按鈕)
 * @returns {Promise<boolean>} 使用者點擊確認回傳 true，取消回傳 false
 */
export function openConfirm({ title, message, confirmText = '確認', cancelText = '取消', isDanger = false }) {
  return new Promise((resolve) => {
    const btnClass = isDanger 
      ? 'bg-rose-500 hover:bg-rose-600 text-white shadow-rose-500/20' 
      : 'bg-brand hover:bg-brand-hover text-white shadow-brand/20';

    const content = `
      <p class="mb-6 text-slate-600">${message}</p>
      <div class="flex justify-end gap-3">
        <button id="modal-cancel-btn" class="px-4 py-2 rounded-xl text-sm font-medium text-slate-600 bg-slate-100 hover:bg-slate-200 transition-colors">
          ${cancelText}
        </button>
        <button id="modal-confirm-btn" class="px-4 py-2 rounded-xl text-sm font-medium transition-all active:scale-95 shadow-md ${btnClass}">
          ${confirmText}
        </button>
      </div>
    `;

    const { close, element } = openModal({
      title,
      content,
      onClose: () => resolve(false) // 若透過右上角 X 或遮罩關閉，視為取消
    });

    // 覆寫按鈕事件
    element.querySelector('#modal-cancel-btn').addEventListener('click', () => {
      close();
    });

    element.querySelector('#modal-confirm-btn').addEventListener('click', () => {
      // 避免觸發 onClose 的 resolve(false)
      element.querySelector('#modal-close-btn').removeEventListener('click', close);
      element.parentElement.removeEventListener('click', close); 
      
      // 直接觸發動畫並移除
      element.parentElement.classList.add('opacity-0');
      element.classList.remove('scale-100');
      element.classList.add('scale-95');
      setTimeout(() => {
        element.parentElement.remove();
        resolve(true);
      }, 300);
    });
  });
}

/* ═══ EOF ═══
F: src/ui/modal.js
V: 1792903509
W: webapp-20261026-001#21
═══ */```

<!-- === FILE END: ./src/ui/modal.js === -->

<!-- === FILE START: ./src/ui/toast.js === -->

## 📄 ./src/ui/toast.js

```js
import { createIcons, CheckCircle2, Info, AlertCircle, X } from 'lucide';

/**
 * 顯示全域 Toast 提示
 * @param {string} message 提示訊息
 * @param {string} type 類型：'success' | 'info' | 'error' (預設 'success')
 * @param {number} duration 顯示時間（毫秒），預設 3000
 */
export function showToast(message, type = 'success', duration = 3000) {
  // 確保容器存在
  let container = document.getElementById('toast-container');
  if (!container) {
    container = document.createElement('div');
    container.id = 'toast-container';
    // 置中頂部，z-index 設高，pointer-events-none 讓不點擊到時不擋路
    container.className = 'fixed top-6 left-1/2 -translate-x-1/2 z-[100] flex flex-col gap-2 pointer-events-none items-center';
    document.body.appendChild(container);
  }

  // 建立 Toast 元素
  const toast = document.createElement('div');
  toast.className = 'bg-white/90 backdrop-blur-md border border-slate-200 shadow-soft rounded-xl px-4 py-3 flex items-center gap-3 text-sm font-medium text-slate-700 min-w-[250px] pointer-events-auto transition-all duration-300 transform -translate-y-4 opacity-0';
  
  // 設定圖示與顏色
  let iconHtml = '';
  let iconColor = 'text-emerald-500';

  if (type === 'success') {
    iconHtml = '<i data-lucide="check-circle-2" class="w-5 h-5"></i>';
  } else if (type === 'info') {
    iconHtml = '<i data-lucide="info" class="w-5 h-5"></i>';
    iconColor = 'text-blue-500';
  } else if (type === 'error') {
    iconHtml = '<i data-lucide="alert-circle" class="w-5 h-5"></i>';
    iconColor = 'text-rose-500';
  }

  toast.innerHTML = `
    <div class="${iconColor}">${iconHtml}</div>
    <span class="flex-1">${message}</span>
  `;

  container.appendChild(toast);

  // 渲染 Lucide 圖示
  // 注意：如果在 Vue/React 中，這裡會是響應式的，但我們是純 JS
  createIcons({
    icons: { CheckCircle2, Info, AlertCircle, X },
    attrs: { class: 'w-5 h-5' }
  });

  // 觸發動畫 (從上滑入)
  requestAnimationFrame(() => {
    toast.classList.remove('-translate-y-4', 'opacity-0');
    toast.classList.add('translate-y-0', 'opacity-100');
  });

  // 自動銷毀
  setTimeout(() => {
    // 觸發滑出動畫
    toast.classList.remove('translate-y-0', 'opacity-100');
    toast.classList.add('-translate-y-4', 'opacity-0');
    
    // 動畫結束後移除 DOM
    toast.addEventListener('transitionend', () => {
      toast.remove();
      // 如果容器空了，也一併移除（可選，保持 DOM 乾淨）
      if (container.children.length === 0) {
        container.remove();
      }
    });
  }, duration);
}

/* ═══ EOF ═══
F: src/ui/toast.js
V: 1792903508
W: webapp-20261026-001#20
═══ */```

<!-- === FILE END: ./src/ui/toast.js === -->

<!-- === FILE START: ./tailwind.config.js === -->

## 📄 ./tailwind.config.js

```js
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./src/**/*.{js,html}"
  ],
  theme: {
    extend: {
      colors: {
        brand: {
          DEFAULT: '#6366F1',
          hover: '#4F46E5',
        },
        bg: '#F4F7FB',
        inset: '#F8FAFC',
      },
      boxShadow: {
        'soft': '0 4px 20px -2px rgba(0,0,0,0.05)',
        'inset-soft': 'inset 0 2px 4px 0 rgba(0,0,0,0.02)',
      },
      borderRadius: {
        '2xl': '20px',
      }
    }
  },
  plugins: [],
}

/* ═══ EOF ═══
F: tailwind.config.js
V: 1792903500
W: webapp-20261026-001#11
═══ */```

<!-- === FILE END: ./tailwind.config.js === -->

<!-- === FILE START: ./vite.config.js === -->

## 📄 ./vite.config.js

```js
import { fileURLToPath, URL } from 'node:url';
import { defineConfig } from 'vite';

// https://vitejs.dev/config/
export default defineConfig({
  resolve: {
    alias: {
      // 設定 @ 指向 src 目錄，避免深層相對路徑 (../../)
      '@': fileURLToPath(new URL('./src', import.meta.url))
    }
  },
  server: {
    port: 3000,
    open: true
  }
});

/* ═══ EOF ═══
F: vite.config.js
V: 1792902801
W: webapp-20261026-001#4
═══ */```

<!-- === FILE END: ./vite.config.js === -->
