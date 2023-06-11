<template>
    <div class="header">
        <div class="logo">
            <div class="circle">
                <div class="text">TT</div>
            </div>
            <span>TestTool</span>
        </div>
        <div class="buttons">
            <input type="file" ref="fileInput" style="display: none" @change="handleFileChange" />
            <button :disabled="selected" :title="descr" class="upload-btn" @click="handleClick">上传</button>
            <button :disabled="selected" class="start-btn" @click="run">启动</button>
        </div>
    </div>
</template>
  
<script>
import axios from 'axios';
//import eventBus from './LeftPart.vue'
//import Vue from 'vue';
//const eventBus = new Vue();
export default {
    data() {
        return {
            testname: null,
            selected: true,
            descr:"名字必须为[测试名]_test.json，如triangle_test.json"
        }
    },
    methods: {
        handleClick() {
            this.$refs.fileInput.click();
        },
        handleFileChange(event) {
            const files = event.target.files;
            if (files.length === 0) {
                return;
            }
            const formData = new FormData();
            formData.append('file', files[0]);
            axios.post('/testCase', formData).then(() => {
                alert('上传成功');
            }).catch(() => {
                alert("上传失败");
            });
        }, 
         run(){
            //let fileName = this.tesname 
            let dotIndex = this.testname.lastIndexOf('.');
            let result = this.testname.substring(0, dotIndex);
            console.log("==result==")
            console.log(result)
            let url='http://localhost:8888/exec'
            if(this.testname=='unit_test'||this.testname=='integration_test'||this.testname=='system_test'){
                url="http://localhost:8888/execBackend"
                result=this.testname
            }
            console.log("==url==")
            console.log(url)
            axios({
                method: 'post',
                url: url,
                params: {
                    test: result
                }
            }).then((data) => {
                this.$parent.eventBus.$emit('result', data)
            }).catch(() => {
                alert("测试启动失败");
            });
            //  axios.post('http://localhost:8888/exec', {
            //     test: result
            // }).then((data) => {//把返回的信息传给main部分                
            //     this.$parent.eventBus.$emit('result', data)

            // }).catch(() => {
            //     //测试
            //     // let data="hello"
            //     // this.$parent.eventBus.$emit('result', data)
            //     alert("测试启动失败");
            // });
        }
    },
mounted() {
    //接受左边栏传来的选择的测试文件名
    this.$parent.eventBus.$on('testname', (selected_name) => {
        this.testname = selected_name;
        console.log("==testname==")
        console.log(this.testname)
        this.selected=false
    });
}
}
</script>
  
<style>
body {
    margin: 0;
    padding: 0;
}

.header {
    width: 100%;
    height: 60px;
    background-color: #006400;
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 20px;
    position: fixed;
    top: 0;
    left: 0;
    z-index: 999;
}

.logo {
    display: flex;
    align-items: center;
    font-size: 24px;
    color: #fff;
}

.circle {
    width: 40px;
    height: 40px;
    border-radius: 50%;
    background-color: #fff;
    margin-right: 10px;
    display: flex;
    justify-content: center;
    align-items: center;
}

.text {
    font-weight: bold;
    color: #006400;
}

.buttons {
    display: flex;
    align-items: center;
    margin-right: 120px;
}

.upload-btn,
.start-btn {
    padding: 10px 20px;
    font-size: 16px;
    color: #fff;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}

.upload-btn {
    background-color: #4169E1;
    margin-right: 30px;
}

.start-btn {
    background-color: #FF8C00;
}

/* 禁用的按钮样式 */
.start-btn:disabled {
  background-color: gray;
  color: white;
  cursor: not-allowed;
}

/* 禁用的按钮样式 */
.upload-btn:disabled {
  background-color: gray;
  color: white;
  cursor: not-allowed;
}
</style>