<script>
    import { supabase } from "../supabase-client";
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

<main>
    <h1>Download a File</h1>
    <div class="download-section">
        <input type="text" bind:value={inputCode}>
        <button onclick={handleFileDownload}>Download</button>
    </div>
	{#if btnPressed && !downloading}
        loading...
    {/if}
</main>