<script>
    import { supabase } from "../supabase-client";
    let uploadedFile;
    let fileUrl;

    const uploadFile = async (file) => {
        let filePath = `${file.name}`
        
        const {error} = await supabase.storage
            .from("supa-files")
            .upload(filePath, file);

        if (error) {
            console.error("Error Uploading File: ", error.message)
            return null
        }
 
        const {data} = await supabase.storage
            .from("supa-files")
            .getPublicUrl(filePath)

        return data.publicUrl;
    }

    const handleFileUpload = async () => {
        if (uploadedFile) {
            fileUrl = await uploadFile(uploadedFile[0]);
        }
    }
</script>

<main>
    <h1>Upload you file</h1>
    <div class="upload-section">
        <input type="file" bind:files={uploadedFile} onchange={() => console.log(uploadedFile[0].name)}>
        <button onclick={handleFileUpload}>upload</button>
    </div>
    {fileUrl}
</main>