<template>
    <div>
        <div class="bg-gray-50 dark:bg-gray-900">
            <div class="flex flex- items-center justify-center px-6 py-8 mx-auto w-auto md:h-screen lg:py-0">
                <!-- <a href="#" class="flex items-center mb-6 text-2xl font-semibold text-gray-900 dark:text-white">
                <img class="w-8 h-8 mr-2" src="https://flowbite.s3.amazonaws.com/blocks/marketing-ui/logo.svg"
                    alt="logo">
                Flowbite
            </a> -->
                <div
                    class="w-full bg-white rounded-lg shadow dark:border md:mt-0 sm:max-w-md xl:p-0 dark:bg-gray-800 dark:border-gray-700">
                    <div class="p-6 space-y-4 md:space-y-6 sm:p-8">
                        <h1
                            class="text-xl font-bold leading-tight tracking-tight text-gray-900 md:text-2xl dark:text-white">
                            Create and account
                        </h1>
                        <form class="space-y-4 md:space-y-6" @submit.prevent="sendPost">
                            <div class="flex flex-row">
                                <div class="flex flex-col mr-6">
                                    <div>
                                        <label :for="f_data_l[0]"
                                            class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">ID
                                        </label>
                                        <input v-model="f_data_l[0]" type="id" :name="f_data_l[0]" :id="f_data_l[0]"
                                            class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                                            placeholder="set your id" required="">
                                    </div>
                                    <div>
                                        <label :for="f_data_l[1]"
                                            class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Password</label>
                                        <input v-model="f_data_l[1]" type="password" :name="f_data_l[0]"
                                            :id="f_data_l[0]" placeholder="••••••••"
                                            class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                                            required="">
                                    </div>
                                    <div>
                                        <label :for="f_data_l[2]"
                                            class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Your
                                            age
                                        </label>
                                        <input v-model="f_data_l[2]" type="number" :name="f_data_l[2]" :id="f_data_l[2]"
                                            class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                                            placeholder="enter your age" required="">
                                    </div>
                                    <div>
                                        <label :for="f_data_l[3]"
                                            class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">
                                            Your name
                                        </label>
                                        <input v-model="f_data_l[3]" type="name" :name="f_data_l[3]" :id="f_data_l[3]"
                                            class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                                            placeholder="enter your name" required="">
                                    </div>


                                    <div>
                                        <label :for="f_data_l[4]"
                                            class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">
                                            Your Gender
                                        </label>
                                        <select v-model="f_data_l[4]" :id="f_data_l[4]"
                                            class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                                            required>
                                            <option value="m">male</option>
                                            <option value="f">female</option>
                                        </select>

                                    </div>
                                </div>

                                <div class="flex flex-col">
                                    <div>
                                        <label :for="f_data_l[5]"
                                            class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">
                                            Your current weight
                                        </label>
                                        <input v-model="f_data_l[5]" type="number" :name="f_data_l[5]" :id="f_data_l[5]"
                                            class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                                            placeholder="ex. 63" required="">
                                    </div>

                                    <div>
                                        <label :for="f_data_l[6]"
                                            class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">
                                            Your height
                                        </label>
                                        <input v-model="f_data_l[6]" type="number" :name="f_data_l[6]" :id="f_data_l[6]"
                                            class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                                            placeholder="ex. 176" required="">
                                    </div>
                                    <div>
                                        <label :for="f_data_l[7]"
                                            class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">
                                            Your goal weight
                                        </label>
                                        <input v-model="f_data_l[7]" type="number" :name="f_data_l[7]" :id="f_data_l[7]"
                                            class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                                            placeholder="ex. 68" required="">
                                    </div>

                                    <div>
                                        <label :for="f_data_l[8]"
                                            class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">
                                            Your goal date
                                        </label>
                                        <input v-model="f_data_l[8]" type="date" :name="f_data_l[8]" :id="f_data_l[8]"
                                            class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                                            placeholder="" required="">
                                    </div>
                                    <div>
                                        <label :for="f_data_l[9]"
                                            class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">
                                            minimum kcal
                                        </label>
                                        <input v-model="f_data_l[9]" type="number" :name="f_data_l[9]" :id="f_data_l[9]"
                                            class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                                            placeholder="ex. 2000" required="">
                                    </div>
                                    <!-- <div>
                            <label for="confirm-password"
                                class="block mb-2 text-sm font-medium text-gray-900 dark:text-white">Confirm
                                password</label>
                            <input type="confirm-password" name="confirm-password" id="confirm-password"
                                placeholder="••••••••"
                                class="bg-gray-50 border border-gray-300 text-gray-900 sm:text-sm rounded-lg focus:ring-primary-600 focus:border-primary-600 block w-full p-2.5 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white dark:focus:ring-blue-500 dark:focus:border-blue-500"
                                required="">
                        </div> -->
                                </div>
                            </div>
                            
                            <div class="flex items-start">
                                <div class="flex items-center h-5">
                                    <input id="terms" aria-describedby="terms" type="checkbox"
                                        class="w-4 h-4 border border-gray-300 rounded bg-gray-50 focus:ring-3 focus:ring-primary-300 dark:bg-gray-700 dark:border-gray-600 dark:focus:ring-primary-600 dark:ring-offset-gray-800"
                                        required="">
                                </div>
                                <div class="ml-3 text-sm">
                                    <label for="terms" class="font-light text-gray-500 dark:text-gray-300">I accept the
                                        <a class="font-medium text-primary-600 hover:underline dark:text-primary-500"
                                            href="#">Terms and Conditions</a></label>
                                </div>
                            </div>
                            <button type="submit"
                                class="w-full text-white bg-blue-500 hover:bg-primary-700 focus:ring-4 focus:outline-none focus:ring-primary-300 font-medium rounded-lg text-sm px-5 py-2.5 text-center dark:bg-primary-600 dark:hover:bg-primary-700 dark:focus:ring-primary-800">Create
                                an account</button>
                            <p class="text-sm font-light text-gray-500 dark:text-gray-400">
                                Already have an account? <a href="/"
                                    class="font-medium text-primary-600 hover:underline dark:text-primary-500">Login
                                    here</a>
                            </p>

                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>


import axios from 'axios';

export default {
    name: 'SignupView',
    components: {
    },
    data() {
        return {
            f_data_l: [],
            f_list: ['user_id', 'user_password', 'age', 'user_name', 'gender', 'current_weight', 'height', 'goal_weight', 'd_day', 'minimum_kcal'],
            f_cal_list: ['bmr', 'kcal_per_day', 'profile_path', 'setting']
        }
    },
    beforeMount() {
        var url = 'http://localhost:3001/api/session';
        axios.get(url, { withCredentials: true })
            .then(function (response) {
                // 성공 핸들링
                console.log(response);
                if(response.data != ""){
                    window.location.href = '/dashboard'
                }
            })
            .catch(function (error) {
                // 에러 핸들링
                console.log(error);
            })
            .then(function () {
                // 항상 실행되는 영역
            });
        
    },
    methods: {
        sendPost: function () {
            var port = window.location.host;
            var protocol = window.location.protocol;
            // var url = protocol + "//" + port + '/api/predict_all';
            var url = 'http://localhost:3001/api/signup';
            console.log(url);
            const frm = new FormData();
            var f_list = this.f_list;
            console.log(f_list);
            for (var step = 0; step < f_list.length; step++) {
                var to_in = this.f_data_l[step]
                frm.append(f_list[step], to_in);
            }

            for (var step = 0; step < this.f_cal_list.length; step++){
                var to_in = 1;
                frm.append(this.f_cal_list[step], to_in);
            }

            axios.post(url, frm, {
                headers: {
                    'Content-Type': 'multipart/form-data'
                }
            })
                .then((Response) => {
                    console.log(Response);
                    var r = protocol + "//" + port + Response.data
                    window.location.href = r;
                })
                .catch((error) => {
                    alert(error);
                    console.log(error);
                })


        }
    }

}
</script>
