<template>
    <modal 
        title="Modal with form"
        @close="$emit('close')">

            <div slot="body">
                <form @submit.prevent="onSubmit">

                    <!-- Registration -->
                <div v-if="!regAuth">
                    <div class="form-item" :class="{ errorInput: $v.name.$error }">
                        <label>Name:</label>
                        <p class="errorText" v-if="!$v.name.required"> Field is required</p>
                        <p class="errorText" v-if="!$v.name.minLength"> Name must have at least {{ $v.name.$params.minLength.min }} symbols</p>
                        <input  
                            v-model="name" 
                            :class="{ error: $v.name.$error }" 
                            @change="$v.name.$touch()">
                    </div>
                    <div class="form-item" :class="{ errorInput: $v.email.$error }">
                        <label>Email:</label>
                        <p class="errorText" v-if="!$v.email.required"> Field is required </p>
                        <p class="errorText" v-if="!$v.email.email"> Email is not correct</p>
                    <input 
                        v-model="email"
                        :class="{ error: $v.email.$error }"
                        @change="$v.email.$touch" >
                    
                    </div>


                        <!-- Password -->


                     <div class="form-item" :class="{ errorInput: $v.password.$error }">
                        <label>Password:</label>
                        <p class="errorText" v-if="!$v.password.required"> Field is required</p>
                        <p class="errorText" v-if="!$v.password.minLength"> Password must have at least {{ $v.password.$params.minLength.min }} symbols</p>
                        <input  
                            v-model="password" 
                            :class="{ error: $v.password.$error }" 
                            @change="$v.password.$touch()">
                    </div>



                     <div class="form-item" :class="{ errorInput: $v.passwordCo.$error }">
                        <label>Password confirm:</label>
                        <p class="errorText" v-if="!$v.passwordCo.sameAs"> Password does not match</p>
                        <input  
                            v-model="passwordCo" 
                            :class="{ error: $v.passwordCo.$error }" 
                            @change="$v.passwordCo.$touch()">
                        <button class="btn btnPrimary">Submit</button>
                        <p class="switchText" @click="regSwitch">I already have an account</p>
                    </div>
                </div>

                    <!-- Authorization -->
                <div v-if="regAuth">
                    <div class="form-item" :class="{ errorInput: $v.email.$error }">
                        <label>Email:</label>
                        <p class="errorText" v-if="!$v.email.required"> Field is required </p>
                        <p class="errorText" v-if="!$v.email.email"> Email is not correct</p>
                    <input 
                        v-model="email"
                        :class="{ error: $v.email.$error }"
                        @change="$v.email.$touch" >
                    
                    </div>


                        <!-- Password -->

                
                     <div class="form-item" :class="{ errorInput: $v.password.$error }">
                        <label>Password:</label>
                        <p class="errorText" v-if="!$v.password.required"> Field is required</p>
                        <p class="errorText" v-if="!$v.password.minLength"> Password must have at least {{ $v.password.$params.minLength.min }} symbols</p>
                        <input  
                            v-model="password" 
                            :class="{ error: $v.password.$error }" 
                            @change="$v.password.$touch()">
                        <button class="btn btnPrimary">Submit</button>
                        <p class="switchText" @click="regSwitch">I haven't got account</p>
                    </div>
                </div>
                </form>
            </div>
    </modal>
</template>

<script>
import { required, minLength, email, sameAs } from 'vuelidate/lib/validators'

import modal from '@/components/UI/Modal.vue'
export default {
    components: {
        modal
    },
    data () {
        return {
            email: '',
            name: '',
            password: '',
            passwordCo: '',
            regAuth: false

        }
    },
    validations: {
        name: {
            required,
            minLength: minLength(4)
        },
        email: {
            required,
            email
        },
        password: {
            required,
            minLength: minLength(6),
        },
        passwordCo: {
            required,
            sameAs: sameAs('password')
        }
    
    },
    methods: {
            onSubmit () {
                this.$v.$touch()
                if(!this.$v.$invalid) {
                    const user = {
                        name: this.name,
                        email: this.email,
                        password: this.password
                    }
                    console.log(user)

                    this.name = '',
                    this.email = '',
                    this.password = '',
                    this.passwordCo = '',
                    this.$v.$reset(),
                    this.$emit('close')

                }
            },
            regSwitch () {
                this.regAuth = !this.regAuth
            }
        }
}
</script>

<style lang="scss">

.form-item .errorText {
    display: none;
    margin-bottom: 8px;
    font-size: 13.4px;
    columns: red;
}

.form-item {
    &.errorInput .errorText {
    display: block;
    }
}


input.error {
    border-color: red;
}

.switchText {
    color: rgb(104, 103, 103);
    padding-top: 0.7em;
    &:hover {
        color: rgb(127, 53, 211);
        cursor: pointer;
    }
}

</style>