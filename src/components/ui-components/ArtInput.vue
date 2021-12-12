<template>
  <div class="art-input">
      <div class="art-input-label">{{label}}</div>
      <input type="text" :value="value" @change="$emit('on-change', $event)" :placeholder="placeholder" />
  </div>
</template>

<script>
import { defineComponent } from "@vue/runtime-core";

export default defineComponent({
    props: {
        label: String,
        placeholder: String,
        value: String
    },
    data() {
        return {

        }
    }
})
</script>

<style lang="scss" scoped>
.art-input {
    width: 100%;
    position: relative;
    &-label {
        font-size: 70%;
        font-weight: 600;
        margin-bottom: .5em;
    }
    input {
        padding: .5em 0;
        font-family: inherit;
        background: transparent;
        border: none;
        border-bottom: 1px solid var(--fade);
        width: 100%;
        &:focus {
            outline: none;
        }
    }
}
</style>