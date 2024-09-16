<script>
  export let model = 'subtask';
  export let container = 'table';

  export let template = `
import { createActionGroup, emptyProps, props } from '@ngrx/store';

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

  const toFileName = (name) => name.split('').reduce((p,c,i) => {
    if(i === 0) return c.toLowerCase();
    const letter = c < 'a' ? `-${c.toLowerCase()}` : c;
    return p + letter;
  }, '');

  const toProperName = (name) => name.split('').reduce((p,c,i) => {
    if(i === 0) return c;
    const letter = c < 'a' ? ` ${c}` : c;
    return p + letter;
  }, '');

  const toCapitalName = (name) => name.split('').reduce((p,c,i) => {
    if(i === 0) return c.toUpperCase();
    return p + c;
  }, '');

  $: modelVariable = `${model[0].toLowerCase()}${model.substring(1)}`;
  $: modelProper = toCapitalName(model);
  $:containerProper = toCapitalName(container);
  $: modelFile = toFileName(model);

  $: toReplace = [
    // ['api', api],
    // ['apiFolder', apiFolder],
    // ['action', action],
    // ['actionLower', actionLower],
    ['container', container],
    ['containerProper', containerProper],
    ['model', model],
    ['modelProper', modelProper],
    ['modelVariable', modelVariable],
    ['modelFile', modelFile]
  ];

  $: generated = toReplace.reduce((p,c)=>p.replaceAll(`%${c[0]}%`, c[1]), template).trim() + '\n ';
</script>

<main>
  <div>
    <!-- <input bind:value={api} placeholder="API" /> -->
    <!-- <input bind:value={action} placeholder="Action" /> -->
    <input bind:value={model} placeholder="Model" />
    <input bind:value={container} placeholder="Container" />
    <textarea bind:value={template} />
  </div>
  <pre>{generated}</pre>
</main>

<style>
  * {
    margin: 0;
    padding: 0;
  }

  main {
    display: flex;
    height: 100%;
    align-items: stretch;
    gap: 10px;
  }

  main > * {
    flex: 1;
  }

  input,
  textarea {
    display: block;
    width: 100%;
  }

  textarea {
    height: 90%;
  }

  pre,textarea,input {
    user-select: all;
  }
</style>