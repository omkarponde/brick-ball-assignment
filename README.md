# brick-ball-assignment
the p5js code for the brick assignment is as(which is also in sketch.js:






let circle_x, circle_y, circle_dx, circle_dy, dia, radius;
let paddle_x, paddle_y, paddle_l, paddle_w;
let x_length, y_length;
let b=[];
var lives = 3;
var score = 0;
function setup() {
  x_length = 400;
  y_length = 400;
  createCanvas(x_length, y_length);
  circle_x = 200;
  circle_y = 200;
  circle_dx = 3;
  circle_dy = 3;

  paddle_x = 150;
  paddle_y = 350;
  paddle_l = 100;
  paddle_w = dia = 25;
  radius = dia / 2;


  initializeBricks()
  background("black");
  fill("white");
  circle(circle_x, circle_y, dia);
  rect(paddle_x, paddle_y, paddle_l, paddle_w);
  show_bricks();
}

function draw() {
  move_paddle();
  move_ball();
  display();
  textSize(18);
  fill("#fff");
  text("Lives: " + lives, 10, 25);
  text("Score: " + score, width - 100, 25);
}
function move_paddle() {
  if (keyIsDown(LEFT_ARROW) && paddle_x > 0) {
    paddle_x -= 3;
  }
  if (keyIsDown(RIGHT_ARROW) && paddle_x < width - paddle_l) {
    paddle_x += 3;
  }
}
function move_ball() {
  is_brick_hit();
  //deciding the direction of motion of the ball in x.
  if (circle_x >= x_length - radius || circle_x <= radius) {
    circle_dx = -circle_dx;
  }
  //deciding the direction of motion of the ball in y.

  if (
    circle_y >= y_length - radius ||
    circle_y <= radius ||
    (circle_x >= paddle_x &&
      circle_x <= paddle_x + paddle_l &&
      circle_y + radius >= paddle_y)
  ) {
    circle_dy = -circle_dy;
    if(keyIsPressed){
      if(keyIsDown(LEFT_ARROW)){
        circle_dx -= 1;
      }
      else if(keyIsDown(RIGHT_ARROW)){
        circle_dx += 1;
      }
    }
  }
 
  circle_x = circle_x + circle_dx;
  circle_y = circle_y + circle_dy;
  
  if (circle_y + radius >= height) {
    console.log("Out of Bound");
    resetGame();
  }
  
}
function display() {
  background("black");
  fill("red");
  circle(circle_x, circle_y, dia);
  fill("yellow");
  rect(paddle_x, paddle_y, paddle_l, paddle_w);
  show_bricks();
}

class Brick {
  constructor(x, y) {
    this.l = 50;
    this.w = 25;
    this.life = 3;
    this.brick_x = x;
    this.brick_y = y;
  }
  show() {
    if (this.life > 0) {
      if (this.life == 3) {
        fill(255);
      }
      if (this.life == 2) {
        fill(200);
      }
      if (this.life == 1) {
        fill(100);
      }
      rect(this.brick_x, this.brick_y, this.l, this.w);
    }
    fill(255);
  }
  hit_brick() {
    if (this.life > 0) {
      if (
        circle_y + radius >= this.brick_y &&
        circle_y - radius <= this.brick_y + this.w &&
        circle_x >= this.brick_x &&
        circle_x <= this.brick_x + this.l
      ) {
        circle_dy = -circle_dy;
        this.life--;
        score+=10;
      }
      else if (
        circle_y >= this.brick_y &&
        circle_y <= this.brick_y + this.w &&
        circle_x +radius>= this.brick_x &&
        circle_x -radius<= this.brick_x + this.l
      ) {
        circle_dx = -circle_dx;
        this.life--;
        score+=10;
      }
    }
  }
}

function show_bricks() {
 
  for(let i=0;i<b.length;i++)
    {
      b[i].show();
    }
}
function is_brick_hit() {
  
  for(let i=0;i<b.length;i++)
    {
      b[i].hit_brick();;
    }
}
function resetGame() {
  circle_x = width / 2 + 40;
  circle_y = height / 2 - 20;
  circle_dx = random(-3, 3);
  circle_dy = 3;

  // Reset lives and score
  lives--;
  if (lives <= 0) {
    // Game over when lives reach zero
    console.log("Game Over!");
    noLoop();
    initializeBricks();
  }
}
function initializeBricks(){
  var yy = 25;
  for(let i=0;i<5;i++){
    
    var xx = 0;
    for(let j=0;j<8;j++)
      {
        let br = new  Brick(xx,yy);
        b.push(br);
        xx+=50;
      }
    yy+=25;
  }
}
