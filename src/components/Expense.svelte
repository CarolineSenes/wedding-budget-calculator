<script>
  import { getContext } from "svelte";
  import { fade, blur, slide, scale, fly } from "svelte/transition";

  //on exporte pour pouvoir modifier la valeur de chaque "expense" de l'extérieur du component (depuis ExpensesList)
  export let expense;

  let displayAmount = false;

  function toogleAmount() {
    displayAmount = !displayAmount;
  }

  const removeExpense = getContext("remove");
  const modifiedExpense = getContext("modify");
</script>

<article class="card p-3 my-3">
  <div class="d-flex">
    <div class="me-auto">
      <h3 class="d-inline">{expense.name}</h3>
      <button class="mx-2" on:click={toogleAmount}
        ><i class="bi bi-caret-down-fill" /></button
      >
    </div>
    <div>
      <button class="btn-edit text-center" on:click={() => modifiedExpense(expense.id)}
        ><i class="bi bi-pencil-fill" /></button
      >
      <button class="btn-delete text-center" on:click={() => removeExpense(expense.id)}
        ><i class="bi bi-trash3-fill" /></button
      >
    </div>
  </div>

  {#if displayAmount}
    <p class="card-body fs-5" transition:slide>
      <i class="bi bi-arrow-right-circle-fill" />{expense.amount}€
    </p>
  {/if}
</article>

<style>
  article{
    background-color: var(--white);
  }
  button {
    all: unset;
    cursor: pointer;
  }
  .btn-edit, .bi-arrow-right-circle-fill{
    margin-right: 1em;
  }
  .btn-delete, .btn-edit{
    transition: all .2s ease-in-out;
  }
  .btn-delete:hover, .btn-edit:hover{
    transform: scale(1.2);
  }
</style>
