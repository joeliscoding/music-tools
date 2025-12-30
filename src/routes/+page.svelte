<script>
    import { onMount } from 'svelte';
    import { on } from 'svelte/events';

    let note_index = 0;
    let display = '';
    const display_notes = ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H', 'I', 'J', 'K', 'L', 'M', 'N'];
    const notes = [
        'B3',
        'C4',
        'D4',
        'E4',
        'F4',
        'G4',
        'A4',
        'B4',
        'C5',
        'D5',
        'E5',
        'F5',
        'G5',
        'A5'
    ];
    const changeNote = () => {
        note_index = Math.round(Math.random() * (display_notes.length - 1));
        display = `'&=${display_notes[note_index]}!`;
    };
    onMount(() => {
        changeNote();
        const interval = setInterval(changeNote, 2000);
        return () => clearInterval(interval);
    });
</script>

<div>
    <p class="sheet-music" id="display">{display}</p>
</div>

<style lang="scss">
    div {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;
    }
    .sheet-music {
        font-family: MusiQwik, sans-serif;
        font-size: 10rem;
    }
</style>
