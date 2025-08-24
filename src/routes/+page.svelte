<script>
	import { fade } from "svelte/transition";
</script>

<main in:fade class="gap-4 mt-6">
    <div class="flex flex-col text-center gap-2">
        <h1 class="h1 font-bold font-sans">Welcome to SupaDrop</h1>
        <p>share files without any hassle</p>
    </div>
    <div>
        <button class="btn preset-filled">
            <a href="/upload">Upload</a>
        </button>
        <button class="btn preset-outlined">
            <a href="/download">Download</a>
        </button>
    </div>
</main>