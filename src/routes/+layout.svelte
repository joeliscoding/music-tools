<script lang="ts">
    import favicon from '$lib/assets/favicon.svg';

    let { children } = $props();
</script>

<svelte:head>
    <link rel="icon" href={favicon} />
</svelte:head>

{@render children()}

<style lang="scss">
    @font-face {
        font-family: MusiQwik;
        src: url(/src/lib/fonts/MusiQwik.woff2); /* url of the font */
    }
</style>
