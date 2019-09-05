<template>
  <div class="widget">
    <p class="text">this text should be purple according to local css</p>
    <a class="externallyStyledA">this text should be left unstyled because there are no local styles</a>
  </div>
</template>

<script>
</script>

<style lang="less" scoped>
.text {
  all: initial;
  color: purple;
}
</style>