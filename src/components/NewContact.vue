<template>
    <div id="new-contact">
        <h3>New Contact</h3>
        <form @submit.prevent="saveContact" class="col s12">
            <div class="row">
                <label for="employeeId">Employee ID</label>
                <input id="employeeId" type="text" v-model="employee_id">
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
        name: "new-contact",
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
        methods: {
            saveContact () {
                db.collection('employees').add({
                    employee_id: this.employee_id,
                    name: this.name,
                    dept: this.dept,
                    position: this.position,
                    text: this.text,
                    logo: this.logo
                })
                    .then(docRef => this.$router.push('/'))
                    .catch(error => console.log(err))
            }
        }
    }
</script>

<style scoped>

</style>