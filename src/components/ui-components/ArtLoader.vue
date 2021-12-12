<template>
  <div class="art-loader flexed">
      <span>{{label}}</span>
      <img src="@/assets/img/loader-dark.svg" width="12" class="icon" alt="">
  </div>
</template>

<script>
import { defineComponent } from "@vue/runtime-core";

export default defineComponent({
    props: {
        label: {
            type: String,
            default: "Please Wait.."
        }
    }
})
</script>

<style lang="scss" scoped>
    .art-loader {
        &.flexed {
            gap: 1em
        }
        .icon {
            animation: spin linear infinite .75s;
        }
    }
    @keyframes spin {
        0% {
            transform: rotate(0deg);
        }
        100% {
            transform: rotate(360deg)
        }
    }
</style>