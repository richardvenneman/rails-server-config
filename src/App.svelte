<script>
	const dynos = [
		{
			name: "Free",
			capacity: "512 MB"
		},
		{
			name: "Standard 1x",
			capacity: "512 MB"
		},
		{
			name: "Standard 2x",
			capacity: "1 GB"
		},
		{
			name: "Performance M",
			capacity: "2.5 GB"
		},
		{
			name: "Performance L",
			capacity: "14 GB"
		}
	];

	let selectedDyno = dynos[0];
</script>

<div class="px-5 flex flex-wrap md:px-0 md:h-screen">
	<div class="w-full py-10 order-2 flex-none md:flex-1 md:p-10">
		<div class="md:flex md:items-center mb-6">
			<div class="md:w-1/3">
				<label class="block text-gray-600 font-medium md:text-right mb-1 md:mb-0 pr-4" for="dyno">
					Dyno
				</label>
			</div>
			<div class="md:w-2/3">
				<div class="inline-block relative w-full md:w-64">
					<select bind:value={selectedDyno} class="block appearance-none w-full bg-white border border-gray-400 hover:border-gray-500 px-4 py-2 pr-8 rounded shadow leading-tight focus:outline-none focus:shadow-outline" name="dyno">
						{#each dynos as dyno}
							<option value={dyno}>
								{dyno.name}
							</option>
						{/each}
					</select>
					<div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700">
						<svg class="fill-current h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><path d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"/></svg>
					</div>
				</div>
			</div>
			<small class="block mt-1 text-sm text-gray-600">{selectedDyno ? selectedDyno.capacity : ''}</small>
		</div>
	</div>
	<aside class="py-16 w-full border-b border-gray-300 text-center order-1 md:max-w-sm md:p-10 md:text-left">
		<h1 class="text-3xl leading-none text-gray-900 font-light">Rails server config</h1>
		<h2 class="mx-5 text-gray-600 leading-snug mt-3 md:mx-0">
			Configure your Ruby web server
			<span class="block">for maximum performance</span>
		</h2>
	</aside>
</div>
