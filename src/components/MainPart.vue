<template>
  <div class="main">
    <div v-if="response">
      <div v-html="formattedOutput"></div>
    </div>
  </div>
</template>


<script>
import AnsiToHtml from 'ansi-to-html';
export default {
  data() {
    return {
      response: true,
      result: null,
      formattedOutput: '',
    }
  },
  methods: {

  },
  mounted() {
    //接受运行测试返回的信息
    this.$parent.eventBus.$on('result', (data) => {
      this.result = data
      // this.result.data = this.result.data.replace(/\n/g, '<br>')

      // 创建一个 AnsiToHtml 实例
      const ansiToHtml = new AnsiToHtml({
        newline: true
      });
      // 将原始文本中的 ANSI 转义码转换为 HTML 标记
      this.formattedOutput = ansiToHtml.toHtml(this.result.data);

    });
  }
}

</script>
<style>
.main {
  margin-left: 378px;
  padding: 10px;
  min-height: 100vh;
  margin-top: 63px;
  background-color: antiquewhite;
}
</style>

<!--
  1.查看有哪些单元测试：
[get]localhost:8888/readAll
返回：一串字符表示可用的单元测试，单元测试之间用空格隔开
返回例子：triangle_test.cpp calendar_test.cpp
说明；返回的测试名为cpp文件名，如triangle_test.cpp

2.上传文件更新测试用例：
[post]localhost:8888/testCase?upload=文件
说明：文件需要为指定格式，并且名字必须为[测试名]_test.json，如triangle_test.json

3.运行指定测试：
[post]localhost:8888/exec?test=测试名
说明：测试名为[测试名]_test，如triangle_test
返回测试编译、运行的结果。
-->