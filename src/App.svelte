<script>

	let setName = ''
	let setAmount = null
	let setId = null
	let isFormOpen = false

	import {setContext, onMount, afterUpdate } from 'svelte'

	const removeExpense = (id) => {
		expenses = expenses.filter(item => item.id !== id)
		// setLocalStorage()
	}

	const clearExpenses = () => {
		expenses = []
		// setLocalStorage()
	}

	const addExpense = ({name, amount}) => {
		let expense = {
			id: Math.random() * Date.now(),
			name,
			amount
		}

		expenses = [expense, ...expenses]
		// setLocalStorage()
	}

	const setModifiedExpense = (id) => {
		let expense = expenses.find(expense => expense.id === id)
		setId = expense.id
		setName = expense.name
		setAmount = expense.amount
		showForm()
	}

	const editExpense = ({name, amount}) => {
		expenses = expenses.map(item => {
			return item.id === setId 
			? {...item, name, amount}
			: {...item}
		})
		setId = null
		setAmount = null
		setName = ''
	}

	const showForm = () => {
		isFormOpen = true
	}

	const hideForm = () => {
		isFormOpen = false
		setName = ''
		setAmount = null
		setId = null
	}

	
	$: isEditing = setId ? true : false
	$: total = expenses.reduce((acc, curr) => {
		return (acc += curr.amount)
	}, 0)

	import Navbar from './components/Navbar.svelte'
	import ExpenseList from './components/ExpenseList.svelte'
	import Totals from './components/Totals.svelte'
	import ExpenseForm from './components/ExpenseForm.svelte'
	import expenseData from './expenses'
	import Modal from './components/Modal.svelte'

	let expenses = []

	setContext('remove', removeExpense)
	setContext('modify', setModifiedExpense)

	const setLocalStorage = () => {
		localStorage.setItem('expenses', JSON.stringify(expenses))
	}

	onMount(() => {
		expenses = localStorage.getItem('expenses') 
			? JSON.parse(localStorage.getItem('expenses')) 
			: []
	})

	afterUpdate(() => {
		console.log("After")
		setLocalStorage()
	})

</script>

<Navbar {showForm}/>
<main class="content">
	{#if isFormOpen}
		<Modal>
			<ExpenseForm {addExpense} name={setName} amount={setAmount} {isEditing} {editExpense} {hideForm} />
		</Modal>
	{/if}
	<Totals title="total expenses" {total} />
	<ExpenseList {expenses} />
	<button type="button" class="btn btn-primary btn-block" on:click={clearExpenses}>
		clear expenses
	</button>
</main>