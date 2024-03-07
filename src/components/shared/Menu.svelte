<script>
    export let activeMode;
</script>

<div class="h-8 px-3 flex justify-center items-center {activeMode === 'Dark' ? 'bg-gray-600' : 'bg-gray-100'}
{activeMode === 'Dark' ? 'text-gray-300' : 'text-gray-600'} {activeMode === 'Dark' ? 'hover:bg-gray-500' : 'hover:bg-gray-200'}   rounded-lg shadow-sm cursor-pointer" on:click>
    <p class="text-sm font-semibold tracking-wide"><slot></slot></p>
</div>