<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>canvas粒子连线</title>
    <style>
        *{margin:0;}
        #canvas{
            background: #000;
            display:block;
        }
    </style>
</head>
<body>
<canvas id="canvas"></canvas>
<script>
    function Starry(){//粒子的构造函数
        this.cxt = canvas.getContext("2d");
        this.num = 200;
        this.data = [];//存储数据
    }
    Starry.prototype = {
        //参数初始化方法
        init:function(){
            canvas.width = window.innerWidth;
            canvas.height = window.innerHeight;
            this.cW = canvas.width;
            this.cH = canvas.height;
            for(var i=0;i<this.num;i++){
                //位置，运动速度 大小(小圆点参数)
                this.data[i] = {
                    //位置
                    x: Math.random() * this.cW,
                    y: Math.random() * this.cH,
                    //速度Ma(- 反方向 +正方向)
                    sX: Math.random() * 0.6 - 0.3,
                    sY: Math.random() * 0.6 - 0.3
                };
            }
        },
        //画小圆对象方法
        drawCircle:function(x,y){
            this.cxt.save();                          //保存路径
            this.cxt.fillStyle = "white";             //填充颜色 实心圆
            this.cxt.beginPath();                     //开始路径
            this.cxt.arc(x,y,0.3,0,Math.PI*2,false);  //arc(x,y,r,start,end,false)
            this.cxt.closePath();                     //结束路径
            this.cxt.fill();                          //画方法
            this.cxt.restore();                       //释放路径
        },
        //绘制线条方法
        drawLine:function(x1,y1,x2,y2){
            var cxt = this.cxt;
            cxt.save();
            var line = cxt.createLinearGradient(x1,y1,x2,y2);
            line.addColorStop(0,'#369');
            line.addColorStop(1,'#333');
            cxt.strokeStyle = line;
            cxt.beginPath();
            cxt.moveTo(x1,y1);
            cxt.lineTo(x2,y2);
            cxt.stroke();
            cxt.restore();
        },
        //小圆移动方法
        moveCircle:function () {
            //清楚上一次绘制的图形
            this.cxt.clearRect(0,0,this.cW,this.cH);
            for(var i=0;i<this.num;i++){//当前的点
                //变量提升原则
                var xValue = this.data[i].x,
                    yValue = this.data[i].y;

                //边界值检测
                if(xValue>this.cW || xValue<0){
                    this.data[i].sX = -this.data[i].sX;
                }
                if(yValue>this.cH || yValue<0){
                    this.data[i].sY = -this.data[i].sY;
                }
                //确定圆心的位置
                this.data[i].x += this.data[i].sX;
                this.data[i].y += this.data[i].sY;
                //调用绘图方法
                this.drawCircle(this.data[i].x,this.data[i].y);
                //用勾股定理判断是否连线
                for(var j=i+1;j<this.num;j++){//下一个点 确定
                    var xV1 = this.data[i].x,
                        yV1 = this.data[i].y,
                        xV2 = this.data[j].x,
                        yV2 = this.data[j].y;
                    if(Math.pow(xV1-xV2,2)+Math.pow(yV1-yV2,2) <=2000){
                        //连线方法
                        this.drawLine(xV1,yV1,xV2,yV2);
                    }
                }
            }
        }
    };
    var starry = new Starry();//实例化对象
    starry.init();
    setInterval(function () {
        starry.moveCircle();
        },1000/60)
</script>
    <!--
        js + canvas
        js:
        canvas:画布
             js驱动canvas
          面向对象
          es5：构造函数 放私有属性的（）
          es6：class
          基于类的拓展
             原型：放公共方法
          this 怎么来
             有对象   new实例化对象
        canvas粒子连线的思路
            1、创建不同位置，相同大小的小圆
            2移动
            dom left top
            canvas
                  绘制运动路径
                     时间自定义
                  粒子根据于东路径在当前路径不断地进行绘制
                  在绘制的过程中清楚掉你上一次的绘制路线
           3、边界值判断
               通过运动路径去进行边界的碰撞检测
           4、粒子连线
               勾股定理

           5，判断
              鼠标有没有进来
              this.drawLine(a,b,c,d)


           学编程最后：计算机编译原理 底层 设计原理 数据算法 数据结构
           码农：只会做项目（重复的写逻辑代码），用框架的、插件的
           前端：也要学习算法，
           工程师：提供解决方案，性能优化...
           5,
    -->
</body>
</html>
