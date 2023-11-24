let ball;
let leftPaddle;
let rightPaddle;
let leftScore = 0;
let rightScore = 0;
let ballSpeedX = 5;
let ballSpeedY = 5;

function setup() {
  createCanvas(400, 200);
  ball = new Ball();
  leftPaddle = new Paddle(true);
  rightPaddle = new Paddle(false);
}

function draw() {
  background(0);
  
  ball.update();
  ball.show();
  
  leftPaddle.show();
  rightPaddle.show();
  
  leftPaddle.update();
  rightPaddle.update();
  
  leftPaddle.checkCollision(ball);
  rightPaddle.checkCollision(ball);
  
  // Display scores
  textSize(32);
  fill(255);
  text(leftScore, 100, 30);
  text(rightScore, width - 100, 30);
}

function keyPressed() {
  if (keyCode === UP) {
    rightPaddle.move(-10);
  } else if (keyCode === DOWN) {
    rightPaddle.move(10);
  }
  
  if (keyCode === 87) { // Tecla 'W'
    leftPaddle.move(-10);
  } else if (keyCode === 83) { // Tecla 'S'
    leftPaddle.move(10);
  }
}

function keyReleased() {
  if (keyCode === UP || keyCode === DOWN) {
    rightPaddle.move(0);
  }
  
  if (keyCode === 87 || keyCode === 83) {
    leftPaddle.move(0);
  }
}

class Ball {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
    this.radius = 10;
    this.xSpeed = ballSpeedX;
    this.ySpeed = ballSpeedY;
  }
  
  update() {
    this.x += this.xSpeed;
    this.y += this.ySpeed;
    
    if (this.y < 0 || this.y > height) {
      this.ySpeed *= -1;
    }
    
    if (this.x < 0) {
      rightScore++;
      this.reset();
    } else if (this.x > width) {
      leftScore++;
      this.reset();
    }
  }
  
  reset() {
    this.x = width / 2;
    this.y = height / 2;
    this.xSpeed = ballSpeedX;
    this.ySpeed = ballSpeedY;
  }
  
  show() {
    fill(255);
    ellipse(this.x, this.y, this.radius * 2);
  }
}

class Paddle {
  constructor(isLeft) {
    this.width = 10;
    this.height = 80;
    this.x = isLeft ? 0 : width - this.width;
    this.y = height / 2 - this.height / 2;
    this.ySpeed = 0;
  }
  
  move(step) {
    this.ySpeed = step;
  }
  
  update() {
    this.y += this.ySpeed;
    this.y = constrain(this.y, 0, height - this.height);
  }
  
  show() {
    fill(255);
    rect(this.x, this.y, this.width, this.height);
  }
  
  checkCollision(ball) {
    if (
      ball.x - ball.radius < this.x + this.width &&
      ball.x + ball.radius > this.x &&
      ball.y > this.y &&
      ball.y < this.y + this.height
    ) {
      ball.xSpeed *= -1;
    }
  }
}
