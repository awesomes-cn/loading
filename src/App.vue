<template>
  <div id="app" >
    <div class="banner">
      <h1>Awesome Loading</h1>
    </div>
    <div class="nav">
      可定制的 CSS 加载动画
    </div>
    <div class="container">
      
      <div class="load-outer">
        <loaditem v-for="item in paginationData()" :dataitem="item" :showCode="showPanel"></loaditem>
      </div>

      <div class="pagination">
        <a href="javascript:void(0)" class="page-btn" @click="gotoPrev" v-if="pagination.page > 1" style="padding: 10px 20px">
          <svg class="icon">
            <use xlink:href="static/icon.svg#arrow-left"></use>
          </svg>
        </a>

        <a href="javascript:void(0)" class="page-btn" @click="gotoNext" v-if="pagination.page < pagination.pageCount">
          <svg class="icon">
            <use xlink:href="static/icon.svg#arrow-down"></use>
          </svg>
        </a>

      </div>

      <transition name="slide">
        <div class="code-panel" v-show="codepanel.show">
          <a href="javascript:void(0)" class="close-code-btn" @click="codepanel.show= false" >
            <icon name="close"></icon>
          </a>
          <div class="inner">
            <h4>HTML</h4>
            <div class="area-html">
              <textarea >{{codepanel.code.html}}</textarea>
            </div>
            <h4>CSS</h4>
            <div class="area-css">
              <textarea>{{codepanel.code.css}}</textarea>
            </div>
          </div>
        </div>
      </transition>
    </div>

    <footer>
      <a href="https://github.com/awesomes-cn/loading" target="_blank">
        <svg class="icon" style="width: 50px; height: 50px;">
          <use xlink:href="static/icon.svg#github"></use>
        </svg>
      </a>
      <div>
        <a href="https://www.awesomes.cn/" target="_blank">
        Awesomes.cn
        </a> 旗下网站
      </div>
    </footer>
  </div>
</template>

<script>
  import Loaditem from './components/loaditem'
  let loadings = require('./loadings/loading')

  loadings.forEach(load => {
    (load.options || []).forEach(item => {
      item.oldval = item.val
    })
    load.result = {
      css: load.css
    }
    load.bgcolor = '#FFF'
  })

  let pageSize = 9
  export default {
    name: 'app',
    data () {
      return {
        codes: loadings,
        pagination: {
          pageSize: pageSize,
          page: 1,
          pageCount: Math.ceil(loadings.length / pageSize)
        },
        codepanel: {
          show: false,
          code: {
            html: '',
            css: ''
          }
        }
      }
    },
    components: {
      Loaditem
    },
    methods: {
      showPanel: function (code, isUpdate) {
        // 查看
        if (!isUpdate) {
          this.codepanel.show = true
          this.codepanel.code.html = code.html
          this.codepanel.code.css = code.result.css
        } else { // 更新当前的视图
          if (this.codepanel.code.html === code.html) {
            this.codepanel.code.css = code.result.css
          }
        }
      },
      paginationData: function () {
        let startIndex = (this.pagination.page - 1) * this.pagination.pageSize
        let endIndex = this.pagination.page * this.pagination.pageSize
        return this.codes.slice(startIndex, endIndex)
      },
      gotoPrev: function () {
        this.pagination.page -= 1
        document.body.scrollTop = document.documentElement.scrollTop = 0
      },
      gotoNext: function () {
        this.pagination.page += 1
        document.body.scrollTop = document.documentElement.scrollTop = 0
      }
    },
    mounted () {
      window.hidels = []
      document.body.onclick = () => {
        window.hidels.forEach(item => {
          item.ishow = false
        })
      }
    }
  }
</script>

<style>
  html,body {
    margin: 0;
    padding: 0; 
    color: #555;
    font-size: 14px;
    background-color: #f7f8fa
  }

  .hidescroll {
    height: 0;
    overflow: hidden
  }

  * {
    font-family: Menlo, Monaco, Consolas, "Helvetica Neue", Helvetica, "Courier New", 微软雅黑, monospace, Arial, sans-serif, 黑体;
  }

  .slide-enter-active,  .slide-leave-active {
    transition: all .3s ease;
  }

  .slide-enter, .slide-leave-to {
    transform: translateX(400px)
  }

  .container {
    margin: 50px auto;
    max-width: 1000px;
  }

  .pagination {
    text-align: center
  }

  .page-btn {
    display: inline-block;
    background-color: #FFF;
    padding: 10px 50px;
    box-shadow: 1px 1px 15px #EEE;
  }

  .side-right .page-btn {
    left: 10px;
  }

  .page-btn svg {
    width: 50px;
    height: 50px;
  }

  .page-btn:link svg, .page-btn:visited svg{
    fill: #dad5d5
  }

  [v-show], ['v-cloak'] {
    display: none
  }

  a, svg {
    transition: all,.5s,ease,0s;
    -webkit-transition: all,.5s,ease,0s;
    text-decoration: none
  }

  a:link, a:visited {
    color: #999;
  }

  a:link svg, a:visited svg {
    fill: #999; 
  }

  a:active, a:hover {
    color: #f34e5c;
  }

  a:active svg, a:hover svg{
    fill: #f34e5c;
  }

  .icon {
    width: 20px;
    height: 20px;
    display: inline-block;
    fill: #000;
  }

  .loading-box {
    display: flex;
    width: 100%;
    flex-grow: 1;
    align-items: center;
    justify-content: center;
  }

  .load-outer {
    width: 100%;
    display: flex;
    flex-wrap:  wrap;
    padding: 50px 0;
    justify-content: space-between;
  }

  .form-item {
    display: flex;
    width: 100%;
    align-items: center;
    position: relative;
    margin-bottom: 15px;
  }

  .form-item .input-name {
    width: 100px;
  }

  .form-item .form-txt {
    border: #DDD 1px solid;
    padding: 8px 10px;
    outline: none;
    display: block;
    flex-grow: 1;
    border-color: #d2d6de;
    width: 100%;
    height: 34px;
    padding: 6px 12px;
  }

  .svg-icon {
    width: 20px;
    height: 20px;
  }

  .code-panel {
    position: fixed;
    right: 0;
    top: 0;
    bottom: 0;
    width: 400px;
    background-color: #FFF;
    box-shadow: 1px 1px 15px #EEE;
    border-left: #ebebeb 1px solid;
    opacity: 0.9;
    z-index: 50;
  }

  .code-panel .inner {
    padding: 20px 40px;
    display: flex;
    flex-direction: column;
    height: 100%;
    box-sizing: border-box;
  }

  .code-panel textarea {
    resize: none;
    width: 100%;
    height: 100%;
    position: absolute;
    line-height: 22px;
    letter-spacing: 1px;
    padding: 0 10px;
  }

  .code-panel .area-html {
    height: 150px;
    position: relative
  }

  .code-panel .area-css {
    flex-grow: 1;
    position: relative
  }

  .code-panel .close-code-btn {
    position: fixed;
    top: 20px;
    right: 20px; 
  }

  .banner {
    text-align: center;
    background-color: #f9d732;
    padding: 50px;
  }

  .banner h1 {
    margin: 0;
  }
  .nav {
    background-color: #000;
    padding: 20px;
    text-align: center;
    color: #FFF;
  }

  footer {
    text-align: center;
    padding: 50px;
    color: #999
  }
</style>
