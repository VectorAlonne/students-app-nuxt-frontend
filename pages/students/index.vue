<template>
    <div class="container mt-5">
        <div class="card">
            <div class="card-header">
                <h4>
                    Students Lists
                    <NuxtLink to="/students/create" class="btn btn-primary float-end">Add Student</NuxtLink>
                </h4>
            </div>
            <div class="card-body">
                <div v-if="isLoading">
                    <Loading title="Loading..."/>
                </div>
                <div v-else>
                    <table class="table table-striped table-bordered">
                        <thead>
                            <tr>
                                <th>ID</th>
                                <th>Name</th>
                                <th>Course</th>
                                <th>Email</th>
                                <th>Phone</th>
                                <th>Created At</th>
                                <th>Action</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr v-for="(students, index) in students" :key="index">
                                <td>{{ students.id}}</td>
                                <td>{{ students.name}}</td>
                                <td>{{ students.course}}</td>
                                <td>{{ students.email}}</td>
                                <td>{{ students.phone}}</td>
                                <td>{{ students.created_at}}</td>
                                <td>
                                    <NuxtLink :to="`/students/${students.id}`" class="btn btn-success btn-sm mx-2">Edit</NuxtLink>
                                    <button type="button" @click="deleteStudent($event, students.id)" to="/" class="btn btn-danger btn-sm mx-2">Delete</button>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios'
    export default {
        name: "student",
        data() {
            return {
                students: {},
                isLoading: true
            }
        },
        mounted() {
            this.getStudents();
        },
        methods: {
            getStudents() {
                this.isLoading = true;
                axios.get(`http://127.0.0.1:8000/api/students`)
                .then(res => {
                    //console.log(res.data.students)
                    this.isLoading = false;
                    this.students = res.data.students;
                });
            },
            deleteStudent(event, studentId) {
                if(confirm('Are you sure, you want to delete this data?'))
                {
                    event.target.innerText = 'Deleting';

                    axios.delete(`http://127.0.0.1:8000/api/students/${studentId}/delete`)
                    .then(res => {

                        event.target.innerText = 'Delete';
                        this.getStudents();
                    })
                }
            }
        }
    }
</script>