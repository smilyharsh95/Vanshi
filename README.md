<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Hey Vanshi</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      text-align: center;
      background-color: #fff0f5;
      padding: 50px;
    }
    #text {
      font-size: 22px;
      color: #ff1493;
      white-space: pre-wrap;
      min-height: 100px;
    }
    button {
      padding: 12px 24px;
      font-size: 18px;
      margin: 15px;
      border: none;
      border-radius: 10px;
      cursor: pointer;
      background-color: #ff69b4;
      color: white;
    }
    button:hover {
      background-color: #ff1493;
    }
    #noBtn[disabled] {
      background-color: #ccc;
      color: #888;
      cursor: not-allowed;
    }
  </style>
</head>
<body>

  <!-- Hidden audio element (starts after button click) -->
  <audio id="bgMusic" loop>
    <source src="https://www.bensound.com/bensound-music/bensound-love.mp3" type="audio/mpeg">
    Your browser does not support the audio element.
  </audio>

  <div id="app">
    <div id="text"></div>
    <div id="buttons"></div>
  </div>

  <script>
    let step = 0;
    const music = document.getElementById("bgMusic");

    const messages = [
      ["Hey Beautiful Girl 💖", "Tap the Love Button 💘"],
      ["Hello Vanshi 🌸", "I want to ask you something. ®", "💌 Click Here"],
      ["😶‍🌫️ Are you Alone Right Now?"],
      ["Now that we're both alone 😚", "How about we date instead? 💑"],
      ["Yaayy!! 🥳", "Vanshi, to be my partner, hehe 😘", "Mark our special date: 📅 14 Dec 2025"],
      ["🚨 Just be alone for now 😅", "Let me know when you're ready 💖"]
    ];

    function typeMessage(lines, callback) {
      const textBox = document.getElementById("text");
      textBox.innerHTML = '';
      let line = 0;
      let char = 0;

      function typeLine() {
        if (line < lines.length) {
          const currentLine = lines[line];
          if (char <= currentLine.length) {
            textBox.innerHTML = lines.slice(0, line).join('<br>') + '<br>' + currentLine.slice(0, char) + '<span>|</span>';
            char++;
            setTimeout(typeLine, 40);
          } else {
            char = 0;
            line++;
            setTimeout(typeLine, 500);
          }
        } else {
          textBox.innerHTML = lines.join('<br>');
          if (callback) callback();
        }
      }

      typeLine();
    }

    function nextStep() {
      const buttons = document.getElementById("buttons");

      // Start music on first interaction
      if (step === 0) {
        typeMessage(messages[0], () => {
          buttons.innerHTML = `<button onclick="nextStep()">❤️ Love</button>`;
        });
        step++;
      } 
      else if (step === 1) {
        music.play(); // Music starts here after click
        typeMessage(messages[1].slice(0, 2), () => {
          buttons.innerHTML = `<button onclick="nextStep()">💌 ${messages[1][2]}</button>`;
        });
        step++;
      } 
      else if (step === 2) {
        typeMessage(messages[2], () => {
          buttons.innerHTML = `
            <button onclick="nextStep()">Yes 🙋‍♀️</button>
            <button onclick="beAlone()">No 🙅‍♀️</button>
          `;
        });
        step++;
      } 
      else if (step === 3) {
        typeMessage(messages[3], () => {
          buttons.innerHTML = `
            <button onclick="nextStep()">Let's Go 💃</button>
            <button id="noBtn" disabled>No 🙅‍♀️</button>
          `;
        });
        step++;
      } 
      else if (step === 4) {
        typeMessage(messages[4], () => {
          buttons.innerHTML = ``;
        });
      }
    }

    function beAlone() {
      typeMessage(messages[5], () => {
        document.getElementById("buttons").innerHTML = `
          <button onclick="continueAlone()">Ok, now I am alone 😇</button>
        `;
      });
    }

    function continueAlone() {
      step = 3; // start from the part after saying yes
      nextStep();
    }

    // Start automatically
    nextStep();
  </script>

</body>
</html>
