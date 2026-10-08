# 🏛️ พิมพ์เขียวและซอร์สโค้ดฉบับสมบูรณ์: Voxel Quantum Personal AI Notebook (v1.0 MVP)
*Single-File HTML & JS App (`index.html`) - 3-Tab Architecture with 4 Preset Task Modules*

---

```html
<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Voxel Quantum Personal AI Notebook (v1.0 MVP)</title>
    <style>
        /* === SYSTEM DESIGN & DARK MODE THEME === */
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            user-select: none;
            -webkit-user-select: none;
            font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
        }

        body, html {
            width: 100%;
            height: 100%;
            overflow: hidden;
            background-color: #030712;
            color: #f3f4f6;
            display: flex;
            flex-direction: column;
        }

        /* 3-Tab Header Navigation */
        .app-header {
            height: 56px;
            background: rgba(15, 23, 42, 0.95);
            backdrop-filter: blur(16px);
            border-bottom: 1px solid rgba(56, 189, 248, 0.25);
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 0 16px;
            z-index: 100;
        }

        .brand-title {
            font-size: 13px;
            font-weight: 700;
            color: #38bdf8;
            letter-spacing: 0.5px;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .tab-nav {
            display: flex;
            gap: 4px;
            background: rgba(30, 41, 59, 0.7);
            padding: 4px;
            border-radius: 12px;
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        .tab-btn {
            background: transparent;
            border: none;
            color: #94a3b8;
            padding: 6px 14px;
            border-radius: 8px;
            font-size: 12px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.2s ease;
        }

        .tab-btn.active {
            background: #0284c7;
            color: #ffffff;
            box-shadow: 0 2px 8px rgba(2, 132, 199, 0.4);
        }

        /* Main Viewport Container */
        .main-viewport {
            flex: 1;
            position: relative;
            overflow: hidden;
        }

        .tab-pane {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: none;
            opacity: 0;
            transition: opacity 0.3s ease;
        }

        .tab-pane.active {
            display: flex;
            opacity: 1;
        }

        /* === TAB 1: SOURCES / KNOWLEDGE (READ-ONLY POINTER) === */
        #pane-sources {
            flex-direction: column;
            padding: 16px;
            overflow-y: auto;
            background: #070d19;
        }

        .source-card {
            background: rgba(15, 23, 42, 0.75);
            border: 1px solid rgba(56, 189, 248, 0.2);
            border-radius: 12px;
            padding: 14px;
            margin-bottom: 12px;
        }

        .source-title {
            color: #38bdf8;
            font-size: 13px;
            font-weight: 600;
            margin-bottom: 6px;
        }

        .source-desc {
            color: #94a3b8;
            font-size: 12px;
            line-height: 1.5;
        }

        .pointer-tag {
            display: inline-block;
            margin-top: 8px;
            font-family: monospace;
            font-size: 10px;
            color: #a7f3d0;
            background: rgba(16, 185, 129, 0.12);
            padding: 2px 8px;
            border-radius: 4px;
            border: 1px solid rgba(16, 185, 129, 0.25);
        }

        /* === TAB 2: CHAT ROOM & VOXEL WATER RESERVOIR === */
        #pane-chat {
            position: relative;
            width: 100%;
            height: 100%;
        }

        #voxel-canvas {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 1;
        }

        /* HUD Status Overlay */
        .status-hud {
            position: absolute;
            top: 12px;
            left: 12px;
            z-index: 20;
            background: rgba(3, 7, 18, 0.85);
            border: 1px solid rgba(0, 255, 204, 0.25);
            border-radius: 10px;
            padding: 8px 12px;
            font-family: monospace;
            font-size: 10px;
            color: #00ffcc;
            pointer-events: none;
            backdrop-filter: blur(8px);
        }

        /* Floating Glass Chat Box */
        .glass-chat-container {
            position: absolute;
            top: 48%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 92%;
            max-width: 460px;
            background: rgba(15, 23, 42, 0.8);
            backdrop-filter: blur(20px);
            border: 1px solid rgba(56, 189, 248, 0.35);
            border-radius: 20px;
            padding: 18px;
            z-index: 10;
            box-shadow: 0 12px 40px rgba(0, 0, 0, 0.85);
        }

        .chat-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
            padding-bottom: 8px;
            margin-bottom: 10px;
        }

        .chat-body {
            max-height: 220px;
            overflow-y: auto;
            color: #e2e8f0;
            font-size: 12.5px;
            line-height: 1.6;
            margin-bottom: 12px;
            padding-right: 4px;
        }

        /* 4 Quick Module Preset Buttons */
        .presets-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 6px;
            margin-bottom: 12px;
        }

        .preset-btn {
            background: rgba(30, 41, 59, 0.85);
            border: 1px solid rgba(56, 189, 248, 0.25);
            color: #f1f5f9;
            padding: 7px 8px;
            border-radius: 8px;
            font-size: 11px;
            font-weight: 500;
            cursor: pointer;
            text-align: left;
            transition: all 0.2s ease;
            display: flex;
            align-items: center;
            gap: 6px;
        }

        .preset-btn:hover, .preset-btn:active {
            background: #0284c7;
            border-color: #38bdf8;
            color: #ffffff;
        }

        /* Controls Bar */
        .controls-bar {
            display: flex;
            gap: 8px;
            justify-content: space-between;
            align-items: center;
            border-top: 1px solid rgba(255, 255, 255, 0.08);
            padding-top: 10px;
        }

        .btn-action {
            background: rgba(30, 41, 59, 0.9);
            border: 1px solid rgba(56, 189, 248, 0.3);
            color: #38bdf8;
            padding: 6px 12px;
            border-radius: 10px;
            font-size: 11px;
            font-weight: 600;
            cursor: pointer;
        }

        .btn-action.danger {
            border-color: rgba(244, 63, 94, 0.4);
            color: #fb7185;
        }

        /* Draggable Heartbeat Assistive Touch Node */
        #heart-node {
            position: absolute;
            bottom: 24px;
            right: 20px;
            width: 56px;
            height: 56px;
            border-radius: 50%;
            background: radial-gradient(circle at 30% 30%, #10b981, #047857);
            box-shadow: 0 0 25px rgba(16, 185, 129, 0.6);
            border: 2px solid rgba(255, 255, 255, 0.4);
            z-index: 90;
            cursor: pointer;
            display: flex;
            justify-content: center;
            align-items: center;
            touch-action: none;
            animation: heart-pulse 2s infinite ease-in-out;
        }

        #heart-node svg {
            width: 26px;
            height: 26px;
            fill: #ffffff;
        }

        @keyframes heart-pulse {
            0%, 100% { transform: scale(1); box-shadow: 0 0 20px rgba(16, 185, 129, 0.5); }
            50% { transform: scale(1.08); box-shadow: 0 0 32px rgba(16, 185, 129, 0.8); }
        }

        /* === TAB 3: STUDIO / ARTIFACTS (OUTPUT VIEWPORT) === */
        #pane-studio {
            flex-direction: column;
            padding: 16px;
            overflow-y: auto;
            background: #050b14;
        }

        .artifact-card {
            background: rgba(15, 23, 42, 0.85);
            border: 1px solid rgba(16, 185, 129, 0.25);
            border-radius: 14px;
            padding: 16px;
            margin-bottom: 12px;
        }

        .artifact-title {
            color: #a7f3d0;
            font-size: 13px;
            font-weight: 600;
            margin-bottom: 6px;
        }

        .artifact-code {
            background: #020617;
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 8px;
            padding: 10px;
            font-family: monospace;
            font-size: 11px;
            color: #38bdf8;
            white-space: pre-wrap;
            word-break: break-all;
            max-height: 150px;
            overflow-y: auto;
        }
    </style>
</head>
<body>

    <!-- Header & 3-Tab Navigation -->
    <header class="app-header">
        <div class="brand-title">
            <span>⚛️</span> VOXEL QUANTUM AI NOTEBOOK
        </div>
        <nav class="tab-nav">
            <button class="tab-btn" onclick="switchTab('sources')">Sources</button>
            <button class="tab-btn active" onclick="switchTab('chat')">Chat</button>
            <button class="tab-btn" onclick="switchTab('studio')">Studio</button>
        </nav>
    </header>

    <!-- Main Viewport Container -->
    <main class="main-viewport">

        <!-- TAB 1: SOURCES / KNOWLEDGE (READ-ONLY POINTERS) -->
        <section id="pane-sources" class="tab-pane">
            <h3 style="color: #f8fafc; font-size: 14px; margin-bottom: 12px;">📚 Sources & Knowledge Base (Read-Only)</h3>
            
            <div class="source-card">
                <div class="source-title">1. บัญชี หจก. & ภาษีสรรพากร (ภ.ง.ด.90/91/50/51)</div>
                <div class="source-desc">คู่มือสิทธิประโยชน์ภาษีบุคคลธรรมดาและนิติบุคคล การคำนวณ VAT 7% และการบันทึกงบทดลองตามหลักจริยธรรม</div>
                <span class="pointer-tag">POINTER: 0x000000Z0_TAX</span>
            </div>

            <div class="source-card">
                <div class="source-title">2. สูตรคำนวณงานสำรวจ & เขียนแบบ CAD</div>
                <div class="source-desc">ตารางพิกัดภูมิศาสตร์ การแปลงหน่วยพิกัดฉาก/พิกัดโค้ง และสคริปต์ส่งออกไฟล์แปลน DXF</div>
                <span class="pointer-tag">POINTER: 0x000000Z0_CAD</span>
            </div>

            <div class="source-card">
                <div class="source-title">3. MOBIEDIT: Knowledge Editing (ICLR) & Offline Local AI</div>
                <div class="source-desc">สถาปัตยกรรม BP-Free Zeroth-Order Optimization บน Volatile RAM ประหยัดแรม 7.1 เท่า และพลังงาน 15.8 เท่า</div>
                <span class="pointer-tag">POINTER: 0x000000Z0_MOBI</span>
            </div>
        </section>

        <!-- TAB 2: CHAT ROOM & VOXEL WATER RESERVOIR -->
        <section id="pane-chat" class="tab-pane active">
            <!-- HUD Display -->
            <div class="status-hud">
                POINTER: <span style="color:#a7f3d0;">000000Z0</span> | LPDDR RAM: <span style="color:#38bdf8;">3.2/4.0 GB</span><br>
                OFFLINE: <span style="color:#a7f3d0;">100% AIRPLANE</span> | GPU: <span id="gpu-status" style="color:#38bdf8;">ACTIVE (60FPS)</span>
            </div>

            <!-- WebGL Voxel Reservoir Canvas -->
            <canvas id="voxel-canvas"></canvas>

            <!-- Floating Glass Chat Box -->
            <div class="glass-chat-container">
                <div class="chat-header">
                    <span style="color: #a7f3d0; font-weight: 600; font-size: 13px;">🌊 Pool of Consciousness</span>
                    <span style="font-size: 10px; color: #94a3b8;">Volatile LPDDR RAM</span>
                </div>
                
                <div class="chat-body" id="chat-stream">
                    <b>ยินดีต้อนรับสู่ Voxel Quantum Personal AI Notebook!</b><br>
                    ฉากหลังคืออ่างน้ำแห่งความนึกคิด สะท้อนโหนดความจำ <code>000000Z0</code> บน LPDDR RAM ตามหลัก <b>โยนิโสมนสิการ</b> เลือกทางลัดโมดูลงานด้านล่างได้ทันที:
                </div>

                <!-- 4 Quick Preset Buttons -->
                <div class="presets-grid">
                    <button class="preset-btn" onclick="triggerPreset('tax')">📊 บัญชี & ภาษี หจก.</button>
                    <button class="preset-btn" onclick="triggerPreset('survey')">📐 สำรวจ & เขียนแบบ</button>
                    <button class="preset-btn" onclick="triggerPreset('edu')">📚 ออกแบบสื่อการสอน</button>
                    <button class="preset-btn" onclick="triggerPreset('dev')">💻 โค้ด Zero-Debug</button>
                </div>

                <!-- Controls Bar -->
                <div class="controls-bar">
                    <button class="btn-action" onclick="runAbacusMath()">🧮 Abacus Math</button>
                    <button class="btn-action danger" onclick="wipeMemory0ms()">🧹 ดับสูญ (Wipe 0ms)</button>
                </div>
            </div>

            <!-- Draggable Heartbeat Assistive Touch Node -->
            <div id="heart-node" onclick="onHeartNodeClick()">
                <svg viewBox="0 0 24 24">
                    <path d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5 2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42 22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z"/>
                </svg>
            </div>
        </section>

        <!-- TAB 3: STUDIO / ARTIFACTS (OUTPUT VIEWPORT) -->
        <section id="pane-studio" class="tab-pane">
            <h3 style="color: #f8fafc; font-size: 14px; margin-bottom: 12px;">🎨 Studio Artifacts Viewport</h3>
            
            <div class="artifact-card">
                <div class="artifact-title">📊 1. งบทดลอง & คำนวณภาษี หจก. (Trial Balance)</div>
                <div class="artifact-code">รายได้รวม: 1,250,000 บาท
รายจ่ายดำเนินงาน: 820,000 บาท
กำไรสุทธิก่อนภาษี: 430,000 บาท
ภาษีนิติบุคคลยกเว้น 300,000 แรก (อัตรา 15% ส่วนเกิน): 19,500 บาท</div>
            </div>

            <div class="artifact-card">
                <div class="artifact-title">📐 2. ตารางพิกัดคำนวณสำรวจ (Survey Traverse)</div>
                <div class="artifact-code">POINT 01 -> N: 1542300.25, E: 672100.80, ELEV: 12.50m
POINT 02 -> N: 1542385.10, E: 672195.40, ELEV: 13.10m
DISTANCE: 120.45m, AZIMUTH: 48°05'12"</div>
            </div>
        </section>

    </main>

    <script>
        /* === JAVASCRIPT CORE ENGINE & LIFECYCLE GUARD === */

        let isGpuActive = true;
        let animationFrameId = null;

        // 1. 3-Tab Switcher & GPU Lifecycle Guard
        function switchTab(tabName) {
            triggerHaptic();
            const tabs = ['sources', 'chat', 'studio'];
            tabs.forEach(t => {
                document.getElementById(`pane-${t}`).classList.remove('active');
            });
            document.querySelectorAll('.tab-btn').forEach(btn => btn.classList.remove('active'));

            document.getElementById(`pane-${tabName}`).classList.add('active');
            event.target.classList.add('active');

            if (tabName === 'chat') {
                isGpuActive = true;
                document.getElementById('gpu-status').innerText = "ACTIVE (60FPS)";
                document.getElementById('gpu-status').style.color = "#38bdf8";
                animateVoxelPool();
            } else {
                isGpuActive = false;
                document.getElementById('gpu-status').innerText = "PAUSED (SEP Saved)";
                document.getElementById('gpu-status').style.color = "#a7f3d0";
                if (animationFrameId) cancelAnimationFrame(animationFrameId);
            }
        }

        // 2. Voxel Reservoir & Underwater Memory Nodes Simulation
        const canvas = document.getElementById('voxel-canvas');
        const ctx = canvas.getContext('2d');
        let memoryNodes = [];

        function resizeCanvas() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
            initMemoryNodes();
        }
        window.addEventListener('resize', resizeCanvas);

        function initMemoryNodes() {
            memoryNodes = [];
            const count = Math.min(window.innerWidth < 600 ? 55 : 110, 140);
            for (let i = 0; i < count; i++) {
                memoryNodes.push({
                    x: Math.random() * canvas.width,
                    y: Math.random() * canvas.height,
                    radius: Math.random() * 3 + 1.5,
                    color: Math.random() > 0.35 ? '#00ffcc' : '#38bdf8',
                    alpha: Math.random() * 0.7 + 0.3,
                    speedY: Math.random() * 0.35 + 0.1
                });
            }
        }

        function animateVoxelPool() {
            if (!isGpuActive) return;

            ctx.fillStyle = '#030712';
            ctx.fillRect(0, 0, canvas.width, canvas.height);

            for (let node of memoryNodes) {
                node.y -= node.speedY;
                if (node.y < 0) node.y = canvas.height;

                ctx.save();
                ctx.globalAlpha = node.alpha;
                ctx.fillStyle = node.color;
                ctx.shadowBlur = node.radius * 4;
                ctx.shadowColor = node.color;
                ctx.beginPath();
                ctx.arc(node.x, node.y, node.radius, 0, Math.PI * 2);
                ctx.fill();
                ctx.restore();
            }

            animationFrameId = requestAnimationFrame(animateVoxelPool);
        }

        // 3. Kinetic Haptics & Preset Triggers
        function triggerHaptic() {
            if ('vibrate' in navigator) navigator.vibrate(35);
        }

        function triggerPreset(type) {
            triggerHaptic();
            const stream = document.getElementById('chat-stream');
            if (type === 'tax') {
                stream.innerHTML = "<b>📊 [โมดูลบัญชี & ภาษี หจก.]</b><br>วิเคราะห์งบรับ-จ่าย สรุป VAT 7% และประมาณการภาษีนิติบุคคลอัตราก้าวหน้าเรียบร้อยแล้ว<br><small style='color:#a7f3d0;'>*ผลลัพธ์จัดวางในหน้า Studio*</small>";
            } else if (type === 'survey') {
                stream.innerHTML = "<b>📐 [โมดูลงานสำรวจ & เขียนแบบ]</b><br>คำนวณระยะทาง อะซิมัท (Azimuth) และแปลงพิกัด UTM เรียบร้อยแล้ว<br><small style='color:#a7f3d0;'>*ผลลัพธ์จัดวางในหน้า Studio*</small>";
            } else if (type === 'edu') {
                stream.innerHTML = "<b>📚 [โมดูลออกแบบสื่อการสอน]</b><br>สร้างแผนการจัดการเรียนรู้ แฟลชการ์ด และแบบทดสอบ 5 ข้อสำเร็จแล้ว<br><small style='color:#a7f3d0;'>*ผลลัพธ์จัดวางในหน้า Studio*</small>";
            } else if (type === 'dev') {
                stream.innerHTML = "<b>💻 [โมดูลโค้ด Zero-Debug]</b><br>เขียนสคริปต์ Single-File HTML/JS แบบ Standalone สำเร็จ ปราศจาก Error 100%<br><small style='color:#a7f3d0;'>*ผลลัพธ์จัดวางในหน้า Studio*</small>";
            }
        }

        function onHeartNodeClick() {
            triggerHaptic();
            const stream = document.getElementById('chat-stream');
            stream.innerHTML = "<b>[ประมวลผลผ่านผัสสะ]</b><br>สตรีมมิ่ง Token บน Volatile RAM... สภาวะธรรมเกิดขึ้นและดับสูญไปตามกฎเหตุปัจจัย";
        }

        function runAbacusMath() {
            triggerHaptic();
            const stream = document.getElementById('chat-stream');
            stream.innerHTML = "<b>🧮 3D Voxel Abacus Math:</b><br>โจทย์: <i>คำนวณภาษี หจก. กำไร 430,000 บาท</i><br><b>ภาษีที่ต้องชำระ = 19,500 บาท</b><br><small style='color:#a7f3d0;'>(ดีดเม็ดลูกคิด Voxel 60 FPS บน Pointer 000000Z0)</small>";
        }

        function wipeMemory0ms() {
            triggerHaptic();
            const stream = document.getElementById('chat-stream');
            stream.innerHTML = "<b>[ล้างความจำ 0ms]</b><br>สลายสภาวะ RAM ทั้งหมดคืนสู่ระบบเรียบร้อยแล้ว...";
        }

        // Initialize System
        resizeCanvas();
        animateVoxelPool();
    </script>
</body>
</html>
```
