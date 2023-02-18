<script lang="ts">
	import { writable } from 'svelte/store';
	import { encode } from 'morse-converter';

	let inputText = writable<string>('');
	let outputText = writable<string>('');

	$: $inputText,
		(() => {
			let morseOutput = encode($inputText);
			$outputText = morseOutput;
		})();

	async function copyToClipboard(
		e: MouseEvent & { currentTarget: EventTarget & HTMLButtonElement }
	) {
		await navigator.clipboard.writeText($outputText);
	}
</script>

<div class="flex  flex-col justify-center items-center h-screen bg-lime-300">
	<div class="bg-white p-10 flex  gap-10 rounded-2xl items-center shadow-xl">
		<div class="flex flex-col items-center gap-10">
			<label for="input" class="text-4xl">English</label>
			<textarea
				name="english"
				id="input"
				rows="10"
				cols="30"
				style="resize: none;"
				bind:value={$inputText}
				class="max-w-full rounded-md  text-lg border-2 border-dashed p-2 border-gray-500 outline-4 outline-blue-500"
			/>
		</div>
		<div class="flex flex-col gap-10">
			<button
				class="p-3 rounded-full bg-lime-400 shadow-xl flex items-center justify-center"
				on:click={copyToClipboard}
			>
				<img src="/arrow.png" alt="icon" />
			</button>
			<button class="p-3 rounded-full bg-lime-400 shadow-xl" on:click={copyToClipboard}>
				Copy
			</button>
		</div>
		<div class="flex flex-col items-center gap-10">
			<label for="input" class="text-4xl">Morse</label>
			<textarea
				name="morese"
				id="output"
				rows="10"
				cols="30"
				style="resize: none;"
				class=" font-sans max-w-full rounded-md  text-lg border-2 border-dashed p-2 border-gray-500 outline-4 outline-blue-500"
				value={$outputText}
			/>
		</div>
	</div>
</div>

<style>
	:global(body) {
		@apply font-mono;
	}
</style>
