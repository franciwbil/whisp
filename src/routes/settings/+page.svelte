<script lang="ts">
    import { goto } from '$app/navigation';
    import { onMount } from 'svelte';
    import { pb, currentUser } from '$lib/pocketbase';

    onMount(() => {
        if (!$currentUser) goto('/login')
        }
    )

    let username: string;
    let display: string;
    let oldPassword: string;
    let password: string;
    let passwordConfirm: string;

    async function updateDisplay() {
        if ($currentUser?.id) {
            const data = {
                display
            };
            await pb.collection('users').update($currentUser.id, data);
        }
    }

    async function updateUsername() {
        if ($currentUser?.id) {
            const data = {
                username,
            };
            await pb.collection('users').update($currentUser.id, data);
        }
    }

    async function updatePassword() {
        if ($currentUser?.id) {
            const data = {
                oldPassword,
                password,
                passwordConfirm,
            };
            await pb.collection('users').update($currentUser.id, data);
            await pb.authStore.clear();
            goto("/login");
        }
    }
</script>   

{#if $currentUser}
    <div class="flex flex-col h-screen">
        <nav class="bg-gray-300 h-15 sticky p-4 shadow-md">    
            <a href="/chat">whisp</a>
            <span class="float-right">
                <p>@{$currentUser.username}</p>
            </span>
        </nav>
        <div class="flex flex-row w-screen">
            <form class="flex flex-col bg-gray-300" on:submit|preventDefault={updateDisplay}>
                <input
                    placeholder="Change Display"
                    type="text"
                    minlength="2"
                    maxlength="32"
                    class="text-center"
                    bind:value={display}
                />
                <input
                    value="Update Display"
                    type="submit"
                />
            </form>
            <form class="flex flex-col bg-gray-300" on:submit|preventDefault={updateUsername}>
                <input
                    placeholder="Change Username"
                    type="text"
                    minlength="2"
                    maxlength="32"
                    class="text-center"
                    bind:value={username}
                />
                <input
                    value="Update Username"
                    type="submit"
                />
            </form>
            <form class="flex flex-col bg-gray-300" on:submit|preventDefault={updatePassword}>
                <input
                    placeholder="Old Password"
                    type="password"
                    minlength="2"
                    maxlength="32"
                    class="text-center"
                    bind:value={oldPassword}
                />
                <input
                    placeholder="New Password"
                    type="password"
                    minlength="2"
                    maxlength="32"
                    class="text-center"
                    bind:value={password}
                />
                <input
                    placeholder="Confirm Password"
                    type="password"
                    minlength="2"
                    maxlength="32"
                    class="text-center"
                    bind:value={passwordConfirm}
                />
                <input
                    value="Update Password"
                    type="submit"
                />
            </form>
        </div>
    </div>
{:else}
    <p>redirecting</p>
{/if}