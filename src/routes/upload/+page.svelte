<script>
	import { supabase } from '../supabase-client';
	let uploadedFile;
	let fileUrl;
	let fileCode;
    let toggleUpload = false;

	const uploadFile = async (file) => {
		let filePath = `${Date.now()}-${file.name}`;

		const { error } = await supabase.storage.from('supa-files').upload(filePath, file);

		if (error) {
			console.error('Error Uploading File: ', error.message);
			return null;
		}

		const { data } = await supabase.storage.from('supa-files').getPublicUrl(filePath);

		generateFileInfo(file, data.publicUrl);
		return data.publicUrl;
	};

	const handleFileUpload = async () => {
        toggleUpload = true;
		if (uploadedFile) {
			fileUrl = await uploadFile(uploadedFile[0]);
		}
	};

	const generateFileInfo = async (file, fileurl) => {
		const { error } = await supabase.from('uploaded-files').insert({
			file_name: file.name,
			generated_code: generateCode(),
			file_url: fileurl,
			expires_at: generateExpiry()
		});

		if (error) {
			console.error('Error generating file info: ', error.message);
			return;
		}
	};

	const generateCode = () => {
		const characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
		let code = '';

		for (let i = 0; i < 6; i++) {
			// Generate a 6-character code
			code += characters.charAt(Math.floor(Math.random() * characters.length));
		}
		fileCode = code;
		return fileCode;
	};

	const generateExpiry = (minutesAhead = 5) => {
		const now = new Date();
		now.setMinutes(now.getMinutes() + minutesAhead);

		let hrs = String(now.getHours()).padStart(2, '0');
		let mins = String(now.getMinutes()).padStart(2, '0');
		let secs = String(now.getSeconds()).padStart(2, '0');

		return `${hrs}:${mins}:${secs}`;
	};
</script>

<main>
	<h1>Upload you file</h1>
	<div class="upload-section">
		<input
			type="file"
			bind:files={uploadedFile}
			onchange={() => {
				fileUrl = null;
				fileCode = null;
                toggleUpload = false;
			}}
		/>
		<button onclick={handleFileUpload}>upload</button>
	</div>
	{#if !fileUrl && toggleUpload}
		<h1>loading...</h1>
	{:else if fileUrl}
		<h1>{fileCode}</h1>
	{/if}
</main>
