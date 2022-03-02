<script>
  import { setContext } from "svelte";

  //COMPONENTS//
  import Navbar from "./components/Navbar.svelte";
  import ExpensesList from "./components/ExpensesList.svelte";
  import Totals from "./components/Totals.svelte";
  import ExpenseForm from "./components/ExpenseForm.svelte";

  //DATA//
  import expensesData from "./expenses";

  //VARIABLES//
  //on récupère tout le contenu de expenses.js dans un array "expenses"
  let expenses = [...expensesData];

  //SET EDITING VARIABLES (for edit expense)//
  let setName = "";
  let setAmount = null;
  let setId = null;

  //REACTIVE//
  //Variable qui dépend d'autres variables

  //accumulator = somme précédente à laquelle se rajoute la somme courante de l'item courant
  $: total = expenses.reduce((accumulator, current) => {
    return (accumulator += current.amount);
  }, 0);

  //sert à savoir si on est en mode "edit" ou non, pour applique une fonction différente au btn submit
  $: isEditing = setId ? true : false;

  //FUNCTIONS//
  //retourne un nouvel array "expenses" avec tous les items, sauf celui qu'on veut supprimer et qu'on passe en parametres.
  function removeExpense(id) {
    expenses = expenses.filter((item) => item.id !== id);
  }

  function clearExpenses() {
    expenses = [];
  }

  //va créer un nouvel object pour la dépense
  function addExpense({ name, amount }) {
    let expense = {
      id: Math.random() * Date.now(),
      name: name,
      amount: amount,
    };
    //on ajoute dans l'array "expenses" la nouvelle dépense
    expenses = [expense, ...expenses];
  }

  //cherche le dépense correspondante à l'id entrée en paramètre
  //et attribue les valeurs aux variables déclarées dans la partie SET EDITING VARIABLES
  function setModifiedExpense(id) {
    let expense = expenses.find((item) => item.id === id);
    setId = expense.id;
    setName = expense.name;
    setAmount = expense.amount;
  }

  //créé un nouveau array "expenses" et itère sur chaque item pour trouver celui
  //dont l'id correspond à setId. S'il correspond il retourne l'item, sinon il fait pareil ^^.
  //Puis supprime toutes ses valeurs.
  function editExpense({name, amount}){
    expenses = expenses.map(item=>{
      return item.id === setId ? {...item, name, amount} : {...item};
    })
    setId = null;
    setName = '';
    setAmount = null;

  }

  //CONTEXT//
  setContext("remove", removeExpense);
  setContext("clearExpenses", clearExpenses);
  setContext("modify", setModifiedExpense);
</script>

<Navbar />
<main class="container">
  <ExpenseForm {addExpense} name={setName} amount={setAmount} {isEditing} {editExpense}/>
  <Totals {total} />
  <ExpensesList {expenses} />
</main>

<style>
</style>
