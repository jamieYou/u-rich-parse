<template>
  <block>
    <block v-for="(item, index) in nodes" :key="index">
      <!--判断是否是标签节点-->
      <block v-if="item.node === 'element'">
        <block v-if="item.tag === 'button'">
          <button type="default" size="mini">
            <render-nodes :nodes="item.nodes"/>
          </button>
        </block>

        <!--li类型-->
        <block v-else-if="item.tag === 'li'">
          <view :class="item.classStr" class="rich-parse-li" :style="item.styleStr">
            <view :class="item.classStr" class="rich-parse-li-inner">
              <view :class="item.classStr" class="rich-parse-li-text">
                <view :class="item.classStr" class="rich-parse-li-circle"></view>
              </view>
              <view :class="item.classStr" class="rich-parse-li-text">
                <render-nodes :nodes="item.nodes"/>
              </view>
            </view>
          </view>
        </block>

        <!--video类型-->
        <block v-else-if="item.tag === 'video'">
          <view class="rich-parse-video-view" :class="item.classStr" :style="item.styleStr">
            <video
              :class="item.classStr" class="rich-parse-video"
              objectFit="fill" :src="item.attr.src"
            ></video>
          </view>
        </block>

        <!--audio类型-->
        <block v-else-if="item.tag === 'audio'">
          <view class="rich-parse-audio-view" :class="item.classStr" :style="item.styleStr">
            <audio
              :class="item.classStr" class="rich-parse-audio"
              controls :src="item.attr.src"
            ></audio>
          </view>
        </block>

        <!--img类型-->
        <block v-else-if="item.tag === 'img'">
          <image
            :class="item.classStr" class="rich-parse-img"
            :src="item.attr.src" mode="widthFix"
            :style="'' + `width: ${imageWidths[item.attr.src_key]}px; ${item.styleStr}`"
            @click="onImgTap(item.attr.src)" @load="onImgLoad(item.attr.src_key, $event)"
          ></image>
        </block>

        <!--a类型-->
        <block v-else-if="item.tag === 'a'">
          <view
            :class="item.classStr"
            class="rich-parse-inline rich-parse-a"
            @click="onLinkTap(item.attr.href)"
            :style="item.styleStr"
          >
            <render-nodes :nodes="item.nodes"/>
          </view>
        </block>

        <!--table类型-->
        <block v-else-if="item.tag === 'table'">
          <view class="rich-parse-table" :class="item.classStr" :style="item.styleStr">
            <render-nodes :nodes="item.nodes"/>
          </view>
        </block>

        <!--br类型-->
        <block v-else-if="item.tag === 'br'">
          <text>\n</text>
        </block>

        <!--其他块级标签-->
        <block v-else>
          <view
            :class="[item.classStr, `rich-parse-${item.tag}`, `rich-parse-${item.tagType}`]"
            :style="item.styleStr"
          >
            <render-nodes :nodes="item.nodes"/>
          </view>
        </block>
      </block>

      <!--判断是否是文本节点-->
      <block v-else-if="item.node === 'text' ">
        <text class="rich-parse-inline" :style="item.styleStr">
          {{ item.text }}
        </text>
      </block>
    </block>
  </block>
</template>

<script>
  import RenderNodes from './render-nodes'

  export default {
    components: {
      RenderNodes
    },

    props: {
      nodes: {
        type: Array,
        default: () => [],
      }
    },

    data() {
      return {
        imageWidths: {}
      }
    },

    methods: {
      onImgLoad(src_key, e) {
        this.$set(this.imageWidths, src_key, e.detail.width)
      },

      onImgTap(src) {
        this.$parent.onImgTap(src)
      },

      onLinkTap(href) {
        this.$parent.onLinkTap(href)
      },
    }
  }
</script>

<style>
  view {
    word-break: break-all;
    overflow: auto;
  }

  .rich-parse-inline {
    display: inline;
    margin: 0;
    padding: 0;
  }

  .rich-parse-div {
    margin: 0;
    padding: 0;
  }

  .rich-parse-p {
    margin-bottom: 13px;
  }

  .rich-parse-h1 {
    font-size: 2em;
    margin: 0.67em 0;
  }

  .rich-parse-h2 {
    font-size: 1.5em;
    margin: 0.75em 0;
  }

  .rich-parse-h3 {
    font-size: 1.17em;
    margin: 0.83em 0;
  }

  .rich-parse-h4 {
    margin: 1.12em 0;
  }

  .rich-parse-h5 {
    font-size: .83em;
    margin: 1.5em 0;
  }

  .rich-parse-h6 {
    font-size: .75em;
    margin: 1.67em 0;
  }

  .rich-parse-h1 {
    font-size: 18px;
    font-weight: 400;
    margin-bottom: .9em;
  }

  .rich-parse-h2 {
    font-size: 16px;
    font-weight: 400;
    margin-bottom: .34em;
  }

  .rich-parse-h3 {
    font-weight: 400;
    font-size: 15px;
    margin-bottom: .34em;
  }

  .rich-parse-h4 {
    font-weight: 400;
    font-size: 14px;
    margin-bottom: .24em;
  }

  .rich-parse-h5 {
    font-weight: 400;
    font-size: 13px;
    margin-bottom: .14em;
  }

  .rich-parse-h6 {
    font-weight: 400;
    font-size: 12px;
    margin-bottom: .04em;
  }

  .rich-parse-h1,
  .rich-parse-h2,
  .rich-parse-h3,
  .rich-parse-h4,
  .rich-parse-h5,
  .rich-parse-h6,
  .rich-parse-b,
  .rich-parse-strong {
    font-weight: bolder;
  }

  .rich-parse-i,
  .rich-parse-cite,
  .rich-parse-em,
  .rich-parse-var,
  .rich-parse-address {
    font-style: italic;
  }

  .rich-parse-pre,
  .rich-parse-tt,
  .rich-parse-code,
  .rich-parse-kbd,
  .rich-parse-samp {
    font-family: monospace;
  }

  .rich-parse-pre {
    white-space: pre;
  }

  .rich-parse-big {
    font-size: 1.17em;
  }

  .rich-parse-small,
  .rich-parse-sub,
  .rich-parse-sup {
    font-size: 0.83em;
  }

  .rich-parse-sub {
    vertical-align: sub;
  }

  .rich-parse-sup {
    vertical-align: super;
  }

  .rich-parse-s,
  .rich-parse-strike,
  .rich-parse-del {
    text-decoration: line-through;
  }

  /*rich-parse-自定义个性化的css样式*/
  /*增加video的css样式*/
  .rich-parse-strong,
  .rich-parse-s {
    display: inline;
  }

  .rich-parse-a {
    color: deepskyblue;
    word-break: break-all;
    overflow: auto;
  }

  .rich-parse-video-view {
    text-align: center;
    margin: 10px 0;
  }

  .rich-parse-video {
    width: 100%;
  }

  .rich-parse-audio-view {
    display: flex;
    justify-content: center;
    overflow: hidden;
  }

  .rich-parse-img {
    overflow: hidden;
    max-width: 100%;
  }

  .rich-parse-blockquote {
    margin: 0;
    padding: 10px 0 10px 5px;
    font-family: Courier, Calibri, "宋体";
    background: #f5f5f5;
    border-left: 3px solid #dbdbdb;
  }

  .rich-parse-code {
    display: inline;
    background: #f5f5f5;
  }

  .rich-parse-ul {
    margin: 10px 5px;
  }

  .rich-parse-li,
  .rich-parse-li-inner {
    display: flex;
    align-items: baseline;
    margin: 5px 0;
  }

  .rich-parse-li-text {
    align-items: center;
    line-height: 20px;
  }

  .rich-parse-li-circle {
    display: inline-flex;
    width: 5px;
    height: 5px;
    background-color: #333;
    margin-right: 5px;
  }

  .rich-parse-li-square {
    display: inline-flex;
    width: 5px;
    height: 5px;
    background-color: #333;
    margin-right: 5px;
  }

  .rich-parse-li-ring {
    display: inline-flex;
    width: 5px;
    height: 5px;
    border: 1PX solid #333;
    border-radius: 50%;
    background-color: #fff;
    margin-right: 5px;
  }

  .rich-parse-table {
    width: 100%;
    height: 400px
  }

  .rich-parse-thead,
  .rich-parse-tfoot,
  .rich-parse-tr {
    display: flex;
    flex-direction: row;
  }

  .rich-parse-th,
  .rich-parse-td {
    display: flex;
    width: 580px;
    overflow: auto;
  }

  .rich-parse-u {
    text-decoration: underline;
  }

  .rich-parse-hide {
    display: none;
  }

  .rich-emoji-view {
    align-items: center;
  }

  .rich-emoji {
    width: 16px;
    height: 16px;
  }

  .rich-parse-tr {
    display: flex;
    border-right: 1PX solid #e0e0e0;
    border-bottom: 1PX solid #e0e0e0;
    border-top: 1PX solid #e0e0e0;
  }

  .rich-parse-th,
  .rich-parse-td {
    flex: 1;
    padding: 5px;
    border-left: 1PX solid #e0e0e0;
    word-break: break-all;
  }

  .rich-parse-td:last-child {
    border-top: 1PX solid #e0e0e0;
  }

  .rich-parse-th {
    background: #f0f0f0;
    border-top: 1PX solid #e0e0e0;
  }

  .rich-parse-del {
    display: inline;
  }

  .rich-parse-figure {
    overflow: hidden;
  }
</style>
