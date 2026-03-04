<script lang="ts">
	import { page } from '$app/state';

	const nav = [
		{ href: '/', label: 'Dashboard', icon: 'home' },
		{ href: '/clusters', label: 'Clusters', icon: 'server' },
		{ href: '/billing', label: 'Billing', icon: 'credit-card' },
		{ href: '/settings', label: 'Settings', icon: 'settings' }
	];

	const icons: Record<string, string> = {
		home: 'M3 12l2-2m0 0l7-7 7 7M5 10v10a1 1 0 001 1h3m10-11l2 2m-2-2v10a1 1 0 01-1 1h-3m-4 0a1 1 0 01-1-1v-4a1 1 0 011-1h2a1 1 0 011 1v4a1 1 0 01-1 1',
		server: 'M5 12h14M5 12a2 2 0 01-2-2V6a2 2 0 012-2h14a2 2 0 012 2v4a2 2 0 01-2 2M5 12a2 2 0 00-2 2v4a2 2 0 002 2h14a2 2 0 002-2v-4a2 2 0 00-2-2m-2-4h.01M17 16h.01',
		'credit-card': 'M3 10h18M7 15h1m4 0h1m-7 4h12a3 3 0 003-3V8a3 3 0 00-3-3H6a3 3 0 00-3 3v8a3 3 0 003 3z',
		settings: 'M10.325 4.317c.426-1.756 2.924-1.756 3.35 0a1.724 1.724 0 002.573 1.066c1.543-.94 3.31.826 2.37 2.37a1.724 1.724 0 001.066 2.573c1.756.426 1.756 2.924 0 3.35a1.724 1.724 0 00-1.066 2.573c.94 1.543-.826 3.31-2.37 2.37a1.724 1.724 0 00-2.573 1.066c-.426 1.756-2.924 1.756-3.35 0a1.724 1.724 0 00-2.573-1.066c-1.543.94-3.31-.826-2.37-2.37a1.724 1.724 0 00-1.066-2.573c-1.756-.426-1.756-2.924 0-3.35a1.724 1.724 0 001.066-2.573c-.94-1.543.826-3.31 2.37-2.37.996.608 2.296.07 2.572-1.065z M15 12a3 3 0 11-6 0 3 3 0 016 0z'
	};

	function isActive(href: string): boolean {
		if (href === '/') return page.url.pathname === '/';
		return page.url.pathname.startsWith(href);
	}
</script>

<aside class="flex w-64 flex-col border-r border-border bg-surface">
	<!-- Logo -->
	<div class="flex h-16 items-center gap-3 border-b border-border px-5">
		<img src="/logos/deployme-icon.svg" alt="Deployme" class="h-8 w-8" />
		<span class="text-lg font-bold text-text-primary">Deployme</span>
	</div>

	<!-- Navigation -->
	<nav class="flex-1 space-y-1 px-3 py-4">
		{#each nav as item}
			<a
				href={item.href}
				class="flex items-center gap-3 rounded-lg px-3 py-2.5 text-sm font-medium transition {isActive(item.href)
					? 'bg-deployme-600/10 text-deployme-400'
					: 'text-text-secondary hover:bg-surface-light hover:text-text-primary'}"
			>
				<svg class="h-5 w-5 shrink-0" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="1.5">
					<path stroke-linecap="round" stroke-linejoin="round" d={icons[item.icon]} />
				</svg>
				{item.label}
			</a>
		{/each}
	</nav>

	<!-- Footer -->
	<div class="border-t border-border p-4">
		<div class="flex items-center gap-3">
			<div class="flex h-8 w-8 items-center justify-center rounded-full bg-deployme-600 text-sm font-bold text-white">
				U
			</div>
			<div class="flex-1 truncate">
				<p class="truncate text-sm font-medium text-text-primary">Utilisateur</p>
				<p class="truncate text-xs text-text-muted">Community</p>
			</div>
		</div>
	</div>
</aside>
