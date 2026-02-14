<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Valentine Card</title> <!-- only in browser tab, won't appear on page -->
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
      transition: all 1s ease;
      position: relative;
      z-index: 10;
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
      z-index: 10;
    }

    button:hover {
      background-color: #cc0066;
    }

    .heart, .floatingHeart, .confetti {
      position: absolute;
      z-index: 5;
    }

    .floatingHeart {
      animation: floatDown linear infinite;
    }

    @keyframes floatDown {
      0% { transform: translateY(-50px); opacity: 1; }
      100% { transform: translateY(110vh); opacity: 0; }
    }

    .confetti {
      font-size: 18px;
      animation: confettiFly 1s linear forwards;
    }

    @keyframes confettiFly {
      0% { transform: translate(0, 0) rotate(0deg); opacity: 1; }
      100% { transform: translate(var(--x), var(--y)) rotate(360deg); opacity: 0; }
    }

    #sadCupid, #arrow {
      position: fixed;
      font-size: 5em;
      display: none;
      z-index: 20;
    }
  </style>
</head>
<body>

  <h1 id="headline">Hey Rob, will you be my Valentine? 💕</h1>

  <button id="yesBtn">Yes</button>
  <button id="noBtn">No</button>

  <div id="sadCupid">😢💘</div>
  <div id="arrow">🏹</div>

  <script>
    const yesBtn = document.getElementById('yesBtn');
    const noBtn = document.getElementById('noBtn');
    const h1 = document.getElementById('headline');
    const sadCupid = document.getElementById('sadCupid');
    const arrow = document.getElementById('arrow');
    let noAttempts = 0;

    // Floating hearts background
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

    // YES button click
    yesBtn.addEventListener('click', () => {
      // hide headline while animation runs
      h1.style.display = 'none';

      // Explode hearts + confetti
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

      // hide buttons
      yesBtn.style.display = 'none';
      noBtn.style.display = 'none';

      // after animation, show the centered text
      setTimeout(() => {
        h1.style.display = 'block';
        h1.textContent = "So you kinda like me huh… sucker! 💕";
        h1.style.fontSize = '6em';
        h1.style.position = 'fixed';
        h1.style.top = '50%';
        h1.style.left = '50%';
        h1.style.transform = 'translate(-50%, -50%)';
        h1.style.zIndex = '20';
      }, 1000);
    });

    // NO button hover
    noBtn.addEventListener('mousemove', () => {
      noAttempts++;
      if (noAttempts < 3) {
        const x = Math.random() * (window.innerWidth - noBtn.offsetWidth);
        const y = Math.random() * (window.innerHeight - noBtn.offsetHeight);
        noBtn.style.position = 'absolute';
        noBtn.style.left = x + 'px';
        noBtn.style.top = y + 'px';
      } else {
        // hide headline during animation
        h1.style.display = 'none';
        document.body.style.backgroundColor = 'black';
        yesBtn.style.display = 'none';
        noBtn.style.display = 'none';

        sadCupid.style.display = 'block';
        sadCupid.style.left = '50%';
        sadCupid.style.top = '50%';
        sadCupid.style.animation = 'sadWalk 3s forwards';

        setTimeout(() => {
          arrow.style.display = 'block';
          arrow.style.left = '50%';
          arrow.style.top = '100%';
          arrow.style.animation = 'shootArrow 1s forwards';
        }, 3000);

        setTimeout(() => {
          arrow.style.display = 'none';
          sadCupid.style.display = 'none';
          yesBtn.style.display = 'inline-block';
          noBtn.style.display = 'inline-block';
          h1.style.display = 'block';
          h1.style.color = '#e60073';
          h1.style.fontSize = '3em';
          h1.style.position = 'relative';
          h1.style.top = '';
          h1.style.left = '';
          h1.style.transform = '';
          h1.textContent = "Hey Rob, will you be my Valentine? 💕";
          document.body.style.backgroundColor = '#ffe6f0';
          noAttempts = 0;
        }, 4500);
      }
    });
  </script>

</body>
</html>
