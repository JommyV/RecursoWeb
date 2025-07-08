<script>
    import { onMount } from "svelte";
  
    let options = ["BotBlast", "JumpyUFO","7 differences", "Punch Line Inc", "EarForceOne", "Skellyboy Adventures", "Red Herring"];
    
    let images = [
        "/images/botblast.png",
        "/images/jumpyufo.png",
        "/images/7differences.png",
        "/images/punchlineinc.png",
        "/images/earforceone.png",
        "/images/skellyboyadventures.png",
        "/images/redherring.png",

    ];
    let links = [
        "/botblast",
        "/redherring",
        "/jumpyufo",
        "/7differences",
        "/punchlineinc",
        "/earforceone",
        "/skellyboy"
    ];
    let currentChoices = [];  // will hold 2 random indices
    let correctIndex;
    let message = "Select a game";
  
    function newRound() {
    // Pick two distinct random indices
    let indices = [];
    while (indices.length < 2) {
      let rand = Math.floor(Math.random() * options.length);
      if (!indices.includes(rand)) {
        indices.push(rand);
      }
    }

    currentChoices = indices;

    // Randomly select which of the two is correct
    correctIndex = Math.floor(Math.random() * 2);
    message = "Pick an option!";
  }
  
    function pick(index) {
      if (index === correctIndex) {
        message = "✅ Correct! Well done!";
      } else {
        message = "❌ Wrong! Try again!";
      }
      // Start a new round after a short delay
      setTimeout(newRound, 1000);
    }
  
    onMount(() => {
      newRound();
    });
    
</script>
      
    <div class="game-container">
        <h2>{message}</h2>
      
        <div class="buttons">
          {#each currentChoices as idx, localIndex}
          
            <a href={links[idx]}>
              <button type="button" on:click={() => pick(localIndex)}>
                <img src={images[idx]} alt={options[idx]}/>
                <div class= "text-black">{options[idx]}</div>
              </button>
            </a>
          {/each}
        </div>
      </div>
  
  <style>
    .game-container {
      text-align: center;
      margin-top: 50px;
      color: azure;
    }
  
    .buttons {
      display: flex;
      justify-content: center;
      gap: 20px;
      margin-top: 20px;
    }
  
    button {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 1rem;
    font-size: 1.2rem;
    cursor: pointer;
    border: 2px solid #333;
    border-radius: 8px;
    background-color: #eee;
    transition: background-color 0.2s;
    }
  
    button:hover {
      background-color: #ddd;
    }

    button img {
    max-width: 400px;
    max-height: 400px;
    min-height: 230px;
    object-fit: contain;
    margin-bottom: 0.5rem;
    border-radius: 0.375rem;
   }
  </style>