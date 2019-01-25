# cookie-clicker
function setup () {
  createCanvas(600, 400);
}

// Timer variables
var seconds;
var counter;

// Cookie variables
var numCookiesNeeded;
var cookieMovementObj;
var ground;

// Progress and screen variables
var currentScreen;
var level;

//cookie and cookie monster images
var menuBkgd;
var cookieMonster;  
var cookie;
var loserBkgd;
var gameBkgd;

// Font variable
var cookieTapFont;

//instructions' screen background
var chocolateXPos = [];
var chocolateYPos = [];
var chocolateSpeed = 5;

function preload () {
    // Images
    menuBkgd = loadImage("images/menuBkgd.jpg");
    cookieMonster = loadImage("images/cookieMonster.png");
    cookie = loadImage("images/cookie.png");
    cookieMonsterBkgd = loadImage("images/manyCookies.jpg");
    loserBkgd = loadImage("images/loserBkgd.jpg");
    gameBkgd = loadImage("images/gameBkgd.jpg");
    
    // Font
    cookieTapFont = loadFont("Fonts/AldotheApache.ttf");
}

function timer () {
    seconds--;
    if (seconds <= -1 || numCookiesNeeded === 0 || keyCode === 32 || currentScreen === 2) {
        clearInterval(seconds); //clearInterval stops the timer
        return;
    }
    
    /* 1000 miliseconds = 1 second
    setInterval calls a function set in miliseconds */
    
    document.getElementById("timer").innerHTML = "You have " + seconds + " more seconds!"; 
  /*-get the element ID = timer, returns the value that it's been assigned to 
    -.innerHTML sets/returns the html content = the text 
    -<p id= "timer" style="text-align:center"></p>
    -<p> defines a paragraph, id was previously defined
    -style is the style information -> text is centered 
  */ 
}

// Variables used to make the cookie move
cookieMovementObj = {
  xPos: 10,
  yPos: 25,
  xSpeed: 3,
  ySpeed: 1,
  gravity: 0.5, // Meters per second
  airFriction: 0.99,
  cookieClicked: false
};
ground = 375;

currentScreen = 1;
numCookiesNeeded = 5;

var drawMenu = function() { //this screen works
  seconds = 16;
  //numCookiesNeeded = 5;
  
  strokeWeight(10);
  stroke(80, 80, 255);
  background(255);
  image(menuBkgd, 0, 0, 600, 400);
  textSize(65);
  textFont(cookieTapFont);
  fill(255, 255, 255);
  text("COOKIE TAP", 145, 100); 
  
  strokeWeight(3);
  // "Instructions" button
  rect(120, 200, 100, 40, 10, 10); 
  
  // "Start Game" button
  rect(365, 200, 100, 40, 10, 10); 
  
  noStroke();
  fill(0);
  textSize(16);
  text("INSTRUCTIONS", 128, 225);
  text("START GAME", 380, 225); 

};

var drawInstructions = function () { 
  background(0);
  
  for (var i = 0; i < chocolateXPos.length; i++) {
    noStroke();
    fill(0, 200, 255);
    if (chocolateYPos[i] > 390) {
      chocolateYPos[i] = 0;
    }
    image(cookie, chocolateXPos[i], chocolateYPos[i], 20, 20); 
    chocolateYPos[i] += 5;
  }

  stroke(100, 100, 255);
  textSize(60);
  fill(255, 255, 255);
  strokeWeight(7);
  text("INSTRUCTIONS", 140, 75); 
  
  noStroke();
  fill(255);
  textSize(29);
  text("CLICK OR TAP THE COOKIES BEFORE THEY BOUNCE", 35, 130);
  text("OFF THE SCREEN.", 215, 160)

  text("THE LEVELS ARE ENDLESS AND THE GOAL IS", 65, 220);
  text("TO REACH THE HIGHEST LEVEL YOU CAN.", 85, 250);
  
  text("IF THE SPECIFIED AMOUNT OF COOKIES NEEDED IS", 30, 310);
  text("NOT REACHED WITHIN THE TIME LIMIT, YOU LOSE!", 30, 340);
  
  textSize(15);
  text("PRESS ESC TO GO BACK", 225, 365);
  text("CLICK ANYWHERE ON THIS SCREEN TO SEE A MAGICAL BACKGROUND!", 125, 385);


  if (keyIsDown(27)) {  // 27 is the keycode for ESCAPE
    currentScreen = 1;
  }
};

// Timer
counter = setInterval(timer, 1000); 

// Level variable
level = 1;

var drawGame = function () { 
  // Background
  background(0);
  
   
  // Draw cookie
  /* The if and else statements ensure that no cookies are drawn outside of the blue space */
  if (cookieMovementObj.xPos < 350 && cookieMovementObj.xPos > -30 && !cookieMovementObj.cookieClicked && numCookiesNeeded > 0) {
    image(cookie, cookieMovementObj.xPos, cookieMovementObj.yPos, 55, 55); 
        
    /* If the cookie hits the "ground", the y speed of the cookie becomes negative, making it bounce back up */
    if (cookieMovementObj.yPos >= ground) {
      cookieMovementObj.ySpeed *= -1;
    }
    
    cookieMovementObj.yPos += cookieMovementObj.ySpeed;
    cookieMovementObj.ySpeed = (cookieMovementObj.ySpeed + cookieMovementObj.gravity) * cookieMovementObj.airFriction;
    cookieMovementObj.xPos += cookieMovementObj.xSpeed;
    
  } else {
    /* spawn new cookie within the boundaries of the blue area if the one spawned in the if statement is not wthin the blue area */
    cookieMovementObj = {
      xPos: random(55, 345),
      yPos: 20,
      xSpeed: 3,
      ySpeed: 1,
      gravity: 0.5,
      airFriction: 0.98
    };
    // If the cookie is spawned in the righter-most half of the blue are, it bounces left
    if (cookieMovementObj.xPos > 200){
      cookieMovementObj.xSpeed *= -1;
    }
  }

  // text bubble
  noStroke();
  fill(255);
  ellipse(500, 90, 175, 135);
  triangle(510, 135, 545, 140, 515, 190);

  // Cookie Monster image, text inside text bubble and next level button
  image(cookieMonster, 397, 190, 225, 250); 
  fill(0);
  
  fill(255);
  textSize(20);
  text("LEVEL " + level, 173, 35)
  noFill();
  stroke(255);
  strokeWeight(4);
  rect(150, 10, 100, 35, 10, 10);

  if (numCookiesNeeded > 0) {
    fill(0);
    textSize(23);
    text("I NEED " + numCookiesNeeded, 465, 65);
    text("MORE COOKIES", 435, 95);
    text("TO BE FULL!", 455, 125);
  } else {
    fill(0);
    textSize(17);
    text("GREAT JOB, I'M FULL!", 435, 70);
    text("PRESS THE NEXT LEVEL", 430, 95);
    text("BUTTON TO PROCEED!", 435, 120);
    fill(0, 0, 255);
    stroke(255);
    strokeWeight(2);
    rect(150, 75, 100, 50, 10, 10);
    noStroke();
    fill(255, 255, 255);
    text("NEXT LEVEL", 165, 105);
  } 
  
  if (numCookiesNeeded > 0 && seconds === 0) {
    currentScreen = 4;
  }
};

var drawLoserScreen = function () {
  image(loserBkgd, 0, 0, 600, 400);
  fill(255);
  textSize(45);
  noStroke();
  text("YOU STARVED COOKIE MONSTER!!!", 15, 90);
  stroke(255);
  strokeWeight(5);
  fill(0);
  textSize(30);
  text("YOU REACHED LEVEL " + level + ".", 140, 200);
  text("PRESS ESC TO GO BACK TO THE MENU SCREEN", 30, 250);
    
  if (keyIsDown(32)) {  // 32 is the keycode for space bar
    currentScreen = 1;
  }
}; 

function mousePressed () {
  /* If the mouse is clicked within the area of the cookie, the number in the text bubble goes down */
  if (mouseX >= cookieMovementObj.xPos && mouseX <= cookieMovementObj.xPos + 55 && mouseY >= cookieMovementObj.yPos && mouseY <= cookieMovementObj.yPos + 55) {
    cookieMovementObj.cookieClicked = true;
    numCookiesNeeded -= 1;
  }
}

function mouseClicked () {
  if (numCookiesNeeded === 0 && mouseX >= 150 && mouseX <= 250 && mouseY >= 75 && mouseY <= 125 && seconds > 0) {
    level++;
    numCookiesNeeded = level * 5;
    seconds = 15 + (level * 2);
  }
    
  if (currentScreen === 1 && mouseX >= 120 && mouseX <= 220 && mouseY >= 200 && mouseY <= 240) {
    currentScreen = 2;
    
  } else if (currentScreen === 1 && mouseX >= 365 && mouseX <= 465 && mouseY >= 200 && mouseY <= 240) {
    numCookiesNeeded = 5;
    level = 1;
    currentScreen = 3;
    // counter = setInterval(timer, 1000); 
  }
  chocolateXPos.push(random(0,600));
  chocolateYPos.push(random(0,600));
};

var keyPressed = function() {
  // To exit the loser screen and go back to the menu screen
  if (keyCode === 27 && currentScreen === 2 || currentScreen === 4) {
    currentScreen = 1;
  }
};

function draw () {
  // currentScreens
  if (currentScreen === 1) {
    drawMenu();
  }
  if (currentScreen === 2) {
    drawInstructions();
  } 
  if (currentScreen === 3) {
    drawGame();
  }
  if (currentScreen === 4) {
    drawLoserScreen();
  }
}; 

