<template>
    <div class="container mt-5">
        <div class="card">
            <div class="card-header">
                <h4>Register Student</h4>
            </div>
            <div class="card-body">
                <ul class="alert alert-warning" v-if="Object.keys(errorList).length > 0">
                    <li class="mb-0 ms-3" v-for="(error, index) in errorList" :key="index">
                        {{ error[0] }}
                    </li>
                </ul>
                <div class="mb-3">
                    <label for="">Firstname</label>
                    <input type="text" class="form-control" v-model="model.student.firstname" />
                </div>
                <div class="mb-3">
                    <label for="">Lastname</label>
                    <input type="text" class="form-control" v-model="model.student.lastname" />
                </div>
                <div class="mb-3">
                    <label for="">Course</label>
                    <input type="text" class="form-control" v-model="model.student.course" />
                </div>
                <div class="mb-3">
                    <label for="">Email</label>
                    <input type="text" class="form-control" v-model="model.student.email" />
                </div>
                <div class="mb-3">
                    <label for="">Phone</label>
                    <input type="text" class="form-control" v-model="model.student.phone" />
                </div>
                <div class="mb-3">
                    <button type="button" @click="submitStudent" class="btn btn-success">Submit</button>
                    <RouterLink to="/students/view" class="btn btn-dark">
                        Go Back
                    </RouterLink>   
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import axios from 'axios'

export default {
    name: 'studentCreate',
    data() {
        return {
            errorList: {}, // Initialize errorList as an empty object
            model: {
                student: {
                    firstname: '',
                    lastname: '',
                    course: '',
                    email: '',
                    phone: '',
                }
            }
        };
    },
    methods: {
        submitStudent() {
            var mythis = this;
            axios.post('http://127.0.0.1:8000/api/students', this.model.student)
                .then(res => {
                    console.log(res.data);
                    alert(res.data.message);
                    this.model.student = {
                        firstname: '',
                        lastname: '',
                        course: '',
                        email: '',
                        phone: '',
                    }
                    this.errorList = '';
                })
                .catch(error => {
                    console.error('Error:', error);
                    if (error.response) {
                        if (error.response.status == 422) {
                            mythis.errorList = error.response.data.errors;
                        }
                    } else if (error.request) {
                        console.log(error.request);
                    } else {
                        console.log('Error', error.message);
                    }
                });
        }
    }
};
</script>
