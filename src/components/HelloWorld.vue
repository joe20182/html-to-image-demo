<script setup>
import { ref } from "vue";
import { toPng } from "html-to-image";
import html2canvas from "html2canvas";
import domtoimage from "dom-to-image";
import QrcodeVue from "qrcode.vue";
import image1Src from "../assets/D22.png";

function downloadImage(imageUrl, filename) {
  const link = document.createElement("a");
  link.download = filename;
  link.href = imageUrl;
  link.click();
  link.remove();
}

async function shareImage({ imageUrl, text, url }, filename = "shared.png") {
  const blob = await (await fetch(imageUrl)).blob();
  const filesArray = [
    new File([blob], filename, {
      type: blob.type,
      lastModified: new Date().getTime(),
    }),
  ];
  const shareData = {
    files: filesArray,
    text: "I have a dream",
    url: "https://shaka-joe.com",
  };
  if (navigator.share) {
    navigator.share(shareData);
  } else {
    downloadImage(imageUrl, filename);
  }
}

defineProps({
  msg: String,
});

const sharedContent = ref(null);
const tmpImgSlot = ref(null);
const share = ref({
  leverage: true,
  pnl: true,
  price: true,
});
const image1 = image1Src;
const qrLogoImg = `https://d2refp30laz1gf.cloudfront.net/btse/sharePnlBg/qrLogo.png`;

const getDom = () => {
  const oldCanvas = sharedContent.value.querySelector("canvas");
  const sharedDom = sharedContent.value.cloneNode(true);
  // 顯示下載圖片要加上的內容
  sharedDom.style.borderRadius = "16px";
  sharedDom.style.overflow = "hidden";
  sharedDom.querySelector(".timestamp").style.display = "flex";
  sharedDom.querySelector(".share-info").style.display = "flex";
  const newCanvas = sharedDom.querySelector("canvas").getContext("2d");
  newCanvas.drawImage(oldCanvas, 0, 0);
  tmpImgSlot.value.append(sharedDom);

  return tmpImgSlot.value;
};

const handleClick1 = () => {
  // console.log(document.getElementById("shaka"));
  toPng(document.getElementById("shaka"))
    .then((dataUrl) => {
      // console.log(dataUrl);
      const filename = "qq.png";
      shareImage({ imageUrl: dataUrl }, filename);
    })
    .catch((err) => {
      console.log(err);
    })
    .finally(() => {
      console.log("done");
    });
};

const handleClick2 = () => {
  const dom = getDom();
  html2canvas(dom, { useCORS: true })
    .then((canvas) => {
      const myImage = canvas.toDataURL();
      const filename = "qq.png";
      shareImage({ imageUrl: myImage }, filename);
    })
    .finally(() => {
      dom.remove();
    });
};

const handleClick3 = () => {
  domtoimage
    .toPng(document.getElementById("shaka"))
    .then(function (dataUrl) {
      const filename = "qq.png";
      shareImage({ imageUrl: dataUrl }, filename);
    })
    .catch(function (error) {
      console.error("oops, something went wrong!", error);
    });
};
</script>

<template>
  <div class="container">
    <!-- shared image -->
    <div ref="sharedContent" class="relative" id="shaka">
      <div class="share-content">
        <div v-show="false" class="timestamp">timestamp: 5566</div>
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
      <!-- invisible shared content -->
      <div v-show="false" class="share-info">
        <div class="qr-wrapper">
          <QrcodeVue :size="68" value="referralLink" />
          <img class="qr-logo" :src="qrLogoImg" />
        </div>
        <div class="info-body">
          <div class="ref-area">
            <div class="ref-text">REFERAL CODE</div>
            <div class="ref-code">{{ "refCode" }}</div>
            <div class="promo-text">Hahaha 87 UCCU :)</div>
          </div>
          <div class="domain">QOO.COM</div>
        </div>
      </div>
      <!-- for rendering download image in a invisible place -->
      <div ref="tmpImgSlot" class="tmp-img-slot"></div>
    </div>
  </div>

  <button id="save-btn" @click="handleClick1">html-to-image</button>
  <button id="save-btn" @click="handleClick2">html2canvas</button>
  <button id="save-btn" @click="handleClick3">dom-to-image</button>
</template>

<style scoped lang="less">
.container {
  width: 600px;
  color: #fff;
}
.share-content {
  background-image: url(https://d2refp30laz1gf.cloudfront.net/btse/sharePnlBg/D31.png);
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

.share-info {
  display: flex;
  gap: 10px;
  align-items: center;
  padding: 20px 14px 20px 28px;
  background: linear-gradient(
      89.29deg,
      rgba(13, 23, 119, 0) 0.23%,
      rgba(22, 64, 144, 0.43) 107.71%
    ),
    #131b29;

  .theme-night & {
    background: linear-gradient(
        89.29deg,
        rgba(13, 23, 119, 0) 0.23%,
        rgba(22, 64, 144, 0.43) 107.71%
      ),
      #131b29;
  }

  .qr-wrapper {
    position: relative;
    display: flex;
    padding: 2px;
    background-color: #fff;

    > div {
      display: flex;
    }

    .qr-logo {
      position: absolute;
      top: 50%;
      left: 50%;
      width: 24px;
      transform: translate(-50%, -50%);
    }
  }

  .info-body {
    display: flex;
    flex: 1;
    align-items: flex-end;
    justify-content: space-between;

    .ref-area {
      .ref-text,
      .ref-code {
        font-weight: 700;
      }

      .ref-text {
        font-size: 14px;
        text-transform: uppercase;
      }

      .ref-code {
        font-size: 16px;
      }

      .promo-text {
        font-size: 12px;
      }
    }

    .domain {
      font-size: 14px;
      font-weight: 500;
      color: blue;
      text-transform: uppercase;
    }
  }
}
</style>
