<script>

    import { getContext } from 'svelte'

    export let name = '' 
    export let amount = 0
    export let id 

    let displayAmount = false

    const toggleAmount = () => {
        displayAmount = !displayAmount
    }

    const remove = getContext('remove')
    const modify = getContext('modify')

</script>

<aritcle class="single-expense">
    <div class="expense-info">
        <h2>{name} 
            <button class="amount-btn" on:click={toggleAmount} >
                <i class="fas fa-caret-down" />
            </button>
        </h2>
        {#if displayAmount}
            <h4>amount : ${amount}</h4>
        {/if}
    </div>
    <div class="expense-buttons">
        <button class="expense-btn edit-btn" on:click={() => modify(id)}>
            <i class="fas fa-pen"/>
        </button>
        <button class="expense-btn delete-btn" on:click={()=>remove(id)}>
            <i class="fas fa-trash"/>
        </button>
    </div>
</aritcle>