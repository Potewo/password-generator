<script lang="ts">
  import { Button, Input, Label, FormGroup, Icon } from "sveltestrap";
  let length: number = 10;
  let password: string = "";
  let containUppercase = true;
  let containDigits = true;
  let containSymbols = true;
  let symbols = "-%&$#()";
  const generatePassword = () => {
    let base = "abcdefghijklmnopqrstuvwxyz";
    if (containUppercase) {
      base += "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
    }
    if (containDigits) {
      base += "0123456789";
    }
    if (containSymbols) {
      base += symbols;
    }
    password = Array.from(crypto.getRandomValues(new Uint32Array(length)))
      .map((n) => {
        return base[n % base.length];
      })
      .join("");
    console.log(password);
  };

  const copy = () => {
    navigator.clipboard.writeText(password);
  };
</script>

<main>
  <h1>Password Generator</h1>
  <FormGroup>
    <Input
      type="checkbox"
      label="Contain uppercase"
      bind:checked={containUppercase}
    />
    <Input
      type="checkbox"
      label="Contain digits"
      bind:checked={containDigits}
    />
    <Input
      type="checkbox"
      label="Contain symbols"
      bind:checked={containSymbols}
    />
    <Label for="length">Length</Label>
    <Input type="number" id="length" bind:value={length} />
    <Label for="symbols">Symbols</Label>
    <Input id="symbols" bind:value={symbols} />
    <Button
      color="primary"
      on:click={() => {
        generatePassword();
      }}>Generate</Button
    >
  </FormGroup>
  {password}{#if password}<button
      class="icon-button"
      on:click={() => {
        copy();
      }}><Icon name="clipboard" /></button
    >{/if}
</main>

<style>
  .icon-button {
    background: none;
    border: none;
  }
  main {
    width: min(500px, 80vw);
    margin: auto;
  }
</style>
