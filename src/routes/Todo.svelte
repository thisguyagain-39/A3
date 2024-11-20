<script>

let todoItem = $state('')

let todoList = $state([])

    function addItem() {

        event.preventDefault()

        if (todoItem == '') {

            return;
        }

        todoList.push(todoItem)

        todoItem = ''
    }

$inspect(todoList)

</script>

<form onsubmit={addItem}>

    <input type="text" maxlength="120" bind:value={todoItem}>

    <button type="submit"> Add </button>

</form>

<ul>

    {#each todoList as item}
    <li> {item}</li>
    {/each}
</ul>

<style>


</style>