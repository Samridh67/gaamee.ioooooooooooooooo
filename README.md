<!DOCTYPE html>
<html>
<head>
  <title>Catch the Stars</title>
  <style>
    body {
      background: #0b0f2b;
      color: white;
      font-family: Arial;
      text-align: center;
    }

    #gameArea {
      width: 400px;
      height: 500px;
      border: 2px solid white;
      margin: auto;
      position: relative;
      overflow: hidden;
      background: linear-gradient(#05091a, #101c4d);
    }

    .star {
      width: 30px;
      height: 30px;
      background: gold;
      border-radius: 50%;
      position: absolute;
      cursor: pointer;
      box-shadow: 0 0 10px yellow;
    }

    #info {
      margin: 10px;
      font-size: 18px;
    }
  </style>
</head>
<body>

  <h1>⭐ Catch the Stars ⭐</h1>

  <div id="info">
    Score: <span id="score">0</span> |
    High Score: <span id="highScore">0</span> |
    Lives: <span id="lives">3</span>
  </div>

  <div id="gameArea"></div>

  <script>
    const gameArea = document.getElementById("gameArea");
    const scoreText = document.getElementById("score");
    const livesText = document.getElementById("lives");
    const highScoreText = document.getElementById("highScore");

    let score = 0;
    let lives = 3;

    // Load high score
    let highScore = localStorage.getItem("highScore") || 0;
    highScoreText.textContent = highScore;

    function createStar() {
      const star = document.createElement("div");
      star.classList.add("star");

      star.style.left = Math.random() * 370 + "px";
      star.style.top = "0px";

      gameArea.appendChild(star);

      let fallSpeed = Math.random() * 3 + 2;

      const fall = setInterval(() => {
        let top = star.offsetTop;
        star.style.top = top + fallSpeed + "px";

        if (top > 470) {
          clearInterval(fall);
          star.remove();
          lives--;
          livesText.textContent = lives;

          if (lives === 0) {
            // Check & save high score
            if (score > highScore) {
              localStorage.setItem("highScore", score);
              alert("🎉 NEW HIGH SCORE: " + score);
            } else {
              alert("Game Over! Your Score: " + score);
            }
            location.reload();
          }
        }
      }, 20);

      star.addEventListener("click", () => {
        clearInterval(fall);
        star.remove();
        score++;
        scoreText.textContent = score;

        // Live high score update
        if (score > highScore) {
          highScore = score;
          highScoreText.textContent = highScore;
        }
      });
    }

    setInterval(createStar, 1000);
  </script>

</body>
</html>
