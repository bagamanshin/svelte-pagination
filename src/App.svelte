<script>
    export
        let
            size = 3,
            current_page = 1,
            last_page = 6;

    const inBorder = id => ((id <= 2) || ((last_page - id) <= 1));
    const inRange = id => ((current_page - ((size - 1) / 2)) <= id) && ((current_page + ((size - 1) / 2)) >= id)

    const showDigit = id => inBorder(id) || inRange(id);
    const singleHiddenDigit = id => showDigit(id - 1) && !showDigit(id) && showDigit(id + 1);

    $: ar = Array.apply(null, Array(last_page)).map(i => `${current_page + Math.random()}`);

    let goToIndex, showGoToBlock;
    $: showGoToBlock = 
                        Math.abs(last_page - (current_page + ((size - 1) / 2))) >= 4 ||
                        Math.abs(1 - (current_page - ((size - 1) / 2))) >= 4;
    $: if (!(ar.map((item, index) => index + 1).includes(Number(goToIndex)))) {
        goToIndex = null;
    }
</script>

{#if last_page > 1}
    <div class="pagination">
        {#if showGoToBlock}
            <div class="pagination__goto">
                <button class={!goToIndex || (Number(goToIndex) === current_page) ? "pagination__btn pagination__btn-outline" : "pagination__btn"} disabled={!goToIndex || (Number(goToIndex) === current_page)} on:click={() => current_page = Number(goToIndex)}>
                    Go to
                </button>
                <input type="text" class="pagination__input" bind:value={goToIndex}>
            </div>
        {/if}
        <ul class="pagination__list">
            <li class="pagination__list__item" class:pagination__list__item-disabled={current_page <= 1 || last_page === 1}>
                <span class="pagination__list__item__link" on:click={() => --current_page}>
                    <span>‹</span>
                </span>
            </li>
            {#each ar as uid_key, index (uid_key)}
                {#if showDigit(index + 1) || singleHiddenDigit(index + 1)}
                    <li class="pagination__list__item" class:pagination__list__item-active="{(index + 1) === current_page}">
                        <span class="pagination__list__item__link" on:click={() => current_page = index + 1}>
                            {index + 1}
                        </span>
                    </li>
                {:else if showDigit(index) && !showDigit(index + 1) && !showDigit(index + 2)}
                    <li class="pagination__list__item pagination__list__item-disabled">
                        <span class="pagination__list__item__link" on:click={() => current_page = index + 1}>
                            ...
                        </span>
                    </li>
                {/if}
            {/each}
            <li class="pagination__list__item" class:pagination__list__item-disabled={current_page >= last_page || last_page === 1}>
                <span class="pagination__list__item__link" on:click={() => ++current_page}>
                    <span>›</span>
                </span>
            </li>
        </ul>
    </div>
{/if}


<style>
    .pagination {
        display: flex;
		align-items: center;
    }
    .pagination__btn {
		cursor: pointer;
        display: inline-block;
        font-weight: 400;
        color: white;
        text-align: center;
        vertical-align: middle;
        user-select: none;
        border: 1px solid #762877;
        background-color: #762877;
        padding: .375rem .75rem;
        font-size: 1rem;
        line-height: 1.5;
        border-radius: .25rem;
        transition: color .15s ease-in-out,background-color .15s ease-in-out,border-color .15s ease-in-out,box-shadow .15s ease-in-out;
		margin-bottom: 0;
	}
    .pagination__btn.pagination__btn-outline {
        color: #762877;
        background-color: white;
        border-color: #762877;
    }
    .pagination__btn:disabled {
        pointer-events: none;
    }
    .pagination__btn:hover {
        background-color: #5f2260;
        border-color: #5f2260;
        color: white;
    }
    .pagination__input {
        display: inline-block;
        width: 75px;
        height: calc(1.5em + .75rem + 2px);
        padding: .375rem .75rem;
        font-size: 1rem;
        font-weight: 400;
        line-height: 1.5;
        color: #495057;
        background-color: #fff;
        background-clip: padding-box;
        border: 1px solid #ced4da;
        border-radius: .25rem;
        transition: border-color .15s ease-in-out,box-shadow .15s ease-in-out;
        margin-left: .5rem;
        margin-right: 1.5rem;
		margin-bottom: 0;
    }
    .pagination__input:focus {
        background-color: #fff;
        outline: 0;
        box-shadow: 0 0 0 0.2rem rgba(118, 40, 119, 0.25);
        border-color: rgba(118, 40, 119, 0.5);
    }
    .pagination__goto {
        display: inline-flex;
    }
    .pagination__list {
        display: inline-flex;
        list-style: none;
        border-radius: .25rem;
        margin-bottom: 0;
        padding-left: 0;
		margin-top: 0;
    }
    .pagination__list__item__link {
        position: relative;
        display: block;
        padding: .5rem .75rem;
        margin-left: -1px;
        line-height: 1.25;
        color: #762877;
        background-color: #fff;
        border: 1px solid #dee2e6;
        text-decoration: none;
        outline: none;
    }
    .pagination__list__item:first-child .pagination__list__item__link {
        margin-left: 0;
        border-top-left-radius: .25rem;
        border-bottom-left-radius: .25rem;
    }
    .pagination__list__item:last-child .pagination__list__item__link {
        border-top-right-radius: .25rem;
        border-bottom-right-radius: .25rem;
    }
    .pagination__list__item.pagination__list__item-disabled .pagination__list__item__link {
        color: #6c757d;
        pointer-events: none;
        cursor: auto;
        background-color: #fff;
        border-color: #dee2e6;
    }
    .pagination__list__item.pagination__list__item-active .pagination__list__item__link {
        background-color: #762877;
        border-color: #762877;
        color: white;
        z-index: 3;
    }
    .pagination__list__item__link:hover {
        background-color: #fff3f1;
        border-color: rgba(118, 40, 119, 0.25);
    }
    .pagination__list__item__link:focus {
        box-shadow: 0 0 0 0.2rem rgba(118, 40, 119, 0.25);
        border: 1px solid rgba(118, 40, 119, 0.5);
        z-index: 1;
    }
</style>