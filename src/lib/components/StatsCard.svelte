<script lang="ts">
	let { title, value, icon }: { title: string; value: string; icon: string } = $props();

	const iconPaths: Record<string, string> = {
		server: 'M5 12h14M5 12a2 2 0 01-2-2V6a2 2 0 012-2h14a2 2 0 012 2v4a2 2 0 01-2 2M5 12a2 2 0 00-2 2v4a2 2 0 002 2h14a2 2 0 002-2v-4a2 2 0 00-2-2m-2-4h.01M17 16h.01',
		cpu: 'M9 3v2m6-2v2M9 19v2m6-2v2M3 9h2m-2 6h2m14-6h2m-2 6h2M7 19h10a2 2 0 002-2V7a2 2 0 00-2-2H7a2 2 0 00-2 2v10a2 2 0 002 2zM9 9h6v6H9V9z',
		clock: 'M12 6v6h4.5m4.5 0a9 9 0 11-18 0 9 9 0 0118 0z',
		check: 'M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z'
	};
</script>

<div class="rounded-xl border border-border bg-surface-light p-5">
	<div class="flex items-center gap-3">
		<div class="flex h-10 w-10 items-center justify-center rounded-lg bg-deployme-600/10">
			<svg class="h-5 w-5 text-deployme-400" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="1.5">
				<path stroke-linecap="round" stroke-linejoin="round" d={iconPaths[icon] ?? iconPaths.check} />
			</svg>
		</div>
		<div>
			<p class="text-sm text-text-secondary">{title}</p>
			<p class="text-xl font-bold">{value}</p>
		</div>
	</div>
</div>
