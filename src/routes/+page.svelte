<script>
    import { onMount, onDestroy } from 'svelte';
  
    let score = 0;
    let num1, num2, answer;
    let input = '';
    let fontSize = '6em';
    const ORIGINAL_TIME = 15;
    let time = ORIGINAL_TIME;
    let timer = time;
    let timerInterval;
    let isBlinking = false;
    let isGameOver = false;
  
    const correctSound = new Audio("https://www.freesoundslibrary.com/wp-content/uploads/2018/01/ding-sound-effect.mp3");
    const incorrectSound = new Audio("https://www.freesoundslibrary.com/wp-content/uploads/2018/03/game-show-buzzer-sound-effect.mp3");
    incorrectSound.volume = 0.1;
      
    function generateQuestion() {
      num1 = Math.floor(Math.random() * 90) + 10;
      num2 = Math.floor(Math.random() * 90) + 10;
      answer = num1 + num2;
    }
    
    function handleKeyPress(event) {
      if (isGameOver) {
        if (event.key === 'Enter') {
          score = 0;
          timer = time = ORIGINAL_TIME;
          isGameOver = false;
          generateQuestion();
          timerInterval = setInterval(() => {
            timer -= 0.1;
            if (timer <= 0) {
              clearInterval(timerInterval);
              isGameOver = true;
              alert(`Game Over! Your final score is ${score}`);
            }
          }, 100);
        }
        return;
      }
      if (event.key === 'Enter') {
        if (parseInt(input) === answer) {
          score += 1;
          time -= 0.1;
          timer = time;
          generateQuestion();
          correctSound.play();
        } else {
          score -= 1;
          isBlinking = true;
          setTimeout(() => {
            isBlinking = false;
          }, 500);
          incorrectSound.play();
        }
        input = '';
      } else if (event.key >= '0' && event.key <= '9') {
        input += event.key;
      }
    }
    
    onMount(() => {
      generateQuestion();
      timerInterval = setInterval(() => {
        timer -= 0.1;
        if (timer <= 0) {
          clearInterval(timerInterval);
          isGameOver = true;
          alert(`Game Over! Your final score is ${score}`);
        }
      }, 100);
      window.addEventListener('keydown', handleKeyPress);
    });
    
    onDestroy(() => {
      clearInterval(timerInterval);
      window.removeEventListener('keydown', handleKeyPress);
    });
  </script>
  
  <div style="font-size: {fontSize}; text-align: center; {isBlinking ? 'animation: blink 0.5s linear infinite;' : ''}">
    {num1} + {num2} = {input}
    
    <style>
      @keyframes blink {
        50% {
          color: red;
        }
      }
    </style>
  </div>
  
  <div style="position: absolute; top: 10px; left: 10px; font-size: 2em;">
    Timer: {timer.toFixed(1)}
  </div>
  
  <div style="position: absolute; top: 10px; right: 10px; font-size: 2em;">
    Score: {score}
  </div>
  