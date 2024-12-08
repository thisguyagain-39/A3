<script>

    import Todo from './Todo.svelte'

    import Settings from './Settings.svelte'

    import '../style.css'

    let { toggle = $bindable(true), pstor = $bindable(true), conc = $bindable(false) } = $props()
        
    
 
</script>

<h1> Spacer. </h1>

<div class="pageContainer"> 

    <div class="pageContainer">

        {#if (toggle)}
    
        <div class="swapcontainer">
    
            <Todo bind:toggle={toggle}/>
    
        </div>
    
        {:else}
    
        <div class="swapcontainer">
    
            <Settings bind:toggle={toggle}/>
    
        </div>
    
        {/if}
    
    </div>

</div>

<nav>

</nav>


<style lang="scss">

    @use "sass:color" as clr;

    
    $dark-purple: #2c1320ff;
    $english-violet: #5f4b66ff;
    $cool-gray: #a7adc6ff;
    $cool-gray-2: #8797afff;
    $paynes-gray: #56667aff;
    $off-white: clr.adjust($cool-gray, $lightness:30%);
    

    h1 {

        margin:0 0 0 0;

        padding: 5%;

        font-family: "Roboto Condensed";

        background-color: $dark-purple;

        color: clr.adjust($cool-gray, $lightness:30%)

    }

    .pageContainer {

        background-color: clr.adjust($dark-purple, $lightness:-7%);

        padding-top: 5%;

        background-image: radial-gradient($english-violet);

        height:100vh;

        margin:0;

        background-size: cover;
    }

    
</style>
