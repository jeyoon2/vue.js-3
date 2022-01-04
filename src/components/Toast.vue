<template>
  <div class="alert toast-box" :class="`alert-${type}`" role="alert"></div>
</template>

<script>
export default {};
</script>

<style>
.toast-box {
  position: fixed;
  top: 10px;
  left: 10px;
}
</style>
