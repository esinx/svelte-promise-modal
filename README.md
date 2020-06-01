# svelte-promise-modal

Yeah you've seen lots of modals. But have you seen a modal that returns a promise?

## Installation

```
npm i -D svelte-promise-modal
```

## How to

Import as shown below

```html
<script>
    import { Modal, AlertModal, AskModal } from "svelte-promise-modal";
    import { get } from "svelte/store"; // you will need this for AskModal
</script>
```

Create references to each components as shown below

```js
let _modal;
let _alertmodal;
let _askmodal;
```

```html
<Modal bind:this="{_modal}">
    <h1>Hi! I'm a Modal!</h1>
</Modal>

<AlertModal bind:this="{_alertmodal}" let:confirm>
    <div slot="footer">
        <button on:click="{confirm}">ok</button>
    </div>
</AlertModal>

<AskModal let:datastore let:confirm let:cancel bind:this="{_askmodal}">
    <h1>Hi! I'm an Ask Modal!</h1>
    <p>I resolve when you press confirm</p>
    <div>
        <input value={get(datastore).text} on:input={e => { datastore.set({ text: e.target.value });
        }} />
    </div>
    <div>
        <button on:click="{cancel}">cancel</button>
        <button on:click="{confirm}">confirm</button>
    </div>
</AskModal>
```

Open them using corresponding methods.

AlertModal and AskModal will return a promise that is fulfilled once `confirm` is called.

```html

    <div>
      <button on:click={_modal.open}>Open Modal</button>
    </div>
    <div>
      <button
        on:click={() => {
          _alertmodal.alert(`<h1>Hi! I'm an Alert Modal!</h1>`);
        }}>
        Open Alert Modal
      </button>
    </div>
    <div>
      <button
        on:click={async () => {
          try {
            const data = await _askmodal.ask({ text: 'test' });
            setTimeout(() => {
              alert(`You typed: ${data.text}`);
            }, 500);
          } catch (e) {
            setTimeout(() => {
              alert(`You closed the ask modal without confirmation!`);
            }, 500);
          }
        }}>
        Open Ask Modal
      </button>
    </div>
```

## AskModal datastore

AskModal's `ask` method expects an object as its first argment. This is the default data that will be stored in the `datastore` store.

Declaring `let:datastore` in the AskModal component will provide a way for its inner contents to communiate with the datastore. `datastore` is a svelte store, so you should use the `get` function exported from `svelte/store` in order to access the default values and the `set` function to edit its values. See example folder for detailed usage.

## License

MIT
