<template>
    <div class="section-body">
        <div class="row">
            <div class="col-12 col-md-6 col-lg-12">
                <div class="card">
                    <div class="card-header">
                        <div class="d-flex justify-content-between">
                            <div>
                                <h4>Update Expense</h4>
                            </div>
                            <div>
                                <router-link :to="{name:'expenseIndex'}" class="btn btn-success">All Expenses</router-link>
                            </div>
                        </div>
                    </div>
                    <div class="card-body">
                        <form @submit.prevent="expenseEdit">
                            <div class="row">
                                <div class="col-md-6">
                                    <div class="form-group">
                                        <label>Expense Details<span class="text-danger">*</span></label>
                                        <textarea v-model="form.details" cols="10" rows="3" type="text" class="form-control" placeholder="Expense Details"></textarea>
                                        <small class="text-danger" v-if="errors.details">{{errors.details[0]}}</small>
                                    </div>
                                </div>
                                  <div class="col-md-6">
                                    <div class="form-group">
                                        <label>Amount <span class="text-danger">*</span></label>
                                        <input v-model="form.amount" type="number" class="form-control" placeholder="Amount">
                                        <small class="text-danger" v-if="errors.amount">{{errors.amount[0]}}</small>
                                    </div>
                                </div>

                                <div class="col-md-12">
                                    <div class="form-group">
                                        <button class="btn btn-info"><i class="fas fa-edit"></i> Update Expense</button>
                                    </div>
                                </div>
                           </div>
                        </form>
                    </div>
                </div>

            </div>
        </div>
    </div>
</template>

<script>
    export default {
        name: "edit",
        data(){
            return {
                form:{
                    details:'',
                    amount:'',
                },  
                errors:{},
            }
        },
        created() {
            let id = this.$route.params.id;
            axios.get('api/expenses/'+id)
                .then(res=>{
                    this.form = res.data;
                })
                .catch(error=>{})
        },
        methods:{
            expenseEdit(){
                let id = this.$route.params.id;
                axios.patch('api/expenses/'+id,this.form)
                    .then(res=>{
                        Notification.success();
                        this.$router.push({name:'expenseIndex'})
                    })
                    .catch(error=>{
                        this.errors = error.response.data.errors;
                        console.log(error.response.data.errors)
                    })
            },
           
        },
        mounted() {
            if(!User.loggedIn()){
                Toast.fire({
                    icon: 'warning',
                    title: 'Login First!',
                });
                this.$router.push({name:'login'})
            }
        },
    }
</script>

<style scoped>

</style>
