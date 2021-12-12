<template>
  <div class="art-button">
      <button @click="$emit('on-clicked')">
          <slot></slot>
      </button>
  </div>
</template>

<script>
import { defineComponent } from "@vue/runtime-core";

export default defineComponent({
    
})
</script>

<style lang="scss" scoped>
    .art-button {
        border-radius: .5em;
        padding: .5em 1.5em;
        max-width: max-content;
        margin: 0;
        cursor: pointer;
        box-shadow: 0 0 .2em rgba(0,0,0,.10);
        &.button-primary {
            background: var(--primary);
            color: var(--secondary)
        }
        &.button-secondary {
            background: var(--secondary);
            color: var(--primary)
        }
        &.inactive {
            background: var(--disabled);
            color: var(--grey)
        }
        &.button-small {
            padding: .25em .75em;
            border-radius: .25em;
        }
        button {
            background-color: transparent;
            border: none;
            color: inherit;
            font-family: inherit;
            width: 100%;
            height: 100%;
            cursor: inherit;
            font-weight: 600;
        }
    }
</style>