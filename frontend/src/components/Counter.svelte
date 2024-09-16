<script lang="ts">
  import type { HTMLAttributes } from "svelte/elements";
  import * as Card from "$lib/components/ui/card/index.js";
	import * as Button from "$lib/components/ui/button/index.js";
  import { cn } from "$lib/utils.js";

  let count = 0;

  function add() {
    count += 1;
  }

  function subtract() {
    count -= 1;
  }

  type $$Props = HTMLAttributes<HTMLDivElement>;
  let className: $$Props["class"] = undefined;
  export { className as class };
</script>

<Card.Root class={cn("w-[380px]", className)}>
  <Card.Header>
    <Card.Title><slot /></Card.Title>
  </Card.Header>
  <Card.Content>
    <div class="counter">
      <button on:click={subtract}>-</button>
      <pre>{count}</pre>
      <button on:click={add}>+</button>
    </div>
    <div class="message"></div>

    <Button.Root variant="outline">
      Unlimited
    </Button.Root>
  </Card.Content>
</Card.Root>

<style>
  .counter {
    display: grid;
    font-size: 2em;
    grid-template-columns: repeat(3, minmax(0, 1fr));
    place-items: center;
  }

  .message {
    text-align: center;
  }
</style>
