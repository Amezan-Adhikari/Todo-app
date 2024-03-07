<script>
	import { onMount } from 'svelte';
	import { v4 as uuidv4 } from 'uuid';
	import TodoDark from "./components/Todo_dark.svelte";
	import TodoLight from "./components/Todo_light.svelte";
	import ThemeBtn from './components/shared/Theme_btn.svelte';

let darkMode=false;
let activeMode = 'Light';
let clientName='My';
let todoContents = [];
const recievedToDoHandler =(e)=> {
	if(e.detail.task=='' || e.detail=='')return;
	todoContents=[...todoContents,e.detail];
}

const clearDone = (e) =>{
	todoContents=e.detail;
}

const changeModes =(e)=>{
	activeMode = e.detail;
	localStorage.setItem('currentMode', JSON.stringify(activeMode));
}

onMount(() => {
    const storedMode = JSON.parse(localStorage.getItem('currentMode'));
    if(storedMode ==='Light'){ activeMode = 'Light'}
	else{activeMode = 'Dark';darkMode=true};

	const storedList = JSON.parse(localStorage.getItem('todoContents'));
	if(storedList){
		todoContents = storedList;
	}
  });

</script>
<div class="h-full w-full relative">
<div class="absolute h-10 w-36 right-0 top-5">
<ThemeBtn {activeMode} {darkMode}on:changeModes={changeModes}/>
</div>
{#if activeMode==='Light'}

<TodoLight {clientName} {todoContents} on:getnewtodo={recievedToDoHandler} on:clearDone={clearDone}/>
{:else}
<TodoDark {clientName} {todoContents} on:getnewtodo={recievedToDoHandler} on:clearDone={clearDone}/>
{/if}
</div>
