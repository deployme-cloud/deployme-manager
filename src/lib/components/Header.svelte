<script lang="ts">
	import { page } from '$app/state';

	const titles: Record<string, string> = {
		'/': 'Dashboard',
		'/clusters': 'Clusters',
		'/clusters/new': 'Nouveau cluster',
		'/billing': 'Billing',
		'/settings': 'Settings'
	};

	function getTitle(): string {
		return titles[page.url.pathname] ?? 'Deployme';
	}
</script>

<header class="flex h-16 items-center justify-between border-b border-border bg-surface px-6">
	<h2 class="text-lg font-semibold">{getTitle()}</h2>

	<div class="flex items-center gap-4">
		<!-- Credits badge -->
		<div class="flex items-center gap-2 rounded-lg bg-surface-light px-3 py-1.5 text-sm">
			<svg class="h-4 w-4 text-deployme-400" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="1.5">
				<path stroke-linecap="round" stroke-linejoin="round" d="M12 6v6h4.5m4.5 0a9 9 0 11-18 0 9 9 0 0118 0z" />
			</svg>
			<span class="text-text-secondary">20h restantes</span>
		</div>

		<!-- Docs link -->
		<a
			href="https://docs.deployme.cloud"
			target="_blank"
			rel="noopener noreferrer"
			class="text-sm text-text-muted transition hover:text-text-primary"
		>
			Docs
		</a>
	</div>
</header>
