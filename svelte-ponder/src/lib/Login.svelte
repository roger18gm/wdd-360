<script lang="ts">
    function submit(event: Event) {
        event.preventDefault();

        console.log("signed in");
    }

    let email = $state("");
    let password = $state("");
    let inputInvalid = $derived(
        email != "" && !email.includes("@") && password.length < 6,
    );
    let submitDisabled = $derived(email === "" || password === "");
</script>

<form onsubmit={submit}>
    <label for="email">Email</label>
    <input id="email" bind:value={email} />
    <label for="password">Password </label>
    <input id="password" bind:value={password} />
    <button type="submit" disabled={submitDisabled}>Sign In</button>
</form>

{#if inputInvalid}
    <div><strong>EMAIL OR PASSWORD WRONG</strong></div>
{/if}

<style>
    form {
        display: flex;
        flex-direction: column;
        padding: 16px;
        border: solid grey 2px;
        gap: 8px;
        margin-bottom: 16px;
    }
    label {
        text-align: left;
    }
    button {
        font-size: large;
    }
</style>
