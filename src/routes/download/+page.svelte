<script>
    import { supabase } from "../supabase-client";
    let inputCode;
    let downloadUrl;

    const handleFileDownload = async () => {
        const {data, error} = await supabase
            .from("uploaded-files")
            .select("file_url")
            .eq("generated_code", inputCode)

        if (error) {
            console.error("Error while fetching file: ", error.message)
            return
        }

        downloadUrl = data[0].file_url;
    }
</script>

<main>
    <h1>Download a File</h1>
    <div class="download-section">
        <input type="text" bind:value={inputCode}>
        <button onclick={handleFileDownload}>Download</button>
    </div>
    <h2>
        {downloadUrl}
    </h2>
</main>