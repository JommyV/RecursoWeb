<script>
    import { onMount } from "svelte";
    import { fade } from 'svelte/transition';
    import { gameStarted } from '$lib/stores.js';
    
  
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
    let videos = [
        "/videos/botblast.mp4",
        "/videos/jumpyufo.mp4",
        "/videos/7differences.mp4",
        "/videos/punchlineinc.mp4",
        "/videos/earforceone.mp4",
        "/videos/skellyboyadventures.mp4",
        "/videos/redherring.mp4"
    ];
    // Unused links from previous idea that sent the user to the game page. Left here for possible future use.
    // let links = [
    //     "/botblast",
    //     "/redherring",
    //     "/jumpyufo",
    //     "/7differences",
    //     "/punchlineinc",
    //     "/earforceone",
    //     "/skellyboy"
    // ];

    let descriptions = [
        "Blast and go Fast.",
        "Flappy Birds, but in SPACE!",
        "A puzzle game where you find 7 differences between two images.",
        "Fight for the funniest words",
        "Mr. President is under attack! Help him avoid the bullets and save his ears.",
        "Navigate a gauntlet of enemies and escape purgatory",
        "Strange Killings in a haunted mansions, can you solve the mistery?"
    ];
    let grades= [
        20.0,
        19.8,
        19.0,
        16.0,
        18.0,
        17.0,
        11.0
    ];

        
    let currentChoices = []; 
    let correctIndex;
    let message = "Select a game";
    let indices = [];
    let shakingButtonIndex = null;
    let buttonsDisabled = false;
    let highscore = 0;
    let showVideo = [false, false]; 
    let hasStarted = false;
    let showInfo = true; 
    let showOption = false;
    let messagePulse = false;
  
    function newRound() {
      indices = []; // Reset indices for new round
      // Pick two distinct random indices
      while (indices.length < 2) {
      let rand = Math.floor(Math.random() * options.length);
      if (!indices.includes(rand)) {
        indices.push(rand);
      }
    }

    currentChoices = [...indices];
    showVideo = [true, false]; // ativa o vídeo para ambos os botões
    message = "Videos playing, please wait...";
    buttonsDisabled = true; 
    messagePulse = true;

  }
  
    function pick(index) {
      if (buttonsDisabled) return;
      buttonsDisabled = true;

      const pickedIndex = currentChoices[index];
      const otherIndex = currentChoices[1 - index];
      if (grades[pickedIndex] > grades[otherIndex]) {
        message = "✅ Correct! Well done! " + options[pickedIndex] + " has a higher grade (" + grades[pickedIndex] + ")" + " than " + options[otherIndex] + " (" + grades[otherIndex] + ")";
        highscore++;
      } else {
        message = "❌ Wrong! Try again!";
        shakingButtonIndex = index;
        setTimeout(() => {
      shakingButtonIndex = null; 
        }, 500);
        currentChoices = [];
      }

      // Esconde os botões
      

      setTimeout(() => {
      newRound(); // sempre chamada após a escolha
      // buttonsDisabled = false; // reabilita os botões após 2 segundos
      }, 2000);
      }

  
    onMount(() => {
      newRound();
    });
    
</script>
      
    <div class="game-container max-w-screen-lg mx-auto px-4">
      <div class= "flex justify-center">
        <h2 style="font-family: 'Press Start 2P', 'VT323', 'Orbitron', 'Anton', monospace; letter-spacing: 2px; font-size: 1 rem; text-shadow: 0 2px 8px #fffff, 0 0px 2px #00ffe7;" class= "text-center w-[50vw] pb-10 {messagePulse ? 'animate-pulse' : ''}">
          {#if (showOption)} 
          {message} 
          {/if}
        </h2>
    </div>
      {#if !hasStarted}
  <div class="flex justify-center mt-10">
    <button
      class="bg-green-600 hover:bg-green-700 text-white font-bold py-3 px-6 rounded-lg text-xl transition-all" id="start-button"
      on:click={() => { hasStarted = true; gameStarted.set(true); newRound(); showOption = true; } }
    >
      🎮 Start Game
    </button>
  </div> 
{:else}
      <div class="buttons flex flex-wrap justify-center gap-6 px-4">
        {#each currentChoices as idx, localIndex(idx)}
          <div in:fade={{ delay: 300, duration: 400 }} out:fade class= "flex flex-col items-center flex-wrap" >
            
              <button
                type="button"
                class={`w-full max-w-xs bg-gray-700 text-white rounded-lg p-4 shadow-[4px_4px_0_0_#1f2937] hover:shadow-[6px_6px_0_0_#1f2937] active:translate-x-[2px] active:translate-y-[2px] active:shadow-[2px_2px_0_0_#1f2937] transition-all duration-150 ease-in-out disabled:opacity-50 disabled:pointer-events-none ${shakingButtonIndex === localIndex ? 'shake' : ''}`}

                on:click={() => pick(localIndex)}
                
                >
                {#if showVideo[localIndex]}
                <video 
                  src={videos[idx]}
                  autoplay
                  playsinline
                  controls
                  volume="0,6"
                  class="w-full max-w-sm rounded mb-2"
                  on:ended={() => { if (localIndex === 0) showVideo = [false, true]; else {buttonsDisabled = false; message = "Choose a game!" ; messagePulse=false;} } }
                ></video>
              {:else}
                <img src={images[idx]} alt={options[idx]} class="mb-2 w-full max-w-sm" />
              {/if}
                <div class="text-black">{options[idx]}</div>
              </button>
            
            <div class="pt-2 w-100 text-center flex flex-row justify-center items-center">
              <p class="text-xs sm:text-sm md:text-base lg:text-lg xl:text-xl" style="font-family: 'Press Start 2P', 'VT323', 'Orbitron', 'Anton', monospace; text-shadow: 0 2px 8px #fffff, 0 0px 2px #00ffe7;">
              {descriptions[idx]}
              </p>
              {#if idx === 0 }
              <a href="/botblast" class="text-blue-400 hover:underline justify-right text-xs">[1]</a>
              {/if}
            </div>
          </div> 
        {/each}
      </div>
      <div class = "flex absolute-50 justify-center pt-10 pb-10 "><p style="font-family: 'Press Start 2P', 'VT323', 'Orbitron', 'Anton', monospace; letter-spacing: 2px; font-size: 1 rem; text-shadow: 0 2px 8px #fffff, 0 0px 2px #00ffe7;">Score: {highscore}</p>
        <link href="https://fonts.googleapis.com/css2?family=Press+Start+2P&family=VT323&family=Orbitron:wght@700&display=swap" rel="stylesheet"> </div> 
        {/if}
      </div>


  <style>
    .game-container {
      text-align: center;
      margin-top: 50px;
      color: azure;
    }
  
    /* .buttons {
      display: flex;
      justify-content: center;
      gap: 20px;
      margin-top: 20px;
    } */
  
    button {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 1rem;
    font-size: 1.2rem;
    cursor: pointer;
    border: 2px solid #333;
    border-radius: 8px;
    background-color: #711212;
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

  #start-button{
    background-color: #32d347;
    color: white;
    font-size: 1.5rem;
    padding: 1rem 2rem;
    border-radius: 0.375rem;
    cursor: pointer;
    transition: background-color 0.3s ease;
   }

   @keyframes shake {
    0% { transform: translateX(0); }
    20% { transform: translateX(-6px); }
    40% { transform: translateX(6px); }
    60% { transform: translateX(-4px); }
    80% { transform: translateX(4px); }
    100% { transform: translateX(0); }
  }

  .shake {
    animation: shake 0.5s ease;
  }
  </style>