<script>
	const dynos = [
		{
			name: "Free",
			capacity: "512MB"
		},
		{
			name: "Standard 1x",
			capacity: "512MB"
		},
		{
			name: "Standard 2x",
			capacity: "1GB"
		},
		{
			name: "Performance M",
			capacity: "2.5GB"
		},
		{
			name: "Performance L",
			capacity: "14GB"
		}
	];

	const servers = [
		"Passenger",
		"Passenger Enterprise",
		"Puma",
		"Unicorn"
	];

	let selectedDyno = null;
	let selectedServer = null;
</script>

<div class="px-5 flex flex-wrap md:px-0 md:h-screen">
	<div class="w-full py-16 order-2 flex-none md:flex-1 md:py-10 md:pr-10 md:max-w-xl">
		<div class="lg:flex lg:items-center">
			<div class="lg:w-1/4">
				<label class="block pr-4 text-gray-600 font-medium lg:text-right mb-1 lg:mb-0" for="dyno">
					Heroku Dyno
				</label>
			</div>
			<div class="lg:w-3/4">
				<div class="inline-block relative w-full">
					<select bind:value={selectedDyno} class="block appearance-none w-full bg-white border border-gray-400 hover:border-gray-500 px-4 py-2 pr-8 rounded shadow leading-tight focus:outline-none focus:shadow-outline" name="dyno">
						<option selected disabled>-- Select dyno --</option>
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
		</div>
		<small class="block mb-6 text-gray-600 mt-1 text-sm lg:ml-auto lg:w-3/4">{@html selectedDyno ? selectedDyno.capacity : '&nbsp;'}</small>

		<div class="lg:flex lg:items-center mb-6">
			<div class="lg:w-1/4">
				<label class="block pr-4 text-gray-600 font-medium lg:text-right mb-1 lg:mb-0" for="server">
					App server
				</label>
			</div>
			<div class="lg:w-3/4">
				<div class="inline-block relative w-full">
					<select bind:value={selectedServer} class="block appearance-none w-full bg-white border border-gray-400 hover:border-gray-500 px-4 py-2 pr-8 rounded shadow leading-tight focus:outline-none focus:shadow-outline" name="server">
						<option selected disabled>-- Select app server --</option>
						{#each servers as server}
							<option value={server}>
								{server}
							</option>
						{/each}
					</select>
					<div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700">
						<svg class="fill-current h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><path d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"/></svg>
					</div>
				</div>
			</div>
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
