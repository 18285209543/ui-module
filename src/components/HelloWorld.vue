<template>
  <div class="hello">
    <div id="select" @click.stop="show()" ref="focus">
      <div class="def-content" v-show="!inputHide">请输入省/市/区</div>
      <div class="sel-input" v-show="inputHide">
        <input type="text" ref="tabs1" readonly="true" v-model="tabs[0].name">
        <span>/</span>
        <input type="text" ref="tabs2" readonly="true" v-model="tabs[1].name">
        <span>/</span>
        <input type="text" ref="tabs3" readonly="true" v-model="tabs[2].name">
      </div>
      <img ref="showImg" :src="OptionImg">
    </div>
    <div class="tab-box" ref="tabBox" v-show="contentHide">
      <ul class="nav-tabs">
        <li
          v-for="(item, index) in tabs"
          @click="change(index)"
          v-show="item.showData"
          :class="current === index ? 'active' : ''"
          :key="index"
        >
          <span>{{item.name}}</span>
        </li>
      </ul>
      <ul class="tab-content">
        <li v-for="(parent, index) in tabCentent" v-show="current === index" :key="index">
          <div v-for="(child, cIndex) in parent.childList" :key="child.index">
            <span
              @click="tabSeleted(cIndex), tabItem(child)"
              :class="curChild === cIndex ? 'bg-active' : ''"
            >{{child}}</span>
          </div>
        </li>
      </ul>
    </div>
  </div>
</template>

<script>
import OptionImg from "./static/option.png";

export default {
  name: "HelloWorld",
  props: {
    msg: String
  },
  data() {
    return {
      // tab内容默认隐藏
      contentHide: false,
      inputHide: false,
      // 表单
      tabs: [
        { name: " 省份", showData: true },
        { name: "城市", showData: false },
        { name: "县区", showData: false }
      ],
      tabCentent: [
        {
          childList: [
            "贵州省我是都",
            "广东省",
            "浙江省",
            "贵州省",
            "贵州省",
            "贵州省"
          ]
        },
        {
          childList: ["遵义市", "仁怀市", "平安县", "遵义市", "遵义市"]
        },
        {
          childList: ["习水县", "习水县", "红花岗区", "红花岗区", "红花岗区"]
        }
      ],
      // tab导航当前下标
      current: 0,
      // tab内容下标
      curChild: Number,
      // 小图片
      OptionImg
    };
  },
  methods: {
    // 控制tab内容的显示于隐藏
    show() {
      this.contentHide = !this.contentHide;
      // 改变信息框的样式
      if (this.contentHide) {
        this.$refs.focus.style.border = "1px solid #3388ff";
        this.$refs.showImg.style.transform = "rotate(180deg)";
      } else {
        this.$refs.focus.style.border = "1px solid #bbbbbb";
        this.$refs.showImg.style.transform = "rotate(360deg)";
      }
    },
    // tab导航选中
    change(index) {
      this.current = index;
    },

    // 选中的内容添加背景颜色
    tabSeleted(index) {
      this.curChild = index;
    },

    // 控制内容
    tabItem(item) {
      // 改变tab-input的宽度
      this.$refs.tabs1.style.width = this.$refs.tabs1.value.length * 20 + "px";

      // 把选中的内容赋值给tab导航title
      this.tabs[this.current].name = item;

      if (this.current === 0 || this.current === 1) {
        this.curChild = Number;
        this.tabs[this.current + 1].showData = true;
      }

      // 根据下标来操作数据
      if (this.current === 0) {
        for (let i = 0; i < this.tabs.length; i++) {
          if (i === 1) {
            this.tabs[i].name = "城市";
          } else if (i === 2) {
            this.tabs[i].name = "县区";
          }
        }

        // 打开下一个列表
        this.current = this.current + 1;
        this.inputHide = true;
      } else if (this.current === 1) {
        this.tabs[this.current + 1].name = "县区";
        this.current = this.current + 1;
      } else if (this.current === 2) {
        // 选中后让tab内容隐藏
        this.contentHide = false;
        this.$refs.focus.style.border = "1px solid #bbbbbb";
        this.$refs.showImg.style.transform = "rotate(360deg)";
      }
    }
  },
  created() {
    // 监听document事件 点击时隐藏tab-content
    document.addEventListener("click", e => {
      if (!this.$refs.tabBox.contains(e.target)) {
        this.contentHide = false;
      }
    });
  }
};
</script>

<style lang="stylus" scoped>
html, body, div, ul, li, h1, h2, h3, h4, h5, h6, p, form, input, textarea, th, td, select {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.hello {
  width: 500px;
  margin: 0 auto;
}

#select {
  position: relative;
  width: 500px;
  height: 50px;
  border: 1px solid #bbbbbb;
  font-size: 18px;
  display: flex;
  align-items: center;
  cursor: pointer;
  user-select: none;

  .def-content {
    width: 95%;
    color: #999999;
    line-height: 50px;
    text-align: left;
    float: left;
  }

  img {
    width: 15px;
    height: 10px;
    transition: all 0.5s;
  }

  .sel-input {
    width: 95%;
    height: 100%;
    text-align: left;

    input {
      width: 16%;
      height: 100%;
      font-size: 18px;
      border: none;
      text-align: center;
      outline: none;
      cursor: pointer;
      user-select: none;
    }
  }
}

ul {
  list-style: none;
}

.tab-box {
  width: 500px;
  height: 300px;
  margin-top: 5px;
  border: 1px solid #e5e5e5;
  border-radius: 2px;
  box-shadow: 1px 0px 5px #e5e5e5;

  // 导航样式
  .nav-tabs {
    width: 100%;
    height: 40px;
    border-bottom: 1px solid #e5e5e5;

    li {
      float: left;
      min-width: 70px;
      max-width: 150px;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      width: auto;
      padding: 0 15px;
      height: 39px;
      background: #f8f8f8;
      text-align: center;
      line-height: 39px;
      border-right: 1px solid #e5e5e5;
      cursor: pointer;

      span {
        color: #666666;
      }
    }

    .active {
      background: #ffffff;
      border-bottom: none;
      color: #333333;
      height: 40px;
    }
  }

  .tab-content {
    padding: 10px;
    width: 100%;
    height: 260px;

    li {
      height: 25px;
      width: 100%;
      text-align: left;

      div {
        display: inline-block;
        width: 25%;
        height: 25px;
        color: #000000;
        line-height: 25px;
        margin-bottom: 10px;
        white-space: nowrap;
        overflow: hidden;
        text-overflow: ellipsis;

        span {
          display: inline-block;
          height: 25px;
          cursor: pointer;
        }
      }

      .bg-active {
        background: #3388ff;
        color: white;
        border-radius: 5px;
      }
    }
  }
}
</style>
