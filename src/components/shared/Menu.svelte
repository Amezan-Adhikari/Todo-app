<script>
    export let activeMode;
</script>

<div class="h-5 px-3 flex justify-center items-center {activeMode === 'Dark' ? 'bg-gray-100' : 'bg-gray-200'}
{activeMode === 'Dark' ? 'text-gray-800' : 'text-gray-100'} {activeMode === 'Dark' ? 'hover:bg-white' : 'hover:bg-gray-100'}   rounded-lg shadow-sm cursor-pointer" on:click>
    <p class="text-gray-600"><slot></slot></p>
</div>