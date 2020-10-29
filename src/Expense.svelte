<script>
  import { getContext } from "svelte";
  export let id;
  export let name = "";
  export let amount = 0;
  let displayAmount = false;

  function toggleAmount() {
    displayAmount = !displayAmount;
  }
  const removeExpense = getContext("remove")
</script>

<article class="single-expense">
  <div class="expense-info">
    <h2>
      {name}
      <button class="amount-btn" on:click="{toggleAmount}">
        <i class="fas fa-caret-down" />
      </button>
    </h2>
    {#if displayAmount }
    <h4>amount: ${amount}</h4>
    {/if}
  </div>
  <div class="expense-buttons" on:click={() => removeExpense(id)} >
    <buton class="expense-btn edit-btn">
      <i class="fas fa-pen" />
    </buton>
    <buton class="expense-btn delete-btn">
      <i class="fas fa-trash" />
    </buton>
  </div>
</article>

<!--
another way:
<script>
  export let expense;
  let {name, amount, id} = expense
</script>

<div>
  <h2>title : {name}</h2>
  <h2>amount : {amount}</h2>
</div>
 -->

<!-- another way:
<script>
  export let expense;
</script>

<div>
  <h2>title : {expense.name}</h2>
  <h2>amount : {expense.amount}</h2>
</div>
 -->
