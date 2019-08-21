<script>
	let selectedDyno = null;
	let selectedServer = "passenger";
	let usage = null;
	const dynos = [
		{
			name: "Free",
			label: "512MB",
			capacity: 512
		},
		{
			name: "Standard 1x",
			label: "512MB",
			capacity: 512
		},
		{
			name: "Standard 2x",
			label: "1GB",
			capacity: 1024
		},
		{
			name: "Performance M",
			label: "2.5GB",
			capacity: 2500
		},
		{
			name: "Performance L",
			label: "14GB",
			capacity: 14000
		}
	];

	$: workersCount = Math.floor((selectedDyno ? selectedDyno.capacity : 0) / (usage * 1.2));
	$: servers = {
		passenger: {
			key: 0,
			name: "Passenger",
			cli: `passenger start --min-instances <span class="text-yellow-300">${workersCount}</span> --max-pool-size <span class="text-yellow-300">${workersCount}</span>`,
			configFile: "Nginx configuration",
			config: `passenger_max_pool_size <span class="text-yellow-300">${workersCount}</span>;<br>passenger_min_instances <span class="text-yellow-300">${workersCount}</span>;`
		},
		passengerEnterprise: {
			key: 1,
			name: "Passenger Enterprise",
			configFile: "Nginx configuration",
			config: `passenger_max_pool_size <span class="text-yellow-300">${workersCount}</span>;<br>passenger_min_instances <span class="text-yellow-300">${workersCount}</span>;`
		},
		puma: {
			key: 2,
			name: "Puma",
			cli: `puma -w <span class="text-yellow-300">${workersCount}</span>`,
			configFile: "config/puma.rb",
			config: `workers <span class="text-yellow-300">${workersCount}</span>`
		},
		unicorn: {
			key: 3,
			name: "Unicorn",
			configFile: "config/unicorn.rb",
			config: `worker_processes <span class="text-yellow-300">${workersCount}</span>`
		}
	};
	$: server = servers[selectedServer]
</script>

<div class="px-5 flex flex-wrap md:px-0 md:h-screen">
	<div class="w-full py-16 order-2 flex-none md:flex-1 md:pr-10 md:max-w-2xl">
		<div class="lg:flex">
			<div class="mb-6 lg:w-1/2 lg:pr-4">
				<label class="block mb-1 pr-4 text-gray-600 font-medium" for="dyno">
					Heroku Dyno
				</label>
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
				<small class="block text-gray-600 mt-1 text-sm">{selectedDyno ? selectedDyno.label : ''}</small>
			</div>

			<div class="lg:w-1/2 lg:pl-4">
				<label class="block mb-1 pr-4 text-gray-600 font-medium" for="usage">
					RAM usage
				</label>
				<input on:change="{event => usage = parseInt(event.target.value, 10)}" class="shadow-inner appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="usage" type="number" placeholder="220">
				<small class="block text-gray-600 mt-1 text-sm">Input the RAM usage per process, in MB's</small>
			</div>
		</div>

		<div class="my-6 md:hidden">
			<label class="block mb-1 pr-4 text-gray-600 font-medium lg:text-right" for="server">
				App server
			</label>
			<div class="inline-block relative w-full">
				<select bind:value={selectedServer} class="block appearance-none w-full bg-white border border-gray-400 hover:border-gray-500 px-4 py-2 pr-8 rounded shadow leading-tight focus:outline-none focus:shadow-outline" name="server">
					<option selected disabled>-- Select app server --</option>
					{#each Object.entries(servers) as server}
						<option value={server[0]}>
							{server[1].name}
						</option>
					{/each}
				</select>
				<div class="pointer-events-none absolute inset-y-0 right-0 flex items-center px-2 text-gray-700">
					<svg class="fill-current h-4 w-4" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20"><path d="M9.293 12.95l.707.707L15.657 8l-1.414-1.414L10 10.828 5.757 6.586 4.343 8z"/></svg>
				</div>
			</div>
		</div>

		{#if selectedDyno && usage}
			<div class="mt-16 pt-5 border-t border-gray-300 md:mt-10 md:border-0 md:pt-0 lg:mt-16">
				<ul class="hidden md:flex mb-5 justify-between border-b border-gray-300">
					{#each Object.entries(servers) as server}
						<li>
							<button on:click="{e => selectedServer = server[0]}" class="inline-block font-medium text-sm lg:text-base pb-3 {server[0] == selectedServer ? 'text-gray-900 pointer-events-none' : 'text-gray-600'}" href="#">{server[1].name}</button>
						</li>
					{/each}
				</ul>

				{#if server.cli}
					<p class="mb-1 text-sm uppercase tracking-tight text-gray-700 font-bold">Command line syntax</p>
					<div class="mb-8 px-4 py-3 rounded bg-gray-800 font-mono text-sm text-gray-300 select-all lg:text-base">
						{@html server.cli}
					</div>
				{/if}

				<p class="mb-1 text-sm uppercase tracking-tight text-gray-700 font-bold">{server.configFile}</p>
				<div class="px-4 py-3 rounded bg-gray-800 font-mono text-sm text-gray-300 select-all lg:text-base">
					{@html server.config}
				</div>
			</div>
		{/if}
	</div>

	<aside class="py-16 w-full border-b border-gray-300 text-center order-1 md:max-w-md md:pl-10 md:pr-24 md:text-left">
		<h1 class="text-4xl leading-none text-gray-900 font-light">Rails server config</h1>
		<h2 class="mt-5 text-lg text-gray-600 leading-snug md:mx-0">
			Configure your Ruby web server
			<span class="block">for maximum performance</span>
		</h2>

		<h3 class="mt-16 mb-1 uppercase tracking-tight text-gray-600 font-bold">What is this?</h3>
		<p class="text-gray-700 max-w-xs mx-auto md:max-w-full">A tool to quickly calculate the maximum number of workers based on your app memory usage and Heroku dyno.</p>

		<h3 class="mt-12 mb-1 uppercase tracking-tight text-gray-600 font-bold">Why is this important?</h3>
		<p class="text-gray-700 max-w-xs mx-auto md:max-w-full">Correctly configuring the number of workers in your web server can have a major impact on the throughput and performance of your application. Read up on this in <a class="font-medium text-blue-500 hover:text-blue-700" href="https://www.speedshop.co/2017/10/12/appserver.html">Configuring Puma, Unicorn and Passenger for Maximum Efficiency</a>.</p>

		<h3 class="mt-12 mb-1 uppercase tracking-tight text-gray-600 font-bold">How was this built?</h3>
		<p class="text-gray-700 max-w-xs mx-auto md:max-w-full">This little app was built because I wanted to give <a class="font-medium text-blue-500 hover:text-blue-700" href="https://svelte.dev">Svelte</a> and <a class="font-medium text-blue-500 hover:text-blue-700" href="https://tailwindcss.com">Tailwind CSS</a> a try. The <a class="font-medium text-blue-500 hover:text-blue-700" href="https://github.com/richardvenneman/rails-server-config">source code is on GitHub</a>.</p>
	</aside>
</div>
