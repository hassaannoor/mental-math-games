<script>
    import { onMount, onDestroy } from 'svelte';
    
    let score = 0;
    let num1, num2, answer;
    let input = '';
    let fontSize = '6em';
    
    function generateQuestion() {
      num1 = Math.floor(Math.random() * 90) + 10;
      num2 = Math.floor(Math.random() * 90) + 10;
      answer = num1 + num2;
    }
    
    function handleKeyPress(event) {
      if (event.key === 'Enter') {
        if (parseInt(input) === answer) {
          score += 1;
        } else {
          score -= 2;
        }
        input = '';
        generateQuestion();
      } else if (event.key >= '0' && event.key <= '9') {
        input += event.key;
      }
    }
    
    onMount(() => {
      generateQuestion();
      window.addEventListener('keydown', handleKeyPress);
    });
    
    onDestroy(() => {
      window.removeEventListener('keydown', handleKeyPress);
    });
  </script>
  
  <div style="font-size: {fontSize}; text-align: center;">
    {num1} + {num2} = {input}
  </div>
  
  <div style="position: absolute; top: 10px; right: 10px; font-size: 2em;">
    Score: {score}
  </div>
  