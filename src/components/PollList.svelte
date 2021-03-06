<script>
  import PollStore from '../stores/PollStore.js';
  import PollDetail from './PollDetail.svelte';
</script>

<div class="poll-list">
  {#each $PollStore as poll (poll.id)}
    <PollDetail {poll} />
  {/each}
</div>

<style>
  .poll-list {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-gap: 20px;
  }
</style>
