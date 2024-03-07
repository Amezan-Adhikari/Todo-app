<script>
    export let activeMode='Dark';
    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();
    export let darkMode=false;
    const toggleModes = () =>{
        if(activeMode == 'Light')darkMode=true,activeMode='Dark';
        else darkMode = false , activeMode = 'Light';
        dispatch('changeModes',activeMode);
    }
</script>

<div class="bg-none h-full w-full justify-center flex flex-row items-center">
<div class="flex flex-row justify-between">
    <label for="dark-toggle" class="flex items-center cursor-pointer">
        <div class="relative border-[1px] border-black">
        <input type="checkbox" name="dark-mode" id="dark-toggle" class="checkbox hidden" bind:checked={darkMode} on:click={toggleModes}>
        <div class="block border {darkMode? 'border-gray-100' :'border-gray-900'} w-14 h-8 rounded-full"></div>
        <div class="dot absolute left-1 top-1  {darkMode? 'bg-gray-100' :'bg-gray-800'} w-6 h-6 rounded-full transition"></div>
        </div>
        <div class="ml-3 font-medium {darkMode? 'text-gray-100' :'text-gray-900'}">
            {activeMode}
        </div>
    </label>
    </div>
</div>

<style>

    input:checked ~ .dot {
    transform: translateX(100%);
    
    }
</style>