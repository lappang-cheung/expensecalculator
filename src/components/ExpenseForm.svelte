<script>
    // import { onMount, onDestroy, beforeUpdate, afterUpdate } from 'svelte'
    import Title from './Title.svelte'

    export let name = ''
    export let amount = null
    export let addExpense
    export let isEditing
    export let editExpense
    export let hideForm
    
    $: isEmpty = !name || !amount

    const handleSubmit = () => {
        if(isEditing) {
            editExpense({name, amount})
        } else {
            addExpense({name, amount})
            name = ''
            amount = null
            hideForm()
        }
    }

    // onMount(() => {
    //     console.log('form has mounted')
    // })

    // beforeUpdate(() => {
    //     console.log('form has before')
    // })

    // afterUpdate(() => {
    //     console.log('form has after')
    // })

    // onDestroy(() => {
    //     console.log('form has destory')
    // })

</script>

<section class="form">
    <Title title={isEditing ? "edit expense" : "add expense"} />
    <form class="expense-form" on:submit|preventDefault={handleSubmit}>
        <div class="form-control">
            <label for="name">name</label>
            <input type="text" id="name" bind:value={name}>
        </div>
        <div class="form-control">
            <label for="amount">amount</label>
            <input type="number" id="amount" bind:value={amount}>
        </div>
        {#if isEmpty}
            <p class="form-empty">
                please fill out all form fields
            </p>
        {/if}
        <button type="submit" class="btn btn-block" class:disabled={isEmpty} disabled={isEmpty}>
            {isEditing ? 'editing expense' : 'add expense'}
        </button>
        <button class="close-btn" on:click={hideForm}>
        <i class="fas fa-times" />
            close
        </button>
    </form>
</section>