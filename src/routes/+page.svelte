<script>
    import { onMount, onDestroy } from 'svelte';
  
    let score = 0;
    let num1, num2, answer;
    let input = '';
    let fontSize = '6em';
    let timer = 5;
    let timerInterval;
    let isBlinking = false;
    
    function generateQuestion() {
      num1 = Math.floor(Math.random() * 90) + 10;
      num2 = Math.floor(Math.random() * 90) + 10;
      answer = num1 + num2;
    }
    
    function handleKeyPress(event) {
      if (event.key === 'Enter') {
        if (parseInt(input) === answer) {
          score += 1;
          timer -= 0.1;
          if (timer <= 0) {
            clearInterval(timerInterval);
            alert(`Game Over! Your final score is ${score}`);
          }
        } else {
          score -= 2;
          isBlinking = true;
          setTimeout(() => {
            isBlinking = false;
          }, 500);
        }
        input = '';
        generateQuestion();
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
  