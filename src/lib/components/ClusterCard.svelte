<script lang="ts">
	type Cluster = {
		id: string;
		name: string;
		status: 'running' | 'provisioning' | 'stopped' | 'error';
		nodes: number;
		version: string;
		provider: string;
		created: string;
	};

	let { cluster }: { cluster: Cluster } = $props();

	const statusColors: Record<string, string> = {
		running: 'bg-green-500/10 text-green-400',
		provisioning: 'bg-yellow-500/10 text-yellow-400',
		stopped: 'bg-gray-500/10 text-gray-400',
		error: 'bg-red-500/10 text-red-400'
	};

	const statusLabels: Record<string, string> = {
		running: 'Running',
		provisioning: 'Provisioning',
		stopped: 'Stopped',
		error: 'Error'
	};
</script>

<a
	href="/clusters/{cluster.id}"
	class="block rounded-xl border border-border bg-surface-light p-5 transition hover:border-deployme-600/50"
>
	<div class="flex items-start justify-between">
		<div>
			<h3 class="font-semibold">{cluster.name}</h3>
			<p class="mt-1 text-sm text-text-secondary">
				{cluster.nodes} noeuds &middot; Talos {cluster.version} &middot; {cluster.provider}
			</p>
		</div>
		<span class="rounded-full px-2.5 py-1 text-xs font-medium {statusColors[cluster.status]}">
			{statusLabels[cluster.status]}
		</span>
	</div>
</a>
