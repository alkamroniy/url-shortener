<script setup>
import { ref } from 'vue'
import { LightningBoltIcon } from '@heroicons/vue/outline'

const url = ref('')
const loading = ref(false)
const input = ref('')

const shortenUrl = async e => {
	loading.value = true
	const res = await fetch('https://gotiny.cc/api', {
		method: 'POST',
		headers: {
			'Content-Type': 'application/json',
		},
		body: JSON.stringify({ input: url.value }),
	})
	input.value.focus()
	const data = await res.json()
	url.value = `https://gotiny.cc/${data[0].code}`
	loading.value = false
}
</script>

<template>
	<div
		class="flex flex-col items-center justify-center w-full min-h-screen px-6 space-y-8 font-sans"
	>
		<h1 class="text-2xl font-bold">Paste the URL to be shortened</h1>
		<div class="space-y-8 w-full sm:w-[36rem]">
			<form
				@submit.prevent="shortenUrl"
				class="flex flex-col space-y-4 sm:flex-row sm:space-y-0"
			>
				<input
					class="w-full px-4 py-3 border shadow-inner outline-none sm:w-3/4 focus:border-blue-500"
					type="url"
					v-model="url"
					ref="input"
					placeholder="Paste long URL and shorten it"
				/>
				<button
					class="flex items-center justify-center px-4 py-3 space-x-2 font-medium text-white bg-blue-500 border border-blue-500 whitespace-nowrap"
					type="submit"
				>
					<span><LightningBoltIcon class="w-6 h-6" /></span>
					<span>Shorten URL</span>
				</button>
			</form>
		</div>
	</div>
</template>
