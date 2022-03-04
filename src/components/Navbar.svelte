<script>
  export let showForm;
</script>

<nav class="container-fluid flex-column flex-sm-row navbar mb-5">
  <h1>Budget Mariage</h1>
  <form class="d-flex">
    <button
      class="btn btn-outline me-2 d-flex align-items-center"
      type="button"
      on:click={showForm}
    >
      <i class="bi bi-plus-square me-2" />
      <span>Ajouter une dépense</span>
    </button>
  </form>
</nav>

<style>
  .btn {
  }
</style>
