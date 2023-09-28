<template>
	<app-header />

	<app-filters :activeFilter="activeFilter" @setFilter="setFilter" />

	<main class="app-main">
		<app-todo-list
			:todos="filteredTodos"
			@toggleTodo="toggleTodo"
			@removeTodo="removeTodo"
		/>

		<app-add-todo @addTodo="addTodo" />
	</main>

	<app-footer :stats="stats" />
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import AppHeader from './components/AppHeader.vue';
import AppFooter, { Stats } from './components/AppFooter.vue';
import AppFilters from './components/AppFilters.vue';

import AppAddTodo from './components/AppAddTodo.vue';
import AppTodoList from './components/AppTodoList.vue';
import { Todo } from './types/Todo';
import { Filter } from './types/Filter';

interface State {
	todos: Todo[];
	activeFilter: Filter;
}

export default defineComponent({
	components: {
		AppHeader,
		AppFooter,
		AppFilters,
		AppTodoList,
		AppAddTodo,
	},

	data(): State {
		return {
			todos: [],
			activeFilter: 'All',
		};
	},

	computed: {
		filteredTodos(): Todo[] {
			switch (this.activeFilter) {
				case 'Active':
					return this.activeTodos;
				case 'Done':
					return this.doneTodos;
				case 'All':
				default:
					return this.todos;
			}
		},
		stats(): Stats {
			return {
				active: this.activeTodos.length,
				done: this.doneTodos.length,
			};
		},
		activeTodos(): Todo[] {
			return this.todos.filter((todo) => !todo.isCompleted);
		},
		doneTodos(): Todo[] {
			return this.todos.filter((todo) => todo.isCompleted);
		},
	},

	methods: {
		addTodo(todo: Todo) {
			this.todos.push(todo);
		},
		toggleTodo(id: number) {
			const targetTodo = this.todos.find((todo: Todo) => todo.id == id);
			if (targetTodo) {
				targetTodo.isCompleted = !targetTodo.isCompleted;
			}
		},
		removeTodo(id: number) {
			// const indexToRemove = this.todos.findIndex((todo:Todo) => todo.id === id);
			// if (indexToRemove !== -1) {
			// 	this.todos.splice(indexToRemove, 1); // Удаляем объект из массива
			// }

			this.todos = this.todos.filter((todo: Todo) => todo.id !== id);
		},
		setFilter(filter: Filter) {
			this.activeFilter = filter;
		},
	},
});
</script>
