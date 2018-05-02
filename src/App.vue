<template>
  <div id="app">
    <div id="picDiv">
      <!-- 宽大于高的图片 -->
      <!-- <img src="./demo.jpg" id="pic" class="dragAble" ref='img' @mousewheel="scroll" @mousedown="imgInit" @mouseup="upinit"> -->
      <!-- 高大于宽的图片 -->
      <img src="./40705468_p0.jpg" id="pic" class="dragAble" ref='img' @mousewheel="scroll" @mousedown="imgInit" @mouseup="upinit">
    </div>

    <router-view/>
  </div>
</template>

<script>
  export default {
    name: 'App',
    data() {
      return {
        isdrag: false, // 不可拖动
        y: null, //点击的时候鼠标 在页面位置
        x: null,
        drageObj: null, // 拖动元素
        nTY: null, // 图片 相对盒子起始坐标
        nTX: null
      }
    },
    methods: {
      scroll(e) {
        // console.log(e)
        var obj = e.target
        // console.dir(e.target)
        // console.log(obj.style.zoom)

        //  let zoom = parseFloat(obj.style.zoom);
        //  console.log('zoom',zoom)
        //  let tZoom = zoom + (event.wheelDelta>0 ? 0.05 : -0.05);
        // console.log(e)
        var oImg = document.getElementById("pic");
        if (e.wheelDelta > 0) {
          // oImg.width = oImg.width * 1.2;
          // oImg.height = oImg.height * 1.2;
          // oImg.style.zoom = parseInt(oImg.style.zoom) + (args ? +20 : -20) + '%';
           oImg.style.width = oImg.width +50  +'px';
           oImg.style.height = 'auto';
          //  position
          // console.dir(oImg.style)
          // if(oImg.style.left!=''){
          //    console.log(oImg.style.left.split('p')[0])
          //    console.log(+oImg.style.left.split('p')[0]+50)
          //   oImg.style.left = +oImg.style.left.split('p')[0]-30+'px';
          // }
         
        } else {
          // oImg.width = oImg.width / 1.2;
          // oImg.height = oImg.height / 1.2;
           oImg.style.width = oImg.width -50+'px';
           oImg.style.height ='auto'
          //  oImg.style.left = '0px'
          // position
          // left
 
          if(oImg.style.left!=''){
            //  console.log(oImg.style.left.split('p')[0])
             console.log(+oImg.style.left.split('p')[0])
            
            oImg.style.left = +oImg.style.left.split('p')[0]+50+'px';
            if(+oImg.style.left.split('p')[0]>0){
              oImg.style.left ='0px';
            }
        
          }
          // top

           if(oImg.style.top!=''){
            //  console.log(oImg.style.left.split('p')[0])
             console.log(+oImg.style.top.split('p')[0])
              var step = oImg.height*50/oImg.width
            oImg.style.top = +oImg.style.top.split('p')[0]+step+'px';
            if(+oImg.style.top.split('p')[0]>0){
              oImg.style.top ='0px';
            }
          }
        }
        /*  缩放 */

        // 两个的区别就是一个字符串哟个数字
        // console.log(e.target.width)
        // console.log(e.target.style.width)
        //  console.log(event)
        //  console.log(e===event)  // true  e 就是event 对象
          // 判断能不更缩放
          console.log(e.target.width)
          // console.dir(e.target.parentElement)
          // console.log(e.target.parentElement.clientWidth)
          if(e.target.width<e.target.height){
            if(e.target.width<=e.target.parentElement.clientWidth){
            // console.log('笑了')
                e.target.style.width = '100%';
                e.target.style.height = 'auto';
            }
          }else{
            if(e.target.height<=e.target.parentElement.clientHeight){
                e.target.style.height = '100%';
                e.target.style.width = 'auto';
                console.log(e.target.style.height)
              
            }
          }

      },
      imgInit(e) {
        var obj = e.target;
        if (obj.className == 'dragAble') {
          console.log('点在鼠标身上了');
          this.isdrag = true;
          this.drageObj = obj;
          this.nTY = parseInt(this.drageObj.style.top + 0);
          console.log(' 厨师nTY', this.nTY)


          this.nTX = parseInt(this.drageObj.style.left + 0);
          this.y = e.clientY;
          console.log('厨师y', this.y);

          this.x = e.clientX;
          document.onmousemove = this.moveMouse // 给 document.mousemove绑定事件
          // document.querySelector('#picDiv').onmousemove = this.moveMouse
          //  document.querySelector('#picDiv').addEventListener('mousemove',this.moveMouse)

          //             // console.log(this.drageObj)
        }
      },
      moveMouse(e) {
        e.preventDefault(); // 不然拖不动
        // console.log(e.target.style.left);
        // console.dir(e.target)
        var mouseX = this.mousePoint(e).x;
        var mouseY = this.mousePoint(e).y;
        // console.log('mouseX',mouseX)
        // console.log('mouseY',mouseY)
        var boxPosL = this.boxPosition().xLeft
        var boxPosR = this.boxPosition().xRigh
        var boxPosT = this.boxPosition().yTop
        var boxPosB = this.boxPosition().yBottom
        //  console.log(boxPosL,boxPosR)
        // console.dir(e.target.offsetTop)
        // console.log(e.target.height)
        // console.dir(e.target.parentElement.offsetHeight)
        //  console.log( e.target.offsetTop>0&& e.target.parentElement.offsetHeight-e.target.offsetHeight>e.target.offsetTop)
        if (e.target != this.drageObj) {
          return
        }
        let flag1 = e.target.offsetTop >= 0 && e.target.parentElement.offsetHeight - e.target.offsetHeight >= e.target.offsetTop
        let flag2 = e.target.offsetLeft <= 0 && e.target.parentElement.offsetWidth - e.target.offsetWidth <= e.target.offsetLeft
        // console.log(flag2)
        // && mouseX>boxPosL&&mouseX<boxPosR&&mouseY>boxPosT&&mouseY<boxPosB
        // console.log(e.clientX,e.clientY)   // 鼠标x y 坐标
        // if( this.isdrag && mouseX>=boxPosL&&mouseX<=boxPosR&&mouseY>=boxPosT&&mouseY<=boxPosB ){
        //  console.log(flag1&&flag2)
        // console.log(flag1);
        // console.log(flag2);
        if (true) {
          // console.log(111)
          //  console.log(' 移动nTY',this.nTY) ;console.log('移动y',this.y);
          // console.log(e)
          this.drageObj.style.top = (this.nTY + e.clientY - this.y) + 'px';
          // console.log(this.drageObj.style.top)


          this.drageObj.style.left = (this.nTX + e.clientX - this.x) + 'px';
        } else {

        }

        /*     边界设置的有问题 没有考虑到 绝对值的问题 */


        // // 左边边界
        if (e.target.offsetLeft >= 0) {
          // console.log(666)
          this.drageObj.style.left = '0px'
        }
        // // 右边边界
        if (e.target.offsetLeft <= e.target.parentElement.offsetWidth - e.target.offsetWidth) {

          this.drageObj.style.left = (e.target.parentElement.offsetWidth - e.target.offsetWidth) + 'px'
        }
        // // 上边界
        if (e.target.offsetTop >= 0) {
          // console.log(666);
          this.drageObj.style.top = '0px';
        }
        // // 下边界
        if (e.target.offsetTop <= e.target.parentElement.offsetHeight - e.target.offsetHeight) {
          this.drageObj.style.top = (e.target.parentElement.offsetHeight - e.target.offsetHeight) + 'px'
        }

      },
      mousePoint(e) {
        var x, y;
        var e = e || window.event;
        return {
          x: e.clientX + document.body.scrollLeft + document.documentElement.scrollLeft,
          y: e.clientY + document.body.scrollTop + document.documentElement.scrollTop
        }
      },
      boxPosition() {
        var odiv = document.getElementById('picDiv');
        var xLeft, xRigh, yTop, yBottom;
        return {
          xLeft: odiv.getBoundingClientRect().left,
          xRigh: odiv.getBoundingClientRect().right,
          yTop: odiv.getBoundingClientRect().top,
          yBottom: odiv.getBoundingClientRect().bottom
        };
      },
      upinit() {
        this.isdrag = false;
        document.onmousemove = null;
      },
      // 图片占满窗口 初始化函数 宽》高 以高位主 ，宽《高 以高为主
      initSize(){
        this.$refs.img.onload=()=>{
             var width = this.$refs.img.width;
              var height= this.$refs.img.height;
              // console.dir(this.$refs.img)
              // console.log(width,height)
              if (width >height){
                // console.log(11)
                  
                    this.$refs.img.style.height = '100%'
                    // console.log(22)
                
              }else{
                // console.log(66)  
                    this.$refs.img.style.width = '100%'
              }
        }
         
          

      }



    },
    watch: {
      isdrag() {
        // console.log(this.isdrag)
      }

    },
    mounted(){
        this.initSize();
    }

  }

</script>

<style>
  #picDiv {
    margin: 8px 8px 4px 0;
    border: 1px solid #666666;
    padding: 0;
    width: 320px;
    height: 320px;
    float: left;
    overflow: hidden;
    position: relative;
    cursor: default;
  }

  .dragAble {
    left: 0px;
    top: 0px;
    position: absolute;
    cursor: move;
    user-select: none;
  }

</style>

