<script lang="ts">
	import { inputZoom, quadkeys } from '../stores';

	let disabled = false;
	let zoom = $inputZoom;

	$: disabled = $quadkeys.length > 1;

	function handleZoomChange() {
		/** Change the zoom level of the quadkey */

		if ($quadkeys.length === 1) {
			if ($quadkeys[0].length < zoom) {
				$quadkeys = [$quadkeys[0].padEnd(zoom, '0')];
			} else if ($quadkeys[0].length > zoom) {
				$quadkeys = [$quadkeys[0].slice(0, zoom)];
			}
		}
		$inputZoom = zoom;
	}

	function handleZoomChangeFromStore(newZoom: number) {
		/** Update the zoom level when it changes in the store */
		zoom = newZoom;
	}

	$: handleZoomChangeFromStore($inputZoom);
</script>

<div class="flex justify-center">
	<div
		class="tooltip"
		data-tip={disabled
			? 'The zoom level cannot be changed when multiple quadkeys are highlighted'
			: 'Change zoom level of quadkey'}
	>
		<label class="input input-bordered flex items-center gap-2">
			Zoom
			<input type="number" class="grow" on:change={handleZoomChange} bind:value={zoom} {disabled} />
		</label>
	</div>
</div>
