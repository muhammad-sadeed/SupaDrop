<script>
    import { supabase } from "../supabase-client";
    import { Download } from "lucide-svelte";
    let inputCode;
    let btnPressed = false;
    let downloading = false;

    const handleFileDownload = async () => {
        btnPressed = true;
        const {data, error} = await supabase
            .from("uploaded-files")
            .select("file_url")
            .eq("generated_code", inputCode)

        if (error) {
            console.error("Error while fetching file: ", error.message)
            return
        }

        downloadFile(data[0].file_url)
    }

    const downloadFile = (publicUrl) => {
        downloading = true;
        fetch(publicUrl)
            .then(response => response.blob())
            .then(blob => {
                // Create a link element to trigger download
                const url = window.URL.createObjectURL(blob);
                    
                const a = document.createElement('a');
                a.style.display = 'none';
                a.href = url;
                    
                // Extract filename from URL or use a default
                const filename = publicUrl.split('/').pop() || 'downloaded-file';
                a.download = filename;
                
                document.body.appendChild(a);
                a.click();
                    
                // Clean up
                window.URL.revokeObjectURL(url);
                document.body.removeChild(a);
            })
            .catch(error => console.error('Download failed:', error));

            resetLoading();
    }

    const resetLoading = () => {
        downloading = false;
        btnPressed = false;
    }
</script>

<main class="gap-4">
    <h3 class="h3 font-sans">Enter the code to download the file</h3>
    <div class="input-group grid-cols-[auto_1fr_auto]">
        <div class="ig-cell preset-tonal">
            <Download size={16} />
        </div>
        <input class="ig-input" type="search" placeholder="Search..." bind:value={inputCode} />
        <button class="ig-btn preset-filled" on:click={handleFileDownload}>Download</button>
    </div>
	{#if btnPressed && !downloading}
        loading...
    {/if}
</main>