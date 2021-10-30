<template>
    <div>
        <div class="col-md-12">
            <h2 class="text-center">Laravel Vue Crud Application</h2>
        </div>
        <div class="from-group">
            <label for="name">Name</label>
            <input type="text" placeholder="Enter Your Name" id="name" class="form-control" v-model="item.name">
        </div>
        <div class="from-group">
            <label for="phone">Phone</label>
            <input type="text" placeholder="Enter Your Phone" id="phone" class="form-control" v-model="item.phone">
        </div>
       
            <button class="btn btn-success btn-block mt-3" @click="Save" >{{isEditing ? 'Update':'Save'}}</button>

         <div class="col-md-12 mt-3" v-if="lists.length>0">
             <h2 class="text-center">Telephone Numbers</h2>
                <ul class="list-group">  
                        <li class="list-group-item" v-for="tel in lists" :key="tel.id">{{tel.name}} -- {{tel.phone}} <span class="float-right">
                            <button class="btn btn-warning btn-sm mr-2"  @click="editTel(tel)">Edit</button>
                            <button class="btn btn-danger btn-sm mr-2" @click="deleteTel(tel.id)">Delete</button>
                            </span></li>
                </ul>
         </div>
    </div>
</template>

<script>
    export default {
        name: "Directory",
        data(){
            return{
                lists: [],
                item:{
                    name:"",
                    phone: ""
                },
                temp_id:null,
                isEditing:false
            }
        },
        mounted(){
            this.fetchAll();
        },
        methods: {

            fetchAll(){
                axios.get('/api/tel')
                    .then(res=>this.lists = res.data)
            },

            Save(){
            let method = axios.post
            let url = "/api/tel"
            if (this.temp_id) {
                method = axios.put
                url = `/api/tel/${this.temp_id}`
            }
            try {
                method(url, this.item)
                    .then(res => {
                        this.fetchAll()
                        this.item = {
                            name: "",
                            phone: ""
                        }
                        this.temp_id = null
                        this.isEditing = false
                    })
            } catch (e) {
                console.log(e)
            }
        },

             deleteTel(id) {
            try {
                axios.delete(`/api/tel/${id}`)
                    .then(res => {
                        this.fetchAll()
                    })
            } catch (e) {
                console.log(e)
            }
        },
        editTel(tel) {
            this.item = {
                name: tel.name,
                phone: tel.phone,
            }
            this.temp_id = tel.id;
            this.isEditing = true
        }
    }
}
</script>


<style scoped>

</style>
