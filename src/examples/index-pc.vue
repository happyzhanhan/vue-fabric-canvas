<template>
    <div class="container">
        <!--<div class="header">
            <div class="logo">
                LOGO
            </div>

        </div>-->
        <div class="content-wrapper">
            <!--    <div class="list-wraper">
                  <div :key="item.id" v-for="item in list" class="image-wrapper">
                    <img :src="item.url" />
                    <i @click="handleAdd(item.url)" class="pt-iconfont icon-plus-circle"></i>
                  </div>
                </div>-->

            <vue-fabric ref="canvas" :width="width" :height="height" @selection:updated="selectionfunction" @selection:changed="selectionchangedfunction" @selection:created="selected"
                        @text:editing:entered = "listentext"
                        @text:changed = "listentextchange" id="cavas"></vue-fabric>
            <!--<div class="tool-wrapper">

                <input type="text" id="code" value='1'>
                <textarea cols="30" rows="10" id="lcq" ></textarea>

                <i @click="handleDelete" class="pt-iconfont icon-delete"></i>
                <i @click="rotate" class="pt-iconfont icon-shuaxin"></i>
                <i @click="createImg" class="pt-iconfont icon-crop"></i>
                <button @click="selectobj">组合拖拽</button>
                <button @click="discardobj">取消选择</button>
            </div>-->
        </div>
        <vue-image-model :close="()=>{imgUrl=''}" v-show="imgUrl.length>0" :url="imgUrl"></vue-image-model>
    </div>
</template>

<script type='text/ecmascript-6'>

    import VueImageModel from '../components/image-model.vue';

    export default {
        components: {
            VueImageModel
        },
        data () {
            return {
                // http://data618.oss-cn-qingdao.aliyuncs.com/ys/3524/img/b.jpg
                imgUrl: '',
                width: 1920,
                height: 800,
                newobj:{},
                list: [
                    {
                        id: 1,
                        url: '/static/images/sticker1.png'
                    },
                    {
                        id: 2,
                        url: '/static/images/sticker2.png'
                    },
                    {
                        id: 3,
                        url: '/static/images/sticker3.png'
                    },
                    {
                        id: 4,
                        url: '/static/images/sticker4.png'
                    },
                    {
                        id: 5,
                        url: '/static/images/sticker5.png'
                    }
                ]
            };
        },
        created () {
            this.width = document.body.offsetWidth - 200;
            this.height = document.body.offsetHeight - 60;
            console.log(document.body.offsetWidth);


        },
        mounted () {
            fabric.Writebox = fabric.util.createClass(fabric.Rect, {
                type: 'writebox',
                initialize: function(element, options) {
                    this.callSuper('initialize', element, options);
                    options && this.set('lockUniScaling', options.lockUniScaling);
                    options && this.set('label', options.label || '');
                    this.set('originX', 'left');
                    this.set('originY', 'top');
                },
                toObject: function() {
                    return fabric.util.object.extend(this.callSuper('toObject'), {
                        label: this.label,
                        lockUniScaling: this.lockUniScaling,
                        width:this.width,
                        height:this.height,
                    });
                },
                _render: function(ctx) {
                    this.callSuper('_render', ctx);
                    ctx.top = 0;
                    ctx.left = 0;
                    ctx.font = this.fontSize + 'px Times';
                    ctx.fillStyle = '#333';
                    ctx.scaleX = 1;
                    ctx.scaleY = 1;
                    ctx.editable = true;
                    ctx.fillText(this.label,-this.width / 2 + this.a, -this.height / 2 + this.b );  //-this.width / 2 + 4, -this.height / 2 + 20
                }
            });
            var labelRect = new fabric.Writebox({
                type:'Writebox',
                width: 300,
                height: 50,
                left: 300,
                top: 100,
                label: '123456A中国的文字',
                a:0,
                b:35,
                fontSize:50,
                fill: '#faa',
                padding:0,
                stroke: 1
            });
            this.$refs.canvas.canvas.add(labelRect);


            // this.$refs.canvas.canvas.createRectLabel();


            var circle = new fabric.Rect({
                width: 100,
                height:50,
                fill: '#f00',
                originX: 'left',
                originY: 'top'
            });
            var text = new fabric.Text('hello world', {
                width:100,
                height:50,
                fontSize: 50,
                originX: 'left',
                originY: 'top',
                scaleX:1,
                scaleY:1,
            });
            var group = new fabric.Group([ circle, text ], {
                left: 650,
                top: 100,
                angle: 0
            });
            this.$refs.canvas.canvas.add(group);





            var LabeledRect = fabric.util.createClass(fabric.Rect, {
                type: 'labeledRect',
                initialize: function(options) {
                    options || (options = { });
                    this.callSuper('initialize', options);
                    this.set('originX', 'left');
                    this.set('originY', 'top');
                    this.set('width', options.width || '');
                    this.set('height', options.height || '');
                    this.set('label', options.label || '');
                    this.set('labelFont', options.labelFont || '');
                    this.set('fill', options.backfill || '');
                    this.set('labelposition', options.labelposition || '');
                },
                toObject: function() {
                    return fabric.util.object.extend(this.callSuper('toObject'), {
                        originX:'left',
                        originY:'top',
                        label: this.get('label'),
                        labelFont: this.get('labelFont'),
                        labelFill: this.get('backfill'),
                        fill: this.get('fill'),
                        labelposition: this.get('labelposition'),
                        width: this.get('width'),
                        height: this.get('height'),

                    });
                },
                _render: function(ctx) {
                    this.callSuper('_render', ctx);
                    // ctx.font = '20px Helvetica';
                    // ctx.fillStyle = '#333';
                    console.log('this', this);
                    ctx.originY = 'top';
                    ctx.originX = 'left';
                    ctx.width = this.width;
                    ctx.height = this.height;
                    ctx.font = this.labelFont;
                    ctx.fillStyle = this.labelFill;
                    ctx.textAlign = this.labelposition;
                    // ctx.fillText(this.label, -this.width/2, -this.height/2 + 20);
                    ctx.fillText(this.text, -this.width/2, -this.height/2 + 20);
                }
            });

            var labeledRect = new LabeledRect({
                width: 300,
                height: 20,
                left: 800,
                top: 20,
                text: 'abcdefg',
                fill: 'rgba(0,0,0,0)',
                labelFont: '20px Helvetica',
                labelFill: '#f00',
                backfill:"#f1f1f1",
                labelposition:'left',
                padding:0,
            });
            this.$refs.canvas.canvas.add(labeledRect);




            this.$refs.canvas.createText('我是text',{top:100,left:500,width:300,height:200,});
            /*   this.$refs.canvas.createIText('我是测试Itext',{type:1,width:200, height:100, top: 200, left: 300,fontSize: 16,angle:0,padding:0,});*/


            /* this.$refs.canvas.createRect({width:878, height:100,fillColor:'#000',scaleX:1,scaleY:1, top: 370, left: 298,angle:0,padding:0,});*/

            /* this.$refs.canvas.createIText('ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz',{type:1,width:100, height:100,scaleX:1,scaleY:5, top: 200, left: 300,fontSize: 30,angle:0,padding:0,lineHeight:2,});*/

            //我是新的文本数据我是新的文本数据我是新的文本数据我是新的文本数据我是新的文本数据padding:-5 -1, lockUniScaling:true,  backgroundColor: '#000',
            this.$refs.canvas.createTextbox('abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ', {
                type:1,width:600,height:300,scaleX:1,scaleY:1, top: 400, left: 700,fontSize:30,angle:0,splitByGrapheme: true,textAlign:'left',horizontal:'center',verticalAlign: 'middle',fillColor:'#000',backgroundColor:'#ff0',selectionBackgroundColor:'#f00',padding:0, drawWidth:20,color:'#f00',stroke:"blue",textBackgroundColor:'#eee',});



            //fillColor： 矩形 的填充颜色
            //strokeWidth:20, 边框粗细
            // stroke:"#f00" 边框颜色


            //圆角矩形
            this.$refs.canvas.createRect({width:50,height:40, top: 10, left: 1500,angle:0,fillColor:'#f1f1f1',strokeWidth:1,stroke:"#f00",padding:0,
                rx:10,ry:10,});
            //平行四边形
            this.$refs.canvas.createRect({width:50,height:40, top:60, left: 1495,angle:0,fillColor:'#f1f1f1',strokeWidth:1,stroke:"#f00",padding:0,
                skewX:-10,});
            //三角形
            this.$refs.canvas.createTriangle({x:10, y: 40, x1: 60, y1: 40, x2: 35, y2:10,
                top: 120, left: 1490,angle:0,color:'green',fillColor:'#f1f1f1',strokeWidth:1,stroke:"#f00",padding:0,
            });
            //椭圆形
            this.$refs.canvas.createEllipse({rx:20,ry:30,top: 180, left: 1550,fillColor:'#f1f1f1',drawWidth:20,color:'#f00',strokeColor:"#f00",padding:0,originX:'left',originY:'top'});
            //矩形
            this.$refs.canvas.createRect({width:50,height:40, top: 240, left: 1490,angle:0,fillColor:'#f1f1f1',strokeWidth:3,stroke:"#f00",padding:0,
            });

            //线条
            this.$refs.canvas.createLine({ x: 0, y: 0, x1: 50, y1: 0, strokeColor: '#B0B0B0',width:50,height:40, top:320, left: 1490,angle:0,fillColor:'#f1f1f1',strokeWidth:2,stroke:"#f00", });

            //虚线
            this.$refs.canvas.drawDottedline({ x: 0, y: 0, x1: 50, y1: 0, color: '#B2B2B2', top:360, left: 1490, drawWidth: 2, offset: 6, empty: 3 });

            //箭头
            this.$refs.canvas.drawArrowLine({  x: 0, y: 0, x1: 0, y1: 0, color: '#B2B2B2', top:400, left: 1490, drawWidth: 2, fillColor: 'rgba(255,255,255,0)', theta: 35, headlen: 35 });

            this.$refs.canvas.createTextboxnew('abcdefghijklmnopqrstuvwxyzAZ', {
                type:1,width:600,height:300,scaleX:1,scaleY:1, top: 300, left: 700,fontSize:30,angle:0,splitByGrapheme: true,textAlign:'left',horizontal:'center',verticalAlign: 'middle',padding:0,});


            this.$refs.canvas.createIText('我abcdefghjklABCDEFGHJ',{type:1,width:100, top: 200, left: 300,fontSize: 50,angle:0,scaleY:1,textAlign:'right',deltaY:0,padding:0,}); //原文本，根据文本缩小宽度 textBackgroundColor:'#eee', backgroundColor:'#ff0',selectionBackgroundColor:'#f00',


            this.$refs.canvas.createShelf('/static/shelf.svg',{
                top:30,
                left:40,
                width:100,
                height:100,
                angle:0,
                id:1,
            });

            this.$refs.canvas.createShelfNumber('/static/shelf.svg',{
                top:230,
                left:40,
                width:100,
                height:100,
                angle:0,
                id:1,
                text:'123',
            });



            /* this.$refs.canvas.creatTexboxWrap('我是中国人，我要测试一下中国的文字！', {type:1,width:100, top: 200, left: 500,fontSize: 23,angle:0, }); //新文本
             this.$refs.canvas.createText('TEXT', {type:1,width:100, top: 50, left: 300,fontSize: 23,angle:0, });  //时间时禁止选择


           this.$refs.canvas.createIText2('我是测试IcreateIText2',{type:1,width:200,scaleX:1,scaleY:5, top: 200, left: 500,fontSize: 16,angle:0,padding: 0,}); //原文本，根据文本缩小宽度
             this.$refs.canvas.createTextbox('测试的', {type:1,width:100,scaleX:1,scaleY:5, top: 400, left: 300,fontSize: 23,angle:0, });  //原动态数据，

             this.$refs.canvas.createRectno({width:100,height:50,fillColor:"#eee",top:200,left:500,angle:0,})

           this.$refs.canvas.createLine({ fillColor:'#ccc',strokeColor:'#ddd',top:100,left:100 })*/

            /* this.$refs.canvas.createTriangle({ id: 'Triangle', x: 100, y: 100, x1: 150, y1: 200, x2: 180, y2: 190, fill: 'yellow', left: 80 });
           //  this.$refs.canvas.createImage('/static/images/sticker1.png', { id: 6, width: 100, height: 100, left: 110, top: 110,angle:0, });
             this.$refs.canvas.createImagenew('http://zkong.com/images/logo.png', { id: '8', width: 100, height: 100, left: 310, top: 210 ,angle:0,});
               this.$refs.canvas.createImage('http://zkong.com/images/logo.png', { id: 9, width: 100, height: 100, left: 510, top: 210 ,angle:0,});
            //  this.$refs.canvas.createImagenomove('/static/images/sticker1.png', { id: 6, width: 100, height: 100, left: 110, top: 110,angle:0, });
             let options = {
               x: 100, y: 100, x1: 600, y1: 600, color: '#B2B2B2', drawWidth: 2, id: 'Triangle'
             };
             this.$refs.canvas.drawDottedline(options);
             this.$refs.canvas.setZoom(2);
              this.$refs.canvas.createEllipse({ rx: 200, ry: 400, left: 300 });


               this.$refs.canvas.createRect({width:100,height:20, top: 80, left: 400,angle:0,fill: 'yellow', });
               this.$refs.canvas.createCircle({width:100,rx:50,ry:30, top:100, left: 500,angle:0, fill: 'yellow',});



            this.$refs.canvas.setCornerIcons({ size: 20, mtr: '/static/images/cow.png' });
               this.$refs.canvas.newgroup();



           var _this = this;
           document.onkeydown = function(e) {
               let key = window.event.keyCode;
               if(event.ctrlKey && key === 67){
                   console.log("ctrl+c");
                   _this.$refs.canvas.toCopy();
               }
               if(event.ctrlKey && key === 86){
                   console.log("ctrl+v"+19);

                   _this.$refs.canvas.toPaste(19);
               }
           }*/
            var _this = this;
            document.onkeydown = function(e) {
                let key = window.event.keyCode;
                if(event.ctrlKey && key === 67){
                    console.log("ctrl+c");
                    // _this.$refs.canvas.toCopy();

                    //获取input并执行选中
                    var value = document.getElementById('code').value;
                    console.log(escape(document.getElementById('code').value));
                    document.getElementById('code').value = escape(document.getElementById('code').value);
                    document.getElementById('code').select();

                    //执行documen的copy事件
                    document.execCommand('copy');
                    document.getElementById('code').value = value;

                }
                if(event.ctrlKey && key === 86){
                    console.log("ctrl+v"+19);

                    _this.$refs.canvas.toPaste(19);
                }
            }

            // console.warn(document.getElementById('cavas'));
           /* document.getElementById('lcq').onpaste = function(e){
                if(e.clipboardData) {
                    console.log('谷歌,火狐');
                    for (var i = 0, len = e.clipboardData.items.length; i < len; i++) {
                        var item = e.clipboardData.items[i];
                        if (item.kind === "string") {
                            item.getAsString(function (str) {
                                console.log(str);
                            })
                        } else if (item.kind === "file") {
                            var f= item.getAsFile();
                            console.log(f);
                        }
                    }
                }else if(window.clipboardData) {
                    console.log('ie9及其上');
                    var txt = window.clipboardData.getData("Text");
                    console.log(txt);
                }else {
                    alert('浏览器版本过低')
                }
            }*/
            document.getElementById('cavas').addEventListener("paste",  function(e){
                console.log("('cavas').onpaste");
                if(e.clipboardData) {
                    console.log('谷歌,火狐');
                    for (var i = 0, len = e.clipboardData.items.length; i < len; i++) {
                        var item = e.clipboardData.items[i];
                        if (item.kind === "string") {
                            item.getAsString(function (str) {
                                console.log(str);
                            })
                        } else if (item.kind === "file") {
                            var f= item.getAsFile();
                            console.log(f);
                        }
                    }
                }else if(window.clipboardData) {
                    console.log('ie9及其上');
                    var txt = window.clipboardData.getData("Text");
                    console.log(txt);
                }else {
                    alert('浏览器版本过低')
                }
            })

            document.addEventListener('paste', function (event) {
                console.warn('text:'+event.clipboardData.getData('Text'))

                var text = event.clipboardData.getData('Text');
                console.warn(unescape(text));

                /*for (var i = 0, len = event.clipboardData.items.length; i < len; i++) {
                  var item = event.clipboardData.items[i];
                  if (item.kind === "string") {
                    item.getAsString(function (str) {
                      console.warn('1'+str);
                    })
                  } else if (item.kind === "file") {
                    var f= item.getAsFile();
                    console.warn('2'+f);
                  }
                }*/
            })


        },
        methods: {
            discardobj(){
                this.$refs.canvas.discardobj();
            },
            selectobj(){
                this.$refs.canvas.multiselect();
            },
            discard(){

            },

            handleAdd (url) {
                this.$refs.canvas.createImage(url);
            },
            handleDelete () {
                this.$refs.canvas.removeCurrentObj();
            },
            rotate () {
                this.$refs.canvas.setRotate();
                // this.$refs.canvas.moveTo();
                let obj = this.$refs.canvas.getEditObj();
                this.$refs.canvas.moveToshow(obj,0);
            },
            createImg () {
                let dataUrl = this.$refs.canvas.toDataUrl();
                // console.log(dataUrl);
                this.imgUrl = dataUrl;
            },
            selected (obj, option) {
                console.log(obj);

                //  this.$refs.canvas.setContronbarcode(obj.selected[0]);
                //  this.$refs.canvas.setContronbarcode(obj[0]);
            },
            selectionfunction (obj, option) {
                console.log(obj.selected);
                //this.$refs.canvas.ungroup(obj.selected);
                console.log(option);
            },
            selectionchangedfunction(){
                console.log('selectionchangedfunction');
            },

            listentext(e){
                e.target.fixedWidth = e.target.width;
                e.target.textLength = e.target.text.length;
            },
            listentextchange(e){

                console.log(e);

                let truetextheight = e.target._cacheContext.measureText(e.target.text).actualBoundingBoxAscent + e.target._cacheContext.measureText(e.target.text).actualBoundingBoxDescent;
                let boxheight = e.target.height;

                console.log(truetextheight);

                // e.target.scaleY = boxheight/truetextheight;
                // e.target.lineHeight = 0.5;
                //  e.target.height = truetextheight;

                // e.target._fontSizeFraction = 0.5;
                // e.target._fontSizeMult=0.66;

                // e.target.deltaY = -(boxheight-truetextheight)/2;

                console.log(e.target._cacheContext.measureText(e.target.text));

                /*var h = this.$refs.canvas.canvas;
                var ctx = h.getContext("2d");//把canvas的画笔给调出来
                console.log(ctx);
                ctx.font="30px Arial";//设置字体大小和字体，这一步很重要，直接影响了测量结果，因为14px和16px的字体的宽度是不一样的
                var width = ctx.measureText('123aaa').width;//开始测量字体的宽度
                console.log('text的宽度为' + width);*/

                //   console.log(e.target._getCursorBoundaries(),e.target._getCursorBoundariesOffsets());
                //   console.log(e.target.getBoundingRect());
                // console.log(e.target.width, e.target._getCursorBoundariesOffsets().left)
                //  console.log(e.target._getCursorBoundaries());

                /* var obj = this.$refs.canvas.getEditObj();
                 var fontwidths = 0;
                 for(var s=0;s < obj.__lineWidths.length ;s++){
                     fontwidths += obj.getLineWidth(s);
                 }

                 console.log(e.target);


                 let scale = e.target.textLength/ e.target.text.length;
                 let scaleX = e.target.fixedWidth/e.target.text.length ;
                 let fontw = fontwidths/e.target.text.length ;
                 let curfont;

                 if(e.target.__charBounds[0].length>2){
                   /!*curfont = e.target.__charBounds[0][e.target.__charBounds[0].length-2].kernedWidth?e.target.__charBounds[0][e.target.__charBounds[0].length-2].kernedWidth:fontw;*!/


                 }else{
                     curfont = fontw;
                 }

                 let newscale = scaleX / fontw;

                 console.log(scaleX , curfont,e.target.text.length);

                 if(e.target.text.length!==0){
                     e.target.scaleX = newscale;
                     e.target.width = e.target.fixedWidth/newscale - 1 ;

                 }else{
                     e.target.scaleX = 1;
                     e.target.width = e.target.fixedWidth;
                 }


                 console.log(scaleX/fontw, e.target.width,scale,e.target.width* scale,e.target.fixedWidth);*/


            },
        }
    };
</script>

<style lang='scss' scoped>
    .container {
        width: 100%;
        position: relative;
        height: 100%;
        display: flex;
        flex-direction: column;

        .header {
            height: 60px;

            border-bottom: 1px solid #efefef;
            display: -ms-flexbox;
            display: -moz-box;
            display: -webkit-box;
            display: -webkit-flex;
            display: flex;

            box-sizing: border-box;
            width: 100%;

            .logo {
                width: 200px;
                box-sizing: border-box;
                border-right: 1px solid #efefef;
                height: 60px;
                display: -ms-flexbox;
                display: -moz-box;
                display: -webkit-box;
                display: -webkit-flex;
                display: flex;
                box-align: center;
                -moz-box-align: center;
                -webkit-box-align: center;
                -webkit-align-items: center;
                align-items: center;
                box-pack: center;
                -webkit--moz-box-pack: center;
                -moz-box-pack: center;
                -webkit-justify-content: center;
                justify-content: center;
            }
        }

        .content-wrapper {
            position: relative;
            display: -webkit-flex;
            display: -ms-flexbox;
            display: -moz-box;
            display: -webkit-box;
            display: flex;
            -moz-box-flex: 1;
            box-flex: 1;
            -webkit-box-flex: 1;
            -webkit-flex: 1;
            flex: 1;

            .tool-wrapper {
                position: absolute;
                top: 0;
                right: 0;
                display: -ms-flexbox;
                display: -moz-box;
                display: -webkit-box;
                display: -webkit-flex;
                display: flex;

                -webkit-box-orient: vertical;
                -webkit-box-direction: normal;
                -moz-box-direction: normal;
                -moz-box-orient: vertical;
                -webkit-flex-direction: column;
                flex-direction: column;

                .pt-iconfont {
                    padding: 20px 30px;
                }
            }

            .list-wraper {
                width: 200px;

                border-right: 1px solid #efefef;
                overflow-y: auto;
                flex-shrink: 0;
                box-sizing: border-box;

                display: -webkit-flex;
                display: -ms-flexbox;
                display: -moz-box;
                display: -webkit-box;
                display: flex;
                -webkit-box-orient: vertical;
                -moz-box-orient: vertical;
                -webkit-flex-direction: column;
                flex-direction: column;

                .image-wrapper {
                    padding: 20px;
                    display: -ms-flexbox;
                    display: -moz-box;
                    display: -webkit-box;
                    display: -webkit-flex;
                    display: flex;

                    flex-shrink: 0;
                    box-pack: center;
                    -webkit--moz-box-pack: center;
                    -moz-box-pack: center;
                    -webkit-justify-content: center;
                    justify-content: center;
                    box-align: center;
                    -moz-box-align: center;
                    -webkit-box-align: center;
                    -webkit-align-items: center;
                    align-items: center;
                    border-bottom: 1px solid #efefef;
                    position: relative;
                    img {
                        width: 120px;
                    }
                    .pt-iconfont {
                        position: absolute;
                        top: 0px;
                        right: 0px;
                        font-size: 18px;
                        color: #777;
                        padding: 10px;
                    }
                }
            }
        }
    }

    /* #lcq{
       position: fixed;
       top:100px;
       left:200px;
       z-index: 9999;
       opacity: 0;
     }*/
</style>
