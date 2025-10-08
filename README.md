<!DOCTYPE html>
<html lang="id">

<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <!-- Font modern untuk teks biasa -->
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap" rel="stylesheet">

  <style>
    body {
      margin: 0;
      padding: 0;
      background: linear-gradient(135deg, #8ab6d6, #ffffff);
      font-family: 'Poppins', sans-serif;
      color: #1a2540;
      overflow: hidden;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      flex-direction: column;
    }

    .card {
      position: relative;
      background: #fff8f0 url('https://i.imgur.com/mB5Gvws.png') center/cover no-repeat;
      border: 3px solid #c6a664;
      border-radius: 25px;
      padding: 30px;
      width: 90%;
      max-width: 520px;
      text-align: center;
      box-shadow: 0 12px 30px rgba(0, 0, 0, 0.2);
      transform: scale(0);
      opacity: 0;
      transition: all 1s ease;
      z-index: 1;
    }

    .card.open {
      transform: scale(1);
      opacity: 1;
    }

    .card::before {
      content: "";
      position: absolute;
      top: 15px;
      left: 15px;
      right: 15px;
      bottom: 15px;
      border: 2px dashed #b5975a;
      border-radius: 20px;
      pointer-events: none;
    }

    .msg {
      background: rgba(255, 255, 255, 0.9);
      border-radius: 15px;
      padding: 20px 25px;
      margin: 10px 0;
      text-align: left;
      box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
      font-size: 1.3rem;
      animation: fadeIn 1s ease;
      z-index: 2;
    }

    @keyframes fadeIn {
      from {
        opacity: 0;
        transform: translateY(10px);
      }

      to {
        opacity: 1;
        transform: translateY(0);
      }
    }

    .btn {
      background: linear-gradient(90deg, #355c7d, #6c5b7b, #c06c84);
      color: white;
      border: none;
      border-radius: 15px;
      padding: 12px 30px;
      cursor: pointer;
      font-size: 1.3rem;
      margin-top: 20px;
      transition: 0.3s;
      z-index: 3;
    }

    .btn:hover {
      transform: scale(1.07);
      filter: brightness(1.1);
    }

    .letter {
      background: url('https://i.imgur.com/VzH4Wng.png');
      background-size: cover;
      border: 2px solid #caa56a;
      border-radius: 20px;
      padding: 30px 25px;
      color: #3b2a17;
      font-size: 1.2rem;
      line-height: 1.9;
      box-shadow: inset 0 0 12px rgba(0, 0, 0, 0.08);
      animation: unfold 1.3s ease forwards;
      transform: scaleY(0);
      transform-origin: top;
      overflow-y: auto;
      max-height: 50vh;
    }

    @keyframes unfold {
      from {
        transform: scaleY(0);
        opacity: 0;
      }

      to {
        transform: scaleY(1);
        opacity: 1;
      }
    }

    .emoji {
      position: absolute;
      font-size: 2rem;
      animation: float 4s ease-in-out infinite;
      opacity: 0.9;
      z-index: 0;
    }

    @keyframes float {
      0% {
        transform: translateY(0) rotate(0deg);
        opacity: 1;
      }

      100% {
        transform: translateY(-120vh) rotate(720deg);
        opacity: 0;
      }
    }

    .confetti {
      position: fixed;
      width: 8px;
      height: 8px;
      border-radius: 50%;
      top: 0;
      animation: confettiFall 3s linear forwards;
    }

    @keyframes confettiFall {
      to {
        transform: translateY(100vh) rotate(720deg);
        opacity: 0;
      }
    }

    .candle {
      position: relative;
      width: 25px;
      height: 80px;
      background: #ffd6d6;
      margin: 35px auto 10px;
      border-radius: 6px;
      box-shadow: inset 0 0 12px rgba(255, 255, 255, 0.7);
    }

    .flame {
      position: absolute;
      top: -28px;
      left: 50%;
      transform: translateX(-50%);
      width: 22px;
      height: 28px;
      background: radial-gradient(circle, #fff176 20%, #ff9800 70%);
      border-radius: 50%;
      animation: flicker 0.2s infinite alternate;
      box-shadow: 0 0 30px 10px rgba(255, 193, 7, 0.5);
    }

    @keyframes flicker {
      from {
        transform: translateX(-50%) scale(1);
        opacity: 1;
      }

      to {
        transform: translateX(-50%) scale(0.9);
        opacity: 0.8;
      }
    }

    .flame.out {
      animation: fadeout 1.5s forwards;
    }

    @keyframes fadeout {
      to {
        opacity: 0;
        transform: scale(0);
        box-shadow: none;
      }
    }

    /* Embed Spotify */
    .spotify-embed {
      margin-top: 20px;
      border-radius: 12px;
      overflow: hidden;
      width: 100%;
      max-width: 500px;
      height: 80px;
    }
  </style>
</head>

<body>

  <div class="card" id="giftCard">
    <div class="msg" id="message">oy lalap 💙</div>
    <div id="candleArea"></div>
    <button class="btn" id="nextBtn">Klik 🎁</button>

    <!-- Embed Spotify -->
    <div class="spotify-embed">
      <iframe src="https://open.spotify.com/embed/track/5TpPSTItCwtZ8Sltr3vdzm" width="100%" height="80" frameborder="0"
        allowtransparency="true" allow="encrypted-media"></iframe>
      width="100%"
      height="80"
      frameborder="0"
      allowtransparency="true"
      allow="encrypted-media">
      </iframe>
    </div>
  </div>

  <script>
    const messages = [
      "oy lalap 💙",
      "coba klik terus 👉",
      "hmm anuu 😳",
      "happy birthday sayang 💙",
      "ciee tambah tua dasar tua awokaokak 😂🤣",
      "💌",
      "i still love you 💙",
      "hehe makasih untuk semuanya 😁",
      "sekarang waktunya tiup lilin 🎂"
    ];

    const emojis = ["💙", "🎉", "💐", "💞", "🎂", "✨", "🎁", "😁"];
    const card = document.getElementById("giftCard");
    const message = document.getElementById("message");
    const button = document.getElementById("nextBtn");
    const candleArea = document.getElementById("candleArea");
    let index = 0;

    window.onload = () => {
      setTimeout(() => card.classList.add("open"), 500);
    };

    function createEmoji() {
      for (let i = 0; i < 6; i++) {
        const emoji = document.createElement("div");
        emoji.classList.add("emoji");
        emoji.innerHTML = emojis[Math.floor(Math.random() * emojis.length)];
        emoji.style.left = Math.random() * 100 + "vw";
        emoji.style.top = "90vh";
        emoji.style.animationDuration = 2 + Math.random() * 3 + "s";
        document.body.appendChild(emoji);
        setTimeout(() => emoji.remove(), 4000);
      }
    }

    function createConfetti() {
      for (let i = 0; i < 100; i++) {
        const confetti = document.createElement("div");
        confetti.classList.add("confetti");
        confetti.style.left = Math.random() * 100 + "vw";
        confetti.style.background = `hsl(${Math.random() * 360}, 80%, 70%)`;
        confetti.style.animationDuration = 2 + Math.random() * 2 + "s";
        document.body.appendChild(confetti);
        setTimeout(() => confetti.remove(), 3000);
      }
    }

    button.addEventListener("click", () => {
      createEmoji();
      createConfetti();
      index++;
      if (index < messages.length) {
        if (messages[index] === "💌") {
          message.innerHTML = `
          <div class="letter">
            Dear lalap,  
            hari ini bkn cuma soal angka yg bertambah, tp jg tentang rasa syukur krna aku masih bs ngucapin selamat ulang tahun buat orang sespesial lalap 🤍  
            Semoga setiap langkahmu penuh cahaya, setiap impianmu mendekat, dan setiap sedihmu cepat terhapus.  
            Aku berdoa smoga km selalu dipertemukan dngn kebahagiaan, orng-orng baik, dan hal-hal kecil yang membuatmu bersyukur setiap hari 👍🏻  
            Makasih untk smua yg lalap ksih ke aku, makasih udah ada di dunia ini, makasih udh selalu perhatian, sederhana tp berarti bgi aku hehe😁.  
            Selamat ulang tahun, sayang. Tetap jadi versi terbaik dari dirimu oke 💙  
          </div>`;
        } else {
          message.textContent = messages[index];
        }
      } else {
        button.style.display = "none";
        candleArea.innerHTML = `
          <div class="candle">
            <div class="flame" id="flame"></div>
          </div>
          <p style="text-align:center;font-weight:600;">Klik apinya buat tiup lilin 🎂</p>
        `;
        const flame = document.getElementById("flame");
        flame.addEventListener("click", () => {
          flame.classList.add("out");
          createConfetti();
          createEmoji();
          message.innerHTML = "Lilin padam ✨<br><br>Selamat ulang tahun sayang 🎂💙<br>Semoga semua doamu dikabulkan dan semesta selalu berpihak padamu 🌙💫";
          setTimeout(() => {
            candleArea.innerHTML = "<p style='text-align:center;'>🌟 Lilin padam dengan indah 🌟</p>";
          }, 1500);
        });
      }
    });
  </script>
</body>

</html>
