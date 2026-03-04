<svelte:head>
	<title>Settings - Deployme Manager</title>
</svelte:head>

<div class="mx-auto max-w-2xl space-y-6">
	<div>
		<h1 class="text-2xl font-bold">Settings</h1>
		<p class="text-text-secondary mt-1">Gerez votre compte et vos preferences</p>
	</div>

	<!-- Profile -->
	<div class="rounded-xl border border-border bg-surface-light p-6">
		<h3 class="mb-4 font-semibold">Profil</h3>
		<div class="space-y-4">
			<div>
				<label for="email" class="mb-1 block text-sm text-text-secondary">Email</label>
				<input
					id="email"
					type="email"
					value="user@example.com"
					disabled
					class="w-full rounded-lg border border-border bg-surface px-4 py-2 text-text-muted"
				/>
				<p class="mt-1 text-xs text-text-muted">Gere via Zitadel OIDC</p>
			</div>
		</div>
	</div>

	<!-- API Keys -->
	<div class="rounded-xl border border-border bg-surface-light p-6">
		<div class="flex items-center justify-between">
			<h3 class="font-semibold">Cles API</h3>
			<button class="rounded-lg bg-surface-lighter px-3 py-1.5 text-sm text-text-secondary transition hover:text-text-primary">
				+ Generer une cle
			</button>
		</div>
		<p class="mt-2 text-sm text-text-muted">
			Utilisez des cles API pour acceder a la plateforme via SDK, Terraform ou CLI.
		</p>
	</div>

	<!-- Danger zone -->
	<div class="rounded-xl border border-red-500/30 bg-red-500/5 p-6">
		<h3 class="font-semibold text-red-400">Zone de danger</h3>
		<p class="mt-1 text-sm text-text-secondary">
			La suppression de votre compte est irreversible et supprimera tous vos clusters.
		</p>
		<button class="mt-4 rounded-lg border border-red-500/50 px-4 py-2 text-sm font-medium text-red-400 transition hover:bg-red-500/10">
			Supprimer mon compte
		</button>
	</div>
</div>
