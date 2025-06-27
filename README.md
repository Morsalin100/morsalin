<!DOCTYPE html>
<html>
<head>
  <title>Flappy Bird for Keypad</title>
  <style>
    body { margin: 0; overflow: hidden; background: skyblue; }
    canvas { display: block; margin: 0 auto; background: #70c5ce; }
  </style>
</head>
<body>
<canvas id="gameCanvas" width="320" height="480"></canvas>

<script>
const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');

let frames = 0;
const DEGREE = Math.PI / 180;

const sprite = new Image();
sprite.src = 'https://i.imgur.com/QXxtrcA.png'; // fallback image

const bird = {
  x: 50,
  y: 150,
  w: 34,
  h: 26,
  gravity: 0.25,
  jump: 4.6,
  speed: 0,
  draw() {
    ctx.fillStyle = 'yellow';
    ctx.fillRect(this.x, this.y, this.w, this.h);
  },
  flap() {
    this.speed = -this.jump;
  },
  update() {
    this.speed += this.gravity;
    this.y += this.speed;
    if (this.y + this.h >= canvas.height) {
      this.y = canvas.height - this.h;
      this.speed = 0;
    }
  }
};

document.addEventListener("keydown", function(evt) {
  if (evt.key === "5") {
    bird.flap();
  }
  if (evt.key === "0") {
    isPaused = !isPaused;
  }
});

let isPaused = false;

function draw() {
  ctx.fillStyle = "#70c5ce";
  ctx.fillRect(0, 0, canvas.width, canvas.height);
  bird.draw();
}

function update() {
  bird.update();
}

function loop() {
  if (!isPaused) {
    update();
    draw();
    frames++;
  }
  requestAnimationFrame(loop);
}

loop();
</script>
</body>
</html>
