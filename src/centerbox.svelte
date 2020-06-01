<style>
  .center-wrapper {
    display: flex;
    align-items: center;
    justify-content: flex-start;
    width: 100vw;
    height: 100vh;
    flex-direction: column;
  }
  .center-wrapper::before {
    content: "";
    width: 100vw;
    flex: 1 0 100px;
    visibility: hidden;
    margin-bottom: auto;
  }
  .center-wrapper::after {
    content: "";
    width: 100vw;
    flex: 1 0 100px;
    visibility: hidden;
    margin-top: auto;
  }
</style>

<div {...$$props} class="center-wrapper {$$props.class || ''}">
  <slot />
</div>
