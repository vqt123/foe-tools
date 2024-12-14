<script>
	// Default values as requested
	let attackerAttack = 4;
	let boostAttack = 190; // 190% means a typed value of 190
	let attackerBonusAttack = 0;

	let defenderDefense = 8;
	let boostDefense = 10; // 10% means a typed value of 10
	let defenderBonusDefense = 0;

	// ----------------------------------------------
	// Reactive declarations for attack and defense
	// ----------------------------------------------
	$: attack = (attackerAttack * (100 + boostAttack)) / 100 + attackerBonusAttack;
	$: defense = (defenderDefense * (100 + boostDefense)) / 100 + defenderBonusDefense;

	// Ratio q
	$: q = defense === 0 ? 0 : attack / defense;

	// ----------------------------------------------
	// min_damage logic
	// ----------------------------------------------
	$: min_damage =
		q >= 19.735714
			? 10
			: q >= 6.04
				? 9
				: q >= 3.407317
					? 8
					: q >= 2.284375
						? 7
						: q >= 1.631579
							? 6
							: q >= 1.214783
								? 5
								: q >= 0.925
									? 4
									: q >= 0.703333
										? 3
										: q >= 0.5175
											? 2
											: q >= 0.0832
												? 1
												: 0;

	// ----------------------------------------------
	// max_damage logic
	// ----------------------------------------------
	$: max_damage =
		q >= 9.324361
			? 10
			: q >= 2.957143
				? 9
				: q >= 1.676923
					? 8
					: q >= 1.110714
						? 7
						: q >= 0.803571
							? 6
							: q >= 0.611765
								? 5
								: q >= 0.479365
									? 4
									: q >= 0.36
										? 3
										: q >= 0.259167
											? 2
											: q >= 0.0832
												? 1
												: 0;
</script>

<!-- Container -->
<div class="mx-auto mt-10 max-w-md rounded-lg bg-white p-6 shadow-md">
	<h2 class="mb-5 text-2xl font-bold">Damage Calculator</h2>

	<!-- Form Inputs Grid -->
	<div class="grid grid-cols-1 gap-4">
		<!-- Attacker -->
		<div>
			<label class="mb-1 block font-semibold text-gray-700">Attacker Attack</label>
			<div class="flex items-center space-x-2">
				<button
					type="button"
					class="flex h-10 w-10 items-center justify-center rounded bg-gray-200 text-gray-700 hover:bg-gray-300"
					on:click={() => (attackerAttack = Math.max(0, attackerAttack - 1))}
				>
					-
				</button>
				<input
					class="flex-1 rounded border border-gray-300 px-3 py-2 text-center"
					type="number"
					bind:value={attackerAttack}
					min="0"
				/>
				<button
					type="button"
					class="flex h-10 w-10 items-center justify-center rounded bg-gray-200 text-gray-700 hover:bg-gray-300"
					on:click={() => attackerAttack++}
				>
					+
				</button>
			</div>
		</div>

		<div>
			<label class="mb-1 block font-semibold text-gray-700">Attacker Boost (%)</label>
			<div class="flex items-center space-x-2">
				<button
					type="button"
					class="flex h-10 w-10 items-center justify-center rounded bg-gray-200 text-gray-700 hover:bg-gray-300"
					on:click={() => (boostAttack = Math.max(0, boostAttack - 5))}
				>
					-
				</button>
				<input
					class="flex-1 rounded border border-gray-300 px-3 py-2 text-center"
					type="number"
					bind:value={boostAttack}
					step="5"
				/>
				<button
					type="button"
					class="flex h-10 w-10 items-center justify-center rounded bg-gray-200 text-gray-700 hover:bg-gray-300"
					on:click={() => (boostAttack += 5)}
				>
					+
				</button>
			</div>
		</div>

		<!-- <div>
			<label class="mb-1 block font-semibold text-gray-700">Attacker Bonus Attack</label>
			<input
				class="w-full rounded border border-gray-300 px-3 py-2"
				type="number"
				bind:value={attackerBonusAttack}
			/>
		</div> -->

		<!-- Defender -->
		<div>
			<label class="mb-1 block font-semibold text-gray-700">Defender Defense</label>
			<div class="flex items-center space-x-2">
				<button
					type="button"
					class="flex h-10 w-10 items-center justify-center rounded bg-gray-200 text-gray-700 hover:bg-gray-300"
					on:click={() => (defenderDefense = Math.max(0, defenderDefense - 1))}
				>
					-
				</button>
				<input
					class="flex-1 rounded border border-gray-300 px-3 py-2 text-center"
					type="number"
					bind:value={defenderDefense}
					min="0"
				/>
				<button
					type="button"
					class="flex h-10 w-10 items-center justify-center rounded bg-gray-200 text-gray-700 hover:bg-gray-300"
					on:click={() => defenderDefense++}
				>
					+
				</button>
			</div>
		</div>

		<div>
			<label class="mb-1 block font-semibold text-gray-700">Defender Boost (%)</label>
			<div class="flex items-center space-x-2">
				<button
					type="button"
					class="flex h-10 w-10 items-center justify-center rounded bg-gray-200 text-gray-700 hover:bg-gray-300"
					on:click={() => (boostDefense = Math.max(0, boostDefense - 5))}
				>
					-
				</button>
				<input
					class="flex-1 rounded border border-gray-300 px-3 py-2 text-center"
					type="number"
					bind:value={boostDefense}
					step="5"
				/>
				<button
					type="button"
					class="flex h-10 w-10 items-center justify-center rounded bg-gray-200 text-gray-700 hover:bg-gray-300"
					on:click={() => (boostDefense += 5)}
				>
					+
				</button>
			</div>
		</div>

		<!-- <div>
			<label class="mb-1 block font-semibold text-gray-700">Defender Bonus Defense</label>
			<input
				class="w-full rounded border border-gray-300 px-3 py-2"
				type="number"
				bind:value={defenderBonusDefense}
			/>
		</div> -->
	</div>

	<!-- Results -->
	<div class="mt-6 border-t pt-4">
		<p class="text-gray-800">
			<span class="font-bold">Effective Attack:</span>
			{attack.toFixed(2)}
		</p>
		<p class="text-gray-800">
			<span class="font-bold">Effective Defense:</span>
			{defense.toFixed(2)}
		</p>
		<p class="text-gray-800">
			<span class="font-bold">Ratio (q):</span>
			{q.toFixed(6)}
		</p>
		<p class="text-gray-800">
			<span class="font-bold">Min Damage:</span>
			{min_damage}
		</p>
		<p class="text-gray-800">
			<span class="font-bold">Max Damage:</span>
			{max_damage}
		</p>
	</div>
</div>
