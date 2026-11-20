// ==UserScript==
// @name         Universal Floating UI & Menu
// @namespace    http://tampermonkey.net/
// @version      1.0
// @description  Nút mở UI chính hiển thị trên mọi trang web
// @match        *://*/*
// @grant        none
// ==/UserScript==

(function () {
    'use strict';

    // Tránh chèn trùng nếu trang web chạy lại script
    if (document.getElementById('custom-ui-root')) return;

    const root = document.createElement('div');
    root.id = 'custom-ui-root';
    root.style.all = 'initial';
    root.style.position = 'fixed';
    root.style.zIndex = '2147483647';
    root.style.top = '0';
    root.style.left = '0';
    root.style.width = '0';
    root.style.height = '0';
    document.documentElement.appendChild(root);

    // Dùng Shadow DOM để CSS của trang web không làm hỏng giao diện UI của bạn
    const shadow = root.attachShadow({ mode: 'open' });

    shadow.innerHTML = `
        <style>
            .toggle-btn {
                position: fixed;
                bottom: 20px;
                right: 20px;
                width: 52px;
                height: 52px;
                border-radius: 50%;
                border: none;
                background: #18181b;
                color: #ffffff;
                font-size: 13px;
                font-weight: 700;
                font-family: system-ui, -apple-system, sans-serif;
                cursor: pointer;
                box-shadow: 0 6px 20px rgba(0, 0, 0, 0.35);
                display: flex;
                align-items: center;
                justify-content: center;
                transition: transform 0.15s ease, background 0.15s ease;
                user-select: none;
                touch-action: none;
            }
            .toggle-btn:hover {
                background: #27272a;
                transform: scale(1.05);
            }
            .main-ui {
                position: fixed;
                bottom: 84px;
                right: 20px;
                width: 320px;
                max-width: calc(100vw - 32px);
                background: #111114;
                color: #f4f4f5;
                border: 1px solid rgba(255, 255, 255, 0.12);
                border-radius: 14px;
                box-shadow: 0 14px 36px rgba(0, 0, 0, 0.5);
                font-family: system-ui, -apple-system, sans-serif;
                overflow: hidden;
                display: none;
            }
            .main-ui.open {
                display: block;
            }
            .ui-header {
                display: flex;
                align-items: center;
                justify-content: space-between;
                padding: 12px 14px;
                background: #18181c;
                border-bottom: 1px solid rgba(255, 255, 255, 0.08);
            }
            .ui-title {
                font-size: 14px;
                font-weight: 700;
            }
            .ui-close {
                background: transparent;
                border: none;
                color: #a1a1aa;
                font-size: 18px;
                line-height: 1;
                cursor: pointer;
                padding: 2px 6px;
            }
            .ui-close:hover {
                color: #ffffff;
            }
            .ui-body {
                padding: 14px;
                display: flex;
                flex-direction: column;
                gap: 10px;
            }
            .ui-info {
                font-size: 12px;
                color: #a1a1aa;
                word-break: break-all;
            }
            .ui-btn {
                width: 100%;
                padding: 9px 12px;
                border-radius: 8px;
                border: 1px solid rgba(255, 255, 255, 0.14);
                background: #27272a;
                color: #ffffff;
                font-size: 13px;
                font-weight: 600;
                cursor: pointer;
            }
            .ui-btn:hover {
                background: #3f3f46;
            }
            .status-msg {
                font-size: 12px;
                color: #4ade80;
                min-height: 16px;
            }
        </style>

        <button class="toggle-btn" id="toggleBtn" title="Mở/Đóng UI">UI</button>

        <div class="main-ui" id="mainUi">
            <div class="ui-header">
                <span class="ui-title">UI Chính</span>
                <button class="ui-close" id="closeBtn">&times;</button>
            </div>
            <div class="ui-body">
                <div class="ui-info" id="siteInfo"></div>
                <button class="ui-btn" id="actionBtn">Chạy chức năng mẫu</button>
                <div class="status-msg" id="statusMsg"></div>
            </div>
        </div>
    `;

    const toggleBtn = shadow.getElementById('toggleBtn');
    const mainUi = shadow.getElementById('mainUi');
    const closeBtn = shadow.getElementById('closeBtn');
    const actionBtn = shadow.getElementById('actionBtn');
    const statusMsg = shadow.getElementById('statusMsg');
    const siteInfo = shadow.getElementById('siteInfo');

    siteInfo.textContent = 'Trang hiện tại: ' + location.hostname;

    let isOpen = false;
    function setOpen(state) {
        isOpen = state;
        mainUi.classList.toggle('open', isOpen);
    }

    closeBtn.addEventListener('click', () => setOpen(false));

    // Kéo thả nút mở UI + bấm để mở/đóng
    let dragging = false;
    let moved = false;
    let startX = 0;
    let startY = 0;
    let origRight = 20;
    let origBottom = 20;

    toggleBtn.addEventListener('pointerdown', (e) => {
        dragging = true;
        moved = false;
        startX = e.clientX;
        startY = e.clientY;
        origRight = parseInt(toggleBtn.style.right || '20', 10);
        origBottom = parseInt(toggleBtn.style.bottom || '20', 10);
        toggleBtn.setPointerCapture(e.pointerId);
    });

    toggleBtn.addEventListener('pointermove', (e) => {
        if (!dragging) return;
        const dx = e.clientX - startX;
        const dy = e.clientY - startY;
        if (Math.abs(dx) > 4 || Math.abs(dy) > 4) moved = true;
        if (!moved) return;

        const nextRight = Math.max(8, Math.min(window.innerWidth - 60, origRight - dx));
        const nextBottom = Math.max(8, Math.min(window.innerHeight - 60, origBottom - dy));
        toggleBtn.style.right = nextRight + 'px';
        toggleBtn.style.bottom = nextBottom + 'px';
    });

    toggleBtn.addEventListener('pointerup', () => {
        if (!dragging) return;
        dragging = false;
        if (!moved) {
            setOpen(!isOpen);
        }
    });

    actionBtn.addEventListener('click', () => {
        statusMsg.textContent = 'Đã kích hoạt lúc ' + new Date().toLocaleTimeString();
    });
})();
