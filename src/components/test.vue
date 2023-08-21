<template>
  {{ number }}
  <!-- <span>60s倒计时</span> -->
  <!-- {{ timeValue }} -->
  <p>数据展示区域</p>
  <ul>
    <li v-for="item in list.showList" :key="item.key">
      <div class="showContent">
        <span> key:{{ item.key }}</span
        ><span> value:{{ item.value }}</span>
        <div
          class="colorDiv"
          :style="{
            width: `${(item.value / maxNumber) * 100}%`,
            background: colorArray[item.key],
          }"
        >
          {{ countries[item.key] }}
        </div>
      </div>
    </li>
  </ul>
</template>

<script>
import { computed, onMounted, reactive, ref, watch } from "vue";
export default {
  name: "test",
  setup() {
    const number = ref("");
    const list = reactive({
      showList: [],
      saveInfoList: [],
    });
    const timeValue = ref("");
    const maxNumber = ref(1);
    const colorArray = reactive([
      "#FF0000",
      "#00FF00",
      "#0000FF",
      "#FFFF00",
      "#00FFFF",
      "#FF00FF",
      "#C0C0C0",
      "#808080",
      "#800000",
      "#808000",
      "#008000",
      "#800080",
      "#008080",
      "#000080",
      "#FFA500",
      "#FFFF66",
      "#00FF00",
      "#FFC0CB",
      "#800080",
      "#ADD8E6",
      "#F08080",
      "#E0FFFF",
      "#FAFAD2",
      "#D3D3D3",
      "#90EE90",
      "#FFB6C1",
      "#FFA07A",
      "#20B2AA",
      "#87CEFA",
      "#778899",
      "#B0E0E6",
      "#FF6347",
      "#40E0D0",
      "#EE82EE",
      "#F5DEB3",
      "#A52A2A",
      "#CD853F",
      "#FFD700",
      "#DA70D6",
      "#FFEFD5",
      "#7B68EE",
      "#BDB76B",
      "#F0E68C",
      "#E6E6FA",
      "#808000",
      "#FF4500",
      "#DDA0DD",
      "#FA8072",
      "#FFA500",
      "#00BFFF",
      "#9370DB",
      "#3CB371",
    ]);
    const countries = reactive([
      "阿富汗",
      "阿尔巴尼亚",
      "阿尔及利亚",
      "安道尔",
      "安哥拉",
      "阿根廷",
      "亚美尼亚",
      "澳大利亚",
      "奥地利",
      "阿塞拜疆",
      "巴哈马",
      "巴林",
      "孟加拉国",
      "巴巴多斯",
      "白俄罗斯",
      "比利时",
      "伯利兹",
      "贝宁",
      "不丹",
      "玻利维亚",
      "波黑",
      "博茨瓦纳",
      "巴西",
      "文莱",
      "保加利亚",
      "布基纳法索",
      "布隆迪",
      "佛得角",
      "柬埔寨",
      "喀麦隆",
      "加拿大",
      "中非共和国",
      "乍得",
      "智利",
      "多米尼克",
      "哥伦比亚",
      "科摩罗",
      "刚果",
      "哥斯达黎加",
      "克罗地亚",
      "古巴",
      "塞浦路斯",
      "捷克共和国",
      "丹麦",
      "吉布提",
      "中国",
      "多米尼加共和国",
      "厄瓜多尔",
      "埃及",
      "日本",
      "美国",
      "韩国",
    ]);

    const formatData = (val) => {
      const value = String(val);
      const query = list.saveInfoList.find((item) => item.key === value);
      if (query) {
        if (query.key === "45") {
          query.value += 66666666;
        }
        query.value += Math.floor(Math.random() * 90000000) + 1;
      } else {
        list.saveInfoList.push({
          key: value,
          value: 1,
        });
      }
      list.saveInfoList.sort((a, b) => b.value - a.value);
      list.showList = list.saveInfoList;
    };
    onMounted(() => {
      const interval = setInterval(() => {
        number.value = Math.floor(Math.random() * 50) + 1;
        formatData(number.value);
      }, 100);
      setTimeout(() => {
        clearInterval(interval);
      }, 1000 * 60);
    });

    watch(() => {
      maxNumber.value = list.showList[0]?.value;
    });

    const countdown = () => {
      let saveTime = localStorage.getItem("time");
      if (!saveTime) {
        localStorage.setItem("time", Date.now());
        saveTime = localStorage.getItem("time");
      }
      const time = 60 - Math.floor((Date.now() - saveTime) / 1000);
      return time;
    };

    const inter = setInterval(() => {
      const time = countdown();
      if (time <= 0) {
        localStorage.removeItem("time");
        clearInterval(inter);
      }
      timeValue.value = time;
    }, 1000);

    return {
      list,
      number,
      maxNumber,
      colorArray,
      countries,
      timeValue,
    };
  },
};
</script>

<style>
/* 定义动画 */
/* @keyframes moveUpDown {
  0% {
    transform: translateY(-20px);
  }
  100% {
    transform: translateY(20px);
  }
} */

li {
  margin: 10px 0;
  /* animation-name: moveUpDown; */
  /* animation-duration: 2s;
  animation-timing-function: ease-in-out;
  animation-iteration-count: infinite; */
  animation-delay: 1s; /* 添加动画延迟为1秒 */
}
.showContent {
  display: flex;
}
.colorDiv {
  border-radius: 6px;
  margin: 0 10px;
}
</style>