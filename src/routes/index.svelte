<script>
	import Footer from '$lib/Footer.svelte';
	import Theme from '$lib/Theme.svelte';
	import { onMount } from 'svelte';

	let books = [];

	function saveBooks(edit = false) {
		if (edit) loadCovers();
		localStorage.setItem('books', JSON.stringify(books));
	}

	function loadBooks() {
		const stored = localStorage.getItem('books');
		if (stored) {
			books = JSON.parse(stored).map((book) => {
				book.edit = false;
				return book;
			});
			loadCovers();
		}
	}
	function loadCovers() {
		Promise.all(
			books.map(async (book) => {
				book.cover = await getCover(book.title);
			})
		).then(() => {
			books = books;
		});
	}

	async function getCover(title) {
		if (!title) return;
		const reply = await fetch(
			`https://cors-anywhere.herokuapp.com/https://bookcoverapi.herokuapp.com/getBookCover?bookTitle=${title.replace(
				/ /g,
				'+'
			)}`
		);
		if (reply.status === 200) {
			const data = await reply.json();
			console.log(data);
			return data.bookCoverUrl;
		} else {
			return '';
		}
	}

	onMount(() => {
		loadBooks();
	});
</script>

<h1 class="font-bold text-4xl m-4">📖 Book tracker</h1>
<div class="flex flex-col grow p-4">
	<ul class="grow">
		{#each books as book}
			{#if book.edit}
				<div class="card card-side bg-base-100 shadow relative  mb-4">
					<div class="card-body">
						<h2 class="card-title">Modifier</h2>
						<div class="form-control w-full">
							<label class="label" for="title">
								<span class="label-text">Titre</span>
							</label>
							<input
								type="text"
								bind:value={book.title}
								id="title"
								placeholder="nom du livre"
								class="input input-bordered w-full"
							/>

							<label class="label" for="pages_cnt">
								<span class="label-text">Nombre de pages</span>
							</label>
							<input
								type="number"
								bind:value={book.pagesCount}
								id="pages_cnt"
								placeholder="total"
								class="input input-sm input-bordered w-full mb-1"
							/>
							<div class="flex gap-1">
								<input
									type="number"
									bind:value={book.pagesStart}
									id="pages_cnt"
									placeholder="Premiere page"
									class="input input-sm input-bordered w-full"
								/>
								<input
									type="number"
									bind:value={book.pagesEnd}
									id="pages_cnt"
									placeholder="Derniere page"
									class="input input-sm input-bordered w-full"
								/>
							</div>

							<label class="label" for="progress">
								<span class="label-text">Progression</span>
							</label>
							<input
								type="number"
								bind:value={book.progress}
								id="progress"
								placeholder="100"
								class="input input-bordered"
							/>
						</div>
						<button
							class="btn btn-block btn-primary"
							on:click={() => {
								saveBooks(true);
								book.edit = false;
							}}>Save</button
						>
						<button
							class="btn btn-block btn-danger"
							on:click={() => {
								books = books.filter((b) => b !== book);
								saveBooks();
							}}>Supprimer</button
						>
					</div>
				</div>
			{:else}
				<div class="card card-side bg-base-100 shadow relative mb-4">
					<figure class="w-48 max-w-[30vw]">
						<img src={book.cover} alt="" class="" class:hidden={!book.cover} />
					</figure>
					<div class="card-body">
						<h2 class="card-title">{book.title || 'Sans nom'}</h2>
						<div class="grow">
							<div class="flex items-center gap-2">
								<progress
									class="progress progress-primary w-full"
									value={book.progress || 0}
									max={book.pagesEnd || 0}
								/>
								<span>{Math.round((book.progress / book.pagesEnd) * 100) || 0}%</span>
							</div>
							<p>
								pages restantes: {book.pagesEnd - book.pagesStart || 0}
							</p>
						</div>
						<div class="flex justify-end items-center gap-4">
							<span>
								<span class="font-semibold text-lg">{book.progress || 0}</span><span class="text-lg"
									>/{book.pagesEnd || 0}</span
								>
							</span>
							<button
								class="btn btn-circle btn-outline"
								on:click={() => {
									book.progress++;
									saveBooks();
								}}
							>
								<svg
									xmlns="http://www.w3.org/2000/svg"
									xmlns:xlink="http://www.w3.org/1999/xlink"
									aria-hidden="true"
									role="img"
									class="iconify iconify--material-symbols"
									width="32"
									height="32"
									preserveAspectRatio="xMidYMid meet"
									viewBox="0 0 24 24"
									><path fill="currentColor" d="M11 19v-6H5v-2h6V5h2v6h6v2h-6v6Z" /></svg
								>
							</button>
						</div>
					</div>
					<button
						class="btn btn-circle btn-ghost absolute top-0 right-0"
						on:click={() => (book.edit = true)}
					>
						<svg
							xmlns="http://www.w3.org/2000/svg"
							xmlns:xlink="http://www.w3.org/1999/xlink"
							aria-hidden="true"
							role="img"
							class="iconify iconify--material-symbols"
							width="16"
							height="16"
							preserveAspectRatio="xMidYMid meet"
							viewBox="0 0 24 24"
							><path
								fill="currentColor"
								d="m19.3 8.925l-4.25-4.2l1.4-1.4q.575-.575 1.413-.575q.837 0 1.412.575l1.4 1.4q.575.575.6 1.388q.025.812-.55 1.387ZM4 21q-.425 0-.712-.288Q3 20.425 3 20v-2.825q0-.2.075-.387q.075-.188.225-.338l10.3-10.3l4.25 4.25l-10.3 10.3q-.15.15-.337.225q-.188.075-.388.075Z"
							/></svg
						>
					</button>
				</div>
			{/if}
		{:else}
			<p class="m-8 text-2xl ">Aucun livre en cours, clique sur le bouton pour en ajouter un.</p>
		{/each}
	</ul>
	<button
		class="btn mx-auto block"
		on:click={() => {
			books.push({
				title: undefined,
				pagesStart: undefined,
				pagesCount: undefined,
				pagesEnd: undefined,
				progress: undefined,
				edit: true,
				cover: undefined
			});
			books = books;
		}}>Ajouter un livre</button
	>
</div>

<Footer />

<Theme />
