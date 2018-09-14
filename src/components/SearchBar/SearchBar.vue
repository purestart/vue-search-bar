<template>

  <div class="image-dialog">
    <div class="image-dialog-trigger" :class="loaded==true?'hidden':''" type="button" @click="showDialog">
        <div ref="thumb" class="input-wrapper">
            <div class="input"  >
                <i class="iconfont icon-search1187938easyiconnet"></i>
                <span class="palceholder">名称/单号/库房</span>
            </div>
        </div>
     </div>
    <transition name="dialog" @enter="enter" @leave="leave">
      <div  class="image-dialog-background" v-show="appearedDialog" ref="dialog">
        <button v-if="false" class="image-dialog-close" type="button" aria-label="Close"></button>
        <!-- <img @click.stop="()=>{}" class="image-dialog-animate" ref="animate" :class="{ loading: !loaded }" :src="loaded ? full : thumb"/> -->
        <div ref="animate"  class="input-wrapper image-dialog-animate">
            <div  class="input"  >
                <i class="iconfont icon-search1187938easyiconnet"></i>
                <span class="palceholder">名称/单号/库房</span>
            </div>
        </div>
        <div ref="full" @load="onLoadFull"  class="input-wrapper image-dialog-full">
            <div  class="input"  >
                <i class="iconfont icon-search1187938easyiconnet"></i>
                <input @input="onInput" v-model="keyword" placeholder="名称/单号/库房" class="palceholder" />
                <i @click="()=>{keyword=''}" class="iconfont icon-close1"></i>
            </div>
            <div ref="cancleBtn" @click="submitForm" class="cancle-btn">
                {{keyword.length>0?'搜索':'取消'}}
            </div>
        </div>
        <div :class="slotVisiable?'visible':''" class="slot-content" >
            <slot></slot>
        </div>
      </div>
    </transition>


  </div>
</template>

<script type='text/ecmascript-6'>
export default {
  props: {
    thumb: String,
    full: String,
    radius:Number,
    onSearch:{
        type:Function,
        required:true,
        default:()=>{}
    },
    onChange:{
        type:Function,
        required:false,
        default:()=>{}
    },
  },

  data() {
    return {
      loaded: false,
      slotVisiable:false,
      appearedDialog: false,
      fullWidth: 0,
      fullHeight: 0,
      keyword:"",
      showCancle:false

    };
  },
  created(){
      this.fullWidth=window.screen.width;
      this.fullHeight=window.screen.height;
  },
  mounted(){


  },
  methods: {
      onInput(e){
          this.onChange(e.target.value);
      },
      submitForm(){
        if(this.keyword.length>0){
            this.appearedDialog=false;
            this.onSearch(this.keyword);
            
        }else{
            this.hideDialog();
        }
      },
    getImgNaturalDimensions(oImg, callback) {
      var nWidth, nHeight;
      if (!oImg.naturalWidth) {
        // 现代浏览器

        nWidth = oImg.naturalWidth;
        nHeight = oImg.naturalHeight;
        callback({ w: nWidth, h: nHeight });
      } else {
        // IE6/7/8
        var nImg = new Image();

        nImg.onload = function() {
          var nWidth = nImg.width,
            nHeight = nImg.height;
          callback({ w: nWidth, h: nHeight });
        };
        nImg.src = oImg.src;
      }
    },

    getEleNaturalDimensions(oImg, callback) {
        var nWidth, nHeight;
        nWidth = oImg.offsetWidth;
        nHeight = oImg.offsetHeight;
        //console.log(oImg);
        callback({ w: nWidth, h: nHeight });
    },

    showDialog() {
    const thumb = this.$refs.thumb;
    this.getEleNaturalDimensions(thumb,(dimension)=>{
        //console.log(dimension);
        if(dimension.w>dimension.h){
            this.fullHeight=(dimension.h/dimension.w)*this.fullWidth;
        }else{
            this.fullWidth=(dimension.w/dimension.h)*this.fullHeight;
        }

        //console.log(this.fullWidth + "--" +this.fullHeight);
        //判断最大允许宽度
        if(this.maxWidth && this.maxWidth>0 && this.fullWidth>this.maxWidth){
            
          this.fullWidth=this.maxWidth;
          this.fullHeight=(dimension.h/dimension.w)*this.fullWidth;
        }

        //判断最大允许高度
        if(this.maxHeight && this.maxHeight>0 && this.fullHeight>this.maxHeight){
            
          this.fullHeight=this.maxHeight;
          this.fullWidth=(dimension.w/dimension.h)*this.fullHeight;
        }
        
    });

      this.appearedDialog = true;
    },

    hideDialog() {
      this.appearedDialog = false;
    },

    enter() {
      this.animateImage(this.$refs.thumb, this.$refs.full,this.$refs.cancleBtn,1);
      setTimeout(()=>{
          this.loaded=true;
          this.slotVisiable=true;
      },300)
    },

    leave() {
      this.animateImage(this.$refs.full, this.$refs.thumb,this.$refs.cancleBtn,0);
      this.slotVisiable=false;
        setTimeout(()=>{
            this.loaded=false;
        },300)
    },

    onLoadFull() {
      this.loaded = true;
    },

    animateImage(startEl, destEl,cancleEl,type) {
      const start = this.getBoundForDialog(startEl);
      this.setStart(start);

      const gapEl=this.getBoundForDialog(cancleEl);

      //console.log("start");
      //console.log(start);


      this.$nextTick(() => {
        const dest = this.getBoundForDialog(destEl);
        //console.log("dest")
        //console.log(dest);
        this.setDestination(start, {
          top: dest.top,
          left: dest.left,
          width: dest.width || this.fullWidth,
          height: dest.height || this.fullHeight
        },gapEl,type);
      });
    },

    getBoundForDialog(el) {
      const bound = el.getBoundingClientRect();
      //console.log(bound);
      const dialog = this.$refs.dialog;
      return {
        top: bound.top + dialog.scrollTop,
        left: bound.left + dialog.scrollLeft,
        width: bound.width,
        height: bound.height
      };
    },

    setStart(start) {
      const el = this.$refs.animate;
      el.style.left = start.left + "px";
      el.style.top = start.top + "px";
      el.style.width = start.width + "px";
      el.style.height = start.height + "px";
      el.style.transitionDuration = "0s";
      el.style.transform = "";
    },

    setDestination(start, dest,gapEl,type) {
      const el = this.$refs.animate;
      el.style.transitionDuration = "";

    //     console.log("变更");
    //   console.log(start);
    //   console.log(dest);
    //   console.log(gapEl)

      if(type==1){
          dest.width=dest.width-gapEl.width;
      }else{
          //start.width=start.width-gapEl.width;
          //dest.width=dest.width-gapEl.width;
         // dest.width=start.width;
      }
      

      const translate = `translate(${dest.left - start.left}px, ${dest.top -
        start.top}px)`;
      const scale = `scale(${dest.width / start.width}, ${dest.height /
        start.height})`;
      
      el.style.transform = `${translate} ${scale}`;
    }
  }
};
</script>

<style lang='scss' scoped>
@import '../../../static/font_demo/iconfont.css';
*,
*::before,
*::after {
  box-sizing: border-box;
}
.image-dialog {
  width: 100%;

  .image-dialog-trigger {
    width: 100%;
    &.hidden{
      opacity: 0;
    }
  }

    .slot-content{
        transition: opacity .6s ease;
        opacity: 0;
        &.visible{
            opacity: 1;
        }
    }

    .image-dialog-full{
        display: flex;
        .cancle-btn{
            display: flex;
            align-items: center;
            font-size: 15px;
            padding-left: 10px;
            padding-right: 4px;
            color: #777777;
        }
    }
    .input-wrapper{
         background-color: #efeff4;
        border-bottom:1px solid #e4e4e4;
        padding: 2vw 2vw;
        box-sizing: border-box;
        width: 100vw;

        .input{
            border: none;
            outline: none;
            padding: 5px;
            font-size: 15px;
            border-radius: 5px;
            
            min-height: 7vw;
            background-color: #ffffff;
            display: flex;
            align-items: center;
            flex: 1;
            transition: width 3s;

            input{
                border: none;
                outline: none;
                padding: 5px;
                font-size: 15px;
                border-radius: 5px;
                padding-left: 0;
                flex: 1;
            }

            .iconfont{
                color: #999;
                margin-left: 1vw;
                margin-right: 2vw;
                font-size: 15px;
                padding-top: 0px;
            }
            .palceholder{
                color: #999;
            }
        }
    }

  
}
.image-dialog-thumb{
    object-fit: cover;
}
.image-dialog-trigger {
  margin: 0;
  padding: 0;
  background: none;
  border: none;
  cursor: pointer;
}
.image-dialog-close {
  position: absolute;
  right: 20px;
  top: 20px;
  width: 40px;
  height: 40px;
  padding: 0;
  background: none;
  border: none;
  cursor: pointer;
  -webkit-transition: 300ms ease-out;
  transition: 300ms ease-out;
  outline: none;
}
.image-dialog-close::before,
.image-dialog-close::after {
  content: "";
  position: absolute;
  left: 50%;
  top: 50%;
  margin-top: -0.5px;
  margin-left: -20px;
  width: 30px;
  height: 1px;
  background-color: #000;
}
.image-dialog-close::before {
  -webkit-transform: rotate(45deg);
  transform: rotate(45deg);
}
.image-dialog-close::after {
  -webkit-transform: rotate(135deg);
  transform: rotate(135deg);
}
.image-dialog-close:hover {
  -webkit-transform: rotate(270deg);
  transform: rotate(270deg);
}
.image-dialog-background {
  overflow: auto;
  position: fixed;
  top: 0;
  right: 0;
  left: 0;
  bottom: 0;
  width: 100vw;
  height: 100vh;
  background-color: rgba(255, 255, 255, 0.95);
  text-align: center;
  z-index: 999;
  
}
.image-dialog-animate {
  display: none;
  position: absolute;
  -webkit-transform-origin: left top;
  transform-origin: left top;
}
.image-dialog-animate.loading {
  display: block;
}

.dialog-enter-active,
.dialog-leave-active {
  -webkit-transition: background-color 300ms ease-out;
  transition: background-color 300ms ease-out;
}
.dialog-enter,
.dialog-leave-to {
  background-color: rgba(255, 255, 255, 0);
}
.dialog-enter-active .image-dialog-animate,
.dialog-leave-active .image-dialog-animate {
  display: block;
  -webkit-transition: -webkit-transform 300ms cubic-bezier(1, 0, 0.7, 1);
  transition: -webkit-transform 300ms cubic-bezier(1, 0, 0.7, 1);
  transition: transform 300ms cubic-bezier(1, 0, 0.7, 1);
  transition: transform 300ms cubic-bezier(1, 0, 0.7, 1),
    -webkit-transform 300ms cubic-bezier(1, 0, 0.7, 1);
}
.dialog-enter-active .image-dialog-full,
.dialog-leave-active .image-dialog-full {
  visibility: hidden;
}
.image-dialog-trigger img {
  width: 100%;
  height: 100%;
}
</style>