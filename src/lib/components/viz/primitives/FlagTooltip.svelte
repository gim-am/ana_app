<script lang="ts">
	import { fmt } from '$lib/utils/format';
	import { getFlagBadge } from '$lib/utils/colors';
	import { uoaLabel } from '$lib/stores/adminFeaturesStore.svelte';
	import TooltipCard from '$lib/components/ui/TooltipCard.svelte';
	import type { TooltipRow, TooltipBadge } from '$lib/components/ui/TooltipCard.svelte';

	interface Props {
		uoa: string;
		value: number;
		threshold: number | null;
		vanThreshold?: number | null;
		direction: string | null;
		flagLabel: string;
		within10: boolean | null;
		within10Van?: boolean | null;
		x: number;
		y: number;
	}

	let {
		uoa,
		value,
		threshold,
		vanThreshold = null,
		direction,
		flagLabel,
		within10,
		within10Van = null,
		x,
		y
	}: Props = $props();

	const rows = $derived.by(() => {
		const r: TooltipRow[] = [{ key: 'Value', value: fmt(value) }];
		if (threshold !== null) {
			const thrStr = direction ? `${fmt(threshold)} (${direction})` : fmt(threshold);
			r.push({ key: 'AN threshold', value: thrStr });
		}
		if (vanThreshold !== null) {
			r.push({ key: 'VAN threshold', value: fmt(vanThreshold) });
		}
		return r;
	});

	const badge = $derived.by((): TooltipBadge | null => {
		const fb = getFlagBadge(flagLabel);
		if (!fb) return null;
		const b: TooltipBadge = { label: fb.label, cls: fb.badgeCls, style: fb.badgeStyle };
		if (within10Van && (flagLabel === 'flag' || flagLabel === 'no_flag')) {
			b.label = `${fb.label} · within 10% (VAN)`;
		} else if (within10 && (flagLabel === 'flag' || flagLabel === 'no_flag')) {
			b.label = `${fb.label} · within 10% (AN)`;
		}
		return b;
	});
</script>

<TooltipCard title={uoaLabel(uoa)} {badge} {rows} {x} {y} />
