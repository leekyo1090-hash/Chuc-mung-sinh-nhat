<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Happy Birthday!</title>
    <style>
        :root {
            --pink-bg: #ffe6ea;
            --pink-main: #ff85a2;
            --pink-dark: #ff5c8a;
        }

        body, html {
            margin: 0; padding: 0;
            height: 100%; width: 100%;
            font-family: 'Arial', sans-serif;
            background-color: var(--pink-bg);
            overflow: hidden;
            display: flex; justify-content: center; align-items: center;
        }

        /* Trái tim bay nền */
        .heart {
            position: absolute;
            color: var(--pink-main);
            animation: fly 5s linear infinite;
            pointer-events: none;
        }
        @keyframes fly {
            0% { transform: translateY(100vh) scale(0.5); opacity: 1; }
            100% { transform: translateY(-10vh) scale(1.5); opacity: 0; }
        }

        .screen { display: none; text-align: center; width: 90%; max-width: 400px; z-index: 10; }
        .active { display: flex; flex-direction: column; align-items: center; }

        /* Màn hình 1 */
        .card { background: white; padding: 30px; border-radius: 20px; box-shadow: 0 10px 30px rgba(0,0,0,0.1); width: 100%; }
        input { width: 100%; padding: 15px; margin-bottom: 20px; border: 2px solid #ffcad4; border-radius: 10px; box-sizing: border-box; }
        button { background: linear-gradient(to right, #ff85a2, #ff5c8a); color: white; border: none; padding: 15px 30px; border-radius: 50px; cursor: pointer; font-size: 16px; width: 100%; }

        /* Màn hình đếm ngược */
        .timer { font-size: 60px; font-weight: bold; color: var(--pink-dark); margin: 20px 0; }

        /* Bao thư */
        .envelope-wrapper { position: relative; width: 300px; height: 200px; background: var(--pink-dark); margin: 50px auto; cursor: pointer; }
        .flap { position: absolute; top: 0; border-left: 150px solid transparent; border-right: 150px solid transparent; border-top: 110px solid var(--pink-main); transform-origin: top; transition: 0.6s ease; z-index: 3; }
        .envelope-wrapper.open .flap { transform: rotateX(180deg); z-index: 1; }
        .letter { position: absolute; bottom: 0; left: 10px; width: 280px; height: 180px; background: white; z-index: 2; transition: 0.8s ease; padding: 20px; box-sizing: border-box; }
        .envelope-wrapper.open .letter { transform: translateY(-130px); height: auto; min-height: 250px; }
        .front { position: absolute; bottom: 0; border-left: 150px solid var(--pink-main); border-right: 150px solid var(--pink-main); border-bottom: 100px solid #ff7096; border-top: 100px solid transparent; z-index: 4; pointer-events: none; }
    </style>
</head>
<body>

    <audio id="bgMusic" src="https://files.catbox.moe/6v73v0.mp3" preload="auto"></audio>

    <div id="screen1" class="screen active">
        <div class="card">
            <h2 style="color: #d63384;">Hôm nay là ngày gì nhỉ?</h2>
            <input type="text" id="nameInput" placeholder="Ví dụ: Sinh nhật em...">
            <button onclick="start()">Nhấn vào đây đi! ✨</button>
        </div>
    </div>

    <div id="screen2" class="screen">
        <h2 style="color: var(--pink-dark)">Happy Birthday To You</h2>
        <div class="timer" id="timer">00:00:05</div>
        <p>Đang chuẩn bị điều bất ngờ...</p>
    </div>

    <div id="screen3" class="screen">
        <div class="envelope-wrapper" id="envelope" onclick="openMe()">
            <div class="flap"></div>
            <div class="front"></div>
            <div class="letter">
                <h4 style="color: var(--pink-dark); margin-top: 0;">Happy Birthday!</h4>
                <div id="typed-text" style="font-size: 14px; line-height: 1.5;"></div>
            </div>
        </div>
        <p style="color: var(--pink-dark); margin-top: 140px;">Chạm vào bao thư để mở nhé! ❤️</p>
    </div>

    <script>
        // Tạo trái tim bay
        setInterval(() => {
            const heart = document.createElement('div');
            heart.classList.add('heart');
            heart.innerHTML = '❤️';
            heart.style.left = Math.random() * 100 + 'vw';
            heart.style.fontSize = (Math.random() * 20 + 10) + 'px';
            document.body.appendChild(heart);
            setTimeout(() => heart.remove(), 5000);
        }, 300);

        function start() {
            document.getElementById('bgMusic').play();
            document.getElementById('screen1').classList.remove('active');
            document.getElementById('screen2').classList.add('active');
            let timeLeft = 5;
            const interval = setInterval(() => {
                timeLeft--;
                document.getElementById('timer').innerText = `00:00:0${timeLeft}`;
                if(timeLeft <= 0) {
                    clearInterval(interval);
                    document.getElementById('screen2').classList.remove('active');
                    document.getElementById('screen3').classList.add('active');
                }
            }, 1000);
        }

        function openMe() {
            document.getElementById('envelope').classList.add('open');
            const text = "Chúc em một ngày sinh nhật thật ý nghĩa, luôn xinh đẹp, rạng rỡ và hạnh phúc bên những người thân yêu. Mong mọi ước mơ của em đều trở thành hiện thực! ❤️";
            let i = 0;
            const target = document.getElementById('typed-text');
            if(target.innerHTML === "") {
                function typing() {
                    if (i < text.length) {
                        target.innerHTML += text.charAt(i);
                        i++;
                        setTimeout(typing, 50);
                    }
                }
                typing();
            }
        }
    </script>
</body>
</html>
