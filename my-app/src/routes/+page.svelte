

<script>
    import TwoChoiceGame from '../lib/options.svelte';
</script>
  
<div>

<h1 class="text-center text-3xl font-bold font-weight-normal font-family-serif text-white pt-10">
    João's Portfolio Analyzer
</h1>

<div class="flex justify-center items-center pt-10 text-white">
    <h2>This is a game about games, read about them and guess which had a higher grade</h2>
</div>

<TwoChoiceGame />
</div>


<style>
    :global(body) {
        background: #000;  
        min-height: 100vh;
        margin: 0;
    }
</style>