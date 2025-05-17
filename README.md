const canvas = document.getElementById('heartCanvas');
const ctx = canvas.getContext('2d');

canvas.width = window.innerWidth;
canvas.height = window.innerHeight;

function randomNumber(min, max) {
  return Math.floor(Math.random() * (max - min + 1)) + min;
}

function randomColorRGB() {
  return `rgb(${randomNumber(0, 255)}, ${randomNumber(0, 255)}, ${randomNumber(0, 255)})`;
}

class Heart {
  constructor() {
    this.x = canvas.width / 2 + Math.random() * 100 - 50;
    this.y = canvas.height / 2 + Math.random() * 100 - 50;
    this.size = Math.random() * 8 + 2;
    this.color = randomColorRGB();
    this.speedX = Math.random() * 2 - 1;
    this.speedY = Math.random() * -2 - 1;
  }

  update() {
    this.x += this.speedX;
    this.y += this.speedY;
    this.size *= 0.98;
  }

  draw() {
    ctx.fillStyle = this.color;
    ctx.beginPath();
    ctx.moveTo(this.x, this.y);
    ctx.bezierCurveTo(this.x - this.size, this.y - this.size,
                      this.x - this.size, this.y + this.size,
                      this.x, this.y + this.size * 1.5);
    ctx.bezierCurveTo(this.x + this.size, this.y + this.size,
                      this.x + this.size, this.y - this.size,
                      this.x, this.y);
    ctx.fill();
  }
}

let hearts = [];

function animate() {
  ctx.fillStyle = "rgba(0, 0, 0, 0.1)";
  ctx.fillRect(0, 0, canvas.width, canvas.height);
  hearts.push(new Heart());
  hearts.forEach((heart, index) => {
    heart.update();
    heart.draw();
    if (heart.size < 0.5) {
      hearts.splice(index, 1);
    }
  });
  requestAnimationFrame(animate);
}

animate();
