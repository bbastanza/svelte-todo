<script lang="ts">
	import { CheckIcon } from 'svelte-feather-icons';
	type TodoItem = {
		hash: string;
		text: string;
		name: string;
		isComplete: boolean;
	};

	let formInput = '';

	let todos: TodoItem[] = [
		{ hash: '1234', name: 'first', text: 'first one', isComplete: false },
		{ hash: '2234', name: 'second', text: 'second one', isComplete: false },
		{ hash: '3234', name: 'three', text: 'third one', isComplete: false },
		{ hash: '4234', name: 'four', text: 'four one', isComplete: false },
		{ hash: '5234', name: 'five', text: 'five one', isComplete: false }
	];

	function toggleTodo(hash: string) {
		const idx = todos.findIndex((t) => t.hash === hash);
		todos[idx].isComplete = !todos[idx].isComplete;
	}

	function addTodo() {
		if (!formInput?.length) {
			return;
		}

		const todo: TodoItem = {
			hash: `${Math.random() * new Date().getSeconds()}`,
			name: formInput,
			text: '',
			isComplete: false
		};

		todos.push(todo);
		todos = todos;

		formInput = '';
	}
</script>

<div class="w-full p-4 rounded">
	<div class="w-1/3 p-4 bg-zinc-700 rounded -mb-0.5">
		<input
			type="text"
			placeholder="Add new todo"
			bind:value={formInput}
			class="w-full rounded ring-0 tracking-wider text-zinc-700 text-lg font-semibold py-2 px-4 outline-none"
			on:keydown={(e) => e.key === 'Enter' && addTodo()}
		/>
	</div>
	<div class="grid grid-cols-12 gap-4 w-full p-4 bg-zinc-700">
		{#each todos as todo (todo.hash)}
			<div
				role="region"
				class="lg:col-span-4 col-span-12 text-yellow-50 p-4 rounded-xl shadow border-4"
				class:border-emerald-300={todo.isComplete}
				class:border-transparent={!todo.isComplete}
				class:bg-amber-100={!todo.isComplete}
				class:bg-emerald-100={todo.isComplete}
			>
				<div class="flex flex-col space-y-2">
					<div class="flex w-full items-center">
						<input
							bind:value={todo.name}
							class="font-semibold bg-zinc-800 text-yellow-50 focus:bg-zinc-100 focus:text-zinc-800 w-full ring-0 outline-none p-2 rounded text-xl"
						/>
						<div>
							<button
								on:click={() => toggleTodo(todo.hash)}
								class="rounded-full text-emerald-800 font-bold h-12 w-12 bg-gray-200 text-lg ml-2 ring-0 outline-none border-2 flex justify-center items-center"
								class:border-emerald-700={todo.isComplete}
								class:border-zinc-700={!todo.isComplete}
							>
								{#if todo.isComplete}
									<CheckIcon />
								{/if}
							</button>
						</div>
					</div>
					<textarea
						class="bg-zinc-800 text-yellow-50 focus:bg-zinc-100 focus:text-zinc-800 text-base px-2 py-1 rounded"
						bind:value={todo.text}
					/>
				</div>
			</div>
		{/each}
	</div>
</div>
