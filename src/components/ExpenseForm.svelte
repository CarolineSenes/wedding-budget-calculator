<script>
  import Title from "./Title.svelte";

  //on exporte pour pouvoir modifier de l'extérieur du component (depuis App)
  export let addExpense;
  export let isEditing;
  export let editExpense;
  export let hideForm;
  export let name = "";
  export let amount = null;

  //isEmpty est true si name ou amount est false (ex: name=true, !name=false)
  //on utilise une variable réactive puisque sa valeur dépend d'autres variables (name et amount)
  $: isEmpty = !name || !amount;

  function handelSubmit() {
    if (isEditing) {
      editExpense({ name, amount });
    } else {
      addExpense({ name, amount });
    }
    name = "";
    amount = null;
    hideForm();
  }
</script>

<section class="shadow ">
  <div class="modal-header">
    <Title title="Ajouter une dépense" />
    <button
      type="button"
      class="btn-close rounded-circle"
      data-bs-dismiss="modal"
      aria-label="Close"
      on:click={hideForm}
    />
  </div>
  <form class="modal-body" on:submit|preventDefault={handelSubmit}>
    <div>
      <label for="name" class="form-label">Nom</label>
      <input class="form-control" type="text" id="name" bind:value={name} />
    </div>
    <div>
      <label for="amount" class="form-label">Montant</label>
      <input
        class="form-control"
        type="number"
        id="amount"
        bind:value={amount}
      />
    </div>
    <div class="text-center mt-4">
      {#if isEmpty}
        <p class="form-text">Merci de compléter tous les champs.</p>
      {/if}
      <button type="submit" class="btn" disabled={isEmpty}>
        {#if isEditing}
          Editer la dépense
        {:else}
          Ajouter la dépense
        {/if}
      </button>
    </div>
  </form>
</section>

<style>
  section {
    background-color: var(--primary-color);
  }
  label{
    font-weight: 700;
    color: var(--secondary-color);
  }
  .form-text{
    color: var(--white);
  }
  .btn-close{
    padding: 1em;
    background-color: var(--secondary-color);
  }
  .btn:hover{
    color: var(--secondary-color);
  }
</style>
