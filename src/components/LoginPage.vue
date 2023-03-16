<template>
    <div>
        Username: <input type="text" v-model="loginInfo.userID" placeholder="Input username"/>
        <br><br>
        <button v-on:click="login">Login</button>
        <br><br>
        <p>{{ this.store.state.registrationId }}</p>
    </div>
</template>

<script>
import {useRouter} from "vue-router";
import router from "@/router";
import {useStore} from "vuex";

export default {
    setup() {
        const store = useStore()  // 该方法用于返回store 实例
        console.log(store)  // store 实例对象
        const router = useRouter();
        return {store};
    },
    name: "LoginPage",
    data() {
        return {
            loginInfo: {
                userID: '',
                registrationID: '233',
            },
            responseResult: [],
        }
    },
    methods: {
        login() {
            this.store.dispatch('registration', this.loginInfo.userID)
                .then(res => {
                    console.log(`res: ${res}`);
                    this.$http.post('/login', {
                        username: this.loginInfo.userID,
                        registrationID: res.registrationID,
                    }).then(successResponse => {
                        if (successResponse.data.code === 200) {
                            // router.replace('/chat')
                            if(res.code === 400){
                                router.replace('/chat');
                            }
                            if(res.code === 200) {
                                this.store.dispatch('send-keys-to-server')
                                    .then(res => {
                                        if (res === 'ok') {
                                            router.replace('/chat');
                                        }
                                    })
                            }
                        }
                    }).catch(failResponse => {
                    })
                });
            // this.loginInfo.registrationID = this.store.state.registrationID;
        }
    },
    computed: {
        // setRegistrationID() {
        //     this.loginInfo.registrationID = this.store.state.registrationID;
        // },
    }
}
</script>

<style></style>
