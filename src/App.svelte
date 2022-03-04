<script>
  import { setContext, onMount, afterUpdate } from 'svelte';

  //COMPONENTS//
  import Navbar from "./components/Navbar.svelte";
  import ExpensesList from "./components/ExpensesList.svelte";
  import Totals from "./components/Totals.svelte";
  import ExpenseForm from "./components/ExpenseForm.svelte";
  import Footer from "./components/Footer.svelte";

  //VARIABLES//
  //on récupère tout le contenu de expenses.js dans un array "expenses"
  let expenses = [];

  //SET EDITING VARIABLES (for edit expense)//
  let setName = "";
  let setAmount = null;
  let setId = null;

  //TOOGLE FORM VARIABLES//
  //état de départ de ExpenseForm
  let isFormOpen = false;

  //REACTIVE//
  //Variable qui dépend d'autres variables

  //accumulator = somme précédente à laquelle se rajoute la somme courante de l'item courant
  $: total = expenses.reduce((accumulator, current) => {
    return (accumulator += current.amount);
  }, 0);

  //sert à savoir si on est en mode "edit" ou non, pour applique une fonction différente au btn submit
  $: isEditing = setId ? true : false;

  //FUNCTIONS//
  //affiche ExpenseForm
  function showForm() {
    isFormOpen = true;
  }

  //cache ExpenseForm et réinitialise les éventuelles valeurs saisies pour qu'elles ne se réaffiche pas 
  //à la prochaine ouverture de ExpenseForm
  function hideForm() {
    isFormOpen = false;
    setName = "";
    setAmount = null;
    setId = null;
  }

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

  //cherche la dépense correspondante à l'id entré en paramètre
  //et attribue les valeurs aux variables déclarées dans la partie SET EDITING VARIABLES
  function setModifiedExpense(id) {
    let expense = expenses.find((item) => item.id === id);
    setId = expense.id;
    setName = expense.name;
    setAmount = expense.amount;
    showForm();
  }

  //créé un nouveau array "expenses" et itère sur chaque item pour trouver celui
  //dont l'id correspond à setId. S'il correspond il retourne l'item, sinon il fait pareil ^^.
  //Puis supprime toutes ses valeurs.
  function editExpense({ name, amount }) {
    expenses = expenses.map((item) => {
      return item.id === setId ? { ...item, name, amount } : { ...item };
    });
    setId = null;
    setName = "";
    setAmount = null;
  }

  //CONTEXT//
  setContext("remove", removeExpense);
  setContext("clearExpenses", clearExpenses);
  setContext("modify", setModifiedExpense);

  //LOCALSTORAGE
  //envoie le contenu de l'array 'expenses' dans le local storage après les avoir convertis en string.
  //La clé des données sera 'expenses'.
  function setLocalStorage(){
    localStorage.setItem('expenses', JSON.stringify(expenses))
  }

  //au montage du component, si il y a qlqch dans le localstorage, on transfert les données l'array "expenses" après les avoir 
  //convertit en JSON, sinon on retourne un array vide
  //cela permet de récupérer à l'écran les dépenses contenues dans le local storage
  onMount(() => {
		expenses = localStorage.getItem('expenses')
    ? JSON.parse(localStorage.getItem('expenses'))
    : []
	});

  //setlocalstorage est lancer pendant l'afterupdate. Ca évite d'écrire la function dans removeExpense, clearExpenses, addExpense,
  // et editExpense
  afterUpdate(()=>{
    setLocalStorage();
  })
</script>

<Navbar {showForm} />
<main class="container">
  {#if isFormOpen}
    <ExpenseForm
      {addExpense}
      name={setName}
      amount={setAmount}
      {isEditing}
      {editExpense}
      {hideForm}
    />
  {/if}
  <Totals {total} />
  <ExpensesList {expenses} />
</main>
<Footer />


<style>
</style>
