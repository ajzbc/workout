<script>
    import Tailwind from "./Tailwind.svelte";

    let goal = 100;

    let local = window.localStorage.getItem("stats");

    // add if doesn't exist
    if (local == null) {
        localStorage.setItem(
            "stats",
            JSON.stringify({
                timestamp: Date.now(),
                count: 0,
            })
        );
    }

    let stats = JSON.parse(local);

    // reset count each day
    const today = new Date().setHours(0, 0, 0, 0);
    const prev = new Date(stats.timestamp).setHours(0, 0, 0, 0);

    if (prev < today) {
        stats.count = 0;
        stats.timestamp = Date.now();
    }

    function set(n) {
        stats.timestamp = Date.now();
        if (n > 0) stats.count += n;
        else stats.count = 0;
    }

    // keep local sotrage updated
    $: localStorage.setItem("stats", JSON.stringify(stats));
</script>

<Tailwind />

<main>
    <p class="mb-6">
        <span class="count {stats.count < goal ? 'incomplete' : ''}">{stats.count}</span>
        <span class="goal">/{goal}</span>
    </p>

    <div class="flex gap-4">
        <button on:click={() => set(1)}>+1</button>
        <button on:click={() => set(10)}>+10</button>
        <button on:click={() => set(0)}>✖</button>
    </div>
</main>

<style lang="postcss">
    main {
        @apply h-full flex justify-center items-center flex-col;
        @apply bg-gray-900 text-white;
    }

    .count {
        @apply text-8xl font-bold;
    }

    .incomplete {
        @apply text-red-500;
    }

    .goal {
        @apply text-4xl font-bold text-gray-400;
    }

    button {
        @apply px-4 py-2 rounded-lg;
        @apply bg-gray-800 text-gray-400 text-2xl font-medium;
        @apply hover:bg-gray-500 active:bg-gray-400;
    }
</style>
