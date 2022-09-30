<template>
    <div class="container h-100">
        <div class="row h-100 align-items-center">
            <div class="col-12 col-md-6 offset-md-3">
                <div class="card shadow sm">
                    <div class="card-body">
                        <h1 class="text-center">Register</h1>
                        <hr/>
                        <form action="javascript:void(0)" @submit="register" class="row" method="post">
                            <div class="col-12" v-if="Object.keys(validationErrors).length > 0">
                                <div class="alert alert-danger">
                                    <ul class="mb-0">
                                        <li v-for="(value, key) in validationErrors" :key="key">{{ value[0] }}</li>
                                    </ul>
                                </div>
                            </div>
                            <div class="form-group col-12">
                                <label for="name" class="font-weight-bold">Name</label>
                                <input type="text" name="name" v-model="user.name" id="name" placeholder="Enter name" class="form-control">
                            </div>
                            <div class="form-group col-12 my-2">
                                <label for="email" class="font-weight-bold">Email</label>
                                <input type="text" name="email" v-model="user.email" id="email" placeholder="Enter Email" class="form-control">
                            </div>
                            <div class="form-group col-12 my-2">
                                <label for="national_id" class="font-weight-bold">National id</label>
                                <input type="text" name="national_id" v-model="user.national_id" id="National_id" placeholder="Enter National id" class="form-control">
                            </div>
                            <div class="form-group col-12 my-2">
                                <label for="country" class="font-weight-bold">Country</label>
                                <input type="text" name="country" v-model="user.country" id="country" placeholder="Enter National id" class="form-control">
                            </div>
                            <div class="form-group col-12 my-2">
                                <label for="constituency" class="font-weight-bold">constituency</label>
                                <input type="text" name="constituency" v-model="user.constituency" id="constituency" placeholder="Enter National id" class="form-control">
                            </div>
                            <div class="form-group col-12 my-2">
                                <label for="ward" class="font-weight-bold">ward</label>
                                <input type="text" name="ward" v-model="user.ward" id="ward" placeholder="Enter National id" class="form-control">
                            </div>
                            <div class="form-group col-12 my-2">
                                <label for="position" class="font-weight-bold">Position</label>
                                <input type="text" name="position" v-model="user.position" id="position" placeholder="Enter Position" class="form-control">
                            </div>
                            
                            <div class="form-group col-12">
                                <label for="password" class="font-weight-bold">Password</label>
                                <input type="password" name="password" v-model="user.password" id="password" placeholder="Enter Password" class="form-control">
                            </div>
                            <div class="form-group col-12 my-2">
                                <label for="password_confirmation" class="font-weight-bold">Confirm Password</label>
                                <input type="password_confirmation" name="password_confirmation" v-model="user.password_confirmation" id="password_confirmation" placeholder="Enter Password" class="form-control">
                            </div>
                            <div class="col-12 mb-2">
                                <button type="submit" :disabled="processing" class="btn btn-primary btn-block">
                                    {{ processing ? "Please wait" : "Register" }}
                                </button>
                            </div>
                            <div class="col-12 text-center">
                                <label>Already have an account? <router-link :to="{name:'login'}">Login Now!</router-link></label>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import { mapActions } from 'vuex'
export default {
    name:'register',
    data(){
        return {
            user:{
                name:"",
                email:"",
                national_id:"",
                country:"",
                constituency:"",
                ward:"",
                position:"",
                password:"",
                password_confirmation:"",
            },
            validationErrors:{},
            processing:false
        }
    },
    methods:{
        ...mapActions({
            signIn:'auth/login'
        }),
        async register(){
            this.processing = true
            await axios.get('/sanctum/csrf-cookie')
            await axios.post('/register',this.user).then(response=>{
                this.validationErrors = {}
                this.signIn()
            }).catch(({response})=>{
                if(response.status===422){
                    this.validationErrors = response.data.errors
                }else{
                    this.validationErrors = {}
                    alert(response.data.message)
                }
            }).finally(()=>{
                this.processing = false
            })
        }
    }
}
</script>