<script>
  import { setContext } from "svelte";

  //COMPONENTS//
  import Navbar from "./components/Navbar.svelte";
  import ExpensesList from "./components/ExpensesList.svelte";

  //DATA//
  import expensesData from "./expenses";

  //VARIABLES//
  //on récupère tout le contenu de expenses.js dans un array "expenses"
  let expenses = [...expensesData];

  //FUNCTIONS//
  //retourne un nouvel array "expenses" avec tous les items, sauf celui qu'on veut supprimer et qu'on passe en parametres.
  function removeExpense(id){
    expenses = expenses.filter(item => item.id !== id);
  }

  function clearExpenses(){
    expenses = [];
  }

  //CONTEXT//
  setContext('remove', removeExpense);
  setContext('clearExpenses', clearExpenses)
</script>

<Navbar />
<main class="container">
  <ExpensesList {expenses} />
</main>

<style>
</style>
