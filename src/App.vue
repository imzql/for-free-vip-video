<script setup>
import { ref, onMounted } from 'vue';
import { LewMessage } from 'lew-ui'; 

//生命周期钩子
onMounted(() => {
  //重要的事情说三遍
  LewNotification.success({
    title: '温馨提示',
    content: '如果您是第一次使用本站，请先阅读新手教程',
    delay: 30000
  })
  LewNotification.success({
    title: '温馨提示',
    content: '如果您是第一次使用本站，请先阅读新手教程',
    delay: 30000
  })
  LewNotification.success({
    title: '温馨提示',
    content: '如果您是第一次使用本站，请先阅读新手教程',
    delay: 30000
  })
});

// 响应式变量
const options = ref([
  { label: '虾米解析', value: 'https://jx.xmflv.com/?url=' },
  { label: '八戒解析', value: 'https://jx.m3u8.tv/jiexi/?url=' },
  { label: '唐僧解析', value: 'https://jx.xmflv.cc/?url=' },
  { label: '悟空解析', value: 'https://bd.jx.cn/?url=' }
]);

const selectedShipping = ref('');
const videoUrl = ref('');
const iframeUrl = ref(options.value[0].value);
const jxUrl = ref(options.value[0].value);
const inputRef = ref(null); // 定义一个 ref 来引用 input 元素

// 更新 iframeUrl
const updateIframeUrl = () => {
  if (jxUrl.value && videoUrl.value) {
    iframeUrl.value = `${jxUrl.value}${videoUrl.value}`;
  } else {
    iframeUrl.value = options.value[0].value;
  }
};

// 处理播放请求
const handleRequest = () => {
  updateIframeUrl();
  if (iframeUrl.value != options.value[0].value) {
    return new Promise((resolve) => {
      setTimeout(() => {
        resolve();
        LewMessage.success('解析成功');
      }, 1000);
    });
  } else {
    LewMessage.error({
    content: '错误：请先填写视频链接再提交,谢谢合作！',
    duration: 3000
  })
  }
};

//抽屉
const visible = ref(false)
const openJC = () => {
  visible.value = true
}
// 点击按钮时打开新标签页
const openNewTab = (val) => {
  window.open(val, '_blank'); // 打开新的标签页
};
</script>

<template>
  <div id="content">
    <div class="head">
      <span style="text-align: center;margin-bottom: 10px;">Vip免费视频解析</span>
      <lew-badge value="important!" round color="red" style="display: inline-block;">
        <lew-button color="black" size="medium" text="点我查看教程" dashed type="ghost" @click="openJC" />
      </lew-badge>
    </div>
    <!-- 抽屉 -->
    <lew-flex gap="20" x="start">
      <lew-drawer title="使用教程" close-by-esc v-model:visible="visible" position="left" hideFooter="true">
        <div class="jc-text">
          <h2>操作指南：</h2>
          <h3>第一步：</h3>
          <p>这里以腾讯视频为例，腾讯视频：<a href="https://v.qq.com/channel/movie">点我跳转腾讯视频</a></p>
          <p>找到想要观看的VIP视频</p>
          <p><img src="https://ice.frostsky.com/2024/09/15/8fd7e548c47cbf69479ad3ea3f03e3d4.png" alt=""></p>
          <h3>第二步：</h3>
          <p>将浏览器上方的网址链接复制一下</p>
          <p><img src="https://ice.frostsky.com/2024/09/15/74f1b4483de8acc98230c4d53dbbac4c.png" alt=""></p>
          <p>将复制的链接粘贴到本站播放地址，并点击播放按钮</p>
          <p><img src="https://ice.frostsky.com/2024/09/15/f60383ddf991d44ca2899d47d85a5830.png" alt=""></p>
          <p>等待解析成功提示，即可免费观看VIP视频</p>
        </div>
      </lew-drawer>
    </lew-flex>
    <!-- 搜索栏 -->
    <div class="nav">
      <div class="route">
        <lew-select v-model="jxUrl" clearable size="medium" placeholder="选择解析方式" :options="options" />
      </div>
      <lew-input v-model="videoUrl" align="left" placeholder="请键入视频链接" class="search" selectByFocus="true" />
      <lew-button class="click" :request="handleRequest" text="播放" type="light" color="var(--lew-text-color-1);" />
      <lew-button class="click clear" text="清空" type="light" color="var(--lew-text-color-1);"
        @click="videoUrl = ''; iframeUrl = options.value[0].value;" />
    </div>

    <!-- 播放器 -->
    <iframe :src="iframeUrl" class="player" allowfullscreen allowtransparency></iframe>

    <div class="footer" style="margin-top: -16px;">
      <lew-flex direction="y" gap="20">
        <lew-mark color="red">注意：视频如果有广告是解析接口内置与本站无关，请勿相信！</lew-mark>
        <lew-flex wrap x="center" gap="10">
          <lew-button size="medium" @click="openNewTab('https://v.qq.com')" text="进入腾讯视频" color="black" dashed
            type="ghost" />
          <lew-button size="medium" @click="openNewTab('https://www.iqiyi.com')" text="进入爱奇艺视频" color="black" dashed
            type="ghost" />
            <lew-button size="medium" @click="openNewTab('https://www.bilibili.com/movie')" text="进入bilibili视频" color="black" dashed
            type="ghost" />
          <lew-button size="medium" @click="openNewTab('https://youku.com')" text="进入优酷视频" color="black" dashed
            type="ghost" />
          <lew-button size="medium" @click="openNewTab('https://www.mgtv.com')" text="进入芒果视频" color="black" dashed
            type="ghost" />
        </lew-flex>
        <lew-mark color="green">本项目已开源Github,欢迎star✌️! </lew-mark>
        <lew-mark type="light" color="green">Github地址 : <a target="_blank" href="https://github.com/imzql/for-free-vip-video">点击这里跳转项目地址</a></lew-mark>
        <lew-mark color="red">免责声明：解析服务仅限小伙伴们学习用途！对于一些不法分子破坏视频版权行为，本站将坚决抵制！</lew-mark>
        © 2019 - 2024 Designed by 小赵同学
      </lew-flex>
    </div>
  </div>
</template>

<style scoped>
#content {
  display: inline-table;
}

.head {
  text-align: center;
}

.head span {
  font-size: 1.5em;
  margin-right: 10px;
}

.jc-text {
  padding: 30px;
}

.jc-text img {
  width: 100%;
}

.nav {
  display: flex;
  justify-content: center;
  align-items: center;
  margin-top: 50px;
}

.route {
  width: 130px;
}

.search {
  width: 666px;
  margin: 0 5px;
}

.nav button {
  width: 80px;
  border-radius: 5px;
  cursor: pointer;
}

.clear {
  margin-left: 5px;
}

.player {
  display: block;
  width: 1000px;
  height: 500px;
  padding: 15px;
  margin: 30px auto;
  border: none;
  overflow: hidden;
  border-radius: 5px;
}
</style>
