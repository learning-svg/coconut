

移至內容
透過螢幕閱讀器使用 Gmail
第 1 個，共 2,143 個
(無主旨)
收件匣

黃語萱 <climax1995830@gmail.com>
附件
下午12:10 (0 分鐘前)
寄給 我




從我的iPhone傳送
 1 個附件
  •  已通過 Gmail 掃描檢查
<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>椰菲英文 - 專屬學習儀表板</title>
    <script charset="utf-8" src="https://static.line-scdn.net/liff/edge/2/sdk.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>

    <meta name="color-scheme" content="light only">
    <meta name="supported-color-schemes" content="light">
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Baloo+2:wght@500;600;700&family=Noto+Sans+TC:wght@400;500;700&display=swap" rel="stylesheet">
    <style>
      :root{
        color-scheme: light only;
        --coffee:#6F4E37; --coffee-deep:#4A3221; --coffee-light:#A67B5B;
        --cream:#FDFBF6; --paper:#FAF8F5;
        --blush:#D97B54; --accent:#C08A3E; --leaf:#8A9A5B;
        --line:#DCCFC0; --text-soft:#8A7A68; --star:#F5C542;
        --primary-color:#D97B54; --border-color:#DCCFC0;
      }
      @media (prefers-color-scheme: dark){ :root{ color-scheme: light only; } }
      html{ background:#FDFBF6 !important; -webkit-text-size-adjust:100%; forced-color-adjust:none; }
      *{ box-sizing:border-box; margin:0; padding:0; forced-color-adjust:none; -webkit-print-color-adjust:exact; print-color-adjust:exact; }
      body{ font-family:'Noto Sans TC',sans-serif; background:#FDFBF6 !important; color:var(--coffee-deep); padding:0 0 40px; -webkit-font-smoothing:antialiased; min-height:100vh; }

      /* 載入畫面 */
      #loading-screen{ position:fixed; inset:0; background:#FDFBF6; display:flex; flex-direction:column; justify-content:center; align-items:center; z-index:9999; }
      .spinner{ width:40px; height:40px; border:3px solid var(--line); border-top-color:var(--coffee); border-radius:50%; animation:spin 1s linear infinite; margin-bottom:15px; }
      #loading-text{ color:var(--text-soft); font-size:14px; }
      @keyframes spin{ to{ transform:rotate(360deg); } }

      /* 分頁 (檔案夾效果) */
      .header{ background:#FDFBF6; padding-top:14px; position:sticky; top:0; z-index:100; }
      .tabs-container{ display:flex; gap:6px; overflow-x:auto; padding:0 18px; scrollbar-width:none; border-bottom:1.5px solid var(--coffee-light); }
      .tabs-container::-webkit-scrollbar{ display:none; }
      .tab{ font-family:'Baloo 2'; font-weight:600; font-size:15px; padding:11px 22px 12px; border-radius:14px 14px 0 0; white-space:nowrap; color:var(--text-soft); cursor:pointer; border:1.5px solid transparent; border-bottom:none; }
      .tab.active{ color:var(--coffee-deep); border-color:var(--coffee-light); background:#FDFBF6; margin-bottom:-1.5px; }

      .content-container{ padding:20px 18px; }

      /* 學生標題 + 堂數膠囊 */
      .student-summary{ text-align:center; margin-bottom:20px; }
      .student-summary h2{ font-family:'Baloo 2'; font-weight:700; font-size:24px; color:var(--coffee-deep); margin-bottom:10px; }
      .badge-pill{ display:inline-flex; align-items:center; gap:8px; background:#FDFBF6; border:2px solid var(--coffee); border-radius:22px; padding:6px 18px; font-size:14px; font-weight:700; color:var(--coffee-deep); }

      /* 卡片 */
      .card{ background:#FDFBF6; border-radius:16px; padding:20px; margin-bottom:16px; border:2px solid var(--coffee); }
      .card-title{ font-family:'Baloo 2'; font-weight:700; font-size:17px; color:var(--coffee-deep); margin-bottom:14px; }

      /* 箭頭切換 */
      .prog-arrow{ width:30px; height:30px; border-radius:50%; border:2px solid var(--coffee); color:var(--coffee); display:flex; align-items:center; justify-content:center; font-size:16px; font-weight:700; cursor:pointer; background:#FDFBF6; user-select:none; }
      .prog-arrow.off{ border-color:var(--line); color:var(--line); }

      .material-text{ font-size:15px; font-weight:700; color:var(--coffee-deep); margin-bottom:10px; line-height:1.5; }
      #class-date-row{ font-weight:500 !important; color:var(--text-soft) !important; font-size:13px !important; }

      /* 回饋泡泡 */
      .feedback-bubble{ background:var(--paper); border-radius:6px 16px 16px 16px; padding:14px 16px; border:1.5px solid var(--line); margin-top:6px; }

      /* 作業 */
      .homework-box{ display:flex; align-items:center; gap:10px; margin-top:16px; padding-top:16px; border-top:1.5px dashed var(--line); }
      .hw-badge{ background:var(--blush); color:#fff; font-size:12px; font-weight:700; padding:6px 12px; border-radius:10px; white-space:nowrap; }
      .hw-content{ font-size:14px; line-height:1.5; word-break:break-word; }
      .hw-content a{ color:var(--blush); font-weight:700; text-decoration:underline; }

      /* 課表列 */
      .class-item{ display:flex; justify-content:space-between; align-items:center; padding:14px 0; border-bottom:1.5px solid var(--line); }
      .class-item:last-child{ border-bottom:none; }
      .class-info{ display:flex; flex-direction:column; gap:3px; }
      .class-date{ font-family:'Baloo 2'; font-weight:600; font-size:16px; color:var(--coffee-deep); }
      .class-type{ font-size:12px; color:var(--text-soft); }

      /* 按鈕 */
      .btn-cancel{ background:#FDFBF6; border:2px solid var(--coffee); color:var(--coffee); padding:8px 16px; border-radius:12px; font-weight:700; font-size:13px; cursor:pointer; white-space:nowrap; font-family:'Noto Sans TC'; }
      .btn-cancel:active{ background:var(--paper); }
      .btn-cancel:disabled{ background:var(--paper); border-color:var(--line); color:var(--text-soft); cursor:not-allowed; }

      /* 星星鼓勵條 */
      .cheer{ display:flex; align-items:center; gap:12px; background:#FDFBF6; border:2px solid var(--coffee); border-radius:16px; padding:14px 18px; margin-bottom:16px; }
      .cheer .star{ font-size:22px; color:var(--star); flex-shrink:0; line-height:1; }
      .cheer .cheer-text{ flex:1; text-align:center; }
      .cheer .en{ font-family:'Baloo 2'; font-weight:700; font-size:15px; color:var(--coffee); }
      .cheer .zh{ font-size:12px; color:var(--text-soft); margin-top:3px; }

      /* 品牌條 */
      .brand-bar{ padding:16px 18px 4px; }
      .brand-name{ font-family:'Baloo 2'; font-weight:700; font-size:18px; color:var(--coffee-deep); }
      .brand-sub{ font-size:11px; color:var(--text-soft); margin-top:-2px; }

      /* 特殊頁面 (email/價格/預約/退費) 共用 */
      .sv-wrap{ padding:24px 18px; }
      .sv-center{ text-align:center; padding:50px 24px; color:var(--text-soft); }
      .sv-emoji{ font-size:44px; margin-bottom:14px; }
      .sv-title{ font-family:'Baloo 2'; font-size:18px; font-weight:700; color:var(--coffee-deep); margin-bottom:8px; }
      .sv-sub{ font-size:14px; color:var(--text-soft); line-height:1.5; }
      .sv-input{ width:100%; border:2px solid var(--coffee); border-radius:12px; padding:12px; font-size:15px; margin-bottom:14px; background:#FDFBF6; color:var(--coffee-deep); font-family:'Noto Sans TC'; }
      .sv-btn{ width:100%; background:var(--blush); color:#fff; border:none; border-radius:12px; padding:13px; font-size:16px; font-weight:700; cursor:pointer; font-family:'Baloo 2'; }
      .price-card{ background:#FDFBF6; border:2px solid var(--coffee); border-radius:16px; padding:18px; margin-bottom:20px; }
      .price-row{ display:flex; justify-content:space-between; align-items:center; padding:11px 0; border-bottom:1.5px solid var(--line); }
      .price-row:last-child{ border-bottom:none; }
      .price-name{ font-size:15px; font-weight:600; color:var(--coffee-deep); }
      .price-val{ font-size:15px; color:var(--blush); font-weight:700; }
      .sv-section-title{ text-align:center; font-family:'Baloo 2'; font-size:20px; font-weight:700; color:var(--coffee-deep); margin-bottom:16px; }
    </style>
</head>
<body>

    <div id="loading-screen"><div class="spinner"></div><div id="loading-text">正在載入專屬學習儀表板...</div></div>

    <!-- 特殊畫面容器 (email註冊 / 價格優惠 / 預約體驗 / 退費擋) -->
    <div id="special-view" style="display:none;"></div>

    <div class="brand-bar" id="brand-bar" style="display:none;">
        <div class="brand-name">Coconut English</div>
        <div class="brand-sub">椰菲英文 · 專屬學習區</div>
    </div>

    <div class="header" id="header" style="display: none;"><div class="tabs-container" id="tabs-container"></div></div>

    <div class="content-container" id="main-content" style="display:none;">
        <div class="student-summary"><h2 id="display-name">學生姓名</h2><div class="badge-pill" id="display-remaining">剩餘堂數：- 堂</div></div>

        <div class="cheer">
            <span class="star">&#10022;</span>
            <div class="cheer-text">
                <div class="en">Every day is a new beginning!</div>
                <div class="zh">每一天都是新的開始，繼續加油</div>
            </div>
            <span class="star">&#10022;</span>
        </div>

        <div class="card">
            <h3 class="card-title" style="display:flex; justify-content:space-between; align-items:center;">
                <span>上次上課進度</span>
                <span id="progress-nav" style="display:none; align-items:center; gap:8px; font-size:14px;">
                    <span id="progress-prev" class="prog-arrow off" onclick="changeProgress(-1)">&#8249;</span>
                    <span id="progress-page" style="font-size:12px; color:var(--text-soft); font-weight:700;">1 / 2</span>
                    <span id="progress-next" class="prog-arrow" onclick="changeProgress(1)">&#8250;</span>
                </span>
            </h3>
            <div class="material-text" id="class-date-row">上課日期　<span id="display-class-date">-</span></div>
            <div class="material-text">目前教材　<span id="display-material">載入中...</span></div>
            <div class="feedback-bubble">
                <div style="font-size:11px; color:var(--leaf); font-weight:700; letter-spacing:.5px; margin-bottom:6px;">TEACHER'S FEEDBACK</div>
                <div style="font-style:italic; color:var(--coffee-deep); font-size:14px; line-height:1.6; word-break:break-word;"><span id="display-feedback-en">Loading...</span></div>
                <div id="feedback-zh-row" style="margin-top:8px; padding-top:8px; border-top:1px solid var(--line); font-size:14px; color:var(--coffee-deep); word-break:break-word;"><span id="display-feedback-zh"></span></div>
            </div>
            <div class="homework-box" id="homework-container"><div class="hw-badge">課後作業</div><div class="hw-content" id="display-homework"></div></div>
        </div>

        <div class="card">
            <h3 class="card-title">繳交作業 / 留言給老師</h3>
            <textarea id="hw-message" placeholder="老師好，這是我的功課 (Teacher, this is my homework...)" style="width:100%; height:70px; margin-bottom:14px; border-radius:12px; border:2px solid var(--border-color); padding:10px; font-size:14px; resize:none; background:#FDFBF6; color:var(--coffee-deep); font-family:'Noto Sans TC';"></textarea>
            <input type="file" id="hw-file" style="margin-bottom:14px; width:100%; font-size:14px;" accept="image/*,.pdf,.mp3">
            <button class="btn-cancel" style="width:100%; background:var(--blush); color:#fff; border-color:var(--blush);" onclick="submitHomeworkToTeacher()">安全送出給老師</button>
        </div>

        <div class="card"><h3 class="card-title">未來一週課表</h3><div id="upcoming-list"></div></div>
    </div>

    <!-- 老師專用介面 (全英文) -->
    <div class="brand-bar" id="teacher-brand-bar" style="display:none;">
        <div class="brand-name">Coconut English</div>
        <div class="brand-sub">Teacher Portal</div>
    </div>
    <div class="header" id="teacher-header" style="display: none;">
        <div class="tabs-container" id="teacher-tabs">
            <div class="tab active" data-ttab="0" onclick="switchTeacherTab(0)">Today</div>
            <div class="tab" data-ttab="1" onclick="switchTeacherTab(1)">Feedback &amp; HW</div>
            <div class="tab" data-ttab="2" onclick="switchTeacherTab(2)">Tomorrow</div>
        </div>
    </div>
    <div class="content-container" id="teacher-content" style="display:none;">
        <div class="student-summary">
            <h2 id="t-name">Teacher</h2>
            <div class="badge-pill" id="t-summary">—</div>
        </div>
        <div id="t-tab-today"></div>
        <div id="t-tab-feedback" style="display:none;"></div>
        <div id="t-tab-tomorrow" style="display:none;"></div>
    </div>
    <script>
        // ==========================================
        //   ⚙️ 填寫您的 LIFF 與 GAS 連動資訊
        // ==========================================
        const LIFF_ID = "2008845693-L2SUJz8X";
        const GAS_WEB_APP_URL = "https://script.google.com/macros/s/AKfycbztBUpKu11R_cPzDsRMLgzkbkdheqjNO7PqMon94X67Zx5ZTXRHq13lk0xg2NSVHSI-/exec";

        let studentDataList = [];
        let currentUserId = "";
        let currentTabIndex = 0;
        let progressRecords = [];      // 👑 最近 N 筆進度紀錄
        let currentProgressIndex = 0;  // 👑 目前顯示第幾筆 (0=最近)
        let teacherData = null;

        async function initializeApp() {
            try {
                await liff.init({ liffId: LIFF_ID });
                if (!liff.isLoggedIn()) { liff.login(); return; }
                currentUserId = (await liff.getProfile()).userId;
                const res = await fetch(`${GAS_WEB_APP_URL}?action=getInit&userId=${currentUserId}`);
                const result = await res.json();
                routeByResult(result);
            } catch (err) { document.getElementById('loading-text').innerText = "LIFF 初始化失敗，請在 LINE 內開啟。"; }
        }

        // 👑 依後端回傳的 role/status 分流到對應畫面
        function routeByResult(result) {
            const role = result.role;
            if (role === 'teacher') { renderTeacherResult(result); return; }
            if (role === 'refunded') { showRefundedBlock(); return; }
            if (role === 'needEmail') { showEmailRegister(result.nextView); return; }
            if (role === 'pricing') { showPricingPage(); return; }
            if (role === 'booking') { showBookingPage(); return; }
            // role === 'student'
            renderStudentResult(result);
        }

        // 👑 退費 → 擋畫面
        function showRefundedBlock() {
            document.getElementById('loading-screen').style.display = 'none';
            const box = document.getElementById('special-view');
            box.style.display = 'block';
            box.innerHTML = `<div style="text-align:center; padding:60px 24px; color:var(--text-soft);">
                <div style="font-size:48px; margin-bottom:16px;">🔍</div>
                <div style="font-size:17px; font-weight:600; color:#333; margin-bottom:8px;">查無資料</div>
                <div style="font-size:14px;">如有疑問請聯繫客服</div>
            </div>`;
        }

        // 👑 email 註冊頁 (nextView = 填完後要去的頁面)
        let pendingNextView = null;
        function showEmailRegister(nextView) {
            pendingNextView = nextView;
            document.getElementById('loading-screen').style.display = 'none';
            const box = document.getElementById('special-view');
            box.style.display = 'block';
            box.innerHTML = `<div class="sv-wrap">
                <div style="text-align:center; margin-bottom:24px;">
                    <div style="width:64px; height:64px; margin:0 auto 14px; border-radius:50%; border:2.5px solid var(--coffee); display:flex; align-items:center; justify-content:center;">
                        <span style="font-family:'Baloo 2'; font-size:30px; font-weight:700; color:var(--coffee);">@</span>
                    </div>
                    <div class="sv-title">請先登記聯絡 Email</div>
                    <div class="sv-sub">為確保重要通知不漏接，請留下您的 Email</div>
                </div>
                <input id="email-input" type="email" placeholder="example@gmail.com" class="sv-input">
                <div id="email-error" style="color:var(--blush); font-size:13px; margin-bottom:10px; display:none;"></div>
                <button id="email-submit" onclick="submitEmail()" class="sv-btn">送出</button>
            </div>`;
        }

        async function submitEmail() {
            const email = document.getElementById('email-input').value.trim();
            const errEl = document.getElementById('email-error');
            if (!email || email.indexOf('@') === -1 || email.indexOf('.') === -1) {
                errEl.style.display = 'block'; errEl.innerText = '請輸入正確的 Email 格式'; return;
            }
            const btn = document.getElementById('email-submit');
            btn.disabled = true; btn.innerText = '送出中...';
            try {
                const res = await fetch(`${GAS_WEB_APP_URL}?action=saveEmail&userId=${currentUserId}&email=${encodeURIComponent(email)}`);
                const result = await res.json();
                if (result.status === 'success') {
                    // 👑 顯示成功過場畫面
                    showEmailSuccess();
                    // 依 nextView 準備下一頁資料，準備好後再切換
                    let nextData = null;
                    if (pendingNextView === 'student') {
                        const r2 = await fetch(`${GAS_WEB_APP_URL}?action=getInit&userId=${currentUserId}`);
                        nextData = await r2.json();
                    }
                    // 停留 1.2 秒讓使用者看到成功訊息，再乾淨跳轉
                    setTimeout(() => {
                        const box = document.getElementById('special-view');
                        box.innerHTML = '';
                        box.style.display = 'none';
                        if (pendingNextView === 'student') {
                            routeByResult(nextData);
                        } else if (pendingNextView === 'pricing') {
                            showPricingPage();
                        } else {
                            showBookingPage();
                        }
                    }, 1200);
                } else {
                    errEl.style.display = 'block'; errEl.innerText = result.message || '儲存失敗，請重試';
                    btn.disabled = false; btn.innerText = '送出';
                }
            } catch (err) {
                errEl.style.display = 'block'; errEl.innerText = '連線失敗，請重試';
                btn.disabled = false; btn.innerText = '送出';
            }
        }

        // 👑 Email 登記成功過場畫面
        function showEmailSuccess() {
            const box = document.getElementById('special-view');
            box.innerHTML = `<div class="sv-center" style="padding-top:100px;">
                <div style="width:72px; height:72px; margin:0 auto 20px; border-radius:50%; background:var(--leaf); display:flex; align-items:center; justify-content:center;">
                    <span style="color:#fff; font-size:38px; line-height:1;">&#10003;</span>
                </div>
                <div class="sv-title" style="font-size:20px;">登記成功！</div>
                <div class="sv-sub">Email 已儲存，正在為您跳轉…</div>
            </div>`;
            box.style.display = 'block';
        }

        // 👑 價格 + 優惠頁 (Trial / 結訓)
        function showPricingPage() {
            document.getElementById('loading-screen').style.display = 'none';
            const box = document.getElementById('special-view');
            box.style.display = 'block';
            box.innerHTML = `<div style="padding:20px 16px;">
                <div style="text-align:center; font-size:20px; font-weight:700; color:#333; margin-bottom:16px;">💰 課程價格</div>
                <div style="background:#FFF; border:1.5px solid var(--coffee); border-radius:12px; padding:16px; margin-bottom:20px;">
                    ${priceRow('體驗課程', '75元 / 25分鐘')}
                    ${priceRow('ESL 課程', '310元 / 50分鐘')}
                    ${priceRow('英檢證照', '330元 / 50分鐘')}
                    ${priceRow('商業英文', '340元 / 50分鐘')}
                    ${priceRow('多益課程', '380元 / 50分鐘')}
                    ${priceRow('雅思課程', '440元 / 50分鐘', true)}
                </div>
                <div style="text-align:center; font-size:20px; font-weight:700; color:#333; margin-bottom:16px;">🎁 優惠折扣</div>
                <div style="background:#FFF; border:1.5px solid var(--coffee); border-radius:12px; padding:16px;">
                    ${priceRow('優惠方案 1', '10堂享 95折')}
                    ${priceRow('優惠方案 2', '20堂享 9折')}
                    ${priceRow('優惠方案 3', '30堂享 85折')}
                    ${priceRow('優惠方案 4', '邀請好友折 100', true)}
                </div>
            </div>`;
        }
        function priceRow(name, price, last) {
            return `<div style="display:flex; justify-content:space-between; align-items:center; padding:10px 0; ${last ? '' : 'border-bottom:1px solid #EEE;'}">
                <span style="font-size:15px; font-weight:600; color:#333;">${name}</span>
                <span style="font-size:15px; color:var(--blush); font-weight:600;">${price}</span>
            </div>`;
        }

        // 👑 預約體驗頁 (第二階段，先放預留位置)
        function showBookingPage() {
            document.getElementById('loading-screen').style.display = 'none';
            const box = document.getElementById('special-view');
            box.style.display = 'block';
            box.innerHTML = `<div style="text-align:center; padding:50px 24px; color:var(--text-soft);">
                <div style="font-size:44px; margin-bottom:14px;">🥥</div>
                <div style="font-size:17px; font-weight:600; color:#333; margin-bottom:8px;">預約體驗課</div>
                <div style="font-size:14px;">預約功能即將開放，敬請期待 😊</div>
            </div>`;
        }


        // 👑 家長資料渲染 (從 getInit 拿到的結果直接用，不再另外 fetch)
        function renderStudentResult(result) {
            if (result.status === 'success' && result.data && result.data.length > 0) {
                studentDataList = result.data;
                renderTabs(); renderStudentData(0);
                document.getElementById('loading-screen').style.display = 'none';
                document.getElementById('special-view').style.display = 'none';
                document.getElementById('header').style.display = 'block';
                document.getElementById('main-content').style.display = 'block'; document.getElementById('brand-bar').style.display = 'block';
            } else {
                document.getElementById('loading-text').innerText = result.message || "找不到您的專屬課程資料，請確認是否綁定。";
            }
        }

        // 👑 老師資料渲染 (從 getInit 拿到的結果直接用)
        function renderTeacherResult(result) {
            if (result.status === 'success') {
                teacherData = result;
                renderTeacher();
                document.getElementById('loading-screen').style.display = 'none';
                document.getElementById('teacher-header').style.display = 'block';
                document.getElementById('teacher-content').style.display = 'block'; document.getElementById('teacher-brand-bar').style.display = 'block';
            } else {
                document.getElementById('loading-text').innerText = result.message || "Unable to load teacher data.";
            }
        }

        // ==========================================
        //   👑 老師端：抓資料 + 渲染三分頁
        // ==========================================
        async function fetchTeacherData(userId) {
            try {
                const res = await fetch(`${GAS_WEB_APP_URL}?action=getTeacherDashboard&userId=${userId}`);
                const result = await res.json();
                renderTeacherResult(result);
            } catch (err) { document.getElementById('loading-text').innerText = "Connection failed. Please try again."; }
        }

        function esc(s) {
            const d = document.createElement('div'); d.textContent = (s == null ? '' : s); return d.innerHTML;
        }

        function renderTeacher() {
            document.getElementById('t-name').innerText = teacherData.teacherName;
            // 分頁1 今日
            document.getElementById('t-summary').innerText = `${teacherData.todayDateStr} · ${teacherData.today.length} classes`;
            document.getElementById('t-tab-today').innerHTML = renderScheduleCards(teacherData.today, "No classes today.");
            // 分頁3 明日
            document.getElementById('t-tab-tomorrow').innerHTML = renderScheduleCards(teacherData.tomorrow, "No classes tomorrow.");
            // 分頁2 回饋作業
            document.getElementById('t-tab-feedback').innerHTML = renderFeedbackCards(teacherData.studentRecords);
        }

        function renderScheduleCards(list, emptyMsg) {
            if (!list || list.length === 0) return `<div style="text-align:center;color:#6c757d;padding:20px 0;">${emptyMsg}</div>`;
            return list.map(c => {
                if (c.cancelled) {
                    return `<div class="card" style="background:#F1EFE8;border-color:#B4B2A9;opacity:0.75;">
                        <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:6px;">
                            <div style="font-size:15px;font-weight:bold;color:#5F5E5A;text-decoration:line-through;">${esc(c.courseKey)}</div>
                            <span style="padding:2px 8px;background:#F7C1C1;border-radius:6px;font-size:11px;font-weight:bold;color:#791F1F;">Cancelled</span>
                        </div>
                        <div style="font-size:13px;color:#5F5E5A;text-decoration:line-through;">${esc(c.timeStr)}</div>
                    </div>`;
                }
                return `<div class="card">
                    <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:6px;">
                        <div style="font-size:15px;font-weight:bold;color:#333;">${esc(c.courseKey)}</div>
                        <div style="font-size:13px;font-weight:bold;color:#333;">${esc(c.timeStr)}</div>
                    </div>
                    ${c.material ? `<div style="font-size:13px;color:var(--text-soft);">${esc(c.material)}</div>` : ''}
                </div>`;
            }).join('');
        }

        function renderFeedbackCards(records) {
            if (!records || records.length === 0) return `<div style="text-align:center;color:#6c757d;padding:20px 0;">No students today.</div>`;
            return records.map(r => {
                const fb = (r.feedbackEn && r.feedbackEn !== 'No feedback yet.')
                    ? `<div style="font-size:13px;color:#555;font-style:italic;">${esc(r.feedbackEn)}</div>` : '';
                let hwBlock;
                if (r.homework && r.homework.hasSubmission) {
                    const link = (r.homework.fileUrl && r.homework.fileUrl !== '無附件')
                        ? `<div style="font-size:13px;"><a href="${esc(r.homework.fileUrl)}" target="_blank" style="color:var(--blush);font-weight:bold;">Open homework file</a></div>` : '';
                    const msg = r.homework.comment ? `<div style="font-size:13px;color:#333;">Message: ${esc(r.homework.comment)}</div>` : '';
                    hwBlock = `<div style="background:#FBF0E4;padding:10px;border-radius:8px;border:1px solid var(--blush);">
                        <div style="font-size:12px;color:var(--blush);font-weight:bold;">📤 Homework submitted</div>
                        <div style="font-size:12px;color:var(--blush);">🕐 ${esc(r.homework.submittedAt)}</div>
                        ${msg}${link}
                    </div>`;
                } else {
                    hwBlock = `<div style="background:#F1EFE8;padding:10px;border-radius:8px;border:1px solid #B4B2A9;font-size:13px;color:#5F5E5A;">📤 No homework submission yet</div>`;
                }
                return `<div class="card">
                    <div style="font-size:15px;font-weight:bold;color:#333;margin-bottom:10px;">${esc(r.courseKey)}</div>
                    <div style="background:var(--paper);padding:10px;border-radius:8px;border:1px solid var(--border-color);margin-bottom:10px;">
                        <div style="font-size:12px;color:var(--text-soft);">💬 Latest feedback${r.feedbackDate ? ' (' + esc(r.feedbackDate) + ')' : ''}</div>
                        <div style="font-size:13px;color:#333;">Material: ${esc(r.material)}</div>
                        ${fb}
                    </div>
                    ${hwBlock}
                </div>`;
            }).join('');
        }

        function switchTeacherTab(idx) {
            document.querySelectorAll('#teacher-tabs .tab').forEach(t => t.classList.remove('active'));
            document.querySelector(`#teacher-tabs .tab[data-ttab="${idx}"]`).classList.add('active');
            document.getElementById('t-tab-today').style.display = (idx === 0) ? 'block' : 'none';
            document.getElementById('t-tab-feedback').style.display = (idx === 1) ? 'block' : 'none';
            document.getElementById('t-tab-tomorrow').style.display = (idx === 2) ? 'block' : 'none';
        }

        async function fetchDashboardData(userId) {
            try {
                const response = await fetch(`${GAS_WEB_APP_URL}?action=getDashboard&userId=${userId}`);
                const result = await response.json();
                if (result.status === 'success' && result.data.length > 0) {
                    studentDataList = result.data;
                    renderTabs(); renderStudentData(0);
                    document.getElementById('loading-screen').style.display = 'none';
                    document.getElementById('header').style.display = 'block';
                    document.getElementById('main-content').style.display = 'block'; document.getElementById('brand-bar').style.display = 'block';
                } else { document.getElementById('loading-text').innerText = result.message || "找不到您的專屬課程資料，請確認是否綁定。"; }
            } catch (err) { document.getElementById('loading-text').innerText = "連線失敗，請檢查網路或稍後再試。"; }
        }

        function renderTabs() {
            const container = document.getElementById('tabs-container'); container.innerHTML = '';
            studentDataList.forEach((student, index) => {
                const tab = document.createElement('div');
                tab.className = `tab ${index === 0 ? 'active' : ''}`;
                tab.innerText = `${student.studentName.split(' ')[0]} (${student.courseType})`;
                tab.onclick = () => {
                    document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
                    tab.classList.add('active'); renderStudentData(index);
                };
                container.appendChild(tab);
            });
        }

        // 👑 顯示目前索引的那筆進度紀錄 (教材/回饋/作業/日期)
        function renderProgress() {
            const rec = progressRecords[currentProgressIndex];
            if (!rec) return;

            // 上課日期
            const dateRow = document.getElementById('class-date-row');
            if (rec.classDate) {
                dateRow.style.display = 'block';
                document.getElementById('display-class-date').innerText = rec.classDate;
            } else {
                dateRow.style.display = 'none';
            }
            // 教材
            document.getElementById('display-material').innerText = rec.material;
            // 回饋
            document.getElementById('display-feedback-en').innerText = rec.feedbackEn;
            document.getElementById('display-feedback-zh').innerText = rec.feedbackZh;
            document.getElementById('feedback-zh-row').style.display = rec.feedbackZh ? 'block' : 'none';
            // 作業 (跟著切換)
            const hwContainer = document.getElementById('homework-container');
            const hwText = rec.homework;
            if (!hwText || hwText.trim() === "" || hwText.trim().toLowerCase() === "無") {
                hwContainer.style.display = 'none';
            } else {
                hwContainer.style.display = 'flex';
                const hwContentEl = document.getElementById('display-homework');
                hwContentEl.innerHTML = '';
                if (hwText.startsWith('http')) {
                    const link = document.createElement('a');
                    link.href = hwText; link.target = '_blank'; link.rel = 'noopener noreferrer';
                    link.textContent = '點擊下載作業附件';
                    hwContentEl.appendChild(link);
                } else {
                    hwContentEl.textContent = hwText;
                }
            }

            // 箭頭與頁碼 (只有超過1筆才顯示切換)
            const nav = document.getElementById('progress-nav');
            if (progressRecords.length > 1) {
                nav.style.display = 'flex';
                document.getElementById('progress-page').innerText = `${currentProgressIndex + 1} / ${progressRecords.length}`;
                // 到頭/到尾時箭頭變灰
                const prev = document.getElementById('progress-prev');
                const next = document.getElementById('progress-next');
                const atFirst = currentProgressIndex === 0;
                const atLast = currentProgressIndex === progressRecords.length - 1;
                prev.style.borderColor = atFirst ? 'var(--line)' : 'var(--blush)';
                prev.style.color = atFirst ? 'var(--line)' : 'var(--blush)';
                next.style.borderColor = atLast ? 'var(--line)' : 'var(--blush)';
                next.style.color = atLast ? 'var(--line)' : 'var(--blush)';
            } else {
                nav.style.display = 'none';
            }
        }

        // 👑 箭頭切換：dir = -1(較新) / +1(較舊)
        function changeProgress(dir) {
            const newIndex = currentProgressIndex + dir;
            if (newIndex < 0 || newIndex >= progressRecords.length) return; // 超出範圍不動作
            currentProgressIndex = newIndex;
            renderProgress();
        }

        function renderStudentData(index) {
            currentTabIndex = index; const data = studentDataList[index];
            document.getElementById('display-name').innerText = data.studentName;
            // 👑 顯示格式：已上 X / 購買 Y ・剩餘 Z 堂 (購買堂數為 0 時只顯示剩餘，避免出現 0/0)
            if (data.totalClasses > 0) {
                document.getElementById('display-remaining').innerText = `已上 ${data.usedClasses} / ${data.totalClasses} 堂 ・ 剩餘 ${data.remainingClasses} 堂`;
            } else {
                document.getElementById('display-remaining').innerText = `剩餘堂數：${data.remainingClasses} 堂`;
            }
            // 👑 最近進度紀錄 (最多2筆，可箭頭切換)；相容舊資料退回單筆
            progressRecords = (data.recentRecords && data.recentRecords.length > 0) ? data.recentRecords : [data.latestProgress];
            currentProgressIndex = 0;
            renderProgress();

            const listContainer = document.getElementById('upcoming-list'); listContainer.innerHTML = '';
            if (data.upcomingClasses.length === 0) {
                listContainer.innerHTML = '<div style="text-align:center; color:#6c757d; padding:15px 0;">目前未來兩週尚無排課紀錄喔！</div>';
                return;
            }

            data.upcomingClasses.forEach(course => {
                const item = document.createElement('div'); item.className = 'class-item';

                if (course.cancelled) {
                    // 👑 已請假的課：日期時間劃掉 + 灰色 + 「請假」標籤 (保留時段、標註請假)
                    item.innerHTML = `<div class="class-info"><span class="class-date" style="text-decoration:line-through; color:var(--text-soft);">${course.dateStr}（${course.weekday}）${course.timeStr}</span><span class="class-type" style="color:#ADB5BD;">${data.courseType} Course</span></div>`;
                    const tag = document.createElement('span');
                    tag.innerText = '請假';
                    tag.style.cssText = 'padding:6px 14px; background:#F7C1C1; color:#791F1F; border-radius:8px; font-weight:bold; font-size:14px; white-space:nowrap;';
                    item.appendChild(tag);
                } else {
                    item.innerHTML = `<div class="class-info"><span class="class-date">${course.dateStr}（${course.weekday}）${course.timeStr}</span><span class="class-type">${data.courseType} Course</span></div>`;
                    const btn = document.createElement('button'); btn.className = 'btn-cancel';
                    if (course.canCancel) {
                        btn.innerText = '申請請假';
                        btn.onclick = () => handleCancelClass(course.eventId, course.startTimeMs, btn);
                    } else { btn.innerText = '不可取消'; btn.disabled = true; }
                    item.appendChild(btn);
                }
                listContainer.appendChild(item);
            });
        }

        function handleCancelClass(eventId, startTimeMs, buttonElement) {
            Swal.fire({ title: '確認請假？', text: "取消後將釋出此時段，確定要申請請假嗎？", icon: 'warning', showCancelButton: true, confirmButtonColor: 'var(--blush)', cancelButtonText: '先不要' }).then(async (result) => {
                if (result.isConfirmed) {
                    buttonElement.innerText = '處理中...'; buttonElement.disabled = true;
                    try {
                        // 👑 改用 POST 傳遞 (原本用 GET，userId/eventId 會留在網址列與伺服器 log 中)
                        const response = await fetch(GAS_WEB_APP_URL, {
                            method: 'POST',
                            headers: { 'Content-Type': 'text/plain;charset=utf-8' },
                            body: JSON.stringify({ action: 'liffCancel', userId: currentUserId, eventId: eventId, startTimeMs: startTimeMs })
                        });
                        const resultJson = await response.json();
                        if (resultJson.status === 'success') { Swal.fire('成功！', '課程已順利請假。', 'success'); buttonElement.innerText = '已請假'; }
                        else { Swal.fire('無法取消', resultJson.message || '距離上課已不足 30 分鐘。', 'error'); buttonElement.innerText = '申請請假'; buttonElement.disabled = false; }
                    } catch (error) { Swal.fire('連線錯誤', '請稍後再試。', 'error'); buttonElement.innerText = '申請請假'; buttonElement.disabled = false; }
                }
            });
        }

        async function submitHomeworkToTeacher() {
            const fileInput = document.getElementById('hw-file');
            const message = document.getElementById('hw-message').value.trim();
            const currentStudent = studentDataList[currentTabIndex].studentName;

            if (!fileInput.files.length && !message) { Swal.fire('提示', '請選擇檔案或輸入留言！', 'info'); return; }
            Swal.fire({ title: '上傳分發中...', text: '正在將檔案安全投遞給外師，請勿關閉', allowOutsideClick: false, didOpen: () => Swal.showLoading() });

            try {
                let fileData = null;
                if (fileInput.files.length > 0) {
                    const file = fileInput.files[0];
                    if (file.size > 5 * 1024 * 1024) { Swal.fire('檔案過大', '檔案請小於 5MB', 'warning'); return; }
                    fileData = { name: file.name, mimeType: file.type, base64: await new Promise((r) => { const reader = new FileReader(); reader.onload = () => r(reader.result.split(',')[1]); reader.readAsDataURL(file); }) };
                }

                const response = await fetch(GAS_WEB_APP_URL, {
                    method: 'POST',
                    headers: { 'Content-Type': 'text/plain;charset=utf-8' },
                    body: JSON.stringify({ action: 'uploadHomework', studentName: currentStudent, userId: currentUserId, message: message, fileData: fileData })
                });

                const resultJson = await response.json();
                if (resultJson.status === 'success') {
                    Swal.fire('成功！', '作業已送達外師 LINE 聊天室！', 'success');
                    document.getElementById('hw-message').value = ''; fileInput.value = '';
                } else { Swal.fire('提示', resultJson.message || '分發失敗，外師目前可能未綁定 LINE 註冊。', 'warning'); }
            } catch (err) { Swal.fire('錯誤', '連線失敗，請檢查設定。', 'error'); }
        }

        window.onload = initializeApp;
    </script>
</body>
</html>
index.html
目前顯示的是「index.html」。
