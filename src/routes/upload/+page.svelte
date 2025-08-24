<script>
	import { supabase } from '../supabase-client';
	import { FileUpload } from '@skeletonlabs/skeleton-svelte';
	// Icons
	import { ImagePlus as IconDropzone, Paperclip as IconFile, CircleX as IconRemove } from 'lucide-svelte';
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

	const handleFileChange = async (event, acceptedFiles) => {
		fileUrl = null;
		fileCode = null;
		toggleUpload = false;
		uploadedFile = event.acceptedFiles;
	}

	const handleFileUpload = async () => {
		if (uploadedFile) {
			toggleUpload = true;
			fileUrl = await uploadFile(uploadedFile[0]);
		}
		else {
			alert("empty input")
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

<main class="gap-4">
	<h2 class="h2 font-sans">Upload your file</h2>
	<div class="flex flex-col justify-center w-128 gap-3.5">
		<FileUpload
		maxFiles={1}
		subtext="Attach up to 1 files."
		onFileChange={handleFileChange}
		onFileReject={console.error}
		classes="w-full"
		>
		{#snippet iconInterface()}<IconDropzone class="size-8" />{/snippet}
		{#snippet iconFile()}<IconFile class="size-4" />{/snippet}
		{#snippet iconFileRemove()}<IconRemove class="size-4" />{/snippet}
		</FileUpload>
		<button class="btn preset-filled" onclick={handleFileUpload}>upload</button>
	</div>
	{#if !fileUrl && toggleUpload}
		<h1>loading...</h1>
	{:else if fileUrl}
		<h1>{fileCode}</h1>
	{/if}
</main>
