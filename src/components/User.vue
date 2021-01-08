<template>
  <div class="col-md-12 data-show-area">
    <button class="btn btn-success" data-toggle="modal" data-target="#exampleModal" @click="addModal"> Add</button>
      <table id="example" class="table table-striped table-bordered" style="width:100%">
          <thead>
              <tr>
                <th>id</th>
                <th>Name</th>
                <th>email</th>
                <th>Number</th>
                <th>actions</th>
              </tr>
          </thead>
          <tbody>
              <tr v-for="(data,id) in datas" :key="id">
                  <td>{{1+id}}</td>
                  <td>{{data.name}}</td>
                  <td>{{data.email}}</td>
                  <td>{{data.phone}}</td>
                  <td>
                      <a class="btn btn-info" style="margin-right:10px" @click="editData(data,id)">edit</a>
                      <a class="btn btn-danger" @click="deleteData(id)">delete</a>
                  </td>
              </tr>
          </tbody>
          <tfoot>
              <tr>
                <th>id</th>
                <th>Name</th>
                <th>email</th>
                <th>Number</th>
                <th>actions</th>
              </tr>
          </tfoot>
      </table>
        <transition name="modal">
           <!-- Modal -->
          <div v-if="showModal" className="modal fade show" id="exampleModal" tabindex="-1" role="dialog"  aria-hidden="true">
            <div class="modal-dialog" role="document">
              <div class="modal-content">
                <div class="modal-header">
                  <p class="modal-title" id="exampleModalLabel">Modal title</p>
                  <button type="button" class="close" data-dismiss="modal" @click="showModal = false" aria-label="Close">
                    <span aria-hidden="true">&times;</span>
                  </button>
                </div>
                <div class="modal-body">
                      
                  <div v-if="errors.length !=null">
                    <div class="text-danger" v-for="(error,id ) in errors" :key='id'>{{ error }}</div>
                  </div>

                  <form >
                    <div className="form-group row">
                        <label for="" className="col-sm-3">Full name</label>
                        <div className="col-sm-9">
                            <input type="text" id="name" v-model="name" class="form-control">
                        </div>
                    </div>
                    <div className="form-group row">
                        <label for="" className="col-sm-3">Email</label>
                        <div className="col-sm-9">
                            <input type="email" v-model="email" className="form-control">
                        </div>
                    </div>
                    <div className="form-group row">
                        <label for="" className="col-sm-3">Phone Number</label>
                        <div className="col-sm-9">
                            <input type="text" v-model="phone" className="form-control">
                        </div>
                    </div>
                      <div class="modal-footer" v-if="edit==false">

                  <button type="button" class="btn btn-secondary" data-dismiss="modal" @click="showModal = false">Close</button>
                  <button class="btn btn-primary" @click.prevent="addData">Save changes</button>
                </div>

                <div class="modal-footer" v-if="edit==true">
                  <button type="button" class="btn btn-secondary" data-dismiss="modal" @click="showModal = false">Close</button>
                  <button type="button" class="btn btn-primary" @click.prevent="updateData">Update</button>
                </div>
                  </form>
                </div>
                

              </div>
            </div>
          </div>
       </transition>
  </div>
</template>

<script>


export default {

      data (){
      return {
          datas:[],
          showModal:false,
          edit:false,
          name:"",
          email:"",
          phone:"",
          id:"",
          errors:[],
      }
    },
   
  methods:{
        addModal:function(){
            this.showModal = true ;
            this.edit=false;
            this.name="";
            this.email="";
            this.phone="";
            this.id="";
            this.errors=[];
        },

        addData:function(){

          this.formValidation();

          if(this.name && this.email  && this.phone){
              this.datas.push({
                name:this.name,
                email:this.email,
                phone:this.phone
           });
            localStorage.setItem('datas',JSON.stringify(this.datas));
            this.name=" ";
            this.email=" ";
            this.phone=" ";
            this.showModal = false;

          }
        },
        editData:function(data,id){
          this.showModal=true,
          this.edit=true,
          this.name=data.name;
          this.email=data.email;
          this.phone=data.phone;
          this.id=id;
          this.errors=[];
        },
        updateData:function(){
            
          this.formValidation();
           
          if(this.name && this.email && this.phone){
             let updatetest={
              name:this.name,
              email:this.email,
              phone:this.phone
           };
            this.datas[this.id]=updatetest;

            localStorage.setItem('datas',JSON.stringify(this.datas));

            let allInfos =JSON.parse(localStorage.getItem('datas')) ;

            this.datas=allInfos;
            this.name="";
            this.email="";
            this.phone="";
            this.id="";
            this.showModal = false;
           
          }
          
        },
        deleteData:function(id){
          this.datas.splice(id,1);
          localStorage.setItem('datas',JSON.stringify(this.datas));
        },
  
        formValidation:function(){
          this.errors = [];
          if(!this.name) {
            this.errors.push('Name required.');
          }
          if (!this.email) {
            this.errors.push(' Email required.');
          }
          if (!this.phone) {
            this.errors.push('Number required.');
          }
      }
    },

    created: function(){
       let allInfos =JSON.parse(localStorage.getItem('datas')) ;

       if(allInfos!=null){
         this.datas=allInfos;
       }else{
         this.datas=[];
       }
       
    },
}
</script>

<style>
    .modal{
      display: block !important;
    }
   .table{
     
      border: 1px solid #dee2e6 !important;
      border-radius: 2px;
   }
     .data-show-area {
          background: #fff;
          box-shadow: 0px 0px 10px 2px rgba(0,0,0,.2);
          border-radius: 2px;
          padding: 25px;
          margin-top: 61px;
      }
      .btn:focus,.btn-success:focus,
      .form-control:focus{
         outline: none !important;
         box-shadow: none !important;
      }
      .btn-success{
         margin-bottom: 20px;
      }
      table.dataTable thead th, table.dataTable thead td {
          border-bottom: none !important;
      }
      table.dataTable tfoot th, table.dataTable tfoot td {
          border-top: none !important;
      }
      .dataTables_wrapper .dataTables_filter {
          float: right;
          text-align: right;
          margin-bottom: 20px !important;
      }
      .modal-enter-active,
      .modal-leave-active {
        transition: opacity .5s;
      }

      .modal-enter,
      .modal-leave-to {
        opacity: 0;
      }
    .form-group {
      margin-bottom: 16px;
    }
    .modal-content {
       
          box-shadow: 0px 0px 10px 2px rgba(0,0,0,.3);
      }
</style>