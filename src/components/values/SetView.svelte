<script lang="ts">
    import Sym from '@nodes/Sym';
    import { SET_CLOSE_SYMBOL, SET_OPEN_SYMBOL } from '@parser/Symbols';
    import type SetValue from '@values/SetValue';
    import Expandable from './Expandable.svelte';
    import SymbolView from './SymbolView.svelte';
    import ValueView from './ValueView.svelte';

    interface Props {
        value: SetValue;
        inline?: boolean;
    }

    let { value, inline = true }: Props = $props();

    const CollapsedLimit = 3;
    const MaxItems = 100;
</script>

<!-- Inline sets are shown as a collapsed list -->
{#if inline}
    <SymbolView
        symbol={SET_OPEN_SYMBOL}
        type={Sym.SetOpen}
    />{#if value.values.length > CollapsedLimit}<Expandable
            >{#snippet expanded()}
                {#each value.values as item, index}<ValueView
                        value={item}
                        {inline}
                    />{#if index < value.values.length - 1}{' '}{/if}{/each}
            {/snippet}
            {#snippet collapsed()}
                {#each value.values.slice(0, CollapsedLimit) as item, index}<ValueView
                        value={item}
                        {inline}
                    />{#if index < value.values.length - 1}{' '}{/if}{/each}…
            {/snippet}</Expandable
        >{:else}{#each value.values as item, index}<ValueView
                value={item}
                {inline}
            />{#if index < value.values.length - 1}{' '}{/if}{/each}{/if}<SymbolView
        symbol={SET_CLOSE_SYMBOL}
        type={Sym.SetClose}
    />
    <!-- Block sets are like inline, but not collapsed -->
{:else}
    <SymbolView symbol={SET_OPEN_SYMBOL} type={Sym.SetOpen} />
    {#each value.values.slice(0, MaxItems) as item, index}<ValueView
            value={item}
            {inline}
        />{#if index < value.values.length - 1}{' '}{/if}{/each}
    {#if value.values.length > MaxItems}…{/if}
    <SymbolView symbol={SET_CLOSE_SYMBOL} type={Sym.SetClose} />
{/if}
