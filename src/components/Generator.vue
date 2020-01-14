<template>
  <div class="row justify-content-center">
    <div class="col-sm-4 col-xs-12">
      <div class="mt-5">
        <h1 class="text-center">Bacod image generator</h1>
        <p class="text-center">Make you understand what they said</p>
      </div>
      <div>
        <img id="imgg" style="display: none;" src="/images/1.jpg" alt="the imgg" />

        <canvas id="myCanvas">Your browser does not support the HTML5 canvas tag.</canvas>

        <div class="scaletool">
          <textarea
            id="custom-text"
            class="form-control"
            placeholder="Bacod text here ...."
            v-model="bacod"
          ></textarea>
        </div>
        <div class="scaletool">
          <button class="btn btn-warning" @click="download">Download</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      bacod: 'Your bacod here',
      canvas: null,
      ctx: null,
      x: null,
      y: null,
      img: null,
    }
  },
  mounted() {

    window.onload = () => {

      this.img = document.getElementById('imgg');

      var deviceWidth = window.innerWidth;
      let canvasWidth = Math.min(600, deviceWidth - 20);
      let canvasHeight = Math.min(480, deviceWidth - 20);
      this.canvas = document.getElementById('myCanvas');
      this.canvas.width = canvasWidth;
      this.canvas.height = canvasHeight;


      this.ctx = this.canvas.getContext('2d');


      this.x = this.canvas.width / 2 - this.img.width / 2;
      this.y = this.canvas.height / 2 - this.img.height / 2;

      this.ctx.drawImage(this.img, this.x, this.y);

      this.ctx.textAlign = 'center';
      this.ctx.lineWidth = 10;
      this.ctx.font = '30pt impact';
      this.ctx.strokeStyle = 'black';
      this.ctx.fillStyle = 'white';
      this.ctx.lineJoin = 'round';
      //   ctx.save();
      //   ctx.clearRect(0, 0, canvas.width, canvas.height);
      let text = this.generatedBacod
      text = text.toUpperCase();
      this.wrapText(this.ctx, text, this.canvas.width / 2, this.canvas.height - this.canvas.height / 4.5, canvasWidth - canvasWidth / 3, 30);
    }
  },
  computed: {
    generatedBacod() {
      return this.bacod.replace(/a|i|u|e|o/gi, 'i')
    }
  },
  watch: {
    bacod() {

      this.ctx.save();
      this.ctx.clearRect(0, 0, this.canvas.width, this.canvas.height);



      // Finally, draw the image
      this.ctx.drawImage(this.img, this.x, this.y);

      this.ctx.restore();

      let text = this.generatedBacod
      text = text.toUpperCase();
      this.wrapText(this.ctx, text, this.canvas.width / 2, this.canvas.height - this.canvas.height / 4.5, this.canvasWidth - this.canvasWidth / 3, 30);
      /*text=document.getElementById('custom-text').value;*/
    }
  },
  methods: {
    download() {
      var link = document.createElement('a');
      link.download = 'Meme Bacod.jpg';
      link.href = this.canvas.toDataURL("image/jpeg");
      link.click();
    },
    wrapText(ctx, text, x, y, maxWidth, lineHeight) {
      var words = text.split(' ');
      var line = '';
      /*function warpText(ctx,text,x,y,maxWidth,lineHeight){var words=text.split('');var line='';for(var n=0;n<word.length;n++){var testLine=line+words[n]+'';var metrics=ctx.measureText(testLine);var testWidth=met}}*/
      for (var n = 0; n < words.length; n++) {        var testLine = line + words[n] + ' '; var metrics = ctx.measureText(testLine); var testWidth = metrics.width; if (testWidth > maxWidth && n > 0) {
          ctx.strokeText(line, x, y);
          ctx.fillText(line, x, y);
          line = words[n] + ' ';
          y += lineHeight;
        }
        else {
          line = testLine;
        }
      }
      ctx.strokeText(line, x, y);
      ctx.fillText(line, x, y);
    }
  }
}
</script>

<style scoped>
#myCanvas {
  background-color: pink;
  margin: auto;
  width: 100%;
  min-width: 300px;
}
.scaletool {
  /*background-color:orange;*/
  border: 1px pink;
  border-bottom-style: dotted;
  margin: 0 10%;
  padding: 10px;
}
</style>