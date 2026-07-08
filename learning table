<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>椰菲英文 - 專屬學習儀表板</title>
    
    <script charset="utf-8" src="https://static.line-scdn.net/liff/edge/2/sdk.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/sweetalert2@11"></script>

    <style>
        /* ==========================================
           🎨 視覺設計：簡約線條 (Simple Line Art) 風格
           ========================================== */
        :root {
            --primary-color: #E63946; /* 椰菲紅 / 警示紅 */
            --bg-color: #F8F9FA;
            --card-bg: #FFFFFF;
            --text-main: #333333;
            --text-muted: #6C757D;
            --border-color: #2B2D42; /* 深色細線條外框 */
            --bubble-bg: #E8F1F2; /* 輕盈的淺藍色回饋框 */
        }

        * { box-sizing: border-box; font-family: 'Noto Sans TC', sans-serif; }
        body { margin: 0; padding: 0; background-color: var(--bg-color); color: var(--text-main); }

        /* 載入畫面 */
        #loading-screen {
            position: fixed; top: 0; left: 0; width: 100%; height: 100%;
            background: var(--bg-color); display: flex; flex-direction: column;
            justify-content: center; align-items: center; z-index: 9999;
        }
        .spinner {
            width: 40px; height: 40px; border: 3px solid rgba(0,0,0,0.1);
            border-top-color: var(--border-color); border-radius: 50%;
            animation: spin 1s linear infinite; margin-bottom: 15px;
        }
        @keyframes spin { to { transform: rotate(360deg); } }

        /* 頂部頁籤 (Tabs) */
        .header { background: var(--card-bg); border-bottom: 2px solid var(--border-color); position: sticky; top: 0; z-index: 100; padding-top: 15px;}
        .tabs-container { display: flex; overflow-x: auto; padding: 0 15px; gap: 10px; scrollbar-width: none; }
        .tab {
            padding: 10px 20px; font-weight: bold; font-size: 16px; border: 2px solid transparent;
            border-bottom: none; border-radius: 12px 12px 0 0; cursor: pointer; color: var(--text-muted);
            transition: all 0.2s; white-space: nowrap;
        }
        .tab.active {
            color: var(--text-main); border-color: var(--border-color); background: var(--card-bg);
            border-bottom: 2px solid var(--card-bg); margin-bottom: -2px; /* 蓋住底線的視覺巧思 */
        }

        /* 內容區塊 */
        .content-container { padding: 20px 15px; display: none; }
        
        .student-summary { text-align: center; margin-bottom: 20px; }
        .student-summary h2 { margin: 0 0 5px 0; font-size: 22px; }
        .badge-pill { display: inline-block; padding: 5px 12px; border: 1.5px solid var(--border-color); border-radius: 20px; font-size: 14px; font-weight: bold;}

        /* 卡片共用樣式 */
        .card {
            background: var(--card-bg); border: 1.5px solid var(--border-color);
            border-radius: 12px; padding: 20px; margin-bottom: 20px;
            box-shadow: 2px 4px 0px rgba(0,0,0,0.05); /* 微微的漫畫風陰影 */
        }
        .card-title { margin-top: 0; margin-bottom: 15px; font-size: 18px; display: flex; align-items: center; gap: 8px;}

        /* 上次進度區塊 */
        .material-text { font-weight: bold; font-size: 16px; margin-bottom: 15px; }
        .feedback-bubble {
            background: var(--bubble-bg); padding: 15px; border-radius: 0 12px 12px 12px;
            border: 1px solid var(--border-color); margin-bottom: 15px; line-height: 1.5;
        }
        .homework-box {
            display: flex; align-items: flex-start; gap: 10px; padding-top: 15px;
            border-top: 1px dashed var(--border-color);
        }
        .hw-badge { background: var(--primary-color); color: white; padding: 3px 8px; border-radius: 6px; font-size: 12px; font-weight: bold; white-space: nowrap;}
        .hw-content { font-size: 15px; line-height: 1.4; word-break: break-all; }
        .hw-content a { color: var(--primary-color); font-weight: bold; text-decoration: underline; }

        /* 課表清單區塊 */
        .class-item {
            display: flex; justify-content: space-between; align-items: center;
            padding: 15px 0; border-bottom: 1px solid #EEEEEE;
        }
        .class-item:last-child { border-bottom: none; padding-bottom: 0; }
        .class-info { display: flex; flex-direction: column; gap: 4px; }
        .class-date { font-weight: bold; font-size: 16px; }
        .class-type { font-size: 14px; color: var(--text-muted); }
        
        /* 請假按鈕 */
        .btn-cancel {
            background: white; border: 1.5px solid var(--border-color); color: var(--primary-color);
            padding: 8px 16px; border-radius: 8px; font-weight: bold; font-size: 14px; cursor: pointer;
            transition: 0.2s; white-space: nowrap;
        }
        .btn-cancel:active { background: #f0f0f0; }
        .btn-cancel:disabled, .btn-cancel.disabled {
            background: #E9ECEF; border-color: #CED4DA; color: #6C757D; cursor: not-allowed;
        }
    </style>
</head>
<body>

    <div id="loading-screen">
        <div class="spinner"></div>
        <div id="loading-text">正在載入專屬資料...</div>
    </div>

    <div class="header" id="header" style="display: none;">
        <div class="tabs-container" id="tabs-container">
            </div>
    </div>

    <div class="content-container" id="main-content">
        
        <div class="student-summary">
            <h2 id="display-name">學生姓名</h2>
            <div class="badge-pill" id="display-remaining">剩餘堂數：- 堂</div>
        </div>

        <div class="card">
            <h3 class="card-title">🚀 上次上課進度</h3>
            <div class="material-text">📚 目前教材：<span id="display-material">載入中...</span></div>
            <div class="feedback-bubble">
                <strong>💡 老師回饋：</strong><br>
                <span id="display-feedback">載入中...</span>
            </div>
            
            <div class="homework-box" id="homework-container">
                <div class="hw-badge">待完成</div>
                <div class="hw-content" id="display-homework"></div>
            </div>
        </div>

        <div class="card">
            <h3 class="card-title">📅 未來兩週課表</h3>
            <div id="upcoming-list">
                </div>
        </div>
    </div>

    <script>
        // ==========================================
        //   ⚙️ 核心設定區 (請填入您的資料)
        // ==========================================
        const LIFF_ID = "請在此填寫您的_LIFF_ID"; 
        const GAS_WEB_APP_URL = "請在此填寫您的_GAS_網頁應用程式網址_部署後的URL";

        let studentDataList = []; // 儲存從後端抓來的所有學生資料
        let currentUserId = "";

        // ==========================================
        //   🚀 初始化與資料抓取
        // ==========================================
        async function initializeApp() {
            try {
                await liff.init({ liffId: LIFF_ID });
                if (!liff.isLoggedIn()) {
                    liff.login();
                    return;
                }
                const profile = await liff.getProfile();
                currentUserId = profile.userId;
                fetchDashboardData(currentUserId);
            } catch (err) {
                showError("LIFF 初始化失敗，請確認在 LINE 內開啟。");
            }
        }

        async function fetchDashboardData(userId) {
            try {
                const response = await fetch(`${GAS_WEB_APP_URL}?action=getDashboard&userId=${userId}`);
                const result = await response.json();
                
                if (result.status === 'success' && result.data.length > 0) {
                    studentDataList = result.data;
                    renderTabs();
                    renderStudentData(0); // 預設顯示第一位學生的資料
                    
                    document.getElementById('loading-screen').style.display = 'none';
                    document.getElementById('header').style.display = 'block';
                    document.getElementById('main-content').style.display = 'block';
                } else {
                    showError("找不到您的專屬課程資料，請確認是否已綁定。");
                }
            } catch (err) {
                showError("讀取資料失敗，請稍後再試或聯繫客服。");
            }
        }

        // ==========================================
        //   🎨 渲染前端畫面
        // ==========================================
        function renderTabs() {
            const tabsContainer = document.getElementById('tabs-container');
            tabsContainer.innerHTML = ''; // 清空
            
            studentDataList.forEach((student, index) => {
                const tab = document.createElement('div');
                tab.className = `tab ${index === 0 ? 'active' : ''}`;
                // 若名字太長，稍微做截斷處理
                const shortName = student.studentName.split(' ')[0].substring(0, 8);
                tab.innerText = `${shortName} (${student.courseType})`;
                tab.onclick = () => {
                    document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
                    tab.classList.add('active');
                    renderStudentData(index);
                };
                tabsContainer.appendChild(tab);
            });
        }

        function renderStudentData(index) {
            const data = studentDataList[index];
            
            // 1. 基本資訊
            document.getElementById('display-name').innerText = data.studentName;
            document.getElementById('display-remaining').innerText = `剩餘堂數：${data.remainingClasses} 堂`;

            // 2. 歷史進度
            document.getElementById('display-material').innerText = data.latestProgress.material;
            document.getElementById('display-feedback').innerText = data.latestProgress.feedback;
            
            // 處理作業 (如果字串是 http 開頭，渲染成超連結)
            const hwContainer = document.getElementById('homework-container');
            const hwText = data.latestProgress.homework;
            if (!hwText || hwText.trim() === "" || hwText.trim().toLowerCase() === "無") {
                hwContainer.style.display = 'none'; // 隱藏無作業區塊
            } else {
                hwContainer.style.display = 'flex';
                const hwDisplay = document.getElementById('display-homework');
                if (hwText.startsWith('http')) {
                    hwDisplay.innerHTML = `<a href="${hwText}" target="_blank">點擊查看/下載作業附件</a>`;
                } else {
                    hwDisplay.innerText = hwText;
                }
            }

            // 3. 未來課表清單
            const listContainer = document.getElementById('upcoming-list');
            listContainer.innerHTML = '';
            
            if (data.upcomingClasses.length === 0) {
                listContainer.innerHTML = '<div style="text-align:center; color:#6c757d; padding: 20px 0;">目前兩週內尚無排課紀錄喔！</div>';
                return;
            }

            data.upcomingClasses.forEach(course => {
                const item = document.createElement('div');
                item.className = 'class-item';
                
                // 左側資訊
                const infoDiv = document.createElement('div');
                infoDiv.className = 'class-info';
                infoDiv.innerHTML = `<span class="class-date">${course.dateStr} ${course.timeStr}</span><span class="class-type">${data.courseType} Course</span>`;
                
                // 右側按鈕
                const btn = document.createElement('button');
                btn.className = 'btn-cancel';
                btn.id = `btn-${course.eventId}`;
                
                if (course.canCancel) {
                    btn.innerText = '申請請假';
                    btn.onclick = () => handleCancelClass(course.eventId, course.startTimeMs, btn);
                } else {
                    btn.innerText = '不可取消';
                    btn.disabled = true;
                }

                item.appendChild(infoDiv);
                item.appendChild(btn);
                listContainer.appendChild(item);
            });
        }

        // ==========================================
        //   ⚡ 請假防呆與後端互動 API
        // ==========================================
        function handleCancelClass(eventId, startTimeMs, buttonElement) {
            // 使用 SweetAlert2 製作精美的防呆確認框
            Swal.fire({
                title: '確認請假？',
                text: "取消後將釋出此時段，確定要申請請假嗎？",
                icon: 'warning',
                showCancelButton: true,
                confirmButtonColor: '#E63946',
                cancelButtonColor: '#6C757D',
                confirmButtonText: '確定請假',
                cancelButtonText: '先不要'
            }).then(async (result) => {
                if (result.isConfirmed) {
                    // UI 先反灰防止重複點擊
                    buttonElement.innerText = '處理中...';
                    buttonElement.disabled = true;
                    
                    try {
                        const response = await fetch(`${GAS_WEB_APP_URL}?action=liffCancel&userId=${currentUserId}&eventId=${eventId}&startTimeMs=${startTimeMs}`);
                        const resJSON = await response.json();
                        
                        if (resJSON.status === 'success') {
                            Swal.fire('成功！', '您的課程已成功取消。', 'success');
                            buttonElement.innerText = '已請假';
                        } else {
                            Swal.fire('無法取消', resJSON.message, 'error');
                            buttonElement.innerText = '申請請假';
                            buttonElement.disabled = false;
                        }
                    } catch (error) {
                        Swal.fire('連線錯誤', '系統忙碌中，請稍後再試。', 'error');
                        buttonElement.innerText = '申請請假';
                        buttonElement.disabled = false;
                    }
                }
            });
        }

        // 錯誤提示小幫手
        function showError(msg) {
            document.getElementById('loading-text').innerText = msg;
            document.querySelector('.spinner').style.display = 'none';
        }

        // 啟動應用
        window.onload = initializeApp;
    </script>
</body>
</html>
