<script>
    const isSSR = import.meta.env.SSR;

    let mode = 'light';

    if (!isSSR) {
        mode = (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) ? 'dark' : 'light';

        const localStorageMode = localStorage.getItem('prefers-color-scheme');
        if (localStorageMode === 'dark' || localStorageMode === 'light') {
            mode = localStorageMode;
        }

        window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', e => {
            mode = e.matches ? 'dark' : 'light';
        });
    }

    $: if (!isSSR) {
        localStorage.setItem('prefers-color-scheme', mode);

        if (mode === 'dark') {
            document.documentElement.classList.add('dark');
        } else {
            document.documentElement.classList.remove('dark');
        }
    }

    function toggleMode() {
        mode = mode === 'light' ? 'dark' : 'light';
    }
</script>

<button
        on:click={toggleMode}
        role="switch"
        class="inline-flex items-center px-0.5 rounded-full w-16 h-8 transition-all duration-200 bg-green-300 dark:bg-gray-900 dark:justify-end">

    {#if mode === 'light'}
        <span class="sr-only">Enable dark mode</span>
    {:else}
        <span class="sr-only">Disable dark mode</span>
    {/if}

    <span class="bg-white rounded-full w-7 h-7"></span>
</button>
