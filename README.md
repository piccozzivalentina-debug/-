<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Be My Valentine? 💕</title>
  <style>
    body {
      background-color: #ffe6f0;
      font-family: Arial, sans-serif;
      text-align: center;
      padding-top: 100px;
      overflow: hidden;
      transition: background-color 0.5s;
    }

    h1 {
      color: #e60073;
      font-size: 3em;
      font-weight: bold;
      transition: color 0.5s;
    }

    button {
      background-color: #ff4da6;
      border: none;
      color: white;
      padding: 15px 30px;
      font-size: 1.2em;
      border-radius: 25px;
      cursor: pointer;
      margin: 10px;
      position: relative;
    }

    button:hover {
      background-color: #cc0066;
    }

    .heart, .floatingHeart, .confetti {
      position: absolute;
      font-size: 24px;
      color: red;
    }

    .floatingHeart {
      animation: floatDown linear infinite;
    }

    @keyframes fly {
      0% { transform: translate(0, 0) scale(1); opacity: 1; }
      100% { transform: translate(var(--x), var(--y)) scale(2); opacity: 0; }
    }

    @keyframes floatDown {
      0% { transform: translateY(-50px); opacity: 1; }
      100% { transform: translateY(110vh); opacity: 0; }
    }

    .confetti {
      font-size: 18px;
      color: hsl(var(--hue), 100%, 50%);
      animation: confettiFly 1s linear forwards;
    }

    @keyframes confettiFly {
      0% { transform: translate(0, 0) rotate(0deg); opacity: 1; }
      100% { transform: translate(var(--x), var(--y)) rotate(360deg); opacity: 0; }
    }

    #cupid, #sadCupid {
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      font-size: 5em;
      display: none;
    }

    #cupidMessage {
      position: fixed;
      top: 70%;
      left: 50%;
      transform: translateX(-50%);
      font-size: 2em;
      color: white;
      display: none;
    }

    @keyframes dance {
      0% { transform: translate(-50%, -50%) rotate(-10deg); }
      100% { transform: translate(-50%, -50%) rotate(10deg); }
    }

    @keyframes sadWalk {
      0% { left: 50%; opacity: 1; }
      100% { left: 110%; opacity: 1; }
    }

    @keyframes shootArrow {
      0% { top: 100%; opacity: 0; }
      50% { top: 50%; opacity: 1; }
      100% { top: 0%; opacity: 0; }
    }

    #arrow {
      position: fixed;
      font-size: 3em;
      display: none;
    }
  </style>
</head>
<body>

  <h1>Will You Be My Valentine? 💕</h1>

  <button id="yesBtn">Yes</button>
  <button id="noBtn">No</button>

  <div id="cupid">💘</div>
  <div id="sadCupid">😢💘</div>
  <div id="cupidMessage">Let's try that again! 💖</div>
  <div id="arrow">🏹</div>

  <script>
    const yesBtn = document.getElementById('yesBtn');
    const noBtn = document.getElementById('noBtn');
    const h1 = document.querySelector('h1');
    const sadCupid = document.getElementById('sadCupid');
    const arrow = document.getElementById('arrow');
    let noAttempts = 0;

    // Spawn floating hearts in background
    function spawnFloatingHeart() {
      const heart = document.createElement('div');
      heart.className = 'floatingHeart';
      heart.textContent = '❤️';
      heart.style.left = Math.random() * window.innerWidth + 'px';
      heart.style.fontSize = (12 + Math.random() * 24) + 'px';
      heart.style.animationDuration = (3 + Math.random() * 5) + 's';
      document.body.appendChild(heart);
      setTimeout(() => heart.remove(), 8000);
    }
    setInterval(spawnFloatingHeart, 500);

    // YES button explosion with confetti
    yesBtn.addEventListener('click', () => {
      for (let i = 0; i < 30; i++) {
        const heart = document.createElement('div');
        heart.className = 'heart';
        heart.textContent = '❤️';
        heart.style.left = yesBtn.offsetLeft + yesBtn.offsetWidth/2 + 'px';
        heart.style.top = yesBtn.offsetTop + yesBtn.offsetHeight/2 + 'px';
        const x = (Math.random() - 0.5) * 400 + 'px';
        const y = (Math.random() - 0.5) * 400 + 'px';
        heart.style.setProperty('--x', x);
        heart.style.setProperty('--y', y);
        document.body.appendChild(heart);
        setTimeout(() => heart.remove(), 1000);

        // Confetti
        const confetti = document.createElement('div');
        confetti.className = 'confetti';
        confetti.textContent = '✨';
        confetti.style.left = yesBtn.offsetLeft + yesBtn.offsetWidth/2 + 'px';
        confetti.style.top = yesBtn.offsetTop + yesBtn.offsetHeight/2 + 'px';
        confetti.style.setProperty('--x', (Math.random() - 0.5) * 500 + 'px');
        confetti.style.setProperty('--y', (Math.random() - 0.5) * 500 + 'px');
        confetti.style.setProperty('--hue', Math.random() * 360);
        document.body.appendChild(confetti);
        setTimeout(() => confetti.remove(), 1000);
      }

      h1.textContent = "You kinda like me, huh? 💕";
    });

    // NO button logic
    noBtn.addEventListener('mousemove', () => {
      noAttempts++;
      if (noAttempts < 3) {
        const x = Math.random() * (window.innerWidth - noBtn.offsetWidth);
        const y = Math.random() * (window.innerHeight - noBtn.offsetHeight);
        noBtn.style.position = 'absolute';
        noBtn.style.left = x + 'px';
        noBtn.style.top = y + 'px';
      } else {
        // 3rd attempt: sad cupid sequence
        document.body.style.backgroundColor = 'black';
        yesBtn.style.display = 'none';
        noBtn.style.display = 'none';
        h1.style.color = 'white';
        sadCupid.style.display = 'block';
        sadCupid.style.left = '50%';
        sadCupid.style.top = '50%';
        sadCupid.style.animation = 'sadWalk 3s forwards';

        // Arrow shoot after cupid walks
        setTimeout(() => {
          arrow.style.display = 'block';
          arrow.style.left = '50%';
          arrow.style.top = '100%';
          arrow.style.animation = 'shootArrow 1s forwards';
        }, 3000);

        // Reset after arrow
        setTimeout(() => {
          arrow.style.display = 'none';
          sadCupid.style.display = 'none';
          yesBtn.style.display = 'inline-block';
          noBtn.style.display = 'inline-block';
          h1.style.color = '#e60073';
          h1.textContent = "Will You Be My Valentine? 💕";
          document.body.style.backgroundColor = '#ffe6f0';
          noAttempts = 0;
        }, 4500);
      }
    });
  </script>

</body>
</html>
