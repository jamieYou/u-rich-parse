<template>
  <view class="rich-parse">
    <render-nodes :nodes="rich.nodes"/>
  </view>
</template>

<script>
  import { html2json } from './helper/html2json'
  import RenderNodes from './render-nodes'

  export default {
    components: {
      RenderNodes
    },

    props: {
      content: {
        type: String,
        default: '',
      },
      preview: {
        type: Boolean,
        default: true,
      },
    },

    data() {
      return {
        rich: null,
        parse_id: null,
      }
    },

    watch: {
      content(value) {
        this.parse(value)
      }
    },

    mounted() {
      this.parse(this.content)
    },

    methods: {
      parse(content) {
        this.rich = html2json(content, 'rich')
      },

      onImgTap(src) {
        const detail = {
          current: src, // 当前显示图片的http链接
          urls: this.rich.imageUrls, // 需要预览的图片http链接列表
        }
        this.$emit('imgTap', detail)
        this.preview && uni.previewImage(detail)
      },

      onLinkTap(href) {
        this.$emit('linkTo', href)
      }
    }
  }
</script>

<style>
  .rich-parse {
    font-family: Helvetica, sans-serif;
    font-size: 13px;
    color: #333;
    line-height: 1.6;
    word-wrap: break-word;
  }
</style>
