<script lang="ts">
	import { goto } from '$app/navigation';
	import axios from 'axios';
	import { onMount } from 'svelte';
	import { config } from '../../internal/config';

	let email: string = '';
	let username: string = '';
	let password: string = '';
	let status: string = '';
	let emailRef: any;

	async function register() {
		status = '';
		try {
			const res = await axios.post(`${config.apiUrl}/user/register`, {
				email,
				username,
				password,
			});
			const data = res.data;

			if (data.success) {
				goto('/login');
			}
		} catch (error) {
			status = error.response.data.error;
		}
	}

	onMount(() => {
		if (emailRef) {
			emailRef.focus();
		}
	});

	function onChange() {
		status = '';
	}
</script>

<svelte:head>
	<title>Register | GO-URL Shortener</title>
</svelte:head>

<div class="flex justify-center w-screen h-screen">
	<div class="flex justify-center w-full p-2 mt-32 space-y-2">
		<div class="w-1/5 min-w-[300px] p-2 space-y-2 h-96">
			<div class="text-4xl font-bold text-center">Register</div>
			<form action="" class="space-y-4">
				<input
					type="text"
					placeholder="Email"
					bind:value={email}
					on:input={onChange}
					bind:this={emailRef}
					class="w-full p-2 border border-gray-500 focus:outline-none focus:border-green-500"
				/>
				<input
					type="text"
					placeholder="Username"
					bind:value={username}
					on:input={onChange}
					class="w-full p-2 border border-gray-500 focus:outline-none focus:border-green-500"
				/>
				<input
					type="password"
					placeholder="Password"
					bind:value={password}
					on:input={onChange}
					class="w-full p-2 border border-gray-500 focus:outline-none focus:border-green-500"
				/>
				<div>
					{#if status}
						<div class="text-xs text-center text-red-500">
							{status}
						</div>
					{/if}
					<button
						type="submit"
						class="w-full p-2 font-semibold text-white bg-green-500 hover:bg-green-600"
						on:click|preventDefault={register}>Register</button
					>
				</div>
				<div class="text-center">
					<span>Have an account? </span>
					<span class="font-semibold text-green-500 hover:underline"
						><a href="/login">login</a></span
					>
				</div>
			</form>
		</div>
	</div>
</div>
