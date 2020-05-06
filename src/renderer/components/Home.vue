<template>
  <div>
    <div class="menu">
      <!-- <router-link to="/settings">设置</router-link> -->
      <router-link to="/about">关于</router-link>
    </div>
    <div v-for="(img, index) in imgs" :key="index">
      <div class="compress-info">
        <div>原图: {{getSize(img.origin.size)}}</div>
        <div>压缩: {{getSize(img.compress.fileLen)}}</div>
        <div>压缩比: {{(compressRate(img.compress.fileLen, img.origin.size))}}</div>
        <div class="pointer button" @click="saveImg(img)">下载图片</div>
      </div>
      <image-view 
        class = 'img-view'
        :leftImage="img.origin.path"
        :rightImage="img.compress.base64"
        leftLabel="原图"
        rightLabel="压缩">
        </image-view>
    </div>
    <!-- <div style="height:80px"></div> -->
    <div v-if="imgs.length <= 0" class="upload-center">
      <file-upload class="upload-min-btn" :multiple=true v-model="uploadImgs" @input-file="addImg" >
        +
      </file-upload>
    </div>
    <div v-else class="pointer upload-min">
      <file-upload class="upload-min-btn" :multiple=true v-model="uploadImgs" @input-file="addImg" >
        上传图片
      </file-upload>
      <div class="clear" @click="clearImgs()">
        清空
      </div>
    </div>
  </div>
</template>

<script>
import lrz from 'lrz'
import vuc from 'vue-upload-component'
import vic from 'vue-compare-image'
import { saveAs } from 'file-saver'

export default {
  components: {
    'file-upload': vuc,
    'image-view': vic
  },
  data () {
    return {
      uploadImgs: [],
      imgs: []
    }
  },
  methods: {
    async compress (img) {
      let rst = await lrz(img)
      console.log(rst)
      return rst
    },

    async addImg (newFile, oldFile) {
      if (newFile && !oldFile) {
        console.log('add', newFile)
        this.imgs.push({
          origin: newFile.file,
          compress: await this.compress(newFile.file)
        })
      }
      if (newFile && oldFile) {
        console.log('update', newFile)
      }
      if (!newFile && oldFile) {
        console.log('remove', oldFile)
      }
    },

    getSize (size) {
      var k = size / 1024
      if (k >= 1024) { return (size / 1024 / 1024).toFixed(1) + 'M' }
      return k.toFixed(1) + 'K'
    },

    compressRate (newsize, oldsize) {
      return ((1 - newsize / oldsize) * 100).toFixed(1) + '%'
    },

    saveImg (img) {
      let name = 'compress-' + (img.origin.name ? img.origin.name : 'new.jpg')
      let data = img.compress.file
      saveAs(data, name)
    },

    clearImgs () {
      this.imgs = []
      this.uploadImgs = []
    }
  },
  mounted () {
  }
}
</script>

<style scoped>

.img-view{
  max-width: 100%;
  margin: 8px auto 60px;
}

.pointer{
  cursor: pointer !important;
}

.upload-min{
  position: fixed;
  left: 0;
  bottom: 0;
  width: 100%;
  background-color: rgba(22, 97, 171, .6);
  color: #eee;
  height: 60px;
  line-height: 60px;
  text-align: center;
  font-family: '微软雅黑';
  font-size: 1.2rem;
  display: flex;
}

.upload-center{
  width: 400px;
  height: 200px;
  line-height: 200px;

  margin: auto;
  margin-top: calc(50vh - 100px);
  background-color: rgba(22, 97, 171, 0.2);
  font-size: 100px;
  font-weight: bold;
  color: rgba(22, 97, 171, 0.6);

  border-radius: 5px;
}

.upload-min-btn{
  width: 100%;
  height: 100%;
}

.compress-info{
  margin: 8px 0;
  text-align: right;
  display: flex;
  justify-content: flex-end;
  align-items: center;
}

.compress-info div{
  margin: 0 8px;
}
.clear{
  width: 200px;
  border-left: 1px solid #eee;
}
</style>