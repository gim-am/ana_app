<script lang="ts">
	interface Props {
		x: number;
		height: number;
		innerWidth?: number;
		label?: string;
		color?: string;
	}

	let { x, height, innerWidth = Infinity, label = 'AN', color = 'var(--color-within10)' }: Props = $props();

	const flipLabel = $derived(x + 20 > innerWidth);
</script>

<g class="threshold-line">
	<line
		x1={x}
		y1={0}
		x2={x}
		y2={height}
		stroke={color}
		stroke-width="1.5"
		stroke-dasharray="4,3"
	/>
	{#if label}
		<text
			x={flipLabel ? x - 3 : x + 3}
			y={9}
			font-size="8"
			fill={color}
			font-weight="600"
			text-anchor={flipLabel ? 'end' : 'start'}
		>{label}</text>
	{/if}
</g>
