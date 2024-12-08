<script> 


import { json } from "@sveltejs/kit";

import {onMount} from 'svelte';

let { toggle = $bindable(), pstor = $bindable(), conc = $bindable() } = $props()

let thisStorage;

let selectedSettings = $state([{

    setting:"pstor",
    value:"true"
},
{
    setting:"conc",
    value:"false"
}])


onMount(() => {

    thisStorage = localStorage.getItem('settingList')

    if (thisStorage) {

        console.log("got stored list of settings")

        selectedSettings = (JSON.parse(thisStorage))

    } else {

        console.log("initializing setting storage...")

        localStorage.setItem('settingList', JSON.stringify(selectedSettings))
    }

})

function applySettings() {

    let allSettings = document.querySelectorAll(".useThisValue");

    console.log("huh")

    for (let i = 0; i < allSettings.length; i ++) {

        selectedSettings[i].value = allSettings[i].value

        /* console.log("updated settings")

        console.log(selectedSettings) */

        // note to self: if the console.logs in the for loop arent working, consider that the for loop is what is messed up and not everything around it

        // context: spent 20 minutes debugging why this for loop wasnt firing; was using > instead of < when specifying the condition.....

    }



    chkPStor()

}

function chkPStor() {

    localStorage.setItem('settingList', JSON.stringify(selectedSettings))

    console.log("updated settings in storage")



}


function toggleProp() {

toggle = !toggle

}



</script>

<div class="container">

    <h2> Settings </h2>

    <div class="oneSetting">

    <h3> Persistient Storage </h3>

    <p> Saves your tasks locally. On by default. </p>

    <p> <em> Warning: Turning this off will delete your tasks. </em></p>

    <select class="useThisValue" value={selectedSettings[0].value}> 

        <option value="true"> On </option>

        <option value="false"> Off </option>

    </select>

    </div>

    <div class="oneSetting">

        <h3> Clear on complete </h3>

        <p> Clears completed tasks automatically. </p>

        <p> <em> Warning: Turning this on will delete all completed tasks. </em></p>

        <select class="useThisValue" value={selectedSettings[1].value}> 

                
            <option value="false"> Off </option>

            <option value="true"> On </option>
    
        </select>

    </div>

    <div class="featureButtonsDiv">

        <button class="feature solidBG floatbutton" type="button" onclick={toggleProp}> Back </button>

        <button class="feature solidBG floatbutton" type="button" onclick={applySettings}> Apply </button>
    </div>

</div>


<style lang="scss">

@use "sass:color" as clr;


@import url('https://fonts.googleapis.com/css2?family=Roboto+Condensed:ital,wght@0,100..900;1,100..900&family=Titillium+Web:ital,wght@0,200;0,300;0,400;0,600;0,700;0,900;1,200;1,300;1,400;1,600;1,700&display=swap');



$dark-purple: #2c1320ff;

$dark-purple-trans: #2c1320c7;

$english-violet: #5f4b66ff;

$cool-gray: #a7adc6ff;

$cool-gray-2: #8797afff;

$paynes-gray: #56667aff;

$off-white: clr.adjust($cool-gray, $lightness:30%);

.container {

    margin: 5%;
}

h2, h3 {


    font-family: "Roboto Condensed";

    color: $off-white;

    display: flex;

    flex-direction: row;

}


p, button  {

    font-family: "Titillium Web", monospace;

    color:$off-white;
}

.featureButtonsDiv {

    display: flex;

    justify-content: center;

    margin: 5%;

}

.featureButtonsDiv > * {

    border-radius: 34px;

    padding: 5%;

    margin: 0 5% 0 5%;

    border-style: none;

    background-color: clr.adjust($dark-purple-trans, $lightness:-30%);

}

    
</style>