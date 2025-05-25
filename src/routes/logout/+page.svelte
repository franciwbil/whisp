<script lang="ts">
    import { goto } from '$app/navigation';
    import { onMount } from 'svelte';
    import { pb, currentUser } from '$lib/pocketbase';

    onMount(() => {
        if ($currentUser) {
            pb.authStore.clear();
            goto('/login');
        }
        }
    )
</script>