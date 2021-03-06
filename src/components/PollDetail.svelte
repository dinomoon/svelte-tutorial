<script>
  import PollStore from '../stores/PollStore';
  import Card from '../shared/Card.svelte';
  
  export let poll;

  // reactive values
  $: totalVotes = poll.votesA + poll.votesB;
  $: percentA = Math.floor(poll.votesA / totalVotes * 100);
  $: percentB = Math.floor(poll.votesB / totalVotes * 100);

  // handling votes
  const handleVote = (option, id) => {
    PollStore.update(currentPolls => {
      let copiedPolls = [...currentPolls];
      let upvotedPoll = copiedPolls.find(poll => poll.id === id);

      if (option === 'a') {
        upvotedPoll.votesA++;
      }
      if (option === 'b') {
        upvotedPoll.votesB++;
      }

  		return copiedPolls;
    })
  }
</script>

<Card>
  <div class="poll">
    <h2>{poll.question}</h2>
    <p>Total votes: {totalVotes}</p>
    <div class="answer" on:click={() => handleVote('a', poll.id)}>
      <div class="percent percent-a" style="width: {percentA}%"></div>
      <span>{poll.answerA} ({poll.votesA})</span>
    </div>
    <div class="answer" on:click={() => handleVote('b', poll.id)}>
      <div class="percent percent-b" style="width: {percentB}%"></div>
      <span>{poll.answerB} ({poll.votesB})</span>
    </div>
  </div>
</Card>

<style>
  h2 {
    font-size: 1.4rem;
    margin: 0 auto;
    color: #555;
  }

  p {
    margin-top: 6px;
    font-size: 14px;
    color: #aaa;
    margin-bottom: 30px;
  }

  .answer {
    background: #fafafa;
    cursor: pointer;
    margin: 10px auto;
    position: relative;
  }

  .answer:hover {
    opacity: 0.6;
  }

  span {
    display: inline-block;
    padding: 10px 20px;
  }

  .percent {
    height: 100%;
    position: absolute;
    box-sizing: border-box;
  }

  .percent-a {
    border-left: 4px solid #d91b42;
    background-color: rgba(217, 27, 66, 0.2);
  }

  .percent-b {
    border-left: 4px solid #45c496;
    background-color: rgba(69, 196, 150, 0.2);
  }
</style>