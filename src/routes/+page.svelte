<script lang="ts">
    /*
        & Treble Clef
        ¯ Bass Clef
    */

    import { onMount } from 'svelte';
    import { on } from 'svelte/events';
    import { Mic } from '@lucide/svelte';
    import { MicOff } from '@lucide/svelte';
    import { Volume2 } from '@lucide/svelte';
    import { VolumeX } from '@lucide/svelte';

    let recorder: MediaRecorder | null = null;
    let sound = false;

    function microphoneAccess() {
        if (navigator.mediaDevices && navigator.mediaDevices.getUserMedia) {
            navigator.mediaDevices
                .getUserMedia({ audio: true })
                .then(SetupStream)
                .catch((err) => {
                    console.error(err);
                });
        }
    }

    function toggleSound() {
        sound = !sound;
    }

    function SetupStream(stream: MediaStream) {
        recorder = new MediaRecorder(stream);
    }

    let noteIndex = 0;
    let display = '';
    const notes = [
        { trebleName: 'A3', bassName: 'C2', whole: 'p', half: '`', quarter: 'P' },
        { trebleName: 'B3', bassName: 'D2', whole: 'q', half: 'a', quarter: 'Q' },
        { trebleName: 'C4', bassName: 'E2', whole: 'r', half: 'b', quarter: 'R' },
        { trebleName: 'D4', bassName: 'F2', whole: 's', half: 'c', quarter: 'S' },
        { trebleName: 'E4', bassName: 'G2', whole: 't', half: 'd', quarter: 'T' },
        { trebleName: 'F4', bassName: 'A2', whole: 'u', half: 'e', quarter: 'U' },
        { trebleName: 'G4', bassName: 'B2', whole: 'v', half: 'f', quarter: 'V' },
        { trebleName: 'A4', bassName: 'C3', whole: 'w', half: 'g', quarter: 'W' },
        { trebleName: 'B4', bassName: 'D3', whole: 'x', half: 'h', quarter: 'X' },
        { trebleName: 'C5', bassName: 'E3', whole: 'y', half: 'i', quarter: 'Y' },
        { trebleName: 'D5', bassName: 'F3', whole: 'z', half: 'j', quarter: 'Z' },
        { trebleName: 'E5', bassName: 'G3', whole: '{', half: 'k', quarter: '[' },
        { trebleName: 'F5', bassName: 'A3', whole: '|', half: 'l', quarter: '\\' },
        { trebleName: 'G5', bassName: 'B3', whole: '}', half: 'm', quarter: ']' },
        { trebleName: 'A5', bassName: 'C4', whole: '~', half: 'n', quarter: '^' }
    ];

    function changeNote() {
        noteIndex = Math.round(Math.random() * (notes.length - 1));
        display = `'&=${notes[noteIndex].whole}!`;
    }

    onMount(() => {
        changeNote();
        const interval = setInterval(changeNote, 2000);
        return () => clearInterval(interval);
    });
</script>

<div class="container">
    <div class="buttons">
        <ul>
            <li>
                <button on:click={microphoneAccess}>
                    {#if recorder}
                        <Mic />
                    {:else}
                        <MicOff />
                    {/if}
                </button>
            </li>
            <li>
                <button on:click={toggleSound}>
                    {#if sound}
                        <Volume2 />
                    {:else}
                        <VolumeX />
                    {/if}
                </button>
            </li>
        </ul>
    </div>

    <div class="sheet-music">
        <p>{display}</p>
    </div>
</div>

<style lang="scss">
    .buttons {
        position: absolute;
        top: 1rem;
        left: 1rem;

        ul {
            list-style: none;
            margin: 0;
            padding: 0;

            li {
                margin-bottom: 0.5rem;

                button {
                    border: none;
                    background: none;
                    color: var(--primary-text-color);
                    transition: opacity 0.2s ease-in-out;

                    &:hover {
                        cursor: pointer;
                        opacity: 0.5;
                    }
                }
            }
        }
    }
    .sheet-music {
        display: flex;
        justify-content: center;
        align-items: center;
        height: 100vh;

        p {
            font-family: MusiQwik, sans-serif;
            font-size: 10rem;
            color: var(--primary-text-color);
        }
    }
</style>
