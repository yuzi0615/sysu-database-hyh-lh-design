<template>
    <div class="container">
        <div class="login_box" v-show="target == 1">
            <div class="head">
                <h2>中山大学深圳校区东园食堂外卖系统</h2>
                <p class="sub-title">欢迎回来，请登录您的账户</p>
            </div>
            <!-- 登录 -->
            <div class="form-container">
                <el-form label-width="0" class="login_form" :model="login_form" :rules="login_rules" ref="login_form">
                    <!-- 用户名 -->
                    <el-form-item prop="userortel" class="form-item">
                        <el-input 
                            v-model="login_form.userortel" 
                            spellcheck="false" 
                            placeholder="手机号"
                            class="custom-input"
                            prefix-icon="el-icon-phone"
                        >
                        </el-input>
                    </el-form-item>
                    <!-- 密码 -->
                    <el-form-item prop="password" class="form-item">
                        <el-input 
                            v-model="login_form.password" 
                            show-password 
                            spellcheck="false" 
                            placeholder="密码"
                            class="custom-input"
                            prefix-icon="el-icon-lock"
                        >
                        </el-input>
                    </el-form-item>


                    <!-- 按钮 -->
                    <el-form-item class="btns">
                        <el-button type="primary" @click="llogin()" class="login-btn">登录</el-button>
                    </el-form-item>

                </el-form>
                <div>
                    <div class="operate">
                        <span id="op1" @click="change(2)" class="operate-link">注册</span>
                        <span id="op2" @click="change(3)" class="operate-link">忘记密码</span>
                    </div>
                </div>
            </div>
        </div>


        <!-- 注册表单 -->
        <div class="reg_box" v-show="target == 2">
            <div class="head">
                <h2>中山大学深圳校区东园食堂外卖系统</h2>
                <p class="sub-title">创建新账户，开始您的美食之旅</p>
            </div>
            <div class="form-container">
                <el-form class="reg_form" :model="reg_form" :rules="reg_rules" ref="reg_form">
                    <!-- 用户名 -->
                    <el-form-item prop="username" class="form-item">
                        <el-input 
                            v-model="reg_form.username" 
                            spellcheck="false"
                            placeholder="用户名"
                            class="custom-input"
                            prefix-icon="el-icon-user"
                        >
                        </el-input>
                    </el-form-item>
                    <!-- 密码 -->
                    <el-form-item prop="password" class="form-item">
                        <el-input 
                            v-model="reg_form.password" 
                            show-password
                            spellcheck="false" 
                            placeholder="密码(包含大小写字母、数字，长度在6-12之间)"
                            class="custom-input"
                            prefix-icon="el-icon-lock"
                        >
                        </el-input>
                    </el-form-item>

                    <el-form-item prop="telephone" class="form-item">
                        <el-input 
                            v-model="reg_form.telephone" 
                            spellcheck="false"
                            placeholder="手机号码"
                            class="custom-input"
                            prefix-icon="el-icon-phone"
                        >
                        </el-input>
                    </el-form-item>

                    <el-form-item prop="vercode" class="form-item">
                        <div class="vercode-container">
                            <el-input 
                                v-model="reg_form.vercode" 
                                spellcheck="false" 
                                placeholder="验证码" 
                                class="custom-input vercode-input"
                                prefix-icon="el-icon-message"
                            >
                            </el-input>
                            <span 
                                class="vercode-btn" 
                                @click="send_vercode_pre()"
                                v-show="getcode_show"
                            >
                                获取验证码
                            </span>
                            <span 
                                class="vercode-btn disabled" 
                                v-show="!getcode_show"
                            >
                                {{ time_count }}s后重新获取
                            </span>
                        </div>
                    </el-form-item>
                    <!-- 按钮 -->
                    <el-form-item class="btns">
                        <el-button type="primary" @click="zhuce()" class="login-btn">注册</el-button>
                    </el-form-item>

                </el-form>
                <div>
                    <div class="back-to-login">
                        <span @click="change(1)" class="operate-link">返回登录</span>
                    </div>
                </div>
            </div>
        </div>
<!-- 找回密码 -->
        <div class="forget_box" v-show="target == 3">
            <div class="head">
                <h2>中山大学深圳校区东园食堂外卖系统</h2>
                <p class="sub-title">重置您的密码</p>
            </div>
            <div class="form-container">
                <el-form class="reg_form" :model="findback_form" :rules="findback_rules" ref="findback_form">
                    <el-form-item prop="telephone" class="form-item">
                        <el-input 
                            v-model="reg_form.telephone" 
                            spellcheck="false"
                            placeholder="手机号码"
                            class="custom-input"
                            prefix-icon="el-icon-phone"
                        >
                        </el-input>
                    </el-form-item>
                    <!-- 密码 -->
                    <el-form-item prop="password" class="form-item">
                        <el-input 
                            v-model="findback_form.password" 
                            show-password
                            spellcheck="false" 
                            placeholder="新密码"
                            class="custom-input"
                            prefix-icon="el-icon-lock"
                        >
                        </el-input>
                    </el-form-item>

                    <el-form-item prop="vercode" class="form-item">
                        <div class="vercode-container">
                            <el-input 
                                v-model="reg_form.vercode" 
                                spellcheck="false" 
                                placeholder="验证码" 
                                class="custom-input vercode-input"
                                prefix-icon="el-icon-message"
                            >
                            </el-input>
                            <span 
                                class="vercode-btn" 
                                @click="send_vercode_pre()"
                                v-show="getcode_show"
                            >
                                获取验证码
                            </span>
                            <span 
                                class="vercode-btn disabled" 
                                v-show="!getcode_show"
                            >
                                {{ time_count }}s后重新获取
                            </span>
                        </div>
                    </el-form-item>
                    <!-- 按钮 -->
                    <el-form-item class="btns">
                        <el-button type="primary" @click="findback()" class="login-btn">确认修改</el-button>
                    </el-form-item>

                </el-form>
                <div>
                    <div class="back-to-login">
                        <span @click="change(1)" class="operate-link">返回登录</span>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'MyLogin',
    data() {
        var checkPassword = (rule, value, cb) => {
            const regPassword = /(?=.*[A-Z])(?=.*[a-z])(?=.*[0-9]).{6,12}$/;
            if (regPassword.test(value)) {
                // 合法的手机号码
                return cb()
            }
            cb(new Error('包含大写字母、小写字母、数字，长度在6-12位之间'))
        };
        var checkMobile = (rule, value, cb) => {
            const regMobile = /^(0|86|17951)?(13[0-9]|15[012356789]|17[678]|18[0-9]|14[57])[0-9]{8}$/;
            if (regMobile.test(value)) {
                // 合法的手机号码
                return cb()
            }
            cb(new Error('手机号码格式不正确'))
        };
        return {
            getcode_show: true,
            time_count: '',
            timer: null,
            target: 1,
            login_form: {
                userortel: '',
                password: '',
            },
            reg_form: {
                username: '',
                password: '',
                telephone: '',
                vercode: ''
            },
            findback_form: {
                telephone: '',
                password: '',
                vercode:'',
            },
            login_rules: {
                userortel: [
                    { required: true, message: '请输入电话', trigger: 'blur' }, { validator: checkMobile, trigger: 'blur' }],
                password: [
                    { required: true, message: '请输入密码', trigger: 'blur' }]
            },
            reg_rules: {
                username: [{ required: true, message: '请设置用户名', trigger: 'blur' }],
                password: [{ required: true, message: '请设置密码', trigger: 'blur' }, { validator: checkPassword, trigger: 'blur' }],
                telephone: [{ required: true, message: '请绑定手机号', trigger: 'blur' }, { validator: checkMobile, trigger: 'blur' }]
            },
            findback_rules: {
                telephone: [
                    { required: true, message: '请输入电话', trigger: 'blur' }, { validator: checkMobile, trigger: 'blur' }],
                password: [
                    { required: true, message: '请输入密码', trigger: 'blur' }],
                vercode: [
                    { required: true, message: '请输入验证码', trigger: 'blur' }]
            },
        }
    },
    methods: {
        findback(){
            this.$refs.findback_form.validate(valid => {
                if (!valid)
                    return;
                else if(this.findback_form.vercode=='')
                    return;
                else{
                    console.log('找回密码', this.findback_form);
                    // 这里可以添加找回密码的API调用
                }
            })
        },
        zhuce(){
            this.$refs.reg_form.validate(valid => {
                if (!valid)
                    return;
                else{
                    if(this.reg_form.vercode=='')
                        return;
                    else{
                        this.$axios.request({
                            method:'POST',
                            url:'/api/user/register/test',
                            data:{
                                username:this.reg_form.username,
                                password:this.reg_form.password,
                                vercode:this.reg_form.vercode,
                                telephone:this.reg_form.telephone
                            }
                        }).then((res)=>{
                            // console.log(res.status);
                            if(res.data.status==200)
                            {
                                this.$message({
                                message: '注册成功',
                                type: 'success'
                                })
                            this.target = 1;
                            // 页面变为登录页面
                            }else{
                                this.$message({
                                message: res.data.msg,
                                type: 'error'
                                })
                            
                            }
                            
                        })
                
                    }
                }
            })
        },
        change(id) {
            this.target = id;
        },
        llogin() {
            this.$refs.login_form.validate(valid => {
                if (!valid)
                    return;
                else //验证通过再发送请求
                    this.login();
            })


        },
        async login() {

            this.$axios.post("/api/user/login", this.login_form).then((res) => {
                console.log(res.status);
                //200登录成功
                if (res.data.code != 200) {
                    return this.$message({
                        message: res.data.msg,
                        type: 'error '
                    })
                } else {
                    this.$message({
                        message: '登录成功',
                        type: 'success'
                    })

                    window.localStorage.setItem("token", res.data.token);

                    if (res.data.role == 0)
                        this.$router.push('/user')
                    else
                        this.$router.push('/manage')
                }
            }).catch(() => {
                // console.log(res.response.data);
                this.$message({
                    message: "网络故障",
                    type: 'error'
                })
            })

        },

        // 获取验证码
        send_vercode_pre() {
            this.$refs.reg_form.validate(valid => {
                if (!valid) {
                    return;
                }
                else {
                    this.send_vercode();
                    this.set_interval();
                }
            })
        },
        send_vercode() {
            this.$axios.request({
                method: 'POST',
                url: "/api/user/register/send_sms",
                data: {
                    telephone: this.reg_form.telephone
                }
            }).then(() => {
                
                this.$message({
                        message: '验证码发送成功',
                        type: 'success'
                    })

                
                    
                
            })
        },
        set_interval() {
            const TIME_COUNT = 60;
            if (!this.timer) {
                this.time_count = TIME_COUNT;
                this.getcode_show = false;
                this.timer = setInterval(() => {
                    if (this.time_count > 0 && this.time_count <= TIME_COUNT) {
                        this.time_count--;
                    } else {
                        this.getcode_show = true;
                        clearInterval(this.timer);
                        this.timer = null;
                    }
                }, 1000);
            }
        },
        // 获取验证码（找回密码）
        send_vercode_findback_pre() {
            this.$refs.findback_form.validate(valid => {
                if (!valid) {
                    return;
                }
                else {
                    this.send_vercode_findback();
                    this.set_interval();
                }
            })
        },
        send_vercode_findback() {
            this.$axios.request({
                method: 'POST',
                url: "/api/user/register/send_sms",
                data: {
                    telephone: this.findback_form.telephone
                }
            }).then(() => {
                
                this.$message({
                        message: '验证码发送成功',
                        type: 'success'
                    })

                
                    
                
            })
        },

    }
}
</script>

<style lang="less" scoped>
.container {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    height: 100vh;
    width: 100vw;
    display: flex;
    align-items: center;
    justify-content: center;
    overflow: hidden;
}

// 卡片样式
.login_box, .reg_box, .forget_box {
    background-color: white;
    border-radius: 12px;
    box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
    width: 450px;
    transition: all 0.3s ease;
    
    &:hover {
        box-shadow: 0 15px 35px rgba(0, 0, 0, 0.15);
        transform: translateY(-5px);
    }
}

.login_box {
    height: 380px;
}

.reg_box {
    height: 490px;
}

.forget_box {
    height: 450px;
}

// 头部样式
.head {
    text-align: center;
    padding: 30px 0 10px;
    
    h2 {
        color: #333;
        font-size: 24px;
        font-weight: 600;
        margin: 0 0 8px;
    }
    
    .sub-title {
        color: #666;
        font-size: 14px;
        margin: 0;
    }
}

// 表单容器
.form-container {
    padding: 0 50px 30px;
}

// 表单项目
.form-item {
    margin-bottom: 20px;
    width: 100%;
    margin-left: 0;
}

// 自定义输入框
.custom-input {
    border-radius: 8px;
    border: 1px solid #e5e5e5;
    transition: all 0.3s ease;
    height: 48px;
    font-size: 14px;
    
    &:focus {
        border-color: #667eea;
        box-shadow: 0 0 0 2px rgba(102, 126, 234, 0.2);
    }
}

// 验证码容器
.vercode-container {
    display: flex;
    align-items: center;
    gap: 10px;
}

.vercode-input {
    flex: 1;
}

.vercode-btn {
    background-color: #667eea;
    color: white;
    padding: 0 16px;
    height: 48px;
    border-radius: 8px;
    display: flex;
    align-items: center;
    justify-content: center;
    cursor: pointer;
    font-size: 14px;
    transition: all 0.3s ease;
    white-space: nowrap;
    
    &:hover {
        background-color: #5a6fd8;
        transform: translateY(-1px);
    }
    
    &.disabled {
        background-color: #e5e5e5;
        color: #999;
        cursor: not-allowed;
        
        &:hover {
            background-color: #e5e5e5;
            transform: none;
        }
    }
}

// 按钮样式
.btns {
    text-align: center;
    margin-top: 30px;
    margin-left: 0;
    width: 100%;
}

.login-btn {
    width: 100%;
    height: 48px;
    border-radius: 8px;
    font-size: 16px;
    font-weight: 500;
    background-color: #667eea;
    border-color: #667eea;
    transition: all 0.3s ease;
    
    &:hover {
        background-color: #5a6fd8;
        border-color: #5a6fd8;
        transform: translateY(-1px);
    }
    
    &:focus {
        box-shadow: 0 0 0 2px rgba(102, 126, 234, 0.4);
    }
}

// 操作链接
.operate {
    text-align: center;
    margin-top: 20px;
    
    .operate-link {
        color: #667eea;
        font-size: 14px;
        cursor: pointer;
        transition: color 0.3s ease;
        padding: 0 15px;
        
        &:hover {
            color: #5a6fd8;
            text-decoration: underline;
        }
        
        &:first-child {
            border-right: 1px solid #e5e5e5;
        }
    }
}

// 返回登录链接
.back-to-login {
    text-align: center;
    margin-top: 20px;
    
    .operate-link {
        color: #667eea;
        font-size: 14px;
        cursor: pointer;
        transition: color 0.3s ease;
        
        &:hover {
            color: #5a6fd8;
            text-decoration: underline;
        }
    }
}

// 响应式设计
@media (max-width: 500px) {
    .login_box, .reg_box, .forget_box {
        width: 90%;
        margin: 0 10px;
    }
    
    .form-container {
        padding: 0 30px 20px;
    }
}
</style>