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
          var obj = e.target
         var oImg = document.getElementById("pic");
            if (e.wheelDelta > 0) {
                oImg.width = oImg.width * 1.2;
                oImg.height = oImg.height * 1.2;
            }
            else {
                oImg.width = oImg.width / 1.2;
                oImg.height = oImg.height / 1.2;
            }        
      },
      imgInit(e){
          var obj = e.target;
          if(obj.className =='dragAble'){
            this.isdrag= true;
            this.drageObj = obj;
            this.nTY = parseInt(this.drageObj.style.top+0);
            this.nTX = parseInt(this.drageObj.style.left+0);
            this.y = e.clientY;       
            this.x = e.clientX;
            document.onmousemove = this.moveMouse // 给 document.mousemove绑定事件
          }
      },
      moveMouse(e){
        e.preventDefault(); // 不然拖不动
        
          var mouseX = this.mousePoint(e).x;
          var mouseY = this.mousePoint(e).y;
          var boxPosL =this.boxPosition().xLeft
          var boxPosR =this.boxPosition().xRigh
          var boxPosT =this.boxPosition().yTop
          var boxPosB =this.boxPosition().yBottom
          if(e.target!= this.drageObj ){
            return
          }
          /* 
            边界判断后面在写
           */
          if( this.isdrag && mouseX>boxPosL&&mouseX<boxPosR&&mouseY>boxPosT&&mouseY<boxPosB){
              this.drageObj.style.top = (this.nTY + e.clientY - this.y)  +'px';
              this.drageObj.style.left = (this.nTX + e.clientX - this.x) +'px';
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
    mounted(){
       // 解绑鼠标抬起
      document.onmouseup=function(){
              this.isdrag = false;
              document.onmousemove = null;
      }
     
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
