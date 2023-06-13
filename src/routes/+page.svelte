<script>
	import { now } from 'svelte/internal';

	let timer = 0;
	let tasks = [];
	let name = '';
	let currentTime = now();

	function addNewTask() {
		tasks = [...tasks, { name: name, startedTime: Date.now(), stoppedTime: 0, finished: false }];
	}

	setInterval(() => (currentTime = Date.now()));
</script>

<div class="navbar bg-base-200">
	<h1 class="text-3xl font-bold">CoreTimer</h1>
</div>

<div class="card card-side bg-base-200 shadow-xl max-w-3xl max-h-48 my-4 mx-auto">
	<figure><img src="/stopwatch.webp" class="max-h-48" alt="Stopwatch" /></figure>
	<div class="card-body items-center">
		<h4 class="card-title">Add new task</h4>
		<input placeholder="Name" bind:value={name} class="input" />
		<div class="card-actions justify-end">
			<button class="btn btn-accent" on:click={addNewTask}>Start timer</button>
		</div>
	</div>
</div>

<table class="table bg-base-200 mx-auto max-w-6xl">
	<thead>
		<tr>
			<th class="text-base">Name</th>
			<th class="text-base">Started at</th>
			<th class="text-base">Stopped at</th>
			<th class="text-base">Running For</th>
			<th class="text-base">Status</th>
			<th class="text-base">Action</th>
		</tr>
	</thead>
	<tbody>
		{#each tasks as task}
			<tr>
				<td>{task.name}</td>

				<td>{new Date(task.startedTime).toLocaleString()}</td>
				{#if !task.finished}
					<td>-</td>
					<td>{new Date(currentTime - task.startedTime).toISOString().slice(11, 19)}</td>
					<td class="text-xl">👟</td>
					<td>
						<button
							class="btn btn-secondary min-w-64"
							on:click={() => {
								task.finished = true;
								task.stoppedTime = Date.now();
							}}
						>
							stop
						</button>
					</td>
				{:else}
					<td>{new Date(task.stoppedTime).toLocaleString()}</td>
					<td>{new Date(task.stoppedTime - task.startedTime).toISOString().slice(11, 19)}</td>
					<td class="text-xl">✋</td>
					<td>
						<button
							disabled
							class="btn btn-secondary min-w-64"
							on:click={() => {
								task.finished = false;
							}}
							>stop
						</button>
					</td>
				{/if}
			</tr>
		{/each}
	</tbody>
</table>
