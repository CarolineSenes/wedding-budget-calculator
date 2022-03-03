<script>
  import Title from "./Title.svelte";
  import Expense from "./Expense.svelte";
  import { getContext } from "svelte";
  import { fly } from "svelte/transition";
  import { flip } from 'svelte/animate';

  //on exporte pour pouvoir modifier l'array "expenses" de l'extérieur du component (depuis App)
  //la valeur par défaut sera un array vide
  export let expenses = [];

  const clearExpenses = getContext("clearExpenses");
</script>

<section class="container">
  <Title title="Liste des dépenses" />

  <ul>
    {#each expenses as expense, index (expense.id)}
      <div in:fly={{x:200, delay:index*150}} out:fly={{x:-200}} animate:flip>
        <Expense {expense} />
      </div>
    {:else}
      <p>La liste des dépenses est actuellement vide</p>
    {/each}
  </ul>
  <div class="text-center">
    <button
      class="btn btn-outline-danger mt-3"
      type="button"
      on:click={clearExpenses}>Supprimer les dépenses</button
    >
  </div>
</section>

<style>
</style>
