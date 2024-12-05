<script>

    import Todo from './Todo.svelte'

    import Settings from './Settings.svelte'

    import '../style.css'

    let { toggle = $bindable(false) } = $props()

    /* if (toggle = true) {

        let swapcontainer = document.querySelector('.swapcontainer')

        swapcontainer.innerHTML = "<Settings/>"

    }

    // it did NOT enjoy when i did this. i don't know why, appaently my variable is null if this bit gets loaded in to the page on the fly but if it gets reloaded it Just Dies.

    // i still have no idea if im doing the props and binding correctly i havent gotten that far

    */
        
    
 
</script>

<h1> Spacer. </h1>

<div class="pageContainer"> 

    <div class="swapcontainer">

    
        <Todo bind:toggle={toggle}/>

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
