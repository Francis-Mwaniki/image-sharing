<template>
  <div class="container">
    <div class="data-container" ref="textToConvert" v-if="!generatedImage">
    <div class="future-top">
      <img src="/logo.svg" alt="Binance Logo" class="binance-logo  future-top">
      <!-- future top -->
      <h1 class="future">Future</h1>
      </div>
    <div class="data-section">
      <div class="data-row">
        <span class="label">Type:</span>
        <span class="value">Long</span>
      </div>

      <div class="data-row">
        <span class="label">Symbol:</span>
        <span class="value">BTCUSDT</span>
      </div>
      <div class="data-row">
        <span class="label">Date:</span>
        <span class="value">2021-01-01</span>
      </div>

      <div class="data-row">
        <span class="label">Entry Price:</span>
        <span class="value">$100</span>
      </div>
      <div class="data-row">
        <span class="label">Exit Price:</span>
        <span class="value">$120</span>
      </div>
      <div class="data-row">
        <span class="label">Profit/Loss:</span>
        <span class="value">$20</span>
      </div>
    </div>
    </div>

    <button @click="generateAndShare" class="share-button">Share Image</button>

    <img v-if="generatedImage" :src="generatedImage" alt="Generated Image" class="generated-image">
  </div>
</template>


<script>
import * as htmlToImage from 'html-to-image';
import { toPng, toJpeg, toBlob, toPixelData, toSvg } from 'html-to-image';


export default {
  data() {
    return {
      generatedImage: null,
    };
  },
  methods: {

    dataURLToBlob(dataURL) {
      const BASE64_MARKER = ';base64,';
      if (dataURL.indexOf(BASE64_MARKER) === -1) {
        const parts = dataURL.split(',');
        const contentType = parts[0].split(':')[1];
        const raw = parts[1];

        return new Blob([raw], { type: contentType });
      }

      const parts = dataURL.split(BASE64_MARKER);
      const contentType = parts[0].split(':')[1];
      const raw = window.atob(parts[1]);
      const rawLength = raw.length;
      const uInt8Array = new Uint8Array(rawLength);

      for (let i = 0; i < rawLength; ++i) {
        uInt8Array[i] = raw.charCodeAt(i);
      }

      return new Blob([uInt8Array], { type: contentType });
    },


    async generateAndShare() {
      try {
        const textElement = this.$refs.textToConvert;

        // Generate image from text using html-to-image library
        const imageDataURL = await htmlToImage.toPng(textElement);

        // Set the generated image
        this.generatedImage = imageDataURL;

        // Share functionality (replace this with your desired sharing method)
        // For example, you might use navigator.share or other sharing mechanisms
        this.shareImage(imageDataURL);
      } catch (error) {
        console.error('Error generating or sharing image:', error);
        // Handle errors
      }
    },
    shareImage(imageDataURL) {
     /* share image as file */
     const blob = this.dataURLToBlob(imageDataURL);
      console.log(`blob`, blob);
      const filesArray = [new File([blob], 'image.png', { type: blob.type, lastModified: new Date().getTime() })];
      console.log(`filesArray`, filesArray);
       
      if(navigator.share) {
        /* share image as blob */
        const blob = this.dataURLToBlob(imageDataURL);
        console.log(`blob`, blob);
        const filesArray = [new File([blob], 'image.png', { type: blob.type, lastModified: new Date().getTime() })];
     console.log(`filesArray`, filesArray);
        navigator.share({
          files: filesArray,
        })

          .then(() => console.log('Successful share'))
          .catch((error) => console.log('Error sharing', error));
      } else {
        console.log('Sharing not supported');
      }
    },
  },
};
</script>
<style scoped>
*{
  box-sizing: border-box;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 16px;
  line-height: 1.5;
  /* color: #ffffff; */
  
}
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 10px;
}

.data-container {
  border: 1px solid #ccc;
  border-radius: 5px;
  width: 300px;
  color: #ffffff;
   background-image: linear-gradient(180deg, #f4f4f4 0%, #000000 100%);
  /* padding: 20px; */
  margin-bottom: 10px;

}

.data-section {
  /* margin-bottom: 20px; */
  background-image: url('/binance.jpeg');
  background-size: cover;
  display: block;
  border-radius: 5px;
  background-position: center;
  padding: 20px;
  background-repeat: no-repeat;
}


.binance-logo {
  width: 100px;
  height: 100px;
  margin: 0 auto;
  display: block;

  margin-bottom: 15px;
}

.data-row {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
}

.future-top {
  display: flex;
  justify-content: space-between;
  margin-bottom: 36px;
  /* color: #ffffff; */
  right: 40px;
  position: relative;

}

.future {
  font-size: 30px;
  font-weight: bold;
  margin: 0 auto;
  display: block;
  color: #000000;
  text-shadow: 2px 2px 4px rgba(255, 255, 255, 0);
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

.label {
  font-weight: bold;
}

.generated-image {
  margin-bottom: 10px;
  border: 1px solid #ccc;
  border-radius: 5px;
  z-index: 1;
  background-color: #ffffff;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  position: fixed;

}

.share-button {
  padding: 10px 20px;
  border: 1px solid #ccc;
  border-radius: 5px;
  background-color: #ffffff;
  color: #000000;
  font-weight: bold;
  cursor: pointer;
}

.share-button:hover {
  background-color: #000000;
  color: #ffffff;
}



</style>