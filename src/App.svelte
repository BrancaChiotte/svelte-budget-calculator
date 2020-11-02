<script>
  // components
  import Navbar from "./Navbar.svelte";
  import ExpensesList from './ExpensesList.svelte';
  import Totals from './Totals.svelte';
  import ExpenseForm from './ExpenseForm.svelte';
  // import Github from "./Github.svelte";
  import GithubAwait from "./GithubAwait.svelte";

  // modal
  import Modal from './Modal.svelte';

  // data
    //if you want to export from a js file
  // import expensesData from './expenses';

  //  general imports
  import { setContext, onMount,  onDestroy, afterUpdate } from "svelte";


  // variables
  let expenses = [];
    //if you want to export from a js file
    // let expenses = [...expensesData];

  // set editing variables
  let setName = "";
  let setAmount = null;
  let setId = null;

  // toggle form variables
  let isFormOpen = false;

  //reactive
  $: isEditing = setId ? true : false ;

  $: total = expenses.reduce((acc,curr)=>{
    return (acc += curr.amount)
  },0);

  // functions
  function showForm () {
    isFormOpen = true;
  }

  function hideForm () {
    isFormOpen = false;
    let setName = "";
    let setAmount = null;
    let setId = null;
  }

  function removeExpense(id) {
    expenses = expenses.filter(item => item.id !== id);
  }

  function clearExpenses() {
    expenses = [];
  }

  function addExpense({ name, amount }) {
    let expense = { id: Math.random() * Date.now(), name, amount };
    expenses = [expense, ...expenses];
  }

  function setModifiedExpense(id) {
    let expense = expenses.find(item => item.id === id);
    setId = expense.id;
    setName = expense.name;
    setAmount = expense.amount;
    showForm();
  }
  function editExpense ({name, amount}) {
    expenses = expenses.map(item => {
      return item.id === setId? {...item, name, amount} : {...item};
  });
    setId = null;
    setAmount = null;
    setName = "";
  }

  // context
  setContext("remove", removeExpense);
  setContext("modify", setModifiedExpense);

  // local storage
  function setLocalStorage () {
   localStorage.setItem('expenses', JSON.stringify(expenses));
  }

  onMount (() =>{
    expenses = localStorage.getItem("expenses") ? JSON.parse(localStorage.getItem("expenses")) : [];
  });

  afterUpdate(() => {
    setLocalStorage();
  });
  $: console.log({ setName, setAmount });
</script>

<Navbar {showForm} />
<main class="content">
  <!-- <Github /> -->
  <!-- <GithubAwait /> -->
  {#if isFormOpen}
    <Modal>
      <ExpenseForm name={setName} amount={setAmount} {addExpense} {editExpense} {isEditing} {hideForm} />
    </Modal>
  {/if}
  <Totals title="total expenses" {total} />
  <ExpensesList {expenses} />
  <button type="button" class="btn btn-primary btn-block" on:click={clearExpenses}>
    clear expenses
  </button>
</main>
