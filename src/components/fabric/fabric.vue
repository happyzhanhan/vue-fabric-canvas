<template>
  <div>
    <canvas id="canvas" :width="width" :height="height"></canvas>
  </div>
</template>

<script type='text/ecmascript-6'>
  import Utils from '../../utils';
  const dotCircleImg = require('../../assets/dot-circle.png');
  const rotateMdrImg = require('../../assets/rotate-mdr.png');

  /*var _wrapLine = function(_line, lineIndex, desiredWidth, reservedSpace) {
      var lineWidth = 0,
          splitByGrapheme = this.splitByGrapheme,
          graphemeLines = [],
          line = [],
          // spaces in different languges?
          words = splitByGrapheme ? fabric.util.string.graphemeSplit(_line) : _line.split(this._wordJoiners),
          word = '',
          offset = 0,
          infix = splitByGrapheme ? '' : ' ',
          wordWidth = 0,
          infixWidth = 0,
          largestWordWidth = 0,
          lineJustStarted = true,
          additionalSpace = splitByGrapheme ? 0 : this._getWidthOfCharSpacing();

      reservedSpace = reservedSpace || 0;
      desiredWidth -= reservedSpace;
      for (var i = 0; i < words.length; i++) {
          // i would avoid resplitting the graphemes
          word = fabric.util.string.graphemeSplit(words[i]);
          wordWidth = this._measureWord(word, lineIndex, offset);
          offset += word.length;

          // Break the line if a word is wider than the set width
          if (this.breakWords && wordWidth >= desiredWidth) {

              if (!lineJustStarted) {
                  line.push(infix);
                  lineJustStarted = true;
              }

              // Loop through each character in word
              for (var w = 0; w < word.length; w++) {
                  var letter = word[w];
                  var letterWidth = this.getMeasuringContext().measureText(letter).width * this.fontSize / this.CACHE_FONT_SIZE;
                  if (lineWidth + letterWidth > desiredWidth -letterWidth) {
                     line.push(letter);
                      //lineWidth += letterWidth;
                      graphemeLines.push(line);
                      line = [];
                      lineWidth = 0;
                  } else {
                      line.push(letter);
                      lineWidth += letterWidth;
                  }
              }
              word = [];
          } else {
              lineWidth += infixWidth + wordWidth - additionalSpace;
          }

          if (lineWidth >= desiredWidth && !lineJustStarted) {
              graphemeLines.push(line);
              line = [];
              lineWidth = wordWidth;
              lineJustStarted = true;
          } else {
              lineWidth += additionalSpace;
          }

          if (!lineJustStarted) {
              line.push(infix);
          }
          line = line.concat(word);

          infixWidth = this._measureWord([infix], lineIndex, offset);
          offset++;
          lineJustStarted = false;
          // keep track of largest word
          if (wordWidth > largestWordWidth && !this.breakWords) {
              largestWordWidth = wordWidth;
          }
      }

      i && graphemeLines.push(line);

      if (largestWordWidth + reservedSpace > this.dynamicMinWidth) {
          this.dynamicMinWidth = largestWordWidth - additionalSpace + reservedSpace;
      }

      return graphemeLines;
  };


  fabric.util.object.extend(fabric.Textbox.prototype, {
      _wrapLine: _wrapLine,
  });*/





  export default {
    name: 'VueFabric',
    props: {
      width: {
        type: Number,
        required: true
      },
      height: {
        type: Number,
        required: true
      }
    },
    data () {
      return {
        canvas: null,
        currentObj: null,
        clipboard:null,
      };
    },
    created () {

    },
    mounted () {
      this.canvas = new fabric.Canvas('canvas', { preserveObjectStacking: true });
      let canvas = this.canvas;
      fabric.Canvas.prototype.rotationCursor = "url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABgAAAAYCAQAAABKfvVzAAAAB3RJTUUH3QYBCzMMpADxbgAAAAlwSFlzAAALEgAACxIB0t1+/AAAAARnQU1BAACxjwv8YQUAAAJvSURBVHjaXVRLaxNRFP7mkZjGUqRWJUZbX9AEikK1QtEsdCFIULLygdCN9BcIgkhBXKoIKr5wo7hyo1FcSBaiohFfO7WIQlAXolKL0po2k5nxu+femUw9l/s833mfGQsdsjgC7t1Yjz50YRqf8D3xHu2GbF6BEVzFJGYRcnj4gnvYj0WGayeUy2U5rsMXqI8W5tHmKeB8jM2C6UJZhA18hA6EBLVEJBot2gkxgzFCb+MZxA2beoZQQ47sFF/ep+vuz9Bu93ujWEN+Gy5+4zM24j72aJFuvKSeec5Gz8HdWaBg9fJ925LFhxmHctEXB+8odxzOYwb+Ir9asmFTpwvbpdDaHD7Ert7SESxFQ/xt9Azw5vZbIqKzlsIEmnTZwxwRN7TAPgkuxFEB6IrIutPFJZOpUIK/SX1klCRwH3XuvkJWrOowrMqbqo0LQxe/rZhb2coFy/x1eK4t1ETDNDbE5TslL6djWwsojdfC/oFVJgVFSYBys6hiohKHq6OV2ZI0RYHZwfD+yj7Lk3oPJA5fdqGqPMyweJDsAOP4hSmuMP1jxSehE6YKY9BJIBXyhXyiyzZhS1Jku7gV4onpy4hlm5hS5DRxKE5BxcIjU4kzAnMSAxUHV0wVzmkRpaMsxVdWTiLTKRznIB6IfeXy2ci2Ws6LDSXyFsexC8MYxQFcY+hRH9V3sCkzSkkfl3KG3a4Me+Y7aJqT6iIF/8jPNhE2RUpZXBZIm9lvG6g6qew/tAYWJhbmi92LV3Gzheb0FUe2ZjtwKyHCf0Ilc7cUqgh6Cf2Dd3harE1OCTj4X0CHr/SqRKd586oa5KQCL4wg/wA4WvtpoKznTAAAAABJRU5ErkJggg==) 12 12, auto";
      fabric.Canvas.prototype.customiseControls({
        tl: {
          action: 'scale'
          // cursor: '../../assets/dot-circle.png',
        },
        tr: {
          action: 'scale'
        },
        bl: {
          action: 'scale',
          cursor: 'pointer'
        },

        br: {
          action: 'scale',
          cursor: 'pointer'
        },



        /*mb: {
            action: function(e, target) {
                console.log(e);
                target.set({
                    // left: target.left+20,
                    scaleX:1,
                    scaleY:1,
                    height:target.height+target.fontSize,
                });
                canvas.renderAll();
            },
            cursor: 'pointer',
        },*/
        mb: {
          action: 'scaleY',
          cursor: 'pointer'
        },
        mt: {
          action: 'scaleY',
          cursor: 'pointer'
        },
        mr:{
          action: 'scaleX',
          cursor: 'pointer'
        },
        ml:{
          action: 'scaleX',
          cursor: 'pointer'
        },

        /*mb: {
          action: 'scale',
          cursor: 'pointer'
        },*/
        /*mr: {
            action: function(e, target) {
                console.log(e);
                target.set({
                   // left: target.left+20,
                    scaleX:1,
                    scaleY:1,
                    height:target.height+20,
                });
                canvas.renderAll();
            },
            cursor: 'pointer',
        },*/
        /* mt: {
           // action: {
           //   rotateByDegrees: 30
           // },
           action: 'scale',
           cursor: 'pointer'
         },*/
        // only is hasRotatingPoint is not set to false
        mtr: {
          action: 'rotate'
          // cursor: '../../assets/cow.png',
        }
      });
      //  this.setCornerIcons({});
      // canvas.add(new fabric.Circle({ radius: 30, fill: '#f55', top: 100, left: 100 }));
      canvas.backgroundColor = '#ffffff';
      // canvas.renderAll();
      // this.canvas.push(canvas);
      canvas.selection = true;
      //canvas.setZoom(2);
      let that = this;
      this.canvas.controlsAboveOverlay = false;
      this.canvas.skipOffscreen = true;
      this.canvas.preserveObjectStacking = true;
      // this.drawControls();
      this.canvas.on('selection:created', function (options) {
        that.$emit('selection:created', options);
      });
      this.canvas.on('mouse:down', function (options) {
        that.$emit('mouse:down', options);
      });
      this.canvas.on('mouse:up', function (options) {
        that.$emit('mouse:up', options);
      });
      this.canvas.on('mouse:move', function (options) {
        that.$emit('mouse:move', options);
      });
      this.canvas.on('mouse:dblclick', function (options) {
        that.$emit('mouse:dblclick', options);
      });
      this.canvas.on('mouse:over', function (options) {
        that.$emit('mouse:over', options);
      });
      this.canvas.on('mouse:out', function (options) {
        that.$emit('mouse:out', options);
      });
      this.canvas.on('object:added', function (options) {
        that.$emit('object:added', options);
      });
      this.canvas.on('object:removed', function (options) {
        that.$emit('object:removed', options);
      });
      this.canvas.on('object:moved', function (options) {
        that.$emit('object:moved', options);
      });
      this.canvas.on('object:modified', function (options) {
        that.$emit('object:modified', options);
      });
      this.canvas.on('object:rotating', function (options) {
        that.$emit('object:rotating', options);
      });
      this.canvas.on('object:scaling', function (options) {
        that.$emit('object:scaling', options);
      });
      this.canvas.on('object:scaled', function (options) {
        that.$emit('object:scaled', options);
      });
      this.canvas.on('object:selected', function (options) {
        that.$emit('object:selected', options);
      });
      this.canvas.on('object:moving', function (options) {
        that.$emit('object:moving', options);
      });
      this.canvas.on('selection:updated', function (options) {
        that.$emit('selection:updated', options);
      });
      this.canvas.on('selection:changed', function (options) {
        that.$emit('selection:changed', options);
      });
      this.canvas.on('selection:cleared', function (options) {
        that.$emit('selection:cleared', options);
      });
      this.canvas.on('before:selection:cleared', function (options) {
        that.$emit('before:selection:cleared', options);
      });
      this.canvas.on('text:changed', function(options) {
        that.$emit('text:changed', options);
      });
      this.canvas.on('text:editing:entered',function(options){
        that.$emit('text:editing:entered', options);
      });
      this.canvas.on('text:editing:exited'),function(options){
        this.$emit('text:editing:exited',options);
      };

      this.canvas.on('before:render'),function(options){
        this.$emit('before:render',options);
      };
      this.canvas.on('after:render'),function(options){
        this.$emit('after:render',options);
      };

      this.canvas.on('dragover'),function(options){
        this.$emit('dragover',options);
      };

      this.canvas.on('dragenter'),function(options){
        this.$emit('dragenter',options);
      };

      this.canvas.on('dragleave'),function(options){
        this.$emit('dragleave',options);
      };

      this.canvas.on('drop'),function(options){
        this.$emit('drop',options);
      };


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
            lockUniScaling: this.lockUniScaling
          });
        },
        _render: function(ctx) {
          this.callSuper('_render', ctx);
          ctx.top = 0;
          ctx.left = 0;
          ctx.font = '20px Times';
          ctx.fillStyle = '#333';
          ctx.fillText(this.label,-this.width / 2 + 4, -this.height / 2 + 20 );  //-this.width / 2 + 4, -this.height / 2 + 20
        }
      });


    },
    methods: {
      setStackingtrue(){
        this.canvas.preserveObjectStacking = true;
      },
// tl = dotCircleImg, tr = dotCircleImg, bl = dotCircleImg, br = dotCircleImg, ml = dotCircleImg, mr = dotCircleImg,mt = dotCircleImg,mb = dotCircleImg,
      setCornerIcons ({ size = 20, borderColor = '#e4e4e4', cornerBackgroundColor = '#ffffff', cornerShape = 'rect', mtr = rotateMdrImg,tl = dotCircleImg, tr = dotCircleImg, bl = dotCircleImg, br = dotCircleImg, ml = dotCircleImg, mr = dotCircleImg,mt = dotCircleImg,mb = dotCircleImg, }) {
        // basic settings
        let that = this;
        fabric.Object.prototype.customiseCornerIcons(
                {
                  settings: {
                    borderColor: borderColor,
                    cornerSize: size,
                    cornerShape: cornerShape,  // 'rect', 'circle'
                    cornerBackgroundColor: cornerBackgroundColor
                  },
                  tl: {
                    icon: tl
                  },
                  tr: {
                    icon: tr
                  },
                  bl: {
                    icon: bl
                  },
                  br: {
                    icon: br,

                  },
                  ml: {
                    icon: ml
                  },
                  mr: {
                    icon: mr
                  },
                  mb:{
                    icon:mb
                  },
                  mt:{
                    icon:mt
                  },
                  // only is hasRotatingPoint is not set to false
                  mtr: {
                    icon: mtr
                  }
                },
                function () {
                  that.canvas.renderAll();
                }
        );
      },
      drawDottedline (options) {
        options = Object.assign({ x: 0, y: 0, x1: 10, y1: 10, color: '#B2B2B2', drawWidth: 2, offset: 6, empty: 3 }, options);
        let canvasObject = new fabric.Line([options.x, options.y, options.x1, options.y1], {
          ...options,
          strokeDashArray: [options.offset, options.empty],
          stroke: options.color,
          strokeWidth: options.drawWidth
        });
        this.canvas.add(canvasObject);
        this.canvas.renderAll();
      },
      drawArrowLine (options) {
        options = Object.assign({ x: 0, y: 0, x1: 10, y1: 10, color: '#B2B2B2', drawWidth: 2, fillColor: 'rgba(255,255,255,0)', theta: 35, headlen: 35 }, options);
        let canvasObject = new fabric.Path(this.drawArrowBase(options.x, options.y, options.x1, options.y1, options.theta, options.headlen), {
          ...options,
          stroke: options.color,
          fill: options.fillColor,
          strokeWidth: options.drawWidth
        });
        this.canvas.add(canvasObject);
        this.canvas.renderAll();
      },
      drawArrowBase (fromX, fromY, toX, toY, theta, headlen) {
        theta = typeof theta !== 'undefined' ? theta : 30;
        headlen = typeof theta !== 'undefined' ? headlen : 10;
        // 计算各角度和对应的P2,P3坐标
        var angle = (Math.atan2(fromY - toY, fromX - toX) * 180) / Math.PI,
                angle1 = ((angle + theta) * Math.PI) / 180,
                angle2 = ((angle - theta) * Math.PI) / 180,
                topX = headlen * Math.cos(angle1),
                topY = headlen * Math.sin(angle1),
                botX = headlen * Math.cos(angle2),
                botY = headlen * Math.sin(angle2);
        var arrowX = fromX - topX,
                arrowY = fromY - topY;
        var path = ' M ' + fromX + ' ' + fromY;
        path += ' L ' + toX + ' ' + toY;
        arrowX = toX + topX;
        arrowY = toY + topY;
        path += ' M ' + arrowX + ' ' + arrowY;
        path += ' L ' + toX + ' ' + toY;
        arrowX = toX + botX;
        arrowY = toY + botY;
        path += ' L ' + arrowX + ' ' + arrowY;
        return path;
      },
      freeDrawConfig (isDrawingMode, color = '#B2B2B2', drawWidth = 2) {
        this.canvas.isDrawingMode = isDrawingMode;
        this.canvas.freeDrawingBrush.color = color; // 设置自由绘颜色
        this.canvas.freeDrawingBrush.width = drawWidth;
      },
      removeCurrentObj () { //单个删除 多个删除 不支持组合删除
        let obj = this.canvas.getActiveObject();
        // console.log(obj._objects);
        if(obj._objects){
          this.canvas.discardActiveObject();
          for(var i in obj._objects){
            this.canvas.remove(obj._objects[i]);
          }
        }else{
          this.canvas.remove(obj);
        }


        this.canvas.renderAll();
      },
      getEditObj () {
        let obj = this.canvas.getActiveObject();
        //this.removeCurrentObj();
        return obj;
      },
      setActiveObject(obj){
        this.canvas.setActiveObject(obj);
        this.canvas.renderAll();
      },
      getActiveObject(){
        var obj = this.canvas.getActiveObject();
        return obj;
      },
      removecurrentObj(){
        let obj = this.canvas.getActiveObject();
        this.canvas.remove(obj);
      },
      removeObj(obj){
        this.canvas.remove(obj);
      },
      addObj(obj){
        this.canvas.add(obj);
        this.canvas.renderAll();
      },
      /* setEditObj (obj) {
         this.canvas.add(obj);
         this.canvas.renderAll();
       },*/
      setZoom(n){
        this.canvas.setZoom(n);
      },
      setbackground(color){
        this.canvas.backgroundColor = color;
        this.canvas.renderAll();
      },
      setNomove(){
        this.canvas.selection = false;
        this.canvas.renderAll();
      },

      setRotate (deg = 36) {
        let obj = this.canvas.getActiveObject();
        let angle = obj.angle;
        obj.rotate(angle + deg);
        this.canvas.renderAll();
      },
      discardActive () {
        this.canvas.discardActiveObject();
        this.canvas.discardActiveGroup();
        this.canvas.renderAll();
      },

      moveTo () {
        let obj = this.canvas.getActiveObject();
        console.log(this.canvas.sendBackwards);
        this.canvas.sendBackwards(obj, true);
        /*  this.canvas.discardActiveObject();
          this.canvas.discardActiveGroup();*/
      },
      moveToshow (obj,index) {
        // let obj = this.canvas.getActiveObject();
        this.canvas.moveTo(obj,index);
      },
      createRect (options) {
        options = Object.assign({ width: 0, height: 0, fillColor: 'rgba(255, 255, 255, 0)', left: 50, top: 50 }, options);
        let rect = new fabric.Rect({
          ...options,
          fill: options.fillColor // 填充的颜色
        });
        this.canvas.add(rect);
        this.canvas.renderAll();
      },
      createRectno (options) {
        options = Object.assign({ width: 0, height: 0, fillColor: 'rgba(255, 255, 255, 0)', left: 50, top: 50 }, options);
        let rect = new fabric.Rect({
          ...options,
          fill: options.fillColor // 填充的颜色
        });
        rect.hasControls = false;
        rect.hasBorders = false;
        rect.selectable = false;
        rect.hoverCursor = 'default';
        this.canvas.add(rect);

        rect.sendBackwards(true);
        this.renderTop();

        this.canvas.renderAll();
      },
      createCircle (options) {
        options = Object.assign({ left: 0, top: 0, radius: 30, fillColor: 'rgba(255, 255, 255, 0)', color: '#B2B2B2', drawWidth: 2 }, options);
        let defaultOption = {
          ...options,
          fill: options.fillColor,
          strokeWidth: options.drawWidth,
          stroke: options.color
        };
        let Circle = new fabric.Circle(defaultOption);
        this.canvas.add(Circle);
        this.canvas.renderAll();
      },
      createTriangle (options) {
        options = Object.assign({ x: 0, y: 0, x1: 0, y1: 0, x2: 0, y2: 0, left: 100, top: 100, color: '#B2B2B2', drawWidth: 2, fillColor: 'rgba(255, 255, 255, 0)', id: 'triangle' }, options);
        var path = 'M ' + options.x + ' ' + options.y + ' L ' + options.x1 + ' ' + options.y1 + ' L ' + options.x2 + ' ' + options.y2 + ' z';
        let canvasObject = new fabric.Path(path, {
          ...options,
          stroke: options.color,
          strokeWidth: options.drawWidth,
          fill: options.fillColor
        });
        this.canvas.add(canvasObject);
        this.canvas.renderAll();
      },
      createEqualTriangle (options) {
        options = Object.assign({ left: 100, top: 100, width: 50, height: 80, fillColor: 'rgba(255, 255, 255, 0)', color: '#B2B2B2', drawWidth: 2 }, options);
        // console.log(defaultOption);
        let triangle = new fabric.Triangle({
          ...options,
          fill: options.fillColor,
          strokeWidth: options.drawWidth,
          stroke: options.color
        });
        this.setContronVisibility(triangle);
        this.canvas.add(triangle);
        this.canvas.renderAll();
      },
      createLine (options) {
        options = Object.assign({ x: 0, y: 0, x1: 10, y1: 10, fillColor: 'rgba(255, 255, 255, 0)', strokeColor: '#B0B0B0' }, options);
        let line = new fabric.Line([options.x, options.y, options.x1, options.y1], {
          ...options,
          fill: options.fillColor,
          stroke: options.strokeColor
        });
        this.canvas.add(line);
        this.canvas.renderAll();
      },
      createEllipse (options) {
        options = Object.assign({ rx: 100, ry: 200, fillColor: 'rgba(255, 255, 255, 0)', angle: 90, strokeColor: '#B0B0B0', strokeWidth: 3, left: 50, top: 50 }, options);
        var ellipse = new fabric.Ellipse({
          ...options,
          fill: options.fillColor,
          stroke: options.strokeColor
        });
        this.canvas.add(ellipse);
        this.canvas.renderAll();
      },
      createText (text, options) {
        options = Object.assign({ left: 100, top: 100 }, options);
        var canvasObj = new fabric.Text(text, { ...options });
        this.canvas.add(canvasObj);
        this.canvas.renderAll();
      },
      createIText (text, options) {
        options = Object.assign({ left: 100, top: 100 }, options);
        var canvasObj = new fabric.IText(text, { ...options });
        this.setContronVisibilityitext(canvasObj);
        this.canvas.add(canvasObj);
        this.canvas.renderAll();
      },
      createIText2 (text, options) {
        options = Object.assign({ left: 100, top: 100 }, options);
        var canvasObj = new fabric.IText(text, { ...options });
        this.setContronVisibilityitext2(canvasObj);
        this.canvas.add(canvasObj);
        this.canvas.renderAll();
      },
      //新增可以自动换行的功能
      creatTexboxWrap(text,option){
        option = Object.assign({ fontSize: 14, fillColor: '#000000', registeObjectEvent: false, width: 100, left: 100, top: 100,lineMax:5, }, option);
        var textbox = new fabric.Textbox(text, {
          ...option,
          width: option.width,
          breakWords: true,
        });
        this.canvas.add(textbox); // 把图片添加到画布上
        if (option && option.registeObjectEvent) {
          Utils.registeObjectEvent(that, textbox);
        }
        this.canvas.renderAll.bind( this.canvas);
      },
      requestRenderAll(){
        this.canvas.requestRenderAll();
      },

      //文本
      createTextbox (text, options) {
        options = Object.assign({ fontSize: 14, fillColor: '#000000', registeObjectEvent: false, width: 100, left: 100, top: 100,lineMax:5, }, options);
        var canvasObj = new fabric.Textbox(text, {
          ...options,
          fill: options.fillColor,
          fixedWidth:options.width,
          lines:options.lineMax,/*
          styles: {
              0: {
                  0: { textDecoration: 'underline', fontSize: 80 },
                  1: { textBackgroundColor: 'red' },
                  2: { textBackgroundColor: 'green' }
              },
              1: {
                  1: {  textBackgroundColor: 'green' },
                  4: { fontSize: 12 }
              }
          },*/
        });





        //console.log(text);
        this.setContronVisibilitytext(canvasObj);

        //行数和宽度限制
        /*    this.canvas.on('text:changed', function(opt) {
                var obj = opt.target;
                var linewidth = obj.width;                  //总共多宽
                var fontnum = obj.text.length;              //文字个数
                var minwidth = obj.fixedWidth;              //最小宽度限制*************
                var rownum = minwidth/(linewidth/fontnum);  //一行显示多少个字（）
                var rows = Math.ceil(linewidth/minwidth);    //显示几行
                var maxrows = obj.lineMax;                   //最大显示行数************
                var start = 0;
                var _this = this;
                var textLines = [];
                var content = obj.text;
                var textshow = '';

                if (maxrows === 1 || (maxrows > 1 && linewidth <= minwidth)){ //一行可以显示完，限制宽度内显示完
                    console.log(rownum,rows,maxrows);
                }else{
                    if (rows < maxrows){  //没超过限制行数
                        for (var i = 0; i < rows; i++) {
                            textLines[i] = content.substring(start, start + rownum)+'\n';
                            start += rownum;
                        }
                    }else{   //超过限制行数
                        for (var i = 0; i < maxrows; i++) {
                            if (i === maxrows - 1) { //超出后的最后一行
                                if (content.substring(start, length).length <= rownum) {
                                    textLines[i] = content.substring(start, start + rownum)+'\n';
                                }else{
                                    textLines[i] = content.substring(start, start + rownum - 2);
                                    textLines[i] += "...";
                                }
                            }else{
                                textLines[i] = content.substring(start, start + rownum)+'\n';
                                start += rownum;

                            }
                        }
                    }
                }
                console.log(textLines);
                for(var j in textLines){
                    textshow += textLines[j];
                }
                console.log(textshow);
           });
    */

        //字体缩放
        /* this.canvas.on('text:changed', function(opt) {

             var t1 = opt.target;
             if (t1.width > t1.fixedWidth) {
                 t1.fontSize *= t1.fixedWidth / (t1.width + 1);
                 t1.width = t1.fixedWidth;
             }
         });*/

        this.canvas.add(canvasObj);


        if (options.registeObjectEvent) {
          Utils.registeObjectEvent(this, canvasObj);
        }
        this.canvas.renderAll();
      },


      //文本
      createTextboxnew (text, options) {
        options = Object.assign({ fontSize: 14, fillColor: '#000000', registeObjectEvent: false, width: 100, left: 100, top: 100,lineMax:5, }, options);
        var canvasObj = new fabric.TextboxNew(text, {
          ...options,
          fill: options.fillColor,
          fixedWidth:options.width,
          lines:options.lineMax,/*
          styles: {
              0: {
                  0: { textDecoration: 'underline', fontSize: 80 },
                  1: { textBackgroundColor: 'red' },
                  2: { textBackgroundColor: 'green' }
              },
              1: {
                  1: {  textBackgroundColor: 'green' },
                  4: { fontSize: 12 }
              }
          },*/
        });





        //console.log(text);
        this.setContronVisibilitytext(canvasObj);

        //行数和宽度限制
        /*    this.canvas.on('text:changed', function(opt) {
                var obj = opt.target;
                var linewidth = obj.width;                  //总共多宽
                var fontnum = obj.text.length;              //文字个数
                var minwidth = obj.fixedWidth;              //最小宽度限制*************
                var rownum = minwidth/(linewidth/fontnum);  //一行显示多少个字（）
                var rows = Math.ceil(linewidth/minwidth);    //显示几行
                var maxrows = obj.lineMax;                   //最大显示行数************
                var start = 0;
                var _this = this;
                var textLines = [];
                var content = obj.text;
                var textshow = '';

                if (maxrows === 1 || (maxrows > 1 && linewidth <= minwidth)){ //一行可以显示完，限制宽度内显示完
                    console.log(rownum,rows,maxrows);
                }else{
                    if (rows < maxrows){  //没超过限制行数
                        for (var i = 0; i < rows; i++) {
                            textLines[i] = content.substring(start, start + rownum)+'\n';
                            start += rownum;
                        }
                    }else{   //超过限制行数
                        for (var i = 0; i < maxrows; i++) {
                            if (i === maxrows - 1) { //超出后的最后一行
                                if (content.substring(start, length).length <= rownum) {
                                    textLines[i] = content.substring(start, start + rownum)+'\n';
                                }else{
                                    textLines[i] = content.substring(start, start + rownum - 2);
                                    textLines[i] += "...";
                                }
                            }else{
                                textLines[i] = content.substring(start, start + rownum)+'\n';
                                start += rownum;

                            }
                        }
                    }
                }
                console.log(textLines);
                for(var j in textLines){
                    textshow += textLines[j];
                }
                console.log(textshow);
           });
    */

        //字体缩放
        /* this.canvas.on('text:changed', function(opt) {

             var t1 = opt.target;
             if (t1.width > t1.fixedWidth) {
                 t1.fontSize *= t1.fixedWidth / (t1.width + 1);
                 t1.width = t1.fixedWidth;
             }
         });*/

        this.canvas.add(canvasObj);


        if (options.registeObjectEvent) {
          Utils.registeObjectEvent(this, canvasObj);
        }
        this.canvas.renderAll();
      },

      createImagenew(url,options){
        let canvas = this.canvas;

        fabric.Image.fromURL(url, function (oImg) {
          oImg.set({
            width:100,
            height:100,
            top:100,
            left:100,
            originX: 'left',
            originY: 'top',
            angle:options.angle,
            id: options.id ? options.id : 'image',
          },options);
          canvas.add(oImg);
          oImg.hasControls = oImg.hasBorders = true;
        });
        this.canvas.renderAll();
      },
      createShelf(url,options){
        let canvas = this.canvas;

        fabric.Image.fromURL(url, function (oImg) {
          oImg.set({
            width:options.width ? options.width : 50,
            height:options.height ? options.height : 50,
            top:options.top ? options.top : 0,
            left:options.left ? options.left : 0,
            originX: 'left',
            originY: 'top',
            angle:options.angle ? options.angle : 0,
            id: options.id ? options.id : 'image',
            lockScalingFlip: true,
          },options);
          canvas.add(oImg);
          oImg.hasControls = oImg.hasBorders = true;
        });
        this.canvas.renderAll();
      },
      createShelfNumber(url,options){
        let canvas = this.canvas;

        options = Object.assign({ left: 100, top: 100 }, options);
        var canvasObj = new fabric.Text(options.text, { ...options });
        this.canvas.add(canvasObj);

        fabric.Image.fromURL(url, function (oImg) {
          oImg.set({
            width:options.width ? options.width : 50,
            height:options.height ? options.height : 50,
            top:options.top ? options.top : 0,
            left:options.left ? options.left : 0,
            originX: 'left',
            originY: 'top',
            angle:options.angle ? options.angle : 0,
            id: options.id ? options.id : 'image',
            lockScalingFlip: true,
          },options);
          //canvas.add(oImg);

         // oImg.hasControls = oImg.hasBorders = true;


          var group = new fabric.Group([ canvasObj, oImg], {
            left: 100,
            top: 300,
          });

          canvas.add(group);

        });



        this.canvas.renderAll();
      },




      createImagenomove (url, options) {
        let canvas = this.canvas;
        canvas.selection = false;
        let that = this;
        fabric.Image.fromURL(url, function (img) {
          // 添加过滤器
          // img.filters.push(new fabric.Image.filters.Grayscale());
          // 应用过滤器并重新渲染画布执行
          // img.applyFilters(canvas.renderAll.bind(canvas));
          // console.log(img);
          let maxWidth = that.width / 2;
          let width = 0;
          let height = 0;
          if (img.width > img.height) {
            if (img.width > maxWidth) {
              width = maxWidth;
              height = (img.height / img.width) * width;
            } else {
              width = img.width;
              height = img.height;
            }
          } else {
            if (img.height > maxWidth) {
              height = maxWidth;
              width = (img.width / img.height) * height;
            } else {
              width = img.width;
              height = img.height;
            }
          }
          if (options && options.width) {
            width = options.width;
          }
          if (options && options.height) {
            height = options.height;
          }
          let leftP = that.width / 2;
          let topP = that.height / 2;
          if (options && options.left || (options && options.left===0)) {
            leftP = options.left;
          }
          if (options && options.top || (options && options.top===0)) {
            topP = options.top;
          }
          img.set({
            id: options.id ? options.id : 'image',
            left: leftP,
            top: topP,
            scaleX: width / img.width,
            scaleY: height / img.height,
            originX: 'left',
            originY: 'top',
            cornerStrokeColor: 'blue',
            angle:options.angle,
          });

          var oldOriginX = img.get('originX');
          var oldOriginY = img.get('originY');
          var center = img.getCenterPoint();
          img.hasControls = false;
          img.hasBorders = false;
          img.selectable = false;



          // img.customiseCornerIcons(
          //   {
          //     settings: {
          //       borderColor: '#b4b4b4',
          //       cornerSize: 20,
          //       cornerBackgroundColor: '#FF0000',
          //       cornerShape: 'circle',
          //       cornerPadding: 0
          //     }
          //     tl: {
          //       icon: dotCircleImg
          //     },
          //     tr: {
          //       icon: dotCircleImg
          //     },
          //     bl: {
          //       icon: dotCircleImg
          //     },
          //     br: {
          //       icon: dotCircleImg
          //     },
          //     mb: {
          //       icon: dotCircleImg
          //     },
          //     mt: {
          //       icon: dotCircleImg
          //     },
          //     mr: {
          //       icon: dotCircleImg
          //     },
          //     mtr: {
          //       icon: dotCircleImg
          //     }
          //   },
          //   function () {
          //     canvas.renderAll();
          //   }
          // );
          img.setControlsVisibility({
            bl: true,
            br: true,
            mb: false,
            ml: false,
            mr: false,
            mt: false,
            mtr: true,
            tl: true,
            tr: true
          });
          canvas.add(img); // 把图片添加到画布上
          if (options && options.registeObjectEvent) {
            Utils.registeObjectEvent(that, img);
          }
          canvas.renderAll.bind(canvas);
        });
      },
      createImage (url, options) {
        let canvas = this.canvas;
        let that = this;
        fabric.Image.fromURL(url, function (img) {
          // 添加过滤器
          // img.filters.push(new fabric.Image.filters.Grayscale());
          // 应用过滤器并重新渲染画布执行
          // img.applyFilters(canvas.renderAll.bind(canvas));
          // console.log(img);
          let maxWidth = that.width / 2;
          let width = 0;
          let height = 0;
          if (img.width > img.height) {
            if (img.width > maxWidth) {
              width = maxWidth;
              height = (img.height / img.width) * width;
            } else {
              width = img.width;
              height = img.height;
            }
          } else {
            if (img.height > maxWidth) {
              height = maxWidth;
              width = (img.width / img.height) * height;
            } else {
              width = img.width;
              height = img.height;
            }
          }
          if (options && options.width) {
            width = options.width;
          }
          if (options && options.height) {
            height = options.height;
          }
          let leftP = that.width / 2;
          let topP = that.height / 2;
          if (options && options.left || (options && options.left===0)) {
            leftP = options.left;
          }
          if (options && options.top || (options && options.top===0)) {
            topP = options.top;
          }
          img.set({
            id: options.id ? options.id : 'image',
            left: leftP,
            top: topP,
            scaleX: width / img.width,
            scaleY: height / img.height,
            originX: 'left',
            originY: 'top',
            cornerStrokeColor: 'blue',
            angle:options.angle,
          });

          var oldOriginX = img.get('originX');
          var oldOriginY = img.get('originY');
          var center = img.getCenterPoint();
          img.hasControls = true;
          img.hasBorders = true;



          // img.customiseCornerIcons(
          //   {
          //     settings: {
          //       borderColor: '#b4b4b4',
          //       cornerSize: 20,
          //       cornerBackgroundColor: '#FF0000',
          //       cornerShape: 'circle',
          //       cornerPadding: 0
          //     }
          //     tl: {
          //       icon: dotCircleImg
          //     },
          //     tr: {
          //       icon: dotCircleImg
          //     },
          //     bl: {
          //       icon: dotCircleImg
          //     },
          //     br: {
          //       icon: dotCircleImg
          //     },
          //     mb: {
          //       icon: dotCircleImg
          //     },
          //     mt: {
          //       icon: dotCircleImg
          //     },
          //     mr: {
          //       icon: dotCircleImg
          //     },
          //     mtr: {
          //       icon: dotCircleImg
          //     }
          //   },
          //   function () {
          //     canvas.renderAll();
          //   }
          // );
          img.setControlsVisibility({
            bl: true,
            br: true,
            mb: false,
            ml: false,
            mr: false,
            mt: false,
            mtr: true,
            tl: true,
            tr: true
          });
          canvas.add(img); // 把图片添加到画布上
          if (options && options.registeObjectEvent) {
            Utils.registeObjectEvent(that, img);
          }
          canvas.renderAll.bind(canvas);
        });
      },
      toJson () {
        let json = this.canvas.toJSON();
        return json;
      },
      toDataUrl () {
        let canvas = this.canvas;
        let dataURL = canvas.toDataURL({
          format: 'jpeg',
          quality: 1
        });
        return dataURL;
      },
      loadFromJSON (json, cb) {
        console.warn(json);
        let canvas = this.canvas;
        canvas.loadFromJSON(json, canvas.renderAll.bind(canvas), function (
                o,
                object
        ) {
          // `o` = json object
          // `object` = fabric.Object instance
          // ... do some stuff ...
          cb(o);
          object.setControlsVisibility({
            bl: true,
            br: true,
            mb: true,
            ml: true,
            mr: true,
            mt: true,
            mtr: true,
            tl: true,
            tr: true
          });
        });
      },
      clear () {
        this.canvas.clear();
      },
      getObjects () {
        return this.canvas.getObjects();
      },

      renderAll () {
        this.canvas.renderAll(this.canvas);
      },
      renderTop () {
        this.canvas.renderTop();
      },
      setBackgroundColor (color) {
        let canvas = this.canvas;
        this.canvas.setBackgroundColor(color, canvas.renderAll.bind(canvas));
        /*setBackgroundImage*/
      },
      setBackgroundImage (
              imgUrl,
              opacity = 1,
              angle = 0,
              left = 0,
              top = 0,
              crossOrigin = null
      ) {
        let canvas = this.canvas;
        canvas.setBackgroundImage(imgUrl, canvas.renderAll.bind(canvas), {
          opacity: opacity,
          angle: angle,
          left: left,
          top: top,
          originX: 'left',
          originY: 'top',
          crossOrigin: crossOrigin
        });
      },
      toSvg () {
        return this.canvas.toSVG();
      },
      drawControls () {
        let canvas = document.createElement('canvas');
        var ctx = canvas.getContext('2d');
        ctx.setLineDash([]);
        ctx.beginPath();
        ctx.ellipse(100, 100, 50, 75, (45 * Math.PI) / 180, 0, 2 * Math.PI); // 倾斜45°角
        ctx.stroke();
        ctx.setLineDash([5]);
        ctx.moveTo(0, 200);
        ctx.lineTo(200, 0);
        ctx.stroke();
        this.canvas.drawControls(ctx);
        // this.canvas.controlsAboveOverlay=true;
      },
      setContronVisibility (obj) {
        obj.setControlsVisibility({
          bl: true,
          br: true,
          mb: false,
          ml: true,
          mr: true,
          mt: false,
          mtr: true,
          tl: true,
          tr: true
        });
      },
      setContronVisibilityitext (obj) {
        obj.setControlsVisibility({
          bl: true,
          br: true,
          mb: true,
          ml: false,
          mr: true,
          mt: true,
          mtr: true,
          tl: true,
          tr: true
        });
      },
      setContronVisibilityitext2 (obj) {
        obj.setControlsVisibility({
          bl: true,
          br: true,
          mb: true,
          ml: true,
          mr: true,
          mt: true,
          mtr: true,
          tl: true,
          tr: true
        });
      },
      setContronbarcode(obj){
        obj.setControlsVisibility({
          mtr: true,   //旋转
          mt: false,   //上中
          tr: true,    //上右
          mr: true,    //右中
          br: true,    //下右
          mb: true,    //下中
          bl: true,    //下左
          ml: false,   //左中
          tl: true,    //左上
        });
      },
      setContronRandom(obj,option){
        obj.setControlsVisibility(option);
      },

      setContronVisibilitytext (obj) {
        obj.setControlsVisibility({
          bl: true,
          br: true,
          mb: true,
          ml: true,
          mr: true,
          mt: false,
          mtr: true,
          tl: true,
          tr: true
        });
      },
      // 设置mirror
      toggleMirror ({ flip = 'X' }) {
        let img = this.canvas.getActiveObject();
        // console.log(img);
        if (img && img.type == 'image') {
          if (flip === 'X') {
            img.toggle('flipX');
          } else {
            img.toggle('flipY');
          }
          this.renderAll();
        }
      },
      // 设置层级
      toNextLayer () {
        let obj = this.canvas.getActiveObject();
        if (!obj) {
          return;
        }
        obj.sendBackwards(true);
        this.renderTop();
        // this.canvas.setActiveObject(obj);
      },
      toBottomLayer () {
        let obj = this.canvas.getActiveObject();
        if (!obj) {
          return;
        }
        obj.sendToBack();
        this.renderTop();
        // this.canvas.setActiveObject(obj);
      },
      toLastLayer () {
        let obj = this.canvas.getActiveObject();
        if (!obj) {
          return;
        }
        obj.bringForward(true);
        this.renderTop();
      },
      toTopLayer () {
        let obj = this.canvas.getActiveObject();
        if (!obj) {
          return;
        }
        obj.bringToFront();
        this.renderTop();
      },

      toCopy(){
        var _this = this;

        this.canvas.getActiveObject().clone(function(cloned) {
          _this.clipboard = cloned;
        });
      },
      clearClipboard (){
        this.clipboard = null;
      },
      toPaste(id) {
        var _this = this;
        let _clipboard = this.clipboard;
        // console.warn(window.clipboardData);
        //  console.warn(this.canvas);

        // clone again, so you can do multiple copies.
        if(_clipboard){
          _clipboard.clone(function(clonedObj) {
            //  console.warn(clonedObj);

            let canvas = _this.canvas;
            //  canvas.discardActiveObject();
            clonedObj.set({
              id :id+1,
              left: clonedObj.left + 5,
              top: clonedObj.top + 5,
              evented: true,
            });

            if (clonedObj.type === 'activeSelection') {
              // active selection needs a reference to the canvas.
              clonedObj.canvas = canvas;
              clonedObj.forEachObject(function(obj) {
                id = id+1;
                obj.set({
                  id:id,
                  evented: true,
                });
                canvas.add(obj);
              });
              // this should solve the unselectability
              clonedObj.setCoords();
            } else {
              canvas.add(clonedObj);
            }
            /* if(id){
                 console.error(id);
                 console.warn(clonedObj);
             }*/
            // console.warn(clonedObj);
            canvas.setActiveObject(clonedObj);
            canvas.requestRenderAll();

            _this.clipboard = null;
          });
        }

      },
      newgroup(){
        var circle1 = new fabric.Circle({
          radius: 50,
          fill: 'red',
          left: 300,
        });
        var circle2 = new fabric.Circle({
          radius: 50,
          fill: 'green',
          left: 400
        });
        var circle3 = new fabric.Circle({
          radius: 50,
          fill: 'blue',
          left: 500
        });

        var group = new fabric.Group([ circle1, circle2, circle3 ], {
          left: 300,
          top: 200,
        });

        this.canvas.add(group);

        this.canvas.renderAll();
      },
      groupthem(){
        if (!this.canvas.getActiveObject()) {
          return;
        }
        if (this.canvas.getActiveObject().type !== 'activeSelection') {
          return;
        }
        console.log(this.canvas.getActiveObject());
        this.canvas.getActiveObject().toGroup();
        this.canvas.requestRenderAll();

      },
      ungroup(){
        var canvas = this.canvas;
        if (!canvas.getActiveObject()) {
          return;
        }
        if (canvas.getActiveObject().type !== 'group') {
          return;
        }

        canvas.getActiveObject().toActiveSelection();
        canvas.requestRenderAll();

      },
      multiobjs(){
        var canvas = this.canvas;

        canvas.getActiveObject().toActiveSelection();
        canvas.requestRenderAll();
      },

      multidata(objs){
        var canvas = this.canvas;
        var sel = new fabric.ActiveSelection(objs, {
          canvas: canvas,
        });
        //  objs.toActiveSelection();
        canvas.setActiveObject(sel);
        canvas.requestRenderAll();
      },

      multiselect(){
        var canvas = this.canvas;
        var sel = new fabric.ActiveSelection(canvas.getObjects(), {
          canvas: canvas,
        });
        canvas.setActiveObject(sel);
        canvas.requestRenderAll();
      },
      stopselect(){
        this.canvas.selection = false;
      },
      discardobj(){
        this.canvas.discardActiveObject();
        this.canvas.requestRenderAll();
      },


    }
  };
</script>

<style lang='scss' scoped>
</style>
