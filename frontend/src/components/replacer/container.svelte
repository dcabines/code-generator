<script lang="ts">
  export let model = 'subtask';
  export let container = 'table';

  export let template = `import { createActionGroup, emptyProps, props } from '@ngrx/store';

export default createActionGroup({
  source: '%modelProper% %containerProper%',
  events: {
    add%modelProper%Clicked: emptyProps(),
    edit%modelProper%Clicked: props<{ %model%Id: number; }>(),
    delete%modelProper%Clicked: props<{ %model%Id: number; }>(),
    delete%modelProper%Canceled: emptyProps(),
    delete%modelProper%Confirmed: props<{ %model%Id: number; }>(),
  }
})
`;

  const toVariableName = (name: string) => {
    if(!name[0]) return '';
    return `${name[0].toLowerCase()}${name.substring(1)}`
  };

  const toCapitalName = (name: string) => name.split('').reduce((p,c,i) => {
    if(i === 0) return c.toUpperCase();
    return p + c;
  }, '');

  type replacer = [string, string];

  $: toReplace = [
    ['container', toVariableName(container)],
    ['containerProper', toCapitalName(container)],
    ['model', toVariableName(model)],
    ['modelProper', toCapitalName(model)]
  ] as replacer[];

  $: generated = toReplace.reduce((p,c)=> p.replaceAll(`%${c[0]}%`, c[1]), template).trim() + '\n ';
</script>

<div class="container">
  <div class="flex flex-col gap-1">
    <div class="flex">
      <label for="model-input">Model</label>
      <input id="model-input" class="border border-solid flex-1" bind:value={model} placeholder="Model" />
    </div>
    <div class="flex">
      <label for="container-input">Container</label>
      <input id="container-input" class="border border-solid flex-1" bind:value={container} placeholder="Container" />
    </div>
    <textarea class="flex-1 border border-solid focus-visible:outline-0" bind:value={template} />
  </div>

  <pre>{generated}</pre>
</div>

<style>
  pre,textarea,input {
    user-select: all;
  }

  label {
    width: 100px;
  }

  .container {
    display: flex;
    height: 100%;
    align-items: stretch;
    gap: 10px;
  }

  .container > * {
    flex: 1;
    display: flex;
  }
</style>