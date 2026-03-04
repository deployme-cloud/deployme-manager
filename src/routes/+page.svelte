<script lang="ts">
	import ClusterCard from '$lib/components/ClusterCard.svelte';
	import StatsCard from '$lib/components/StatsCard.svelte';
</script>

<svelte:head>
	<title>Dashboard - Deployme Manager</title>
</svelte:head>

<div class="space-y-6">
	<div>
		<h1 class="text-2xl font-bold">Dashboard</h1>
		<p class="text-text-secondary mt-1">Vue d'ensemble de vos clusters Kubernetes</p>
	</div>

	<!-- Stats -->
	<div class="grid grid-cols-1 gap-4 sm:grid-cols-2 lg:grid-cols-4">
		<StatsCard title="Clusters actifs" value="0" icon="server" />
		<StatsCard title="Noeuds totaux" value="0" icon="cpu" />
		<StatsCard title="Credits restants" value="20h" icon="clock" />
		<StatsCard title="Statut" value="OK" icon="check" />
	</div>

	<!-- Clusters -->
	<div>
		<div class="mb-4 flex items-center justify-between">
			<h2 class="text-lg font-semibold">Mes clusters</h2>
			<a
				href="/clusters/new"
				class="rounded-lg bg-deployme-600 px-4 py-2 text-sm font-medium text-white transition hover:bg-deployme-700"
			>
				+ Nouveau cluster
			</a>
		</div>

		<div class="rounded-xl border border-border bg-surface-light p-12 text-center">
			<div class="mx-auto mb-4 flex h-16 w-16 items-center justify-center rounded-full bg-surface-lighter">
				<svg class="h-8 w-8 text-text-muted" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="1.5">
					<path stroke-linecap="round" stroke-linejoin="round" d="M21 7.5l-9-5.25L3 7.5m18 0l-9 5.25m9-5.25v9l-9 5.25M3 7.5l9 5.25M3 7.5v9l9 5.25m0-9v9" />
				</svg>
			</div>
			<h3 class="text-lg font-medium text-text-primary">Aucun cluster</h3>
			<p class="mt-1 text-sm text-text-secondary">
				Creez votre premier cluster Kubernetes Talos Linux en quelques minutes.
			</p>
			<a
				href="/clusters/new"
				class="mt-4 inline-block rounded-lg bg-deployme-600 px-6 py-2.5 text-sm font-medium text-white transition hover:bg-deployme-700"
			>
				Deployer un cluster
			</a>
		</div>
	</div>
</div>
