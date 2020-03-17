<template>
  <div class="box">
    <div class="lbox">
      <ul class="content">
        <div :id="index" v-for="(list,index) in CN" :key="list">
          <p class="title">{{list.initial}}</p>
          <p v-for="item in list.city" :key="item" class="city">{{item}}</p>
        </div>
      </ul>
    </div>
    <div class="rbox">
      <p
      :class="cusindex==index?'select':''"
        v-for="(ini,index) in CN"
        :key="index"
        @click="clickRini(index)"
      >{{ini.initial}}</p>
    </div>
  </div>
</template>

<script>
import BScroll from "better-scroll";
export default {
  data() {
    return {
      cusindex: "",

      CN: [
        {
          initial: "热门城市",
          city: ["北京", "上海", "深圳", "广州", "成都", "重庆"]
        },
        {
          initial: "A",
          city: ["阿图什", "安康", "阿克苏", "阿拉善", "阿勒泰", "安庆"]
        },
        {
          initial: "B",
          city: ["北京", "保定", "包头", "宝鸡", "滨州", "巴音郭楞"]
        },
        {
          initial: "C",
          city: ["成都", "重庆", "长沙", "长春", "常德", "潮州"]
        },
        {
          initial: "D",
          city: ["大连", "丹东", "东莞", "敦化", "敦煌", "德阳"]
        },
        {
          initial: "E",
          city: ["鄂尔多斯", "鄂州", "恩施"]
        },
        {
          initial: "F",
          city: ["佛山", "阜阳", "富蕴", "福州"]
        },
        {
          initial: "G",
          city: ["广安", "广州", "贵阳", "桂林", "广元", "赣州"]
        }
      ]
    };
  },
  mounted() {
    //左
    this.leftDiv = new BScroll(document.querySelector(".lbox"), {
      probeType: 3
    });
    this.leftDiv.on("scroll", ({ y }) => {
      let _y = Math.abs(y);
      for (let leftdivObj of this.scroll) {
        if (_y >= leftdivObj.min && _y < leftdivObj.max) {
          this.cusindex = leftdivObj.index;
          return;
        }
      }
    });

    // 右
    var rightDiv = new BScroll(document.querySelector(".rbox"), {
      click: true //允许点击
    });

    console.log(rightDiv);
  },
  methods: {
    clickRini(index) {
      this.cusindex = index; //获取索引保存
      this.leftDiv.scrollToElement(document.getElementById(index), 500);
    }
  },
  computed: {
    scroll() {
      let arr = [];
      let total = 0;
      for (let i = 0; i < this.CN.length; i++) {
        var DivHeght = document.getElementById(i).offsetHeight;
        arr.push({ min: total, max: total + DivHeght, index: i });
        total += DivHeght;
      }
      return arr;
    }
  }
};
</script>

<style scoped>
.select {
  background: skyblue;
}

.box {
  display: flex;
}
.lbox {
  flex: 9;
  max-height: 400px;
  overflow: scroll;
}
.rbox {
  flex: 1;
  flex-direction: column !important;
  text-align: center;
  background: #f2f2f2;
}
.title {
  height: 20px;
  background: #f2f2f2;
}
.city {
  border-bottom: 1px solid #f2f2f2;
  align-items: center;
}
</style>
