<script lang="ts">
    import { pb, currentUser } from '$lib/pocketbase';
</script>

{#if $currentUser}
    <p>logged in as {$currentUser.username}</p>
{:else}
    <p>not logged in</p>
{/if}