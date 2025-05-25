<script lang="ts">
    import { goto } from '$app/navigation';
    import { onMount } from 'svelte';
    import { pb, currentUser } from '$lib/pocketbase';

    onMount(() => {
        if ($currentUser) goto('/chat')
        }
    )

    let username: string;
    let display: string;
    let email: string;
    let password: string;

    async function login() {
        pb.collection('users').authWithPassword(email, password);
    }

    async function register() {
        try { 
            const data = {
                username,
                display,
                email,
                password,
                passwordConfirm: password
            }
            await pb.collection('users').create(data);
            await login();
            } catch (err) {
            console.error(err);
        }
    }

</script>

{#if $currentUser}
    <p>logged in as {$currentUser.username}</p>
{:else}
    <form on:submit|preventDefault>
        <input
            placeholder="display" 
            type="text"
            bind:value={display}
        />

        <input
            placeholder="username" 
            type="text"
            bind:value={username}
        />

        <input
            placeholder="email" 
            type="text"
            bind:value={email}
        />

        <input
            placeholder="password" 
            type="password"
            bind:value={password}
        />

        <button on:click={register}>register</button>
        <a href="/login">login</a>
    </form>
{/if}