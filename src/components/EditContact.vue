<template>
    <div id="edit-contact">
        <h3>Edit Contact</h3>
        <form @submit.prevent="updateContact" class="col s12">
            <div class="row">
                <label for="employeeId">Employee ID</label>
                <input id="employeeId" type="text" v-model="employee_id" disabled>
            </div>
            <div class="row">
                <label for="name">Name</label>
                <input id="name" type="text" v-model="name" required>
            </div>
            <div class="row">
                <label for="dept">Department</label>
                <input id="dept" type="text" v-model="dept" required>
            </div>
            <div class="row">
                <label for="position">Position</label>
                <input id="position" type="text" v-model="position" required>
            </div>
            <div class="row">
                <label for="text">About me</label>
                <input id="text" type="text" v-model="text" required>
            </div>
            <div class="row">
                <label for="logo">Logo URL</label>
                <input id="logo" type="text" v-model="logo" required>
            </div>
            <router-link to="/" class="btn grey">Cancel</router-link>
            <button type="submit" class="btn">SUBMIT</button>
        </form>
    </div>
</template>

<script>
    import db from './firebaseInit';
    export default {
        name: "edit-contact",
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
                        this.employee_id = doc.data().employee_id
                        this.name = doc.data().name
                        this.dept = doc.data().dept
                        this.position = doc.data().position
                        this.logo = doc.data().logo
                        this.text = doc.data().text
                    })
                })
            },
            updateContact(){
                db.collection('employees').where('employee_id', '==', this.$route.params.employee_id).get().then(querySnapshot => {
                    querySnapshot.forEach(doc => {
                        doc.ref.update({
                            employee_id: this.employee_id,
                            name: this.name,
                            dept: this.dept,
                            position: this.position,
                            logo: this.logo,
                            text: this.text
                        })
                        .then(()=>{
                            this.$router.push({name:'view-contact', params:{employee_id: this.employee_id}})
                        })
                    })
                })
            }
        }
    }
</script>

<style scoped>

</style>