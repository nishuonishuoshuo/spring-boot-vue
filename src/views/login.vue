<style lang="less">
    @import './login.less';
</style>

<template>
    <div class="login" @keydown.enter="handleSubmit">
        <div class="login-con">
            <Card :bordered="false">
                <p slot="title">
                    <Icon type="log-in"></Icon>
                    欢迎登录
                </p>
                <div class="form-con">
                    <Form ref="loginForm" :model="form" :rules="rules">
                        <FormItem prop="userName">
                            <Input v-model="form.userName" placeholder="请输入用户名">
                            <span slot="prepend">
                                    <Icon :size="16" type="person"></Icon>
                                </span>
                            </Input>
                        </FormItem>
                        <FormItem prop="password">
                            <Input type="password" v-model="form.password" placeholder="请输入密码">
                            <span slot="prepend">
                                    <Icon :size="16" type="locked"></Icon>
                                </span>
                            </Input>
                        </FormItem>
                        <FormItem>
                            <Button @click="handleSubmit" type="primary" long>登录</Button>
                        </FormItem>
                    </Form>
                    <p class="login-tip">输入任意用户名和密码即可</p>
                </div>
            </Card>
        </div>
        <div class="image-bg"><img src="../images/login-bg2.jpg" /></div>
    </div>
</template>

<script>
    import Cookies from 'js-cookie';
    import util from '@/libs/util';
    import Qs from 'qs';

    export default {
        data() {
            return {
                form: {
                    userName: 'xx',
                    password: ''
                },
                rules: {
                    userName: [
                        {required: true, message: '账号不能为空', trigger: 'blur'}
                    ],
                    password: [
                        {required: true, message: '密码不能为空', trigger: 'blur'}
                    ]
                },
                backgrounds:{
                    backgroundImage: 'url('+require('../images/login-bg.jpg')+')',
                    backgroundRepeat: 'no-repeat',
                    backgroundSize: '100% 100%'
                }
            };
        },
        methods: {
            handleSubmit() {
                this.$refs.loginForm.validate((valid) => {
                    console.log(valid)
                    if (valid) {
//                        var params = new URLSearchParams();
//                        params.append('data',this.form);
//                        console.log(params.toString());
                        util.ajax.post('/login',Qs.stringify(this.form)).then(res => {
                            if (res.data == 'success') {
                                Cookies.set('user', this.form.userName);
                                this.$store.commit('setAvator', 'http://pic.3h3.com/up/2012-12/2012121227271453316926.jpg');
                                if (this.form.userName === 'xx') {
                                    Cookies.set('access', 0);
                                } else {
                                    Cookies.set('access', 1);
                                }
                                this.$router.push({
                                    name: 'home_index'
                                });
                            } else {
                                this.$router.push({
                                    name: 'login'
                                });
                            }
                        }).catch(res => {
                            console.log(res);
                        });
                    }
                });
            }
        }
    };
</script>

<style>

</style>
