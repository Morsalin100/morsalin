<html>
<head>
  <title>Retro Flappy Bird - 240×320</title>
  <style>
    body { margin: 0; background: #70c5ce; }
    canvas { display: block; margin: auto; background: #70c5ce; }
  </style>
</head>
<body>
<canvas id="gameCanvas" width="240" height="320"></canvas>
<script>
const canvas = document.getElementById("gameCanvas");
const ctx = canvas.getContext("2d");

const birdSprite = new Image();
birdSprite.src = "https://i.imgur.com/OdL0XPt.png"; // 16x16 pixel bird

let bird = {
  x: 40,
  y: 140,
  width: 16,
  height: 16,
  velocity: 0,
  gravity: 0.4,
  lift: -6,
  update() {
    this.velocity += this.gravity;
    this.y += this.velocity;

    if (this.y + this.height > canvas.height) {
      this.y = canvas.height - this.height;
      this.velocity = 0;
    }
    if (this.y < 0) {
      this.y = 0;
      this.velocity = 0;
    }
  },
  flap() {
    this.velocity = this.lift;
  },
  draw() {
    ctx.drawImage(birdSprite, this.x, this.y, this.width, this.height);
  }
};

let pipes = [];
let frameCount = 0;
let score = 0;

function createPipe() {
  const gap = 60;
  const topPipeHeight = Math.random() * (canvas.height - gap - 40) + 20;
  const pipe = {
    x: canvas.width,
    width: 30,
    top: topPipeHeight,
    bottom: canvas.height - (topPipeHeight + gap)
  };
  pipes.push(pipe);
}

function updatePipes() {
  if (frameCount % 90 === 0) {
    createPipe();
  }

  pipes.forEach((pipe, index) => {
    pipe.x -= 2;

    // Collision detection
    if (
      bird.x < pipe.x + pipe.width &&
      bird.x + bird.width > pipe.x &&
      (bird.y < pipe.top || bird.y + bird.height > canvas.height - pipe.bottom)
    ) {
      alert("Game Over! Your Score: " + score);
      document.location.reload();
    }

    // Increase score when pipe passes bird
    if (pipe.x + pipe.width === bird.x) {
      score++;
    }

    // Remove offscreen pipes
    if (pipe.x + pipe.width < 0) {
      pipes.splice(index, 1);
    }
  });
}

function drawPipe(pipe) {
  ctx.fillStyle = "green";
  ctx.fillRect(pipe.x, 0, pipe.width, pipe.top);
  ctx.fillRect(pipe.x, canvas.height - pipe.bottom, pipe.width, pipe.bottom);
}

function drawScore() {
  ctx.fillStyle = "white";
  ctx.font = "16px monospace";
  ctx.fillText("Score: " + score, 10, 20);
}

document.addEventListener("keydown", (e) => {
  if (e.key === "5") {
    bird.flap();
  }
});

function gameLoop() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);

  bird.update();
  bird.draw();

  updatePipes();
  pipes.forEach(drawPipe);

  drawScore();

  frameCount++;
  requestAnimationFrame(gameLoop);
}

birdSprite.onload = () => {
  gameLoop();
};
</script>
</body>
</html>
