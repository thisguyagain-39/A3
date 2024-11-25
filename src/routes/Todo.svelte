<script>

import editIcon from "$lib/img/edit.svg"
import removeIcon from  "$lib/img/remove.svg"

let todoItem = $state([])

let todoList = $state([])

function addItem() {

        event.preventDefault()

        if (todoItem == '') {

            return;
        }

        todoList.push(todoItem)

        todoItem = ''
    }

// $inspect(todoList)

function reset() {

    todoList.length = 0;
}

function removeThisTask(thisTask) {

    if (todoList.includes(thisTask)) {

        let thisTaskIndex = todoList.indexOf(thisTask)

        todoList.splice(thisTaskIndex)
    }

    console.log("ragghhh")
}

</script>

<form onsubmit={addItem}>

    <div class="formcontainer"> 

        <div class="oneForm"> 

            <label for="taskname"> Task Name </label>

            <input type="text" name="taskname" maxlength="120" bind:value={todoItem[0]}>

        </div>

        <div class="oneForm"> 

            <label for="taskdesc"> Task Description </label>

            <input type="text" name="taskdesc" maxlength="480" bind:value={todoItem[1]}>

        </div>
  
    </div>

    <button class="feature solidBG inlinebutton" type="submit"> Add </button>

</form>

<div class="taskList">

    {#each todoList as item}

    <div class="oneTask"> 

        <h2 class="todoHeader"> {item[0]} 
            <button class="button itemSetting edit" aria-label="edit task"> <img class="icon" alt="" src={editIcon}> </button>
            <button type="button "class="button itemSetting remove" aria-label="remove task" onclick= {() => removeThisTask(item)}> <img class="icon" alt="" src={removeIcon}> </button>  
        </h2> 
        
        <p class="todoDesc"> {item[1]}</p>
    
    </div>

    {/each}

</div>

<button class="feature solidBG floatbutton" onclick={reset}> Clear All </button>
<button class="feature solidBG floatbutton"> Settings </button>

<style lang="scss">

    body {

        
    }

    .formcontainer {

        display: flex;

        flex-direction: column;

        max-width: 50vw;

    }


</style>