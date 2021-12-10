# bounce_game_javascript.html
bounce game in java script
<html>
  <head>
    <title>Bounce</title>
  </head>
  <body>
    <canvas width="500" height="500" id="ctx" style="border:2px solid #000000"/>
    <script type="text/javascript">

      var ctx = document.getElementById('ctx').getContext('2d');
      var WIDTH = 500;
      var HEIGHT = 500;
      var score, intervalVar, hitcount, running = false;
      ctx.font = '20px Calibri';
      ctx.fillText('click me to restart',250,250);

      var ball = {
        x:0,
        y:0,
        radius:5,
        color:'green',
        spdX:-5,
        spdY:-5
      };

      var base = {
        x:0,
        y:400,
        height:20,
        width:100,
        color:'red',
        pressingLeft:false,
        pressingRight:false,
        lives:3
      };

      document.getElementById('ctx').onmousedown = function(){
        if (running){
          running = false;
          clearInterval(intervalVar);
        }
        startGame();
      }

      document.onkeydown = function(event) {
        if (event.keyCode == 37) {
          base.pressingLeft = true;
          base.pressingRight = false;
        }
        else if (event.keyCode == 39) {
          base.pressingLeft = false;
          base.pressingRight = true;
        }
      }

      document.onkeyup = function(event) {
        if (event.keyCode == 37) {
          base.pressingLeft = false;
        }
        else if (event.keyCode == 39) {
          base.pressingRight = false;
        }
      }
      testCollision = function(base,ball){
        return ((base.x < ball.x + 2*ball.radius) && (ball.x < base.x + base.width) && (base.y < ball.y + 2*ball.radius) && (ball.y < base.y + base.height));

      }


      drawBall = function() {
        ctx.save();
        ctx.fillStyle = ball.color;
        ctx.beginPath();
        ctx.arc(ball.x,ball.y,ball.radius,0,2 * Math.PI);
        ctx.fill();
        ctx.restore();
      }

      drawBase = function() {
        ctx.save();
        ctx.fillStyle = base.color;
        ctx.fillRect(base.x,base.y,base.width,base.height);
        ctx.restore();
      }

      updateBarPosition = function() {
        if (base.pressingLeft) {
          base.x = base.x - 5;
        }
        else if (base.pressingRight) {
          base.x = base.x + 5;
        }
        if (base.x < 0) {
          base.x = 0;
        }
        if (base.x > WIDTH-base.width) {
          base.x = WIDTH-base.width;
        }
      }

      updateBallPosition = function() {
        ball.x += ball.spdX;
        ball.y += ball.spdY;
        if (ball.x > WIDTH || ball.x < 0) {
          hitcount++;
          if(hitcount%50==0){
            if(ball.spdX < 0){
              ball.spdX = -(Mth.abs(ball.spdX)+1);

            }
            else {
              ball.spdX += 1;
            }
          }
          ball.spdX = -ball.spdX;
        }
        if (ball.y < 0) {
          hitcount++;
          if(hitcount%10==0){
            if(ball.spdY < 0){
              ball.spdY = -(Mth.abs(ball.spdY)+1);

            }
            else {
              ball.spdY += 1;
            }
          }
          ball.spdY = -ball.spdY;
        }
        if (ball.y > HEIGHT){
          hitcount++;
          if(hitcount%10==0){
            if(ball.spdY < 0){
              ball.spdY = -(Math.abs(ball.spdY)+1);

            }
            else {
              ball.spdY += 1;
            }
          }
          ball.spdY = -ball.spdY;
          base.lives--;
        }
      }
      isgameover = function() {
        if(base.lives<0 || score == 330){
          clearInterval(intervalVar);
          ctx.fillText('Game over! click to restart',150,250);

        }
      }

      update = function() {
        ctx.clearRect(0,0,WIDTH,HEIGHT);

        drawBall();
        drawBase();
        if(testCollision(base,ball)){
          ball.spdY = -ball.spdY;
          score += 3;
        }

        ctx.fillText("Score: "+score,5,490);
        ctx.fillText("Lives: "+base.lives,430,490);
        isgameover();
        updateBarPosition();
        updateBallPosition();
      }

      startGame = function() {
        base.x = 150;
        ball.x = base.x + 100;
        ball.y = base.y - 100;


        score = 0;
        base.lives = 3;
        hitcount = 0;
        running = true;


        intervalVar = setInterval(update,20);
      }



    </script>
  </body>
</html>
