<script lang="ts">
    import { goto } from '$app/navigation';
    import { onMount } from 'svelte';
    import { pb, currentUser } from '$lib/pocketbase';

    onMount(() => {
        if ($currentUser) goto('/chat')
        }
    )

    let email: string;
    let password: string;

    async function login() {
        try
        { 
            await pb.collection('users').authWithPassword(email, password);
            goto('/chat');
        } 
        catch (err)
        {
            console.error(err);
        }
    }
    
</script>

{#if $currentUser}
    <p>logged in as {$currentUser.username}</p>
{:else}
    <form>
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

        <button on:click={login}>login</button>
        <a href="/register">register</a>
    </form>
{/if}