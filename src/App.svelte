<script>
	const dynos = [
		{
			name: "Free",
			capacity: 512
		},
		{
			name: "Standard 1x",
			capacity: 512
		},
		{
			name: "Standard 2x",
			capacity: 1024
		},
		{
			name: "Performance M",
			capacity: 2500
		},
		{
			name: "Performance L",
			capacity: 14000
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
	let usage = null;

	$: workersCount = Math.floor((selectedDyno ? selectedDyno.capacity : 0) / (usage * 1.2));
</script>

<div class="px-5 flex flex-wrap md:px-0 md:h-screen">
	<div class="w-full py-16 order-2 flex-none md:flex-1 md:py-10 md:pr-10 md:max-w-xl">
		<div class="lg:flex lg:items-center mb-6">
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

		<div class="lg:flex lg:items-center">
			<div class="lg:w-1/4">
				<label class="block pr-4 text-gray-600 font-medium lg:text-right mb-1 lg:mb-0" for="usage">
					RAM usage
				</label>
			</div>
			<div class="lg:w-3/4">
				<input on:change="{event => usage = parseInt(event.target.value, 10)}" class="shadow-inner appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="usage" type="number" placeholder="220">
			</div>
		</div>
		<small class="block mb-6 text-gray-600 mt-1 text-sm lg:ml-auto lg:w-3/4">Input the RAM usage per process, in MB's</small>

		{#if selectedDyno && selectedServer && usage}
			<div class="px-4 py-3 rounded bg-gray-800 font-mono text-gray-400">
				{#if selectedServer === "Passenger" || selectedServer === "Passenger Enterprise"}
					passenger_max_pool_size <span class="text-yellow-300">{workersCount}</span>;<br>
					passenger_min_instances <span class="text-yellow-300">{workersCount}</span>;
				{:else if selectedServer == "Puma"}
					workers <span class="text-yellow-300">{workersCount}</span>
				{:else if selectedServer == "Unicorn"}
					worker_processes <span class="text-yellow-300">{workersCount}</span>
				{/if}
			</div>
		{/if}
	</div>

	<aside class="py-16 w-full border-b border-gray-300 text-center order-1 md:max-w-sm md:p-10 md:text-left">
		<h1 class="text-3xl leading-none text-gray-900 font-light">Rails server config</h1>
		<h2 class="mx-5 text-gray-600 leading-snug mt-3 md:mx-0">
			Configure your Ruby web server
			<span class="block">for maximum performance</span>
		</h2>
	</aside>
</div>
