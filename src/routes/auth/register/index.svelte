<script context="module" lang="ts">
	export const load: Load = ({ session, props }) => {
		if (session.user) {
			return {
				status: 302,
				redirect: '/'
			};
		}
		return { props };
	};
</script>

<script lang="ts">
	import { send } from '$lib/api';
	import type { Load } from '@sveltejs/kit';

	export let error: string;
	export let success: string;

	async function register(event: SubmitEvent) {
		error = '';

		const formEl = event.target as HTMLFormElement;
		const response = await send(formEl);

		if (response.error) {
			error = response.error;
		}
		if (response.success) {
			success = response.success;
		}
		formEl.reset();
	}
</script>

register
<form on:submit|preventDefault={register} method="POST" autocomplete="off">
	<div>
		<label for="username">Username</label>
		<input type="text" name="username" required />
	</div>
	<div>
		<label for="password">Password</label>
		<input type="password" name="password" id="password" required />
	</div>
	{#if error}
		<p class="error">{error}</p>
	{/if}
	{#if success}
		<div>
			<p>Thank you for signing up</p>
			<p>
				<a href="/auth/login">You can log in.</a>
			</p>
		</div>
	{/if}
	<button type="submit">Sign up</button>
</form>

<style>
	.error {
		color: tomato;
	}
</style>
