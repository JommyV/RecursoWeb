

<script>
  import { fade } from 'svelte/transition';
    import TwoChoiceGame from '../lib/options.svelte';
    import { gameStarted } from '$lib/stores.js';

</script>
  
<div class= "flex flex-col">

    <div>
        <h1
        class="text-center text-5xl font-extrabold uppercase tracking-widest text-transparent bg-clip-text bg-gradient-to-r from-white via-gray to-white drop-shadow-lg pt-12 pb-4 animate-pulse"
        style="font-family: 'Press Start 2P', 'VT323', 'Orbitron', 'Anton', monospace; letter-spacing: 2px; font-size: 1 rem; text-shadow: 0 2px 8px #fffff, 0 0px 2px #00ffe7;">
        THE PORTFOLIO CAGE MATCH
        </h1>
        <link href="https://fonts.googleapis.com/css2?family=Press+Start+2P&family=VT323&family=Orbitron:wght@700&display=swap" rel="stylesheet">
    </div>



        <div class="flex justify-center items-center pt-10 text-white flex-col">
            <h2 style="font-family: 'Press Start 2P', 'VT323', 'Orbitron', 'Anton', monospace; letter-spacing: 2px; font-size: 1 rem; text-shadow: 0 2px 8px #fffff, 0 0px 2px #00ffe7;" class= "text-center w-[50vw]">
                This is a game about games, learn about them and guess which had a higher grade 
        </h2>
            {#if !$gameStarted}
        
                <h2
                    style="font-family: 'Press Start 2P', 'VT323', 'Orbitron', 'Anton', monospace; letter-spacing: 2px; font-size: 1 rem; text-shadow: 0 2px 8px #fffff, 0 0px 2px #00ffe7;"
                    class="text-center w-[50vw] pt-10"
                    out:fade
                >
                    Videos autoplay and buttons become clickable after both videos end
                </h2>
            {/if}
      </div>

    <TwoChoiceGame />
</div>

<!-- <p class="text-white text-right ">
    This is a sentence with a footnote.<sup><a href="#footnote-1" class="text-blue-400 hover:underline justify-right">[1]</a></sup>
  </p> -->




<style>
    :global(body) {
        background: linear-gradient(180deg, #000000 0%, #ab4545 100%);
        min-height: 100vh;
        margin: 0;
    }

    
    :global(body) {
         
        min-height: 100vh;
        margin: 0;
    }
        
    :global(.animate-ping-slow) {
            animation: ping 1s cubic-bezier(0, 0, 0.2, 1) infinite;
            animation-iteration-count: 3;
            
        }
        @keyframes ping {
            50% {
                transform: scale(1.5);
                opacity: 0;
            }
            100% {
                transform: scale(1);
                opacity: 1;
            }
        }

</style>