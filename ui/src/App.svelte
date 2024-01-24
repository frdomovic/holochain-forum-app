<script lang="ts">
  import { onMount, setContext } from 'svelte';
  import type { ActionHash, AppAgentClient } from '@holochain/client';
  import { AppAgentWebsocket } from '@holochain/client';
  import '@material/mwc-circular-progress';

  import { clientContext } from './contexts';
  import AllPosts from './forum/posts/AllPosts.svelte';
  import CreatePost from './forum/posts/CreatePost.svelte';

  let client: AppAgentClient | undefined;

  let loading = true; 


  onMount(async () => {
    // We pass '' as url because it will dynamically be replaced in launcher environments
    client = await AppAgentWebsocket.connect('', 'my_forum_app');

    loading = false;
  });

  setContext(clientContext, {
    getClient: () => client,
  });
</script>

<main>
  {#if loading}
    <div style="display: flex; flex: 1; align-items: center; justify-content: center">
      <mwc-circular-progress indeterminate />
    </div>
  {:else}
    <div id="content" style="display: flex; flex-direction: column; flex: 1;">
     <CreatePost></CreatePost>
    <AllPosts></AllPosts>
    </div>
  {/if}
</main>

<style>
  main {
    text-align: center;
    padding: 1em;
    max-width: 240px;
    margin: 0 auto;
  }

  @media (min-width: 640px) {
    main {
      max-width: none;
    }
  }
</style>
