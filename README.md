<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>椰菲英文 - 專屬學習儀表板</title>
    <script charset="utf-8" src="https://static.line-scdn.net/liff/edge/2/sdk.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>

    <style>
        :root { --primary-color: #E63946; --bg-color: #F8F9FA; --card-bg: #FFFFFF; --text-main: #333333; --text-muted: #6C757D; --border-color: #2B2D42; --bubble-bg: #E8F1F2; }
        * { box-sizing: border-box; font-family: 'Noto Sans TC', sans-serif; }
        body { margin: 0; padding: 0; background-color: var(--bg-color); color: var(--text-main); }
        #loading-screen { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: var(--bg-color); display: flex; flex-direction: column; justify-content: center; align-items: center; z-index: 9999; }
        .spinner { width: 40px; height: 40px; border: 3px solid rgba(0,0,0,0.1); border-top-color: var(--border-color); border-radius: 50%; animation: spin 1s linear infinite; margin-bottom: 15px; }
        @keyframes spin { to { transform: rotate(360deg); } }
        .header { background: var(--card-bg); border-bottom: 2px solid var(--border-color); position: sticky; top: 0; z-index: 100; padding-top: 15px;}
        .tabs-container { display: flex; overflow-x: auto; padding: 0 15px; gap: 10px; scrollbar-width: none; }
        .tab { padding: 10px 20px; font-weight: bold; font-size: 16px; border: 2px solid transparent; border-bottom: none; border-radius: 12px 12px 0 0; cursor: pointer; color: var(--text-muted); transition: all 0.2s; white-space: nowrap; }
        .tab.active { color: var(--text-main); border-color: var(--border-color); background: var(--card-bg); border-bottom: 2px solid var(--card-bg); margin-bottom: -2px; }
        .content-container { padding: 20px 15px; display: none; }
        .student-summary { text-align: center; margin-bottom: 20px; }
        .student-summary h2 { margin: 0 0 5px 0; font-size: 22px; }
        .badge-pill { display: inline-block; padding: 5px 12px; border: 1.5px solid var(--border-color); border-radius: 20px; font-size: 14px; font-weight: bold;}
        .card { background: var(--card-bg); border: 1.5px solid var(--border-color); border-radius: 12px; padding: 20px; margin-bottom: 20px; box-shadow: 2px 4px 0px rgba(0,0,0,0.05); }
        .card-title { margin-top: 0; margin-bottom: 15px; font-size: 18px; display: flex; align-items: center; gap: 8px;}
        .material-text { font-weight: bold; font-size: 16px; margin-bottom: 15px; }
        .feedback-bubble { background: var(--bubble-bg); padding: 15px; border-radius: 0 12px 12px 12px; border: 1px solid var(--border-color); margin-bottom: 15px; line-height: 1.5; }
        .homework-box { display: flex; align-items: flex-start; gap: 10px; padding-top: 15px; border-top: 1px dashed var(--border-color); }
        .hw-badge { background: var(--primary-color); color: white; padding: 3px 8px; border-radius: 6px; font-size: 12px; font-weight: bold; white-space: nowrap;}
        .hw-content { font-size: 15px; line-height: 1.4; word-break: break-all; }
        .hw-content a { color: var(--primary-color); font-weight: bold; text-decoration: underline; }
        .class-item { display: flex; justify-content: space-between; align-items: center; padding: 15px 0; border-bottom: 1px solid #EEEEEE; }
        .class-info { display: flex; flex-direction: column; gap: 4px; }
        .class-date { font-weight: bold; font-size: 16px; }
        .class-type { font-size: 14px; color: var(--text-muted); }
        .btn-cancel { background: white; border: 1.5px solid var(--border-color); color: var(--primary-color); padding: 8px 16px; border-radius: 8px; font-weight: bold; font-size: 14px; cursor: pointer; white-space: nowrap;}
        .btn-cancel:disabled { background: #E9ECEF; border-color: #CED4DA; color: #6C757D; cursor: not-allowed; }
    </style>
</head>
<body>

    <div id="loading-screen"><div class="spinner"></div><div id="loading-text">正在載入專屬學習儀表板...</div></div>
    <div class="header" id="header" style="display: none;"><div class="tabs-container" id="tabs-container"></div></div>

    <div class="content-container" id="main-content">
        <div class="student-summary"><h2 id="display-name">學生姓名</h2><div class="badge-pill" id="display-remaining">剩餘堂數：- 堂</div></div>

        <div class="card">
            <h3 class="card-title">🚀 上次上課進度</h3>
            <div class="material-text" id="class-date-row" style="font-weight: normal; color: #6C757D; font-size: 14px;">📅 上課日期：<span id="display-class-date">-</span></div>
            <div class="material-text">📚 目前教材：<span id="display-material">載入中...</span></div>
            <div class="feedback-bubble">
                <div style="font-style: italic; color: #555555; margin-bottom: 10px; font-size: 14px; word-break: break-word;">🇺🇸 <span id="display-feedback-en">Loading...</span></div>
                <div style="font-weight: bold; border-top: 1px solid var(--border-color); padding-top: 10px; font-size: 15px; word-break: break-word;">🇹🇼 <span id="display-feedback-zh">載入中...</span></div>
            </div>
            <div class="homework-box" id="homework-container"><div class="hw-badge">課後作業</div><div class="hw-content" id="display-homework"></div></div>
        </div>

        <div class="card" style="background-color: #FAFAFA;">
            <h3 class="card-title">📤 繳交作業 / 留言給授課老師</h3>
            <textarea id="hw-message" placeholder="老師好，這是我的功課 (Teacher, this is my homework...)" style="width: 100%; height: 70px; margin-bottom: 15px; border-radius: 8px; border: 1.5px solid var(--border-color); padding: 10px; font-size: 14px; resize: none;"></textarea>
            <input type="file" id="hw-file" style="margin-bottom: 15px; width: 100%; font-size: 14px;" accept="image/*,.pdf,.mp3">
            <button class="btn-cancel" style="width: 100%; background-color: var(--primary-color); color: white;" onclick="submitHomeworkToTeacher()">安全送出給老師</button>
        </div>

        <div class="card"><h3 class="card-title">📅 未來兩週課表</h3><div id="upcoming-list"></div></div>
    </div>

    <!-- 👑 老師專用介面 (全英文，預設隱藏，身分為 teacher 時顯示) -->
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
        let teacherData = null;

        async function initializeApp() {
            try {
                await liff.init({ liffId: LIFF_ID });
                if (!liff.isLoggedIn()) { liff.login(); return; }
                currentUserId = (await liff.getProfile()).userId;
                // 👑 先問身分：老師 → 老師介面；其他 → 家長儀表板
                const roleRes = await fetch(`${GAS_WEB_APP_URL}?action=getRole&userId=${currentUserId}`);
                const roleJson = await roleRes.json();
                if (roleJson.role === 'teacher') {
                    fetchTeacherData(currentUserId);
                } else {
                    fetchDashboardData(currentUserId);
                }
            } catch (err) { document.getElementById('loading-text').innerText = "LIFF 初始化失敗，請在 LINE 內開啟。"; }
        }

        // ==========================================
        //   👑 老師端：抓資料 + 渲染三分頁
        // ==========================================
        async function fetchTeacherData(userId) {
            try {
                const res = await fetch(`${GAS_WEB_APP_URL}?action=getTeacherDashboard&userId=${userId}`);
                const result = await res.json();
                if (result.status === 'success') {
                    teacherData = result;
                    renderTeacher();
                    document.getElementById('loading-screen').style.display = 'none';
                    document.getElementById('teacher-header').style.display = 'block';
                    document.getElementById('teacher-content').style.display = 'block';
                } else {
                    document.getElementById('loading-text').innerText = result.message || "Unable to load teacher data.";
                }
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
                    ${c.material ? `<div style="font-size:13px;color:#6C757D;">${esc(c.material)}</div>` : ''}
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
                        ? `<div style="font-size:13px;"><a href="${esc(r.homework.fileUrl)}" target="_blank" style="color:#854F0B;font-weight:bold;">Open homework file</a></div>` : '';
                    const msg = r.homework.comment ? `<div style="font-size:13px;color:#333;">Message: ${esc(r.homework.comment)}</div>` : '';
                    hwBlock = `<div style="background:#FAEEDA;padding:10px;border-radius:8px;border:1px solid #854F0B;">
                        <div style="font-size:12px;color:#854F0B;font-weight:bold;">📤 Homework submitted</div>
                        <div style="font-size:12px;color:#854F0B;">🕐 ${esc(r.homework.submittedAt)}</div>
                        ${msg}${link}
                    </div>`;
                } else {
                    hwBlock = `<div style="background:#F1EFE8;padding:10px;border-radius:8px;border:1px solid #B4B2A9;font-size:13px;color:#5F5E5A;">📤 No homework submission yet</div>`;
                }
                return `<div class="card">
                    <div style="font-size:15px;font-weight:bold;color:#333;margin-bottom:10px;">${esc(r.courseKey)}</div>
                    <div style="background:#E8F1F2;padding:10px;border-radius:8px;border:1px solid var(--border-color);margin-bottom:10px;">
                        <div style="font-size:12px;color:#6C757D;">💬 Latest feedback${r.feedbackDate ? ' (' + esc(r.feedbackDate) + ')' : ''}</div>
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
                    document.getElementById('main-content').style.display = 'block';
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

        function renderStudentData(index) {
            currentTabIndex = index; const data = studentDataList[index];
            document.getElementById('display-name').innerText = data.studentName;
            // 👑 顯示格式：已上 X / 購買 Y ・剩餘 Z 堂 (購買堂數為 0 時只顯示剩餘，避免出現 0/0)
            if (data.totalClasses > 0) {
                document.getElementById('display-remaining').innerText = `已上 ${data.usedClasses} / ${data.totalClasses} 堂 ・ 剩餘 ${data.remainingClasses} 堂`;
            } else {
                document.getElementById('display-remaining').innerText = `剩餘堂數：${data.remainingClasses} 堂`;
            }
            document.getElementById('display-material').innerText = data.latestProgress.material;
            // 👑 上課日期：有值才顯示，無紀錄時整列隱藏
            const dateRow = document.getElementById('class-date-row');
            if (data.latestProgress.classDate) {
                dateRow.style.display = 'block';
                document.getElementById('display-class-date').innerText = data.latestProgress.classDate;
            } else {
                dateRow.style.display = 'none';
            }
            document.getElementById('display-feedback-en').innerText = data.latestProgress.feedbackEn;
            document.getElementById('display-feedback-zh').innerText = data.latestProgress.feedbackZh;

            const hwContainer = document.getElementById('homework-container');
            const hwText = data.latestProgress.homework;
            if (!hwText || hwText.trim() === "" || hwText.trim().toLowerCase() === "無") {
                hwContainer.style.display = 'none';
            } else {
                hwContainer.style.display = 'flex';
                // 👑 改用 textContent / 動態建立 DOM 節點，避免 Sheet 內容含有 HTML/script 時被直接執行 (XSS 風險)
                const hwContentEl = document.getElementById('display-homework');
                hwContentEl.innerHTML = '';
                if (hwText.startsWith('http')) {
                    const link = document.createElement('a');
                    link.href = hwText;
                    link.target = '_blank';
                    link.rel = 'noopener noreferrer';
                    link.textContent = '點擊下載作業附件';
                    hwContentEl.appendChild(link);
                } else {
                    hwContentEl.textContent = hwText;
                }
            }

            const listContainer = document.getElementById('upcoming-list'); listContainer.innerHTML = '';
            if (data.upcomingClasses.length === 0) {
                listContainer.innerHTML = '<div style="text-align:center; color:#6c757d; padding:15px 0;">目前未來兩週尚無排課紀錄喔！</div>';
                return;
            }

            data.upcomingClasses.forEach(course => {
                const item = document.createElement('div'); item.className = 'class-item';
                item.innerHTML = `<div class="class-info"><span class="class-date">${course.dateStr} ${course.timeStr}</span><span class="class-type">${data.courseType} Course</span></div>`;

                const btn = document.createElement('button'); btn.className = 'btn-cancel';
                if (course.canCancel) {
                    btn.innerText = '申請請假';
                    btn.onclick = () => handleCancelClass(course.eventId, course.startTimeMs, btn);
                } else { btn.innerText = '不可取消'; btn.disabled = true; }

                item.appendChild(btn); listContainer.appendChild(item);
            });
        }

        function handleCancelClass(eventId, startTimeMs, buttonElement) {
            Swal.fire({ title: '確認請假？', text: "取消後將釋出此時段，確定要申請請假嗎？", icon: 'warning', showCancelButton: true, confirmButtonColor: '#E63946', cancelButtonText: '先不要' }).then(async (result) => {
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
