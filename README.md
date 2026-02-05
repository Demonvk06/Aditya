<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>For Unnati ✨</title>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Dancing+Script:wght@700&family=Montserrat:wght@300;400&display=swap');

        :root {
            --primary: #d63384;
            --accent: #e7accf;
            --bg: #fff5f7;
        }

        * { margin: 0; padding: 0; box-sizing: border-box; }

        body {
            background-color: var(--bg);
            background-image: radial-gradient(#e7accf 0.5px, transparent 0.5px);
            background-size: 20px 20px;
            font-family: 'Montserrat', sans-serif;
            color: #444;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            overflow: hidden;
            text-align: center;
        }

        .main-card {
            background: rgba(255, 255, 255, 0.75);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.5);
            padding: 30px 20px;
            border-radius: 30px;
            box-shadow: 0 20px 50px rgba(0,0,0,0.05);
            width: 90%;
            max-width: 450px;
            z-index: 10;
        }

        h1 { font-family: 'Dancing Script', cursive; font-size: 2.5rem; color: var(--primary); }
        .timer { font-size: 2rem; font-weight: bold; margin: 15px 0; color: #333; }

        .section { display: none; }
        .active { display: block; animation: zoomIn 0.5s ease forwards; }

        @keyframes zoomIn { from { opacity: 0; transform: scale(0.9); } to { opacity: 1; transform: scale(1); } }

        /* --- Improved Gift Game --- */
        #game-canvas {
            height: 280px;
            width: 100%;
            background: rgba(255,255,255,0.4);
            border-radius: 20px;
            margin: 15px 0;
            position: relative;
            overflow: hidden;
            border: 2px dashed var(--accent);
        }
        #gift {
            font-size: 3rem;
            position: absolute;
            cursor: pointer;
            transition: 0.1s linear;
            user-select: none;
            left: 42%; top: 40%;
            z-index: 5;
        }

        /* --- The Letter Reveal --- */
        .letter-popup {
            background: white;
            padding: 20px;
            border-radius: 15px;
            border-left: 5px solid var(--primary);
            text-align: left;
            margin: 15px 0;
            font-size: 0.9rem;
            line-height: 1.6;
            display: none;
            box-shadow: 0 10px 20px rgba(0,0,0,0.05);
        }

        /* --- Buttons --- */
        .btn-container { height: 100px; position: relative; margin-top: 20px; }
        .btn { padding: 12px 25px; border-radius: 50px; border: none; cursor: pointer; font-weight: bold; transition: 0.3s; }
        .btn-yes { background: var(--primary); color: white; }
        .btn-no { background: #555; color: white; position: absolute; }

        .floating {
            position: fixed; bottom: -60px; pointer-events: none;
            animation: rise 5s linear forwards; z-index: 1;
        }
        @keyframes rise { to { transform: translateY(-120vh) rotate(360deg); opacity: 0; } }

        .tease-text { height: 25px; font-size: 0.95rem; color: var(--primary); font-style: italic; margin-bottom: 10px; }
        
        #music-toggle {
            position: fixed; top: 20px; right: 20px;
            background: white; border: none; padding: 10px;
            border-radius: 50%; box-shadow: 0 5px 15px rgba(0,0,0,0.1);
            cursor: pointer; z-index: 100;
        }
    </style>
</head>
<body>

    <button id="music-toggle" onclick="toggleMusic()">🎵</button>
    <audio id="bgm" loop>
        <source src="https://www.chosic.com/wp-content/uploads/2021/05/Everyday-Feels-Like-A-Dream.mp3" type="audio/mpeg">
    </audio>

    <div class="main-card">
        <div id="sec-countdown" class="section active">
            <h1 onclick="handleCheat()">Dear Unnati...</h1>
            <div id="timer" class="timer">00:00:00:00</div>
            <p style="font-size: 0.8rem; letter-spacing: 2px;">FEBRUARY 6, 2026</p>
            <p style="font-size: 0.6rem; color: #ccc; margin-top: 20px;">(Hint: Tap heading 3x to skip)</p>
        </div>

        <div id="sec-game" class="section">
            <h1>It's Your Day! 🎂</h1>
            <p class="tease-text" id="game-msg">Catch it 7 times to unlock! (0/7)</p>
            <div id="game-canvas">
                <div id="gift" onclick="handleGiftClick()">🎁</div>
            </div>
            <div id="letter" class="letter-popup">
                <strong>A Little Note... 💌</strong><br>
                ✨ Effortlessly elegant — every single time.<br>
                ✨ Some people glow, you redefine it.<br>
                ✨ Proof that beauty doesn't need to try.<br>
                ✨ You aren't just 20; you're a masterpiece.
                <button class="btn btn-yes" style="margin-top:15px; width:100%" onclick="changeSection('sec-game', 'sec-truth')">Next Surprise →</button>
            </div>
        </div>

        <div id="sec-truth" class="section">
            <h1>Honest Answer? 😏</h1>
            <p id="truth-q" style="margin: 15px 0;">Are you the most gorgeous person alive?</p>
            <div class="btn-container">
                <button class="btn btn-yes" onclick="showFinal()">Obviously YES ✨</button>
                <button class="btn btn-no" id="noBtn" onmouseover="dodgeNo()">Actually NO 🤡</button>
            </div>
        </div>

        <div id="sec-final" class="section">
            <h1 style="font-size: 3rem;">💖</h1>
            <p style="line-height: 1.8; margin-bottom: 20px;">
                <strong>Happy Birthday Unnati!</strong><br>
                May your 20s be as beautiful and unforgettable as you are.
            </p>
            <button class="btn btn-yes" onclick="spawnDecor('🧸')">Send a Hug 🧸</button>
        </div>
    </div>

    <script>
        const bgm = document.getElementById('bgm');
        function toggleMusic() {
            return bgm.paused ? bgm.play() : bgm.pause();
        }

        // 1. Countdown Logic
        const bdayDate = new Date("February 6, 2026 00:00:00").getTime();
        const countInterval = setInterval(() => {
            const now = new Date().getTime();
            const diff = bdayDate - now;
            if (diff <= 0) { clearInterval(countInterval); unlockBirthday(); }
            else {
                const d = Math.floor(diff / (1000 * 60 * 60 * 24));
                const h = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
                const m = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60));
                const s = Math.floor((diff % (1000 * 60)) / 1000);
                document.getElementById("timer").innerText = `${d}d ${h}h ${m}m ${s}s`;
            }
        }, 1000);

        // Cheat Code
        let cCount = 0;
        function handleCheat() { if(++cCount >= 3) unlockBirthday(); }

        function unlockBirthday() {
            changeSection('sec-countdown', 'sec-game');
            spawnDecor('🎈');
            bgm.play();
        }

        function changeSection(oldId, newId) {
            document.getElementById(oldId).classList.remove('active');
            document.getElementById(newId).classList.add('active');
        }

        // 2. Advanced Gift Game
        let giftClicks = 0;
        const gift = document.getElementById('gift');
        const gameMsg = document.getElementById('game-msg');
        const gTeases = [
            "Wait, you're actually slow? 😂",
            "Missed me! 🏃‍♀️",
            "Are you even trying, Unnati? 💅",
            "Nice try, grandpa! 👵",
            "You click like an aunty! 🤭",
            "Almost there, don't give up! 😜",
            "Is your screen lagging or you? 😏"
        ];

        function handleGiftClick() {
            giftClicks++;
            if (giftClicks < 7) {
                moveGift();
                gameMsg.innerText = gTeases[Math.floor(Math.random()*gTeases.length)] + ` (${giftClicks}/7)`;
                // Make it harder
                gift.style.transition = `${0.15 - (giftClicks * 0.01)}s`; 
            } else {
                gift.style.display = 'none';
                gameMsg.innerText = "Wow, you actually did it! 🎉";
                document.getElementById('letter').style.display = 'block';
                spawnDecor('✨');
            }
        }

        function moveGift() {
            const canvas = document.getElementById('game-canvas');
            const x = Math.random() * (canvas.clientWidth - 70);
            const y = Math.random() * (canvas.clientHeight - 70);
            gift.style.left = x + 'px';
            gift.style.top = y + 'px';
        }

        // 3. Dodge No Button
        const noBtn = document.getElementById('noBtn');
        const truthQ = document.getElementById('truth-q');
        const noTeases = ["Wrong choice! 🧐", "Liar! 🔥", "Still trying? 😂", "Error 404: No not found 🤖"];

        function dodgeNo() {
            noBtn.style.left = Math.random() * 75 + '%';
            noBtn.style.top = Math.random() * 75 + '%';
            truthQ.innerText = noTeases[Math.floor(Math.random()*noTeases.length)];
        }

        function showFinal() {
            changeSection('sec-truth', 'sec-final');
            spawnDecor('❤️');
        }

        function spawnDecor(emoji) {
            for(let i=0; i<30; i++) {
                setTimeout(() => {
                    const d = document.createElement('div');
                    d.className = 'floating';
                    d.innerText = emoji;
                    d.style.left = Math.random() * 100 + 'vw';
                    d.style.fontSize = Math.random() * 20 + 20 + 'px';
                    document.body.appendChild(d);
                    setTimeout(() => d.remove(), 5000);
                }, i * 100);
            }
        }
    </script>
</body>
</html>
