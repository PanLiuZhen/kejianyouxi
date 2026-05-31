[8robinson.html](https://github.com/user-attachments/files/28441939/8robinson.html)
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>鲁滨逊漂流记 · 完美布局版 | 材料1.5倍大小（无挤压）</title>
    <style>
        /* ---------- 极简样式，无挤压，无偏移 ---------- */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            user-select: none;
        }

        body {
            background: linear-gradient(145deg, #1a4a2a, #0d2e1a);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            font-family: 'Segoe UI', 'Quicksand', 'Poppins', 'Comic Neue', system-ui, sans-serif;
            padding: 20px;
        }

        .game-container {
            width: 1024px;
            height: 576px;
            max-width: 95vw;
            max-height: 90vh;
            background: #ffda99;
            border-radius: 40px;
            box-shadow: 0 30px 40px rgba(0, 0, 0, 0.5), inset 0 1px 4px rgba(255, 245, 180, 0.8);
            position: relative;
            overflow: hidden;
            cursor: default;
        }

        /* 沙滩背景 */
        .beach-bg {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: url('beach.png') center/cover no-repeat;
            z-index: 1;
            transition: filter 0.4s ease;
        }

        /* 三种材料 —— 使用 width 直接放大，不挤压布局 */
        .material {
            position: absolute;
            cursor: pointer;
            transition: transform 0.2s ease;
            z-index: 15;
            filter: drop-shadow(8px 14px 20px rgba(0, 0, 0, 0.35));
        }
        .material:hover {
            transform: scale(1.05);
            filter: drop-shadow(12px 18px 26px rgba(0, 0, 0, 0.45));
        }
        /* 材料图片直接设置宽度为原来的 1.5 倍: 原 76px → 114px */
        .material img {
            width: 114px;
            display: block;
        }

        /* 红点提示（跟随图片大小调整位置） */
        .red-dot {
            position: absolute;
            width: 22px;
            height: 22px;
            background: #ff4d4d;
            border-radius: 50%;
            animation: pulse-glow 1.5s infinite;
            z-index: 20;
            pointer-events: none;
            top: -8px;
            right: -8px;
            border: 2px solid white;
        }
        @keyframes pulse-glow {
            0% { transform: scale(0.8); opacity: 1; box-shadow: 0 0 0 0 #ff6b4a;}
            70% { transform: scale(1.4); opacity: 0.6; box-shadow: 0 0 0 12px rgba(255, 80, 50, 0);}
            100% { transform: scale(0.9); opacity: 0.8; box-shadow: 0 0 0 0 rgba(255, 80, 50, 0);}
        }

        /* 遮罩层 */
        .dialogue-overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.6);
            backdrop-filter: blur(5px);
            z-index: 30;
            opacity: 0;
            visibility: hidden;
            transition: opacity 0.35s ease;
        }
        .dialogue-overlay.active {
            opacity: 1;
            visibility: visible;
        }

        /* 鲁滨逊面板 */
        .robinson-panel {
            position: absolute;
            bottom: 0px;
            left: 0;
            right: 0;
            display: flex;
            align-items: flex-end;
            justify-content: center;
            padding: 0;
            z-index: 40;
            pointer-events: none;
        }
        .character-3d {
            width: 950px;
            height: auto;
            filter: drop-shadow(8px 14px 16px rgba(0,0,0,0.3));
            pointer-events: none;
            margin-bottom: -30px;
        }
        .character-3d img {
            width: 100%;
            display: block;
            border-radius: 42px;
        }
        .speech-bubble-3d {
            position: absolute;
            bottom: 80px;
            left: 50%;
            transform: translateX(-50%);
            width: 65%;
            background: #fffef7;
            border-radius: 42px 42px 42px 32px;
            padding: 16px 22px;
            box-shadow: 10px 14px 0 rgba(0, 0, 0, 0.12);
            border: 2px solid #ffc27a;
            font-size: 1rem;
            font-weight: 600;
            color: #32200c;
            pointer-events: auto;
            z-index: 41;
        }
        .speech-text {
            line-height: 1.45;
        }
        .continue-hint {
            margin-top: 12px;
            text-align: right;
            font-size: 0.75rem;
            color: #bc752e;
            cursor: pointer;
            display: inline-block;
            font-weight: 600;
            background: #f7e9cf;
            padding: 4px 16px;
            border-radius: 60px;
            float: right;
            transition: 0.1s;
        }
        .continue-hint:hover {
            background: #ffe2aa;
            transform: scale(0.96);
        }

        /* 背景音乐控制 */
        .sound-control {
            position: absolute;
            bottom: 12px;
            left: 18px;
            z-index: 100;
            background: #2a241acc;
            backdrop-filter: blur(8px);
            padding: 5px 12px;
            border-radius: 40px;
            display: flex;
            gap: 12px;
            font-size: 0.7rem;
            color: #f9e0a0;
            border: 1px solid #ffd58c;
            pointer-events: auto;
            cursor: pointer;
            font-weight: bold;
        }
        .sound-control span {
            cursor: pointer;
            transition: 0.1s;
        }
        .sound-control span:hover {
            color: #ffbc6e;
            transform: scale(1.05);
        }

        .complete-badge {
            position: absolute;
            bottom: 38%;
            left: 25%;
            right: 25%;
            background: #fff0ccee;
            backdrop-filter: blur(8px);
            padding: 12px 20px;
            border-radius: 70px;
            font-size: 1.1rem;
            font-weight: bold;
            text-align: center;
            color: #cc7b2a;
            z-index: 45;
            border: 2px solid #ffbc6e;
            animation: bounceUp 0.4s ease-out;
        }
        @keyframes bounceUp {
            0% { transform: scale(0.7) translateY(30px); opacity: 0; }
            100% { transform: scale(1); opacity: 1;}
        }

        .material.disabled-click {
            opacity: 0.7;
            cursor: default;
            filter: grayscale(0.1);
        }
        .material.disabled-click .red-dot {
            display: none;
        }
        .beach-dim .beach-bg {
            filter: brightness(0.5) saturate(0.65);
        }

        footer {
            position: absolute;
            bottom: 8px;
            right: 18px;
            font-size: 9px;
            color: #f3e5b0;
            z-index: 55;
            background: #00000055;
            padding: 2px 10px;
            border-radius: 40px;
            pointer-events: none;
        }
    </style>
</head>
<body>
<div class="game-container" id="gameApp">
    <!-- 沙滩背景 -->
    <div class="beach-bg"></div>

    <!-- 三种材料：木头，竹子，石头（1.5倍大小，通过 img width 实现，无布局挤压） -->
    <div class="material" data-material="wood" id="materialWood" style="left: 8%; top: 58%;">
        <img src="wood.png" alt="木头"><div class="red-dot"></div>
    </div>
    <div class="material" data-material="bamboo" id="materialBamboo" style="left: 42%; top: 66%;">
        <img src="bamboo.png" alt="竹子"><div class="red-dot"></div>
    </div>
    <div class="material" data-material="stone" id="materialStone" style="left: 74%; top: 52%;">
        <img src="stone.png" alt="石头"><div class="red-dot"></div>
    </div>

    <!-- 遮罩层 -->
    <div class="dialogue-overlay" id="dimOverlay"></div>

    <!-- 鲁滨逊面板 -->
    <div class="robinson-panel" id="robinsonPanel" style="visibility: hidden; opacity: 0; transition: 0.2s;">
        <div class="character-3d">
            <img src="robinson.png" alt="鲁滨逊">
        </div>
        <div class="speech-bubble-3d" id="speechBubble">
            <div class="speech-text" id="speechText">哟嚯！沙滩上散落着三种宝藏材料，点击它们，我来教你造船的奥秘！🏖️</div>
            <div class="continue-hint" id="continueBtn">✨ 继续 ✨</div>
        </div>
    </div>

    <!-- 音乐控制 -->
    <div class="sound-control" id="soundControlBtn">
        <span>🎵 双背景音乐 (2%音量)</span> <span id="musicToggleIcon">🔊 关闭</span>
    </div>
    <footer>🏝️ 材料尺寸1.5倍（无挤压）| 点击音效增强 | 完美布局版</footer>
</div>

<script>
    (function(){
        // ============================================================
        //  🎵【音频配置区域】—— 请在此处填写您的音频文件名 🎵
        // ============================================================
        const WAVES_MUSIC_FILE = 'sea.mp3';           // 海浪背景音乐
        const ADVENTURE_MUSIC_FILE = 'adventure.mp3'; // 冒险背景音乐
        const BG_VOLUME = 0.02;                       // 双背景音量 2%
        
        // 鲁滨逊语音文件（按需修改）
        const VOICE_WOOD = 'voice_wood.mp3';
        const VOICE_BAMBOO = 'voice_bamboo.mp3';
        const VOICE_STONE = 'voice_stone.mp3';
        
        // ============================================================
        //  核心引擎
        // ============================================================
        let wavesAudio = null;
        let adventureAudio = null;
        let isBothPlaying = false;
        let audioInitialized = false;
        let currentVoiceAudio = null;
        
        // DOM 元素
        const dimOverlay = document.getElementById('dimOverlay');
        const robinsonPanel = document.getElementById('robinsonPanel');
        const speechSpan = document.getElementById('speechText');
        const continueBtn = document.getElementById('continueBtn');
        const gameContainer = document.getElementById('gameApp');
        const woodEl = document.getElementById('materialWood');
        const bambooEl = document.getElementById('materialBamboo');
        const stoneEl = document.getElementById('materialStone');
        const musicCtrl = document.getElementById('soundControlBtn');
        const musicIconSpan = document.getElementById('musicToggleIcon');
        
        // 停止当前语音
        function stopCurrentVoice() {
            if (currentVoiceAudio) {
                currentVoiceAudio.pause();
                currentVoiceAudio.currentTime = 0;
                currentVoiceAudio = null;
            }
        }
        
        // 播放语音
        function playVoice(voiceFile) {
            stopCurrentVoice();
            if (!voiceFile) return;
            const audio = new Audio(voiceFile);
            audio.volume = 0.9;
            currentVoiceAudio = audio;
            audio.onended = () => { if (currentVoiceAudio === audio) currentVoiceAudio = null; };
            audio.onerror = () => { if (currentVoiceAudio === audio) currentVoiceAudio = null; };
            audio.play().catch(e => console.warn("语音文件缺失:", voiceFile));
        }
        
        // 背景音乐初始化
        function initBackgroundMusic() {
            if (audioInitialized) return;
            wavesAudio = new Audio(WAVES_MUSIC_FILE);
            wavesAudio.loop = true;
            wavesAudio.volume = BG_VOLUME;
            adventureAudio = new Audio(ADVENTURE_MUSIC_FILE);
            adventureAudio.loop = true;
            adventureAudio.volume = BG_VOLUME;
            audioInitialized = true;
        }
        
        function startBothBackgrounds() {
            if (!wavesAudio || !adventureAudio) initBackgroundMusic();
            if (isBothPlaying) return;
            if (wavesAudio && wavesAudio.paused) wavesAudio.play().catch(e=>console.log);
            if (adventureAudio && adventureAudio.paused) adventureAudio.play().catch(e=>console.log);
            isBothPlaying = true;
            musicIconSpan.innerHTML = "🔊 关闭";
        }
        
        function stopBothBackgrounds() {
            if (wavesAudio) wavesAudio.pause();
            if (adventureAudio) adventureAudio.pause();
            isBothPlaying = false;
            musicIconSpan.innerHTML = "🔇 开启";
        }
        
        function toggleMusic() {
            if (!audioInitialized) initBackgroundMusic();
            if (isBothPlaying) stopBothBackgrounds();
            else startBothBackgrounds();
        }
        
        function ensureMusicPlaying() {
            if (!audioInitialized) initBackgroundMusic();
            if (!isBothPlaying) startBothBackgrounds();
        }
        
        // 点击音效（音量增强）
        function playClickSound() {
            try {
                const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
                const now = audioCtx.currentTime;
                const gain = audioCtx.createGain();
                gain.gain.value = 0.45;
                gain.connect(audioCtx.destination);
                const osc = audioCtx.createOscillator();
                osc.type = 'sine';
                osc.frequency.setValueAtTime(880, now);
                osc.frequency.exponentialRampToValueAtTime(1180, now + 0.07);
                osc.connect(gain);
                osc.start();
                osc.stop(now + 0.12);
                gain.gain.exponentialRampToValueAtTime(0.0001, now + 0.13);
                if (audioCtx.state === 'suspended') audioCtx.resume();
                setTimeout(() => gain.disconnect(), 200);
            } catch(e) { /* 静默降级 */ }
        }
        
        // ---------- 游戏逻辑 ----------
        const materialsDB = {
            wood: { introduced: false, dialog: "木头材质较轻，且质地不硬，非常适合作为造船材料！用它们可以制造结实的木筏，浮力超棒！🪵✨", voiceFile: VOICE_WOOD },
            bamboo: { introduced: false, dialog: "竹子是一种内里空心的材质，可轻松浮于水面，韧性极佳，是造船的绝佳搭档～🎋 漂流必备！", voiceFile: VOICE_BAMBOO },
            stone: { introduced: false, dialog: "石头是比较沉的材料，不会在水中浮起，但可以打磨成石斧、石锤，用来加工木材和防御野兽！🪨💪", voiceFile: VOICE_STONE }
        };
        let allLearned = false, currentKey = null, isDialogOpen = false;
        
        function updateUI() {
            [woodEl, bambooEl, stoneEl].forEach((el, idx) => {
                const key = idx === 0 ? 'wood' : (idx === 1 ? 'bamboo' : 'stone');
                const dot = el.querySelector('.red-dot');
                if (materialsDB[key].introduced) {
                    if (dot) dot.style.display = 'none';
                    el.classList.add('disabled-click');
                } else {
                    if (dot) dot.style.display = 'block';
                    el.classList.remove('disabled-click');
                }
            });
        }
        
        function openDialog(type, msg, voiceFile) {
            if (isDialogOpen) return;
            currentKey = type;
            speechSpan.innerHTML = msg;
            gameContainer.classList.add('beach-dim');
            dimOverlay.classList.add('active');
            robinsonPanel.style.visibility = 'visible';
            robinsonPanel.style.opacity = '1';
            isDialogOpen = true;
            if (voiceFile) playVoice(voiceFile);
            [woodEl, bambooEl, stoneEl].forEach(m => m.style.pointerEvents = 'none');
        }
        
        function closeDialog() {
            if (!isDialogOpen) return;
            stopCurrentVoice();
            if (currentKey) {
                materialsDB[currentKey].introduced = true;
                updateUI();
                const allDone = materialsDB.wood.introduced && materialsDB.bamboo.introduced && materialsDB.stone.introduced;
                if (allDone && !allLearned) {
                    allLearned = true;
                    setTimeout(() => {
                        const toast = document.createElement('div');
                        toast.className = 'complete-badge';
                        toast.innerHTML = '🎉 完美！三种材料特性全掌握！🎉';
                        gameContainer.appendChild(toast);
                        setTimeout(() => toast.remove(), 2800);
                    }, 150);
                }
                currentKey = null;
            }
            gameContainer.classList.remove('beach-dim');
            dimOverlay.classList.remove('active');
            robinsonPanel.style.visibility = 'hidden';
            robinsonPanel.style.opacity = '0';
            isDialogOpen = false;
            [woodEl, bambooEl, stoneEl].forEach(m => m.style.pointerEvents = 'auto');
        }
        
        function onMaterialClick(key, data) {
            if (isDialogOpen) return;
            if (data.introduced) {
                const tip = document.createElement('div');
                tip.innerText = '✨ 你已经了解这种材料啦！试试其他的吧 ✨';
                tip.style.position = 'absolute'; tip.style.bottom = '30%'; tip.style.left = '25%'; tip.style.right = '25%';
                tip.style.background = '#fff0c0'; tip.style.padding = '8px'; tip.style.borderRadius = '50px';
                tip.style.textAlign = 'center'; tip.style.zIndex = '80'; tip.style.border = '1px solid #ffbc6e';
                gameContainer.appendChild(tip);
                setTimeout(() => tip.remove(), 1200);
                return;
            }
            ensureMusicPlaying();
            playClickSound();
            openDialog(key, data.dialog, data.voiceFile);
        }
        
        // 事件绑定
        woodEl.addEventListener('click', (e) => { e.stopPropagation(); onMaterialClick('wood', materialsDB.wood); });
        bambooEl.addEventListener('click', (e) => { e.stopPropagation(); onMaterialClick('bamboo', materialsDB.bamboo); });
        stoneEl.addEventListener('click', (e) => { e.stopPropagation(); onMaterialClick('stone', materialsDB.stone); });
        continueBtn.addEventListener('click', (e) => { e.stopPropagation(); if(isDialogOpen) closeDialog(); });
        dimOverlay.addEventListener('click', () => { if(isDialogOpen) closeDialog(); });
        const bubbleZone = document.querySelector('.speech-bubble-3d');
        if(bubbleZone) bubbleZone.addEventListener('click', (e) => e.stopPropagation());
        musicCtrl.addEventListener('click', (e) => { e.stopPropagation(); toggleMusic(); });
        
        // 首次交互自动播放背景音乐
        function firstInteraction() {
            ensureMusicPlaying();
            document.body.removeEventListener('click', firstInteraction);
            document.body.removeEventListener('touchstart', firstInteraction);
        }
        document.body.addEventListener('click', firstInteraction);
        document.body.addEventListener('touchstart', firstInteraction);
        
        // 初始化
        initBackgroundMusic();
        updateUI();
        gameContainer.classList.remove('beach-dim');
        dimOverlay.classList.remove('active');
        robinsonPanel.style.visibility = 'hidden';
        robinsonPanel.style.opacity = '0';
        isDialogOpen = false;
    })();
</script>
</body>
</html>
