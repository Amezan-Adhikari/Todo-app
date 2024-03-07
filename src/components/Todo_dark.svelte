<script>
    import { v4 as uuidv4 } from 'uuid';
    import { createEventDispatcher } from "svelte";
  import Menu from './shared/Menu.svelte';
    const dispatch = createEventDispatcher();
    export let clientName='';
    export let todoContents;
    let newContent='';
    let activeMode = 'Dark';
    let currentDate = new Date();
    let completedTime;
    const sendDataToApp = () =>{
        if (newContent=='') return;
        let newTodo = {
            task:newContent,
            completionStatus:false,
            id:uuidv4(),
            startedTime:formatTime(currentDate),
            completedTime:completedTime
        }
        todoContents = [...todoContents,newTodo];
        dispatch('getnewtodo',newTodo);
        saveinfo();
        
        newContent = '';
    }

    const clearAll =()=>{
        todoContents = [];
        dispatch('clearDone',todoContents);
        saveinfo();
    }

    const clearDone = ()=>{
        todoContents = todoContents.filter(todo => todo.completionStatus === false);
        saveinfo();
        dispatch('clearDone',todoContents);
    }
    const changeCompletionStatus=(index)=>{
        todoContents[index].completionStatus = !todoContents[index].completionStatus;
        todoContents[index].completedTime = formatTime(currentDate);
        
        saveinfo();
    }


    const saveinfo = ()=>{
        localStorage.setItem('todoContents', JSON.stringify(todoContents));
    }

    const handleKeyDown = (e)=>{
       if(e.key ==="Enter"){
        sendDataToApp();
       }
    }

    function formatTime(date) {
    let hours = date.getHours().toString().padStart(2, '0');
    if(hours > 12){
        hours -=12;
    }
    const minutes = date.getMinutes().toString().padStart(2, '0');
    return `${hours} : ${minutes}`;
  }
</script>

<div class="flex flex-grow items-center justify-center bg-gray-900 h-full">

    <div class="max-w-full p-8 bg-gray-800 rounded-lg shadow-lg w-96 text-gray-200">
        <div class="flex items-center justify-between mb-6">
            <div class="flex">
            <svg class="h-8 w-8 text-indigo-500 stroke-current" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20 13V6a2 2 0 00-2-2H6a2 2 0 00-2 2v7m16 0v5a2 2 0 01-2 2H6a2 2 0 01-2-2v-5m16 0h-2.586a1 1 0 00-.707.293l-2.414 2.414a1 1 0 01-.707.293h-3.172a1 1 0 01-.707-.293l-2.414-2.414A1 1 0 006.586 13H4" />
            </svg>
            <h4 class="font-semibold ml-3 text-lg">{clientName} Tasks</h4>
            </div>
            <div class="flex gap-1">
            <Menu {activeMode} on:click={clearAll}>Clear All</Menu><Menu {activeMode} on:click={clearDone}>Clear Done</Menu>
            </div>
        </div>
        {#each todoContents as todoContent, index (todoContent.id)}
        <div class="py-1 pb-4 mb-2 relative" on:click={()=>changeCompletionStatus(index)}>
            <input class="hidden" type="checkbox" id={todoContent.id} bind:checked={todoContent.completionStatus} disabled>
            <label class="flex items-center px-2 rounded cursor-pointer hover:bg-gray-900" for={todoContent.id} >
                <span class="flex checkbox items-center justify-center w-5 h-5 text-transparent border-2 border-gray-500 rounded-full">
                    <svg class="w-4 h-4 fill-current" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20" fill="currentColor">
                        <path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd" />
                    </svg>
                </span>
                <span class="ml-4 my-2 text-sm todoContent">{todoContent.task}</span>
            </label>	
            {#if todoContent.completionStatus!=true}
            <div class="absolute right-0 bottom-0 h-4 px-2 text-xs bg-gray-700 flex justify-center items-center rounded-lg text-gray-400">
                {todoContent.startedTime}
            </div>
            {:else}
            <div class="absolute right-0 bottom-0 h-4 px-2 text-xs bg-green-200 flex justify-center items-center rounded-lg text-gray-600">
                {todoContent.completedTime}
            </div>
            <div class="absolute right-left bottom-0 h-4 px-2 text-xs bg-green-200 flex justify-center items-center rounded-lg text-gray-600">
               completed
            </div>
            {/if}
        </div>
        {/each}
        
        <div class="flex items-center w-full h-8 px-2 mt-2 text-sm font-medium rounded bg-gray-700">
            <button class="border-none outline-none active:bg-gray-700" on:click={sendDataToApp}>
            <svg class="w-5 h-5 text-gray-400 fill-current" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6v6m0 0v6m0-6h6m-6 0H6" />
            </svg>
            </button>
            <input class="flex-grow h-8 ml-4 bg-transparent border-none outline-none font-medium" type="text" placeholder="add a new task" bind:value={newContent} on:keydown={handleKeyDown}>
        </div>
    </div>
    
</div>

<style>
    input[type=checkbox]:checked+label .checkbox {
	background-color: #10B981;
	border-color: #10B981;
	color: #fff;
}

input[type=checkbox]:checked+label .todoContent {
	text-decoration: line-through;
	color: #9CA3AF;
}
</style>