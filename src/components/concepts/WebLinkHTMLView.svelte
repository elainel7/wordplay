<script lang="ts">
    import type WebLink from '../../nodes/WebLink';
    import type Spaces from '../../parser/Spaces';
    import Link from '../app/Link.svelte';

    interface Props {
        link: WebLink;
        spaces: Spaces;
    }

    let { link, spaces }: Props = $props();

    let url = link.url
        ? link.url.getText().startsWith('://')
            ? link.url.getText().replace('://', '/')
            : link.url.getText()
        : '';
</script>

{#if link.url && link.description}
    {#if spaces.getSpace(link.open).length > 0}&nbsp;{/if}<Link
        external={!url.startsWith('/')}
        to={url}>{link.description.getText()}</Link
    >
{:else if link.description}
    {link.description.getText()}
{/if}
