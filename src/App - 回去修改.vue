<template>
  <div id="app">
    <div id="picDiv">
        <img src="./demo.jpg" 
          id="pic"
          class="dragAble"
        @mousewheel="scroll" 
        @mousedown="imgInit"
        @mouseup="upinit"
        >
    </div>
    
    <router-view/>
  </div>
</template>

<script>
  export default {
    name: 'App',
    data(){
      return {
          isdrag: false, // 不可拖动
          y:null, //点击的时候鼠标 在页面位置
          x:null,
          drageObj:null, // 拖动元素
          nTY:null, // 图片 相对盒子起始坐标
          nTX:null
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
        console.log(e)
         var oImg = document.getElementById("pic");
            if (e.wheelDelta > 0) {
                oImg.width = oImg.width * 1.2;
                oImg.height = oImg.height * 1.2;
			   // oImg.style.zoom = parseInt(oImg.style.zoom) + (args ? +20 : -20) + '%';
            }
            else {
                oImg.width = oImg.width / 1.2;
                oImg.height = oImg.height / 1.2;
            }

        //  console.log(event)
        //  console.log(e===event)  // true  e 就是event 对象
         
      },
      imgInit(e){
          var obj = e.target;
          if(obj.className =='dragAble'){
            console.log('点在鼠标身上了');
            this.isdrag= true;
            this.drageObj = obj;
            this.nTY = parseInt(this.drageObj.style.top+0);
 console.log(' 厨师nTY',this.nTY)


            this.nTX = parseInt(this.drageObj.style.left+0);
            this.y = e.clientY;
            console.log('厨师y',this.y);
            
            this.x = e.clientX;
            document.onmousemove = this.moveMouse // 给 document.mousemove绑定事件
            // document.querySelector('#picDiv').onmousemove = this.moveMouse
//  document.querySelector('#picDiv').addEventListener('mousemove',this.moveMouse)

//             // console.log(this.drageObj)
          }
      },
      moveMouse(e){
        e.preventDefault(); // 不然拖不动
        
          var mouseX = this.mousePoint(e).x;
          var mouseY = this.mousePoint(e).y;
          // console.log('mouseX',mouseX)
          // console.log('mouseY',mouseY)
          var boxPosL =this.boxPosition().xLeft
          var boxPosR =this.boxPosition().xRigh
          var boxPosT =this.boxPosition().yTop
          var boxPosB =this.boxPosition().yBottom
//  console.log(boxPosL,boxPosR)
          // console.dir(e.target.offsetTop)
          // console.log(e.target.height)
          // console.dir(e.target.parentElement.offsetHeight)
//  console.log( e.target.offsetTop>0&& e.target.parentElement.offsetHeight-e.target.offsetHeight>e.target.offsetTop)
if(e.target!= this.drageObj ){
  return
}
 let flag1 = e.target.offsetTop>=0&& e.target.parentElement.offsetHeight-e.target.offsetHeight>=e.target.offsetTop
let flag2 = e.target.offsetLeft<=0&& e.target.parentElement.offsetWidth-e.target.offsetWidth<=e.target.offsetLeft
// console.log(flag2)
          // && mouseX>boxPosL&&mouseX<boxPosR&&mouseY>boxPosT&&mouseY<boxPosB
          // console.log(e.clientX,e.clientY)   // 鼠标x y 坐标
          // if( this.isdrag && mouseX>=boxPosL&&mouseX<=boxPosR&&mouseY>=boxPosT&&mouseY<=boxPosB ){
//  console.log(flag1&&flag2)
console.log(flag1);
console.log(flag2);
         


          if( true){
            // console.log(111)
//  console.log(' 移动nTY',this.nTY) ;console.log('移动y',this.y);
              // console.log(e)
              this.drageObj.style.top = (this.nTY + e.clientY - this.y)  +'px';
              // console.log(this.drageObj.style.top)


              this.drageObj.style.left = (this.nTX + e.clientX - this.x) +'px';
          }else{

          }

          /*     边界设置的有问题 没有考虑到 绝对值的问题 */
           // 左边边界
          if( e.target.offsetLeft>=0){
              // console.log(666)
              this.drageObj.style.left = '0px' 
          }
          // 右边边界
          if(e.target.offsetLeft<= e.target.parentElement.offsetWidth-e.target.offsetWidth){
              
              this.drageObj.style.left = (e.target.parentElement.offsetWidth-e.target.offsetWidth) +'px'
          }
          // 上边界
          if(e.target.offsetTop<=0){
              console.log(666);
              this.drageObj.style.top = '0px';
          }
          // 下边界
          if(e.target.offsetTop>=e.target.parentElement.offsetHeight-e.target.offsetHeight){
              this.drageObj.style.top = (e.target.parentElement.offsetHeight-e.target.offsetHeight)+'px'
          }

      },
      mousePoint(e){
          var x,y;
           var e = e || window.event;
          return{
            x:e.clientX+document.body.scrollLeft+document.documentElement.scrollLeft,
			      y:e.clientY+document.body.scrollTop+document.documentElement.scrollTop
          }
      },
      boxPosition(){
          var odiv=document.getElementById('picDiv');
          var xLeft,xRigh,yTop,yBottom;
          return {
            xLeft:odiv.getBoundingClientRect().left,
            xRigh:odiv.getBoundingClientRect().right, 
            yTop:odiv.getBoundingClientRect().top,
            yBottom:odiv.getBoundingClientRect().bottom
          };
      },
      upinit(){
         this.isdrag = false;
         document.onmousemove = null;
      }



    },
    watch:{
      isdrag(){
        // console.log(this.isdrag)
      }

    },
    mounted:{
      // 解绑鼠标抬起
      // freemouseup(){
      //   document.onmouseup=function(){
      //       this.isdrag = false;
      //    document.onmousemove = null;
      // }
      // }
      
    }

  }

</script>

<style>
    #picDiv
		{
		margin:8px 8px 4px 0;
		border:1px solid #666666;
		padding:0;
	    width:320px;
		height:320px;
		float:left;
		overflow:hidden;
		position:relative;
		cursor:default ;
		}

	 .dragAble
        {
          left: 0;
          top: 0;
            position: absolute;
            cursor: move;
            user-select:none;
        }

</style>
