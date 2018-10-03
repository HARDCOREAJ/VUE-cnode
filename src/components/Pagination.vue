<template>
    <div class="pagination">
        <button @click="changeBtn">«</button>
        <button v-if="judge">......</button>
        <button v-for="btn in pageBtn" :key="btn" :class="[{currentPage: btn === currentPage},'pageBtn']" @click="changeBtn(btn)">
            {{btn}}
        </button>
        <button>......</button>
        <button @click="changeBtn">»</button>
    </div>
</template>

<script>
import $ from 'jquery'
export default {
    name: 'Pagination',
    data(){
        return {
            pageBtn: [1,2,3,4,5],
            currentPage: 1,
            true: false,
            judge: false,
            articleTopic: '',
            topic: []
        }
    },
    methods:{
        changeBtn(page){
            if(typeof page !== 'number'){
                switch(page.target.innerText){
                    case '«':
                        $('button.currentPage').prev().click()
                        break
                    case '»':
                        $('button.currentPage').next().click()
                        break
                    default: 
                        break
                }
                return 
            }
            this.currentPage = page
            this.topic = document.querySelectorAll('.topic-tab')
            for(let i = 0;i<this.topic.length; i++){
                if(this.topic[i].classList.item(1)){
                    this.articleTopic = this.topic[i].innerText
                }
            }
            if(page>4){
                this.judge = true
            }else{
                this.judge = false
            }
            if(page == this.pageBtn[4]){
                this.pageBtn.shift()
                this.pageBtn.splice(4,0,this.pageBtn[3]+1)
            }else if(page == this.pageBtn[0] && page !== 1){
                this.pageBtn.unshift(this.pageBtn[0]-1)
                this.pageBtn.splice(5,1)
            }
            this.$emit("handleList",[this.currentPage,this.articleTopic,this.changeBtn])
        },
        changePage(){
            if(this.pageBtn !== [1,2,3,4,5]){
                this.pageBtn = [1,2,3,4,5]
                this.judge = false
                this.currentPage = 1
            }
        }
    }
}
</script>

<style scoped>
.currentPage{
    background: #80BD01;
    outline: #80BD01;
    color: white;
}
button{
    width: 35px;
    height: 30px;
    border: 1px solid #CCC;
    background: white;
    margin: 5px 0px;
}
</style>