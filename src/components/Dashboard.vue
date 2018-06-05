<template>
    <div id="dashboard">
        <ul class="collection with-header">
            <li class="collection-header"><h4>Empoyees</h4></li>
            <li v-for="employee in employees" v-bind:key="employee.id" class="collection-item avatar">
                <img v-bind:src="employee.logo"  alt="" class="circle">
                <span class="title"><b style="color: red">{{employee.name}}</b></span>
                <p><b>Department: </b>{{employee.dept}}<br>
                    <b>Position: </b>{{employee.position}}
                </p>
                <router-link class="secondary-content" v-bind:to="{name:'view-contact', params:{employee_id:employee.employee_id}}">
                    <i class="fa fa-eye"></i>
                </router-link>
            </li>
        </ul>
        <div class="fixed-action-btn">
            <router-link to="/new" class="btn-floating btn-large red">
                <i href="" class="fa fa-plus"></i>
            </router-link>
        </div>
    </div>
</template>

<script>
    import db from './firebaseInit';
    export default {
        name: "dashboard",
        data () {
            return {
                employees: []
            }
        },
        created(){
            db.collection('employees').orderBy('dept').get().then
            (querySnapshot =>{
                querySnapshot.forEach(doc =>{
                    const data = {
                        'id': doc.id,
                        'employee_id': doc.data().employee_id,
                        'name': doc.data().name,
                        'logo': doc.data().logo,
                        'dept': doc.data().dept,
                        'position': doc.data().position
                    };
                    this.employees.push(data)
                })
            })
        }
    }
</script>

<style scoped>

</style>