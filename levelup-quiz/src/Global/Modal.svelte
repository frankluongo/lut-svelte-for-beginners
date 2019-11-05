<script>
  import { fly, fade } from "svelte/transition";
</script>

<style>
  .modal-bg {
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;

    display: flex;
    justify-content: center;
    align-items: center;

    background-color: rgba(0, 0, 0, 0.5);
  }

  .modal {
    width: 90%;
    max-width: 30rem;
    padding: 1rem;

    background-color: #ffffff;
  }
</style>

<div class="modal-bg" transition:fade>
  <div class="modal" in:fly={{ y: 100 }} out:fly={{ y: -100 }}>
    <button>Close</button>
    <!-- <slot>Optional Fallback</slot> -->
    <slot name="modal">
      <!-- optional fallback -->
    </slot>
  </div>
</div>
