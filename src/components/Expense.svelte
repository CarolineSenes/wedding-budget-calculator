<script>
  import { getContext } from "svelte";
  import { fade, blur, slide, scale, fly } from 'svelte/transition';
  
  //on exporte pour pouvoir modifier la valeur de chaque "expense" de l'extérieur du component (depuis ExpensesList)
  export let expense;

  let displayAmount = false;

  function toogleAmount() {
    displayAmount = !displayAmount;
  }

  const removeExpense = getContext('remove');
  const modifiedExpense = getContext('modify');
</script>

<article class="card p-3 m-3">
  <div class="d-flex">
    <div class="me-auto">
      <h3 class="d-inline">{expense.name}</h3>
      <button class="mx-2" on:click={toogleAmount}
        ><i class="bi bi-caret-down-fill" /></button
      >
    </div>
    <div>
      <button on:click={() => modifiedExpense(expense.id)}><i class="bi bi-pencil-fill text-success mx-2" /></button>
      <button on:click={() => removeExpense(expense.id)}
        ><i class="bi bi-trash3-fill text-danger" /></button
      >
    </div>
  </div>

  {#if displayAmount}
    <div class="card-body" transition:slide>
      {expense.amount}€
    </div>
  {/if}
</article>

<style>
  button {
    all: unset;
  }
</style>
