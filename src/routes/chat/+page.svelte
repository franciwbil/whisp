<script lang="ts">
    import { pb, currentUser } from '$lib/pocketbase';
    import { onMount } from 'svelte';
    import type { RecordModel } from "pocketbase";

    let channels: RecordModel[] = [];

    onMount(async () => {
        const resultList = await pb.collection('channels').getList(1, 100, {
            sort: 'updated',
            expand: 'members',
        });
        channels = resultList.items;
    })
</script>

<div class="flex flex-col h-screen">
    <nav class="bg-gray-300 h-15 sticky p-4 shadow-md">
        <a href="./chat">whisp</a>
        <span class="float-right">
            {#if $currentUser}
                <a href="/settings">{$currentUser.username}
            </a>
            {:else}
                <a href="/login" class="text-sm">login</a>
            {/if}
        </span>
    </nav>
    <div id="sidebar" class="bg-gray-300 h-full w-55 flex flex-col">
        <ul class="overflow-scroll p-3">
            {#each channels as channel (channel.name)}
                <li class="bg-blue-500 my-2 p-2 rounded-md truncate">
                    <a href="./chat">{channel.name}</a>
                </li>
            {/each}
        </ul>
    </div>
</div>  
