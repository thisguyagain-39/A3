<script>

import editIcon from "$lib/img/edit.svg"

import removeIcon from  "$lib/img/remove.svg"

import { json } from "@sveltejs/kit";

import {onMount} from 'svelte';

let { toggle = $bindable()} = $props()

let thisStorage;

let pstor; // persistient storage: boolean

let conc; // clear on complete: boolean

let todoItem = $state("")

let todoDesc = $state("")

let todoList = $state([])

onMount(() => {


    let settings = localStorage.getItem("settingList")

    if (settings) {

        let settingsParsed = JSON.parse(settings)

        pstor = (settingsParsed[0].value === "true");
        
        conc = (settingsParsed[1].value === "true");

        $inspect(pstor)

        $inspect(conc)
        
    }


    if (pstor == true) {

        thisStorage = localStorage.getItem('storedList')

        if (thisStorage) {

        todoList = (JSON.parse(thisStorage))

        }

// todo: change to make this check go first, since we can skip all of this if pstor is false and its annoying me that i did it like now

    } else if (pstor == false) {

        reset(); 
    }


})

function updateList() {

    console.log("started function")

    switch (pstor) {

        case true:

            thisStorage = localStorage.setItem('storedList', JSON.stringify(todoList))

            console.log("finished pstor check")

        break;
    
        case false:

            // do nothing, localstorage off

            console.log("finished pstor check")

        break;

       
    }

    console.log("about to start conc check")

    switch (conc) {
        
        case true:

            console.log("why no go")

            todoList.forEach(task => {

            if (task.done) {

            removeThisTask(task)
        }

});

        break;

        case false:

            console.log("huh")

            // do nothing

        break;

    }

    console.log("function stop")
        
}



function addItem() {

        event.preventDefault()

        if (todoItem == "") {

            return;
        }

        todoList = [...todoList, {
            text:todoItem,
            desc:todoDesc,
            done: false
        }]

        // spread syntax takes array, makes new array with new data appended

        // almost same thing as push but non mutating

        todoItem = ""

        todoDesc = ""

        updateList()
    }

// $inspect(todoList)

function reset() {

    todoList = [];

    localStorage.setItem("storedList", []);

    // if localstorage is off in settings this might make something but it doesnt matter because its empty anyways

    // cant use localstorage.clear anymore because that'll kill the settings and itll be

}

function removeThisTask(thisTask) {

    if (todoList.includes(thisTask)) {

        let thisTaskIndex = todoList.indexOf(thisTask)

        todoList = todoList.toSpliced(thisTaskIndex, 1)
    }

    updateList();

}


function clearComplete() {

    todoList.forEach(task => {

        if (task.done) {

            removeThisTask(task)
        }
        
    });

    updateList()
}

function editThisTask(thisTask) {


    if (todoList.includes(thisTask)) {

        let thisTaskIndex = todoList.indexOf(thisTask)

       /*  console.log(thisTask) */

        todoItem = thisTask.text 

        todoDesc = thisTask.desc

        todoList = todoList.toSpliced(thisTaskIndex, 1)

    }

    updateList();


}

function toggleProp() {

    toggle = !toggle
    
}

$inspect(toggle)

</script>

<form onsubmit={addItem}>

    <div class="formcontainer"> 

        <div class="oneForm"> 

            <label for="taskname"> Task Name </label>

            <input type="text" name="taskname" maxlength="120" bind:value={todoItem}>

        </div>

        <div class="oneForm"> 

            <label for="taskdesc"> Task Description </label>

            <input type="text" name="taskdesc" maxlength="480" bind:value={todoDesc}>

        </div>
  
    </div>

    <div class="featureButtonsDiv"> 

        <button class="feature solidBG inlinebutton" type="submit"> Add </button>

        <button class="feature solidBG floatbutton" onclick={reset}> Clear All </button>

        <button class="feature solidBG floatbutton"  onclick={clearComplete} type="button"> Clear All Complete </button>

        <button class="feature solidBG floatbutton" type="button" onclick={toggleProp}> Settings </button>

        
    
    </div>

</form>

<div class="taskList">

    {#each todoList as item}

    <div class="oneTask" class:done={item.done}> 

        <div class="taskHead">

            <h2 class="todoHeader"> {item.text} </h2> 

            <div>

                <input type="checkbox" bind:checked={item.done} class="checkbox" onchange={updateList}>

                <button class="button itemSetting edit" aria-label="edit task" onclick= {() => editThisTask(item)}> <img class="icon" alt="" src={editIcon} > </button>

                <button type="button" class="button itemSetting remove" aria-label="remove task" onclick= {() => removeThisTask(item)}> <img class="icon" alt="" src={removeIcon}> </button>

            </div>
        
        </div>
        
        <p class="todoDesc"> {item.desc}</p>
    
    </div>

    {/each}

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


    .done {

        text-decoration:line-through;
    }

    h2 {

        font-family: "Roboto Condensed";

        color: $off-white;

        display: flex;

        flex-direction: row;


    }

    p, button, a, label  {

        color:$off-white;
    }

    .taskHead {


        align-items: center;

        justify-content: space-between;

        display: flex;

        flex-direction: row;

        div {

            display: flex;

            align-items: center;

            justify-content: center;
        }
    }

    button.itemSetting, .checkbox {

        vertical-align: 10px;

        flex-direction: row;
        
        margin:0;

        height: 30px;

        width: 30px;

        background-color: #00000000;

        border-style: none;

        padding: 5px;

        :hover {

            color:#00000084;
        }

    }

    .formcontainer {

        font-family: "Titillium Web"; // of course its titillium web. are we surprised

        font-size: 1.5rem;

        display: flex;

        flex-direction: column;


    }

    form {

        padding: 0 5% 0 5%;
        
        input {

            font-size: 1.5rem;

            margin:5%;

            background-color: $cool-gray-2;

            border-style: none;

            padding:2%;

            border-radius: 34px;

            max-width: 40vw;
        }

        .oneForm {

            display: flex;

            align-items: center;

            justify-content: space-between;

            margin:0;

            text-align: left;
        }
    }

    .formcontainer > * {

        margin: 5% 0 5% 0;
        
    }

    .oneTask {

        animation: 0.5s ease-out 1 slide-right;

        justify-content: space-between;

        font-size: 1.5rem;

        background-color: $english-violet;

        padding:1% 5% 1% 5%;

        border-radius: 34px;

        margin-bottom: 5%;

        p {

            font-family: "Titillium Web";
        }
    }

    .taskList {

        margin: 5%;

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

    .checkbox {

        padding: 5%;
    }

    /* yeah i used a generator for my css animations
    i did modify this though... needs to be exactly how i want */

    /* ----------------------------------------------
 * Generated by Animista on 2024-12-3 16:9:8
 * Licensed under FreeBSD License.
 * See http://animista.net/license for more info. 
 * w: http://animista.net, t: @cssanimista
 * ---------------------------------------------- */

@-webkit-keyframes slide-right {
  0% {
    -webkit-transform: translateX(-100px);
            transform: translateX(-100px);
  }
  100% {
    -webkit-transform: translateX(0px);
            transform: translateX(0px);
  }
}
@keyframes slide-right {
  0% {
    -webkit-transform: translateX(-100px);
            transform: translateX(-100px);
  }
  100% {
    -webkit-transform: translateX(0px);
            transform: translateX(0px);
  }
}


</style>
