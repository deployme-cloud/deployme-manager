<svelte:head>
	<title>Clusters - Deployme Manager</title>
</svelte:head>

<div class="space-y-6">
	<div class="flex items-center justify-between">
		<div>
			<h1 class="text-2xl font-bold">Clusters</h1>
			<p class="text-text-secondary mt-1">Gerez vos clusters Kubernetes Talos Linux</p>
		</div>
		<a
			href="/clusters/new"
			class="rounded-lg bg-deployme-600 px-4 py-2 text-sm font-medium text-white transition hover:bg-deployme-700"
		>
			+ Nouveau cluster
		</a>
	</div>

	<div class="rounded-xl border border-border bg-surface-light p-12 text-center">
		<p class="text-text-secondary">Aucun cluster pour le moment.</p>
	</div>
</div>
