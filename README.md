# Car-game
The project is about to develop a small car game. In this game the player has tendency to move his car to and fro, left and right on the road , and the enemy cars are coming from the oppisite directions at different positions and if the player car get in touch with the enemy car then the game will be over and the final score of the player will be shown on the screen. Again if the player wants to start a new game then he can start. The project is made by using javascript language.

<!DOCTYPE html>
<html>
<head>
<title></title>
<style>
	*{ margin: 0 ; padding: 0; }
	.hide { display: none; }
	.car , .enemy{
		width: 50px; height: 70px; background: red; position: absolute; bottom:120px;
      background-image: url('car2.png');
      background-repeat: no-repeat;
      background-size: 90% 90%;

	}
	.carGame
	{
		width:100%;
		height:100vh;
		background-image: url('atul.png');
		background-repeat: no-repeat;
		background-size: 100% 100%;
	}
	.score{
		 position: absolute;
		 top:15px;
		 left:40px;
		 background: #10ac84;
		 width:300px;
		 
		 line-height:70px;
		 text-align: center;
         color:white;
         font-size: 1.5em;
         font-family:fantasy;
         box-shadow: 0 5px 5px #777;
	}
	.startscreen
	{
		position: absolute;
		background-color: #ee5253;
		left:50%;
		top:50%;
		transform: translate(-50%, -50%);
		color:white;
		z-index:1;
		text-align:center;
		border: 1px solid #ff6b6d;
		padding: 15px;
		margin: auto;
		width: 50%;
		cursor: pointer;
		letter-spacing:5;
		font-size:20px;
		word-spacing:3;
		line-height:30px;
		text-transform: uppercase;
         box-shadow:0 5px 5px #777;
	}
	.lines
	{
		width:10px;
		height:100px;
		background:white;
		position:absolute;
		margin:auto;
		margin-left:195px;
	}
	.gameArea{
		 width:400px;
		 height:100vh;
		 background: #2c3e50;
		  margin: auto;
		  position :relative;
		  overflow:hidden;
      border-right: 7px dashed #c8d6e5;
      border-left:7px dashed #c8d6e5;
	}
</style>
</head>
	<body>
 <div class="carGame">
 	<div class="score"></div>
 		<div class="startscreen">
 			<p> Press here to start <br>
 				Arrow keys to move <br>
 			If you hit another you will lose.
 		     </p>
 			</div>	
 			 <div class="gameArea"></div>
            </div>
 		<script>

      
       const score=document.querySelector('.score');
       const startscreen=document.querySelector('.startscreen');
       const gameArea=document.querySelector('.gameArea');
       console.log(gameArea);
        document.addEventListener('keydown', keyDown);
         document.addEventListener('keyup', keyUp);
          
           let player={ speed : 10, score:0}
          startscreen.addEventListener('click', start);
          let keys={Arrowup: false, ArrowDown: false, ArrowLeft: false, ArrowRight: false}
          function  keyDown(e)
          {
          	e.preventDefault();
          	keys[e.key]=true;
          	//console.log(e.key);//
          //	console.log(keys);//
          }
             function  keyUp(e)
          {
          	e.preventDefault();
          	keys[e.key]=false;
          //	console.log(e.key);//
          	//console.log(keys);//
          }
     
      function isCollide(a,b)
      {
      	aRect=a.getBoundingClientRect();
        bRect=b.getBoundingClientRect();
        return !((aRect.bottom < bRect.top)||(aRect.top > bRect.bottom)||(aRect.right < bRect.left)||(aRect.left> bRect.right))
      }
  function moveLines()
  {
  	let lines=document.querySelectorAll('.lines');
  	lines.forEach(function(item)
  	{
  		if(item.y>=700)
  		{
  			item.y-=750
  		}
  		item.y+=player.speed;
  		  item.style.top=item.y + "px";
  	}
  	)
  }
 
 function endGame()
 {
 	player.start=false;
 	 startscreen.classList.remove('hide');
   startscreen.innerHTML="Game Over <br> Your final score is " + player.score + "<br>  Press here to restart the game";
 }
   function moveEnemy(car)
  {
  	let enemy=document.querySelectorAll('.enemy');
  	enemy.forEach(function(item)
  	{
  		 if(isCollide(car, item))
  		 {
  		 	console.log("boom hit");
  		 	 endGame();

  		 }
  		if(item.y>=750)
  		{
  			item.y=-300;
  			 item.style.left=Math.floor(Math.random() * 350 + "px")
  		}
  		item.y+=player.speed;
  		  item.style.top=item.y + "px";
  	}
  	)
  }
  
        function gamePlay()
          { 
          	console.log("hey i am clicked");//
          	let car=document.querySelector('.car');
          	let road=gameArea.getBoundingClientRect();
          	  if(player.start)
          	  {
          	  	  moveLines();
          	  	  moveEnemy(car);
          	  	if(keys.ArrowUp && player.y>(road.top+70))
          	  	{
          	  		player.y-=player.speed;
          	  	}
          	  		if(keys.ArrowLeft && player.x>0)
          	  		{
          	  		player.x-=player.speed;
          	  	}
          	  		{	if(keys.ArrowDown && player.y<(road.bottom-70))
          	  		player.y+=player.speed;
          	  	}
          	  		{	if(keys.ArrowRight && player.x< (road.width-60))
          	  		player.x+=player.speed;
          	  	}
          	  	car.style.top=player.y + "px";
          	  	car.style.left=player.x + "px";
          	 window.requestAnimationFrame(gamePlay);
          	  player.score++;
              let ps=player.score-1;
          	  score.innerText= "score:" + ps;
                }
          }
   function start()
   {
   	 
   	   //gameArea.classList.remove('hide');
   	   startscreen.classList.add('hide');
   	   gameArea.innerHTML="";
   	   player.start=true;
   	   player.score=0;
   	  window.requestAnimationFrame(gamePlay);
    
     for(x=0;x<5;x++)
     {
   	  let roadline=document.createElement('div');
   	      roadline.setAttribute('class', 'lines');
   	      roadline.y=(x*150);
   	      roadline.style.top=roadline.y + "px";
          gameArea.appendChild(roadline);
      }       
   	   let car=document.createElement('div');
   	   car.setAttribute('class', 'car');
   	   gameArea.appendChild(car);
   	   player.x=car.offsetLeft;
   	   player.y=car.offsetTop;
   	   for(x=0;x<6;x++)
     {
   	  let enemyCar=document.createElement('div');
   	      enemyCar.setAttribute('class', 'enemy');
   	      enemyCar.y=((x+1) * 350)* -1;
   	      enemyCar.style.top=enemyCar.y + "px";
   	      enemyCar.style.backgroundColor='blue';
   	      enemyCar.style.left=Math.floor(Math.random() * 350) + "px" ;
          gameArea.appendChild(enemyCar);
      }      
   	  
   }
   </script>
    </body>
       </html>
