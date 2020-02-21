<template>
  <div class="container">
    <h1> Danh Sách Công Việc </h1>
    <h6>Tổng Số Công Việc: <strong>{{total}}</strong></h6>
    <div class="input-group mb-3">
      <input
        @keyup.enter="addWorks"
        v-model="name"
        type="text"
        class="form-control"
        placeholder=" Nhập tên công việc "
        aria-label="Recipient's username"
        aria-describedby="basic-addon2"
      />
      <div class="input-group-append">
        <button @click="addWorks()" class="btn btn-primary" type="button">Thêm</button>
      </div>
    
    </div>
      <div class="action d-flex justify-content-end my-3">
        <button @click="clearAll()" class="btn btn-danger mx-3" type="button"> Xóa Hết </button>
        <button @click="completeAll()" class="btn btn-success" type="button"> Đánh dấu tất cả </button>
      </div>


    <div v-for="(work, index) in works" :key="index" class="mb-3 d-flex">
      <div :class="{active__complete: work.isActive}" class="alert alert-warning w-100 text-uppercase" role="alert" @click="complete(index)">
          {{work.name}}
      </div>
      <div class="input-group-append">
        <button @click="removeItem(index)" style="height: 50px" class="btn btn-danger" type="button"> Xóa </button>
      </div>
    </div>
    <div v-if="this.works.length === 0" class="test">
      <h3> Danh Sách Công Việc Của Bạn Trống </h3> <i class="fa fa-frown-o fa-2x" aria-hidden="true"></i>
    </div>
    <div v-else="" class="alert alert-success " role="alert" v-show="elementVisible">
      Nếu công việc hoàn thành hãy click vào công việc để dễ theo dõi nhé!
    </div>
  </div>
</template>

<script>
export default {
  name: "HelloWorld",
  data() {
    return {
      works:[],
      name:'',
      total:'0',
      isActive: false,
      elementVisible:true,
    };
  },
  created() {
    setTimeout(() => this.elementVisible = false, 3000)
  },
  mounted() {
    this.getList();
  },
  computed:{

  },
  methods:{
    complete(index) {
        this.works[index].isActive = !this.works[index].isActive;
        this.storeWork();
    },
    completeAll(){
      for(let i = 0; i < this.works.length;i++){
        // console.log(this.works[i].isActive);
        this.works[i].isActive = true;
      }
      this.storeWork();
    },
    addWorks(){
      if(this.name === ''){
        return 0;
      }
      this.works.push({
        name: this.name,
        isActive: this.isActive
      });
      // if(this.name === ''){
      //     return;
      // }
      // localStorage.setItem('works', JSON.stringify(this.works));
      this.storeWork();
    },
    getList(){
      if(localStorage.getItem('works')){
        this.works = JSON.parse(localStorage.getItem('works'));
        this.total = this.works.length;
      }
    },
    storeWork(){
      let saveAs = JSON.stringify(this.works);
      localStorage.setItem('works', saveAs);
      this.getList();
    },
    removeItem(task){
      this.works.splice(task, 1);
      this.storeWork();
    },
    clearAll(){
      this.works = [];
      this.storeWork();
    },
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .active__complete{
    width: 100%;
    text-decoration: line-through;
    background: #47ED86;
    position: relative;
    color: #fff;
  }
  .alert{
    cursor: pointer;
  }
  .active__complete:after{
    content: " Hoàn Thành ";
    font-weight: bold;
    color: #fff;
    position: absolute;
    top:50%;
    transform: translate(-50%,-50%);
    left: 20%;
    padding: 10px;
  }
  .alert-success:before{
    content: "";
    width: 20px;
    height: 20px;
    background: #D4EDDA;
    position: absolute;
    top:-10px;
    left: 50%;
    transform: rotate(45deg);
  }
</style>
