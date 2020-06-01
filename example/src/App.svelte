<script>
  import { Modal, AlertModal, AskModal } from "svelte-promise-modal";
  import { get } from "svelte/store";
  let _modal;
  let _alertmodal;
  let _askmodal;
</script>

<style>
  .root {
    width: 100vw;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .root .centerbox {
    margin: auto;
    width: 90vw;
    max-width: 800px;
    padding: 40px 20px;
    border: solid #ccc 1px;
    border-radius: 10px;
  }
</style>

<Modal bind:this={_modal}>
  <h1>Hi! I'm a Modal!</h1>
</Modal>

<AlertModal bind:this={_alertmodal} let:confirm>
  <div slot="footer">
    <button on:click={confirm}>ok</button>
  </div>
</AlertModal>

<AskModal let:datastore let:confirm let:cancel bind:this={_askmodal}>
  <h1>Hi! I'm an Ask Modal!</h1>
  <p>I resolve when you press confirm</p>
  <div>
    <input
      value={get(datastore).text}
      on:input={e => {
        datastore.set({ text: e.target.value });
      }} />
  </div>
  <div>
    <button on:click={cancel}>cancel</button>
    <button on:click={confirm}>confirm</button>
  </div>
</AskModal>

<div class="root">
  <div class="centerbox">
    <h1>Svelte Promise Modal</h1>
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
  </div>
</div>
