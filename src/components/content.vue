<!--  -->
<template>
  <div>
    <Tabs :oArray="tabList" ></Tabs>
  </div>
</template>

<script>

import Tabs from './HelloWorld'

export default {
  components: {
    Tabs
  },
  data () {
    return {
      tabList: [
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
      oarray: "你好dsdsds"
    }
  },

  methods: {},

  computed: {},

  mounted () {}

  
}

</script>
<style  scoped>

</style>
