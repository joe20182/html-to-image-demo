<script setup>
import { ref } from "vue";
import { toPng } from "html-to-image";
import html2canvas from "html2canvas";
import image1Src from "../assets/D21.png";

function downloadImage(imageUrl, filename) {
  const link = document.createElement("a");
  link.download = filename;
  link.href = imageUrl;
  link.click();
  link.remove();
}

defineProps({
  msg: String,
});

const count = ref(0);
const image1 = image1Src;

const handleClick1 = () => {
  // console.log(document.getElementById("shaka"));
  toPng(document.getElementById("shaka"), { cacheBust: false })
    .then((dataUrl) => {
      // console.log(dataUrl);
      const filename = "qq.png";
      downloadImage(dataUrl, filename);
    })
    .catch((err) => {
      console.log(err);
    })
    .finally(() => {
      console.log("done");
    });
};

const handleClick2 = () => {
  html2canvas(document.getElementById("shaka"), { useCORS: true }).then(
    (canvas) => {
      const myImage = canvas.toDataURL();
      const filename = "qq.png";
      downloadImage(myImage, filename);
    }
  );
};
</script>

<template>
  <div class="container">
    <!-- shared image -->
    <div ref="sharedContent" class="relative" id="shaka">
      <div class="share-content" :style="{ backgroundImage: `url(${image1})` }">
        <!-- invisible shared content -->
        <div class="timestamp">timestamp: 5566</div>
        <h2 class="type-title">FUTURES</h2>
        <div class="roe-area positive">
          <div class="percentage">+ 87%</div>
          <div class="values">33456.78</div>
        </div>
        <div class="position-info">
          <div class="market-name">ETH Perpetual</div>
          <div class="side positive">LONG</div>
          <div class="leverage">100X</div>
        </div>
        <div>
          <div class="price-row">
            <div class="label">Entry Price</div>
            <div class="number">1111.11</div>
          </div>
          <div class="price-row">
            <div class="label">Mark Price</div>
            <div class="number">1888.88</div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <button id="save-btn" @click="handleClick1">Save 1</button>
  <button id="save-btn" @click="handleClick2">Save 2</button>
</template>

<style scoped>
.container {
  width: 600px;
  color: #fff;
}
.share-content {
  /* background-image: url(https://d2refp30laz1gf.cloudfront.net/btse/sharePnlBg/D21.png); */
}
.share-dialog-body {
  padding: 0 8px 12px;
}
.select-theme {
  padding: 0 16px;
  margin-bottom: 10px;
  font-size: 14px;
  font-weight: 500;
}
.share-content {
  position: relative;
  padding: 28px;
  background-position: center center;
  background-size: cover;
}
.timestamp {
  position: absolute;
  top: 40px;
  right: 30px;
  font-size: 12px;
  /* color: #666; */
}
.type-title {
  margin-top: 4px;
  font-size: 16px;
  font-weight: 700;
  /* color: #666; */
}
.roe-area {
  display: flex;
  gap: 8px;
  align-items: center;
  margin: 28px 0;
  font-weight: 700;
}
.percentage {
  font-size: 48px;
}
.values {
  font-size: 18px;
}
.position-info {
  display: flex;
  font-size: 16px;
  font-weight: 700;
}
.market-name {
  display: flex;
  gap: 4px;
  align-items: center;
}
.side,
.leverage {
  padding-left: 12px;
  margin-left: 12px;
  border-left: 1px solid #ccc;
}
.price-row {
  display: flex;
  gap: 8px;
  align-items: flex-end;
}
.number {
  font-size: 16px;
  font-weight: 700;
}
.positive {
  color: #00b15d;
}
</style>
