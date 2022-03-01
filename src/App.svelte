<script>
  import { setContext } from "svelte";

  //COMPONENTS//
  import Navbar from "./components/Navbar.svelte";
  import ExpensesList from "./components/ExpensesList.svelte";
  import Totals from "./components/Totals.svelte";

  //DATA//
  import expensesData from "./expenses";

  //VARIABLES//
  //on récupère tout le contenu de expenses.js dans un array "expenses"
  let expenses = [...expensesData];

  //REACTIVE//
  //Variables qui dépendent d'autres variables
  //accumulator = somme précédente à laquelle se rajoute la somme courante de l'item courant
  $: total = expenses.reduce((accumulator,current)=>{
    return (accumulator += current.amount)
  },0);

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
  <Totals {total} />
  <ExpensesList {expenses} />
</main>

<style>
</style>
