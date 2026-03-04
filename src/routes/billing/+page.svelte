<svelte:head>
	<title>Billing - Deployme Manager</title>
</svelte:head>

<div class="space-y-6">
	<div>
		<h1 class="text-2xl font-bold">Billing</h1>
		<p class="text-text-secondary mt-1">Gerez votre abonnement et vos credits</p>
	</div>

	<!-- Current plan -->
	<div class="rounded-xl border border-border bg-surface-light p-6">
		<div class="flex items-center justify-between">
			<div>
				<h3 class="font-semibold">Plan Community</h3>
				<p class="mt-1 text-sm text-text-secondary">20h/mois de credits gratuits inclus</p>
			</div>
			<span class="rounded-full bg-deployme-600/10 px-3 py-1 text-sm font-medium text-deployme-400">
				Gratuit
			</span>
		</div>
		<div class="mt-4">
			<div class="mb-2 flex justify-between text-sm">
				<span class="text-text-secondary">Credits utilises</span>
				<span>0h / 20h</span>
			</div>
			<div class="h-2 overflow-hidden rounded-full bg-surface-lighter">
				<div class="h-full rounded-full bg-deployme-500" style="width: 0%"></div>
			</div>
		</div>
	</div>

	<!-- Upgrade -->
	<div class="rounded-xl border border-deployme-600/30 bg-deployme-600/5 p-6">
		<h3 class="font-semibold">Passer a Pro</h3>
		<p class="mt-1 text-sm text-text-secondary">
			Clusters heberges illimites, support email 24h, sessions live mensuelles.
		</p>
		<button
			class="mt-4 rounded-lg bg-deployme-600 px-4 py-2 text-sm font-medium text-white transition hover:bg-deployme-700"
		>
			Voir les offres
		</button>
	</div>
</div>
