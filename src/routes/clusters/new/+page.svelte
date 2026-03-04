<script lang="ts">
	let clusterName = $state('');
	let plan = $state('small');

	const plans = [
		{ id: 'small', label: 'Small', spec: '1 control plane + 1 worker', desc: '2 vCPU, 4 GB RAM par noeud' },
		{ id: 'medium', label: 'Medium', spec: '1 control plane + 3 workers', desc: '4 vCPU, 8 GB RAM par noeud' },
		{ id: 'large', label: 'Large', spec: '3 control planes + 3 workers', desc: '4 vCPU, 16 GB RAM par noeud' }
	];
</script>

<svelte:head>
	<title>Nouveau cluster - Deployme Manager</title>
</svelte:head>

<div class="mx-auto max-w-2xl space-y-8">
	<div>
		<h1 class="text-2xl font-bold">Deployer un cluster</h1>
		<p class="text-text-secondary mt-1">Configurez et lancez un nouveau cluster Kubernetes Talos Linux</p>
	</div>

	<form class="space-y-6">
		<!-- Cluster name -->
		<div>
			<label for="cluster-name" class="mb-2 block text-sm font-medium">Nom du cluster</label>
			<input
				id="cluster-name"
				type="text"
				bind:value={clusterName}
				placeholder="mon-cluster"
				class="w-full rounded-lg border border-border bg-surface px-4 py-2.5 text-text-primary placeholder:text-text-muted focus:border-deployme-500 focus:outline-none focus:ring-1 focus:ring-deployme-500"
			/>
		</div>

		<!-- Plan selection -->
		<div>
			<label class="mb-3 block text-sm font-medium">Taille du cluster</label>
			<div class="grid gap-3 sm:grid-cols-3">
				{#each plans as p}
					<button
						type="button"
						onclick={() => (plan = p.id)}
						class="rounded-xl border p-4 text-left transition {plan === p.id
							? 'border-deployme-500 bg-deployme-600/10'
							: 'border-border bg-surface-light hover:border-surface-lighter'}"
					>
						<p class="font-semibold">{p.label}</p>
						<p class="mt-1 text-sm text-text-secondary">{p.spec}</p>
						<p class="mt-0.5 text-xs text-text-muted">{p.desc}</p>
					</button>
				{/each}
			</div>
		</div>

		<!-- Submit -->
		<button
			type="submit"
			disabled={!clusterName}
			class="w-full rounded-lg bg-deployme-600 px-4 py-3 text-sm font-medium text-white transition hover:bg-deployme-700 disabled:cursor-not-allowed disabled:opacity-50"
		>
			Deployer le cluster
		</button>
	</form>
</div>
