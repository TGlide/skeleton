<script lang="ts">
	// Docs
	import DocsShell from '$docs/layouts/DocsShell/DocsShell.svelte';
	import { DocsFeature, type DocsShellSettings } from '$docs/layouts/DocsShell/types';
	import DocsPreview from '$docs/components/DocsPreview/DocsPreview.svelte';
	// Components
	import Avatar from '$lib/components/Avatar/Avatar.svelte';
	// Utilities
	import CodeBlock from '$lib/utilities/CodeBlock/CodeBlock.svelte';
	// Actions
	import { filter } from '$lib/actions/Filters/filter';
	// Filter SVGs
	import Apollo from '$lib/actions/Filters/svg-filters/Apollo.svelte';
	import BlueNight from '$lib/actions/Filters/svg-filters/BlueNight.svelte';
	import Emerald from '$lib/actions/Filters/svg-filters/Emerald.svelte';
	import GreenFall from '$lib/actions/Filters/svg-filters/GreenFall.svelte';
	import Noir from '$lib/actions/Filters/svg-filters/Noir.svelte';
	import NoirLight from '$lib/actions/Filters/svg-filters/NoirLight.svelte';
	import Rustic from '$lib/actions/Filters/svg-filters/Rustic.svelte';
	import Summer84 from '$lib/actions/Filters/svg-filters/Summer84.svelte';
	import XPro from '$lib/actions/Filters/svg-filters/XPro.svelte';
	// Sveld
	import sveldAvatar from '$lib/components/Avatar/Avatar.svelte?raw&sveld';

	// Docs Shell
	const settings: DocsShellSettings = {
		feature: DocsFeature.Component,
		name: 'Avatars',
		description: 'Display user avatars with an image or initials.',
		imports: ['Avatar'],
		source: 'components/Avatar',
		components: [{ sveld: sveldAvatar }]
	};

	// Local
	const imgPlaceholder = 'https://source.unsplash.com/YOErFW8AfkI/128x128';
	const borderStyles = 'border-4 	border-surface-300-600-token hover:!border-primary-500';

	const roundedMapping = {
		0: 'rounded-none',
		1: 'rounded-sm',
		2: 'rounded',
		3: 'rounded-md',
		4: 'rounded-lg',
		5: 'rounded-xl',
		6: 'rounded-2xl',
		7: 'rounded-3xl',
		8: 'rounded-full'
	};
	let rangeSliderValue: keyof typeof roundedMapping = 8;

	// Reactive
	$: actionParams = '#Apollo';
</script>

<!-- Filter SVG Components - NOTE: Keep these localized -->
<Apollo />
<BlueNight />
<Emerald />
<GreenFall />
<Noir />
<NoirLight />
<Rustic />
<Summer84 />
<XPro />

<DocsShell {settings}>
	<!-- Slot: Sandbox -->
	<svelte:fragment slot="sandbox">
		<DocsPreview regionFooter="grid grid-cols-[100px_1fr] gap-4 items-center">
			<svelte:fragment slot="preview">
				<Avatar src={imgPlaceholder} width="w-32" rounded={roundedMapping[rangeSliderValue]} />
			</svelte:fragment>
			<svelte:fragment slot="source">
				<CodeBlock language="html" code={`<Avatar src="${imgPlaceholder}" width="w-32" rounded="${roundedMapping[rangeSliderValue]}" />`} />
			</svelte:fragment>
			<svelte:fragment slot="footer">
				<div class="text-center"><code>{roundedMapping[rangeSliderValue]}</code></div>
				<input type="range" bind:value={rangeSliderValue} max={Object.keys(roundedMapping).length - 1} step={1} />
			</svelte:fragment>
		</DocsPreview>
	</svelte:fragment>

	<!-- Slot: Usage -->
	<svelte:fragment slot="usage">
		<section class="space-y-4">
			<h2>Using Initials</h2>
			<p>Display up to two text characters. (ex: Jane Doe would be JD)</p>
			<DocsPreview background="neutral">
				<svelte:fragment slot="preview">
					<Avatar initials="JD" background="bg-primary-500" />
				</svelte:fragment>
				<svelte:fragment slot="source">
					<CodeBlock language="html" code={`<Avatar initials="JD" background="bg-primary-500" />`} />
				</svelte:fragment>
			</DocsPreview>
		</section>
		<section class="space-y-4">
			<h2>Interactive Border</h2>
			<p>Apply the following styles using the <code>border</code> and <code>cursor</code> properties.</p>
			<DocsPreview background="neutral">
				<svelte:fragment slot="preview">
					<Avatar border={borderStyles} cursor="cursor-pointer" />
				</svelte:fragment>
				<svelte:fragment slot="source">
					<CodeBlock
						language="html"
						code={`
<Avatar
	border="${borderStyles}"
	cursor="cursor-pointer"
/>
`}
					/>
				</svelte:fragment>
			</DocsPreview>
		</section>
		<section class="space-y-4">
			<h2>Applying Filters</h2>
			<p>
				See <a href="/actions/filters">Filters</a> to learn how to import and configure the filters action and SVG filter components.
			</p>
			<CodeBlock language="ts" code={`import { filter, ${actionParams.replace('#', '')} } from '@skeletonlabs/skeleton';`} />
			<DocsPreview background="neutral" regionFooter="text-center">
				<svelte:fragment slot="preview">
					<Avatar src={imgPlaceholder} action={filter} {actionParams} />
				</svelte:fragment>
				<svelte:fragment slot="source">
					<p>Via Filter Action</p>
					<CodeBlock language="html" code={`<Avatar src="https://i.pravatar.cc/" action={filter} actionParams="${actionParams}" />`} />
					<p>Via Style Attribute</p>
					<CodeBlock language="html" code={`<Avatar src="https://i.pravatar.cc/" style="filter: url({'${actionParams}'})" />`} />
				</svelte:fragment>
				<svelte:fragment slot="footer">
					<select class="select w-auto" name="filter" id="filter" bind:value={actionParams}>
						<option value="#Apollo">#Apollo</option>
						<option value="#BlueNight">#BlueNight</option>
						<option value="#Emerald">#Emerald</option>
						<option value="#GreenFall">#GreenFall</option>
						<option value="#Noir">#Noir</option>
						<option value="#NoirLight">#NoirLight</option>
						<option value="#Rustic">#Rustic</option>
						<option value="#Summer84">#Summer84</option>
						<option value="#XPro">#XPro</option>
					</select>
				</svelte:fragment>
			</DocsPreview>
		</section>
	</svelte:fragment>
</DocsShell>
