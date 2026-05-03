<!DOCTYPE html>
<html lang="ja">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Flick Bomb Battle</title>
    <style>
        body { font-family: sans-serif; text-align: center; background: #f0f0f0; margin: 0; padding: 20px; }
        #game-container { background: white; border-radius: 20px; padding: 20px; box-shadow: 0 4px 10px rgba(0,0,0,0.1); }
        #timer { font-size: 48px; color: red; font-weight: bold; }
        #target-word { font-size: 32px; margin: 20px 0; color: #333; }
        input { font-size: 24px; width: 80%; padding: 10px; border-radius: 10px; border: 2px solid #ddd; }
        .player-turn { padding: 10px; margin: 10px; border-radius: 10px; background: #eee; }
        .active { background: #ffeb3b; border: 2px solid #fbc02d; }
    </style>
</head>
<body>

<div id="game-container">
    <h1>Flick Bomb Battle</h1>
    <div id="timer">10.0</div>
    
    <div id="player1" class="player-turn active">プレイヤー1の番</div>
    <div id="target-word">ここに文字が出るよ</div>
    <input type="text" id="type-input" autocomplete="off" placeholder="ここをタップして入力">
    <div id="player2" class="player-turn">プレイヤー2の番</div>
    
    <button id="start-btn" style="margin-top:20px; padding:10px 20px;">ゲーム開始</button>
</div>

<script>
    const words = ["りんご", "すいか", "みかん", "めろん", "いちご", "ぶどう", "バナナ", "スマホ", "フリック", "タイピング"];
    let currentWord = "";
    let timeLeft = 10.0;
    let currentPlayer = 1;
    let gameActive = false;
    let timerId = null;

    const targetDisplay = document.getElementById('target-word');
    const inputField = document.getElementById('type-input');
    const timerDisplay = document.getElementById('timer');
    const startBtn = document.getElementById('start-btn');

    function nextWord() {
        currentWord = words[Math.floor(Math.random() * words.length)];
        targetDisplay.innerText = currentWord;
        inputField.value = "";
    }

    function switchPlayer() {
        currentPlayer = currentPlayer === 1 ? 2 : 1;
        document.getElementById('player1').classList.toggle('active');
        document.getElementById('player2').classList.toggle('active');
        // ターンが回るごとに少し時間を回復させる（ピクタイ風）
        timeLeft = Math.min(timeLeft + 2, 10); 
    }

    inputField.addEventListener('input', () => {
        if (!gameActive) return;
        if (inputField.value === currentWord) {
            switchPlayer();
            nextWord();
        }
    });

    function startGame() {
        gameActive = true;
        timeLeft = 10.0;
        currentPlayer = 1;
        startBtn.style.display = "none";
        inputField.focus();
        nextWord();
        
        timerId = setInterval(() => {
            timeLeft -= 0.1;
            timerDisplay.innerText = timeLeft.toFixed(1);
            if (timeLeft <= 0) {
                clearInterval(timerId);
                gameActive = false;
                alert("ドカーン！ プレイヤー" + currentPlayer + "の負け！");
                startBtn.style.display = "block";
            }
        }, 100);
    }

    startBtn.addEventListener('click', startGame);
</script>
</body>
</html>
# Pict-Respective
