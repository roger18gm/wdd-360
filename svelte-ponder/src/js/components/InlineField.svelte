<script lang="ts">
    let { label, value = $bindable(), type = "text" } = $props();

    let isEditing = $state(false);
    let originalValue = value;
    let buttonRef: HTMLElement | null = null;

    function startEdit() {
        isEditing = true;
    }
    function commitEdit() {
        isEditing = false;
        buttonRef?.focus();
    }
    function cancelEdit() {
        value = originalValue;
        isEditing = false;
        buttonRef?.focus();
    }

    function onInputKeydown(e: KeyboardEvent) {
        if (e.key === "Enter" && type !== "textarea") {
            e.preventDefault();
            commitEdit();
        }
        if (e.key === "Escape") {
            e.preventDefault();
            cancelEdit();
        }
    }
</script>

<label for="{value}-field">{label}</label>
{#if isEditing}
    {#if type === "textarea"}
        <textarea
            aria-label="{label} field"
            onkeydown={onInputKeydown}
            onblur={commitEdit}
            bind:value></textarea>
    {:else}
        <input
            aria-label="{label} field"
            onkeydown={onInputKeydown}
            onblur={commitEdit}
            id="{value}-field"
            bind:value
            {type}
        />
    {/if}
{:else}
    <button aria-label="edit {label}" onclick={startEdit} bind:this={buttonRef}>
        {value}
    </button>
{/if}
