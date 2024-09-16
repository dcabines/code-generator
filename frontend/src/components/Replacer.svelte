<script>
  export let api = 'API';
  export let model = 'MODEL';
  export let action = 'ACTION';
  export let template = '';

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

  $: actionLower = action.toLowerCase()
  $: modelVariable = `${model[0].toLowerCase()}${model.substring(1)}`;
  $: modelProper = toProperName(model);
  $: modelFile = toFileName(model);
  $: apiFolder = toFileName(api);

  $: toReplace = [
    ['api', api],
    ['apiFolder', apiFolder],
    ['action', action],
    ['actionLower', actionLower],
    ['model', model],
    ['modelProper', modelProper],
    ['modelVariable', modelVariable],
    ['modelFile', modelFile]
  ];

  $: generated = toReplace.reduce((p,c)=>p.replaceAll(`%${c[0]}%`, c[1]), template).trim() + '\n ';
</script>

<main>
  <div>
    <input bind:value={api} placeholder="API" />
    <input bind:value={action} placeholder="Action" />
    <input bind:value={model} placeholder="Model" />
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