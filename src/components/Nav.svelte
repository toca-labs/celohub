<script>
	import { isActive, url, layout } from '@roxi/routify'

	function capitalize(str){
		return str.split(' ').map(str => str[0].toUpperCase() + str.slice(1)).join(' ')
	}

	import { fade } from 'svelte/transition'
</script>

<style>
	nav {
		margin: 0 calc(-1 * var(--space-outer));
		gap: 0.5em;
		font-size: 1.1em;

		overflow: hidden auto;
	}

	ul {
		display: grid;
		list-style-type: none;
		--level: 0;
	}
	ul ul {
		gap: 0;
		--level: 1;
		/* font-size: 0.9em; */
	}

	li:focus, li:focus-within, a:focus {
		background-color: rgba(0, 0, 0, 0.2);
		outline: none;
	}
	li:hover, li:focus-within, a:hover {
		background-color: rgba(0, 0, 0, 0.25);
	}

	.row {
		display: flex;
	}

	a {
		flex: 1;

		display: block;
		/* padding-block: calc(0.5em - var(--level) * 0.33em);
		padding-inline-start: calc(var(--space-outer) + var(--level) * 1em);
		padding-inline-end: var(--space-outer); */
		padding-top: calc(0.33em - var(--level) * 0.2em);
		padding-bottom: calc(0.33em - var(--level) * 0.2em);
		padding-left: calc(var(--space-outer) + var(--level) * 1em);
		padding-right: var(--space-outer);
		font-weight: calc(600 - var(--level) * 200);
	}
	a.active {
		background-color: rgba(0, 0, 0, 0.25);
		/* color: var(--celo-gold); */
		font-weight: 700;
		text-decoration: underline;
		text-shadow: 0 0 1px;
	}

	.nav-dropdown {
		display: grid;
		justify-content: center;
		align-items: center;
		/* margin-inline-end: var(--space-outer); */
		width: 2.5em;
		transition: 0.1s;
	}
	li:focus .nav-dropdown, li:focus-within .nav-dropdown {
		transform: rotate(90deg);
	}


	@media (max-height: 30rem) {
		ul li:not(:focus):not(:focus-within) ul {
			position: absolute;
			visibility: hidden;
			pointer-events: none;
		}
	}
	@media (min-height: 30rem) {
		.nav-dropdown {
			display: none;
		}
	}
</style>

<nav>
	<ul>
		{#each console.log($layout.children) || $layout.children as { path, title, children }}
			<li tabindex="0">
				<div class="row">
					<a class:active={$isActive(path)} href={$url(path)}>{capitalize(title)}</a>
					{#if children?.length}
						<span class="nav-dropdown">▶</span><!-- ⟩ -->
					{/if}
				</div>
				{#if children?.length}
					<!-- <ul transition:fade={{duration: 200}}> -->
					<ul>
						{#each children as { path, title }}
							<li class:active={$isActive(path)}>
								<a href={$url(path)}>{capitalize(title)}</a>
							</li>
						{/each}
					</ul>
				{/if}
			</li>
		{/each}
	</ul>
</nav>
