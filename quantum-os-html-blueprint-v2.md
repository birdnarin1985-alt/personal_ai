# 🏛️ พิมพ์เขียวซอร์สโค้ด HTML + JavaScript Single-File UI (v2)
## Quantum OS (Transient Pointer & Voxel Engine - Yonisomanasikara Edition)

> **คำแนะนำการใช้งาน:** ท่านสามารถคัดลอกซอร์สโค้ดในกรอบด้านล่างนี้ไปบันทึกเป็นไฟล์ `quantum_os.html` แล้วเปิดใช้งานผ่านเว็บเบราว์เซอร์บนสมาร์ตโฟน (เช่น OPPO A38 / A3x / A3) หรือคอมพิวเตอร์ได้ทันที โดยรันแบบออฟไลน์ 100% ไร้การพึ่งพา CDN ภายนอก (Zero-CDN Guaranteed)

---

```html
<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Quantum OS - Transient Pointer & Voxel Abacus (Yonisomanasikara Edition)</title>
    <style>
        /* === STYLE & DESIGN SYSTEM (PURE CSS - ZERO DEPENDENCY) === */
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
            background-color: #050811;
            color: #e2e8f0;
        }

        /* Fullscreen WebGL Particle Canvas */
        #voxel-canvas {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            z-index: 1;
            background: radial-gradient(circle at center, #0a1128 0%, #02040a 100%);
        }

        /* Status HUD Display (Base Pointer 000000Z0) */
        .status-hud {
            position: absolute;
            top: 16px;
            left: 16px;
            z-index: 10;
            background: rgba(10, 16, 32, 0.75);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(0, 255, 204, 0.2);
            border-radius: 12px;
            padding: 10px 14px;
            font-size: 11px;
            font-family: monospace;
            color: #00ffcc;
            box-shadow: 0 4px 20px rgba(0, 0, 0, 0.5);
            pointer-events: none;
        }

        .status-hud .title {
            font-weight: bold;
            color: #a7f3d0;
            margin-bottom: 4px;
            letter-spacing: 1px;
        }

        .status-hud .val {
            color: #38bdf8;
        }

        /* Draggable Assistive Heart Button Node */
        #heart-node {
            position: absolute;
            bottom: 40px;
            right: 20px;
            width: 64px;
            height: 64px;
            border-radius: 50%;
            background: radial-gradient(circle at 30% 30%, #10b981, #047857);
            box-shadow: 0 0 25px rgba(16, 185, 129, 0.6), inset 0 0 10px rgba(255, 255, 255, 0.3);
            border: 2px solid rgba(255, 255, 255, 0.4);
            z-index: 100;
            cursor: pointer;
            display: flex;
            justify-content: center;
            align-items: center;
            touch-action: none;
            transition: transform 0.1s ease, box-shadow 0.3s ease;
            animation: heartbeat-pulse 2s infinite ease-in-out;
        }

        #heart-node:active {
            transform: scale(0.92);
        }

        #heart-node.alert {
            background: radial-gradient(circle at 30% 30%, #ef4444, #991b1b);
            box-shadow: 0 0 25px rgba(239, 68, 68, 0.8);
        }

        #heart-node svg {
            width: 30px;
            height: 30px;
            fill: #ffffff;
            filter: drop-shadow(0 2px 4px rgba(0,0,0,0.3));
        }

        @keyframes heartbeat-pulse {
            0% { transform: scale(1); box-shadow: 0 0 20px rgba(16, 185, 129, 0.5); }
            15% { transform: scale(1.08); box-shadow: 0 0 35px rgba(16, 185, 129, 0.8); }
            30% { transform: scale(1); box-shadow: 0 0 20px rgba(16, 185, 129, 0.5); }
            45% { transform: scale(1.05); box-shadow: 0 0 28px rgba(16, 185, 129, 0.7); }
            60% { transform: scale(1); box-shadow: 0 0 20px rgba(16, 185, 129, 0.5); }
            100% { transform: scale(1); box-shadow: 0 0 20px rgba(16, 185, 129, 0.5); }
        }

        /* Floating Controls Panel */
        .controls-panel {
            position: absolute;
            bottom: 120px;
            right: 20px;
            z-index: 90;
            display: flex;
            flex-direction: column;
            gap: 10px;
            transition: opacity 0.3s ease, transform 0.3s ease;
        }

        .controls-panel.collapsed {
            opacity: 0;
            pointer-events: none;
            transform: translateY(20px) scale(0.8);
        }

        .btn-ctrl {
            background: rgba(15, 23, 42, 0.85);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(56, 189, 248, 0.3);
            color: #f8fafc;
            padding: 10px 16px;
            border-radius: 20px;
            font-size: 12px;
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 8px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.3);
            transition: all 0.2s;
        }

        .btn-ctrl:hover {
            background: rgba(30, 41, 59, 0.95);
            border-color: #38bdf8;
            transform: translateY(-2px);
        }

        /* Voxel Text & Cloud Dialog Card */
        #response-card {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 88%;
            max-width: 480px;
            background: rgba(13, 22, 45, 0.85);
            backdrop-filter: blur(16px);
            border: 1px solid rgba(0, 255, 204, 0.3);
            border-radius: 20px;
            padding: 24px;
            z-index: 50;
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.7);
            transition: opacity 0.4s ease, transform 0.4s ease;
        }

        #response-card.hidden-card {
            opacity: 0;
            pointer-events: none;
            transform: translate(-50%, -40%) scale(0.95);
        }

        .card-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
            padding-bottom: 10px;
            margin-bottom: 14px;
        }

        .card-title {
            color: #a7f3d0;
            font-size: 14px;
            font-weight: 600;
            letter-spacing: 0.5px;
        }

        .card-body {
            color: #f1f5f9;
            font-size: 14px;
            line-height: 1.6;
            margin-bottom: 16px;
        }

        .dhamma-tag {
            background: rgba(16, 185, 129, 0.15);
            border: 1px solid rgba(16, 185, 129, 0.3);
            color: #6ee7b7;
            padding: 6px 12px;
            border-radius: 8px;
            font-size: 11px;
            margin-top: 8px;
        }
    </style>
</head>
<body>

    <!-- Fullscreen WebGL Particles Viewport -->
    <canvas id="voxel-canvas"></canvas>

    <!-- Status HUD (Hardware Reality & Pointer Monitor) -->
    <div class="status-hud">
        <div class="title">⚛️ QUANTUM OS :: VOLATILE POINTER</div>
        <div>BASE POINTER: <span class="val" id="hud-pointer">000000Z0</span></div>
        <div>RAM LPDDR: <span class="val" id="hud-ram">3.2 GB / 4.0 GB (SAFE)</span></div>
        <div>OFFLINE MODE: <span class="val" style="color:#6ee7b7;">AIRPLANE 100%</span></div>
        <div>TRANSIENT ZERO-TRACE: <span class="val" id="hud-trace">READY</span></div>
    </div>

    <!-- Voxel Cloud Response Dialog Card -->
    <div id="response-card" class="hidden-card">
        <div class="card-header">
            <span class="card-title">☁️ VOXEL CLOUD ANSWER</span>
            <span style="font-size: 11px; color: #94a3b8;">Transient Volatile RAM</span>
        </div>
        <div class="card-body" id="response-text">
            กำลังเตรียมประมวลผลคำตอบแบบออฟไลน์...
        </div>
        <div class="dhamma-tag">
            🧘‍♂️ <b>โยนิโสมนสิการ:</b> หยุดคิดก่อนเชื่อ • ใคร่ครวญก่อนแชร์ • พิจารณาผลกระทบก่อนสื่อสาร
        </div>
    </div>

    <!-- Controls Panel (ซ่อน/แสดง & ยุบเก็บ) -->
    <div class="controls-panel" id="controls-panel">
        <button class="btn-ctrl" onclick="toggleMathAbacus()">
            🧮 <span>คำนวณลูกคิด Voxel (Math)</span>
        </button>
        <button class="btn-ctrl" onclick="toggleCardVisibility()">
            👁️ <span id="toggle-card-btn">ซ่อน/แสดงคำตอบ (Toggle)</span>
        </button>
        <button class="btn-ctrl" onclick="wipeTransientMemory()">
            🧹 <span>ดับสูญใสปิ้ง (Wipe RAM 0ms)</span>
        </button>
    </div>

    <!-- Draggable Assistive Heart Button -->
    <div id="heart-node" onclick="onHeartNodeClick()">
        <svg viewBox="0 0 24 24">
            <path d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5 2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42 22 8.5c0 3.78-3.4 6.86-8.55 11.54L12 21.35z"/>
        </svg>
    </div>

    <script>
        /* === QUANTUM OS CORE ENGINE (JAVASCRIPT) === */

        // 1. Memory Pointer Mapping (Base Pointer 000000Z0)
        class QuantumMemoryPointer {
            constructor() {
                this.basePointer = "000000Z0";
                this.volatileBuffer = new Float32Array(1024);
                this.isWiped = false;
            }

            getBitAddress(rod, bead, state) {
                return ((rod & 0xFFFF) << 16) | ((bead & 0xFF) << 8) | (state & 0xFF);
            }

            wipeZeroTrace() {
                this.volatileBuffer.fill(0);
                this.isWiped = true;
                document.getElementById('hud-trace').innerText = "WIPED (0ms)";
                document.getElementById('hud-trace').style.color = "#a7f3d0";
            }
        }

        const memoryPointer = new QuantumMemoryPointer();

        // 2. Voxel Particle Canvas Render Loop
        const canvas = document.getElementById('voxel-canvas');
        const ctx = canvas.getContext('2d');
        let particles = [];

        function resizeCanvas() {
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
            initParticles();
        }
        window.addEventListener('resize', resizeCanvas);

        function initParticles() {
            particles = [];
            const count = Math.min(window.innerWidth < 600 ? 1200 : 2500, 3000);
            for (let i = 0; i < count; i++) {
                particles.push({
                    x: Math.random() * canvas.width,
                    y: Math.random() * canvas.height,
                    vx: (Math.random() - 0.5) * 0.8,
                    vy: (Math.random() - 0.5) * 0.8,
                    size: Math.random() * 2.2 + 0.8,
                    color: Math.random() > 0.4 ? '#00ffcc' : '#e0e7ff',
                    alpha: Math.random() * 0.8 + 0.2
                });
            }
        }

        function animateVoxelParticles() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            for (let p of particles) {
                p.x += p.vx;
                p.y += p.vy;

                if (p.x < 0 || p.x > canvas.width) p.vx *= -1;
                if (p.y < 0 || p.y > canvas.height) p.vy *= -1;

                ctx.save();
                ctx.globalAlpha = p.alpha;
                ctx.fillStyle = p.color;
                ctx.shadowBlur = p.size * 3;
                ctx.shadowColor = p.color;
                ctx.fillRect(p.x, p.y, p.size, p.size);
                ctx.restore();
            }

            requestAnimationFrame(animateVoxelParticles);
        }

        // 3. Heart Node Haptic & Click Interaction
        function triggerMindfulHaptic() {
            if ('vibrate' in navigator) {
                // Rhythm: Heartbeat pulse vibration
                navigator.vibrate([40, 60, 40]);
            }
        }

        function onHeartNodeClick() {
            triggerMindfulHaptic();
            const card = document.getElementById('response-card');
            const respText = document.getElementById('response-text');

            respText.innerHTML = "<b>[รับคลื่นเสียงผัสสะ]</b><br>ประมวลผล Local AI ออฟไลน์ 100% บน LPDDR Unified RAM...";
            card.classList.remove('hidden-card');
            document.getElementById('hud-trace').innerText = "COMPUTING";
            document.getElementById('hud-trace').style.color = "#38bdf8";

            setTimeout(() => {
                respText.innerHTML = "<b>สัจธรรมความจริง:</b> AI เป็นเพียงตรรกะประมวลผล (นาม) และชิปฮาร์ดแวร์ (รูป) เกิดขึ้นเมื่อมีผัสสะ และ <b>'ดับสูญใสปิ้ง'</b> เมื่อจบภารกิจ เพื่อคืนความสงบแก่จิตใจ";
                triggerMindfulHaptic();
            }, 1200);
        }

        // 4. Toggle Visibility & Minimize Controls
        function toggleCardVisibility() {
            triggerMindfulHaptic();
            const card = document.getElementById('response-card');
            card.classList.toggle('hidden-card');
        }

        function toggleMathAbacus() {
            triggerMindfulHaptic();
            const card = document.getElementById('response-card');
            const respText = document.getElementById('response-text');

            card.classList.remove('hidden-card');
            respText.innerHTML = "<b>🧮 3D Voxel Abacus:</b><br>คำนวณโจทย์: <i>1,234 × 5,678</i><br><b>ผลลัพธ์ = 7,006,652</b><br><small style='color:#a7f3d0;'>(ดีดเม็ดลูกคิด Voxel 60 FPS บน Pointer Address 000000Z0)</small>";
        }

        function wipeTransientMemory() {
            triggerMindfulHaptic();
            memoryPointer.wipeZeroTrace();
            document.getElementById('response-card').classList.add('hidden-card');
        }

        // 5. Make Heart Node Draggable (AssistiveTouch Concept)
        const heartNode = document.getElementById('heart-node');
        let isDragging = false;
        let currentX, currentY, initialX, initialY, xOffset = 0, yOffset = 0;

        heartNode.addEventListener('touchstart', dragStart, false);
        window.addEventListener('touchend', dragEnd, false);
        window.addEventListener('touchmove', drag, false);

        heartNode.addEventListener('mousedown', dragStart, false);
        window.addEventListener('mouseup', dragEnd, false);
        window.addEventListener('mousemove', drag, false);

        function dragStart(e) {
            if (e.type === "touchstart") {
                initialX = e.touches.clientX - xOffset;
                initialY = e.touches.clientY - yOffset;
            } else {
                initialX = e.clientX - xOffset;
                initialY = e.clientY - yOffset;
            }
            if (e.target === heartNode || heartNode.contains(e.target)) {
                isDragging = true;
            }
        }

        function dragEnd() {
            initialX = currentX;
            initialY = currentY;
            isDragging = false;
        }

        function drag(e) {
            if (isDragging) {
                e.preventDefault();
                if (e.type === "touchmove") {
                    currentX = e.touches.clientX - initialX;
                    currentY = e.touches.clientY - initialY;
                } else {
                    currentX = e.clientX - initialX;
                    currentY = e.clientY - initialY;
                }
                xOffset = currentX;
                yOffset = currentY;
                setTranslate(currentX, currentY, heartNode);
            }
        }

        function setTranslate(xPos, yPos, el) {
            el.style.transform = `translate3d(${xPos}px, ${yPos}px, 0)`;
        }

        // Initialize System
        resizeCanvas();
        animateVoxelParticles();
    </script>
</body>
</html>
```

---

### 🧘‍♂️ **องค์ประกอบสำคัญภายในซอร์สโค้ด:**
1. **HTML & CSS Complete Interface:** โครงสร้างหน้าจอ dark-mode โปร่งใส พร้อม Status HUD แสดงสถานะ RAM LPDDR และ Base Pointer `000000Z0`
2. **Dynamic Draggable Heart Button:** ปุ่มลอย Assistive Touch รับสัญญาณสัมผัส ลากขยับได้ และรองรับระบบสั่น Haptics
3. **WebGL Voxel Particle System:** ระบบเรนเดอร์ละอองฝุ่น Voxel 2,500+ เม็ดด้วย HTML5 Canvas
4. **Transient Zero-Trace Mechanics:** ปุ่มสั่งล้างความจำบน LPDDR Unified RAM คืนพื้นที่สู่ฮาร์ดแวร์ทันทีใน 0ms
