<template>
    <div class="dealPostSave">
        <h1>글 저장 컴포넌트 입니다.</h1>
        <form class="dealPostSaveForm">
            <p>
                <label for="titleInput">제목 </label>
                <input type="text" id="dealPostTitleInput" v-model="dealPost.title" class="inputText" ref="dealPostTitleInput" placeholder="글 제목">
            </p>
            <p>
                <label for="contentInput">내용 </label>
                <input type="text" id="dealPostContentInput" v-model="dealPost.content" class="inputText" ref="dealPostContentInput" placeholder="글 내용">
            </p>
            <p>
                <label for="categoryInput">카테고리 </label>
                <input type="text" id="dealPostCategoryInput" v-model="dealPost.category" class="inputText" ref="dealPostCategoryInput" placeholder="글 카테고리">
            </p>
            <p>
                <label for="priceInput">가격 </label>
                <input type="number" id="dealPostPriceInput" v-model.number="dealPost.price" class="inputText" ref="dealPostPriceInput" placeholder="가격 ">
            </p>
            <p>
                <label for="imageInput">이미지 </label>
                <input type="file" v-on:change="onFileChange" accept="image/*" multiple="multiple">
                <!-- eslint-disable -->
                <div class="row">
                    <div class="column" v-for="image in uploadedImages">
                        <img v-bind:src="image" style="width:100%">
                    </div>
                </div>            
            </p>
            <p class="buttons">
                <button @click.prevent="doSave" class="buttonBlue">저장</button>&nbsp;
                <button @click.prevent="doCancel" class="button">취소</button>&nbsp;
            </p>
        </form>
    </div>
</template>

<script>
import { mapActions } from 'vuex'

export default{
    name:'DealPostSave',
    data(){
        return {
            dealPost:{
                title:'',
                content:'',
                category:'',
                price:0,
                files:[],
            },
            uploadedImages:[],
        }
    },
    methods:{
        doSave(){
            this.saveDealPost({
                title : this.dealPost.title,
                content : this.dealPost.content,
                category : this.dealPost.category,
                price : this.dealPost.price,
                files : this.dealPost.files,
            })
        },
        doCancel(){
            this.dealPost.title = '',
            this.dealPost.content = '',
            this.dealPost.category = '',
            this.dealPost.price = 0,
            this.dealPost.files = []
            this.uploadedImages = []
        },
        onFileChange(e){
            var files = e.target.files || e.dataTransfer.files;
            if(files.length<=0){
                console.log('no files');
                return;
            }
            this.dealPost.files=[];
            this.uploadedImages=[];
            for(var i=0;i<files.length;i++){
                var reader=new FileReader();
                var file=files[i];
                reader.onload = (e) => {
                    this.uploadedImages.push(e.target.result);
                };
                this.dealPost.files.push(file);
                reader.readAsDataURL(file);
            }
        },
        ...mapActions('dealPostStore', ['saveDealPost']),
    },
}
</script>

<style scoped>
* {
  box-sizing: border-box;
}

.column {
  float: left;
  width: 20%;
  padding: 5px;
}

/* Clearfix (clear floats) */
.row::after {
  content: "";
  clear: both;
  display: table;
}
</style>