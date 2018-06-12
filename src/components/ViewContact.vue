<template>
    <div id="view-contact">
        <div class="col s12 m8 offset-m2 l6 offset-l3">
            <div class="card-panel grey lighten-5 z-depth-1">
                <div class="row">
                    <div class="col s2">
                        <img v-bind:src="logo" alt="" class="circle responsive-img">
                    </div>
                    <div class="col s10">
                        <span class="title"><h4><b style="color: red">{{name}}</b></h4></span>
                        <h5><b>Department: </b>{{dept}}<br>
                            <b>Position: </b>{{position}}
                        </h5>
                        <p>{{text}}</p>
                    </div>
                </div>
            </div>
        </div>
        <router-link to="/" class="btn grey">Back</router-link>
        <button @click="deleteContact" class="btn red">Deleate</button>
        <div class="fixed-action-btn">
            <router-link v-bind:to="{name: 'edit-contact', params: {employee_id: employee_id}}" class="btn-floating btn-large red">
                <i class="fa fa-pencil-alt"></i>
            </router-link>
        </div>
    </div>
</template>

<script>
    import db from './firebaseInit'
    export default {
        name: "view-contact",
        data () {
            return {
                'employee_id': null,
                'name': null,
                'dept': null,
                'logo': null,
                'position': null,
                'text': null
            }
        },
        beforeRouteEnter(to, from, next){
            db.collection('employees').where('employee_id', '==', to.params.employee_id).get().then(querySnapshot => {
                querySnapshot.forEach(doc =>{
                    next(vm => {
                        vm.employee_id = doc.data().employee_id;
                        vm.name = doc.data().name;
                        vm.dept = doc.data().dept;
                        vm.logo = doc.data().logo;
                        vm.position = doc.data().position;
                        vm.text = doc.data().text
                    })
                })
            })
        },
        watch: {
            '$route': 'fetchData'
        },
        methods: {
            fetchData(){
                db.collection('employees').where('employee_id', '==', this.$route.params.employee_id).get().then(querySnapshot => {
                    querySnapshot.forEach(doc => {
                        this.employee_id = doc.data().employee_id;
                        this.name = doc.data().name;
                        this.dept = doc.data().dept;
                        this.position = doc.data().position;
                        this.text = doc.data().text
                    })
                })
            },
            deleteContact (){
                if (confirm('Are you sure?')) {
                    db.collection('employees').where('employee_id', '==', this.$route.params.employee_id).get()
                        .then(querySnapshot => {
                        querySnapshot.forEach(doc => {
                            doc.ref.delete();
                            this.$router.push('/')
                        })
                    })
                }
            }
        }
    }
</script>

<style scoped>

</style>

