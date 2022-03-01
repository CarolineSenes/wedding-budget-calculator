<script>
  import Title from "./Title.svelte";
  let name = "";
  let amount = null;

  //on exporte pour pouvoir modifier de l'extérieur du component (depuis App)
  export let addExpense;

  //isEmpty est true si name ou amount est false (ex: name=true, !name=false)
  //on utilise une variable réactive puisque sa valeur dépend d'autres variables (name et amount)
  $: isEmpty = !name || !amount;

  function handelSubmit() {
    addExpense({ name, amount });
    name = "";
    amount = null;
  }
</script>

<section>
  <div class="modal-header">
    <Title title="Ajouter une dépense" />
    <button
      type="button"
      class="btn-close"
      data-bs-dismiss="modal"
      aria-label="Close"
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
        <p class="form-text text-danger">Merci de compléter tous les champs.</p>
      {/if}
      <button type="submit" class="btn btn-primary" disabled={isEmpty}
        >Ajouter la dépense</button
      >
    </div>
  </form>
</section>

<style>
  section {
    border: 1px solid black;
    border-radius: 10px;
  }
</style>
