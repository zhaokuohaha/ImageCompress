<template>
    <div class="main"> 
      <div class="menu">
        <router-link to="/">Home</router-link>
      </div>
      <h2>设置:</h2>
      <div class="setting-container">
          <div class="label">压缩质量</div>
          <div class="setting-input"><input v-model="qua" type="text" />%</div>
      </div>

      <div class="button">保存</div>
    </div>
</template>

<script>
export default {
  data () {
    return {
      qua: 70,
      lrz: {
        width: null,
        height: null,
        quality: 0.7
      }
    }
  },
  watch: {
    qua: function (val) {
      var res = 0
      if (isNaN(val) || val <= 0) { res = 0 }
      if (val > 100) { res = 100 }

      this.lrz.quality = (res / 100).toFixed(2)
    }
  }
}
</script>

<style lang="scss" scoped>
.main{
  font-family: '微软雅黑';
  padding: 16px;
  margin: 16px;
}

.setting-container{
    display: grid;
    grid-template-columns: 80px 1fr;
}

.label,.setting-input{
  margin: 8px 0;
}
</style>>
