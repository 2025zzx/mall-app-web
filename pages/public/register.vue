 <template>
    <view class="container">
        <!-- 左下角装饰图案 -->
        <view class="left-bottom-sign"></view>
        <!-- 返回按钮 -->
        <view class="back-btn yticon icon-zuojiantou-up" @click="navBack"></view>
        <!-- 右上角装饰图案 -->
        <view class="right-top-sign"></view>
        <!-- 设置白色背景防止软键盘把下部绝对定位元素顶上来盖住输入框等 -->
        <view class="wrapper">
            <!-- 注册标题 -->
            <view class="left-top-sign">REGISTER</view>
            <!-- 欢迎提示 -->
            <view class="welcome">
                欢迎加入！
            </view>
            <!-- 输入框内容 -->
            <view class="input-content">
                <!-- 用户名输入框 -->
                <view class="input-item">
                    <text class="tit">用户名</text>
                    <input type="text" v-model="username" placeholder="请输入用户名" maxlength="11"/>
                </view>
                <!-- 密码输入框 -->
                <view class="input-item">
                    <text class="tit">密码</text>
                    <input type="password" v-model="password" placeholder="请输入密码" maxlength="20"/>
                </view>
                <!-- 确认密码输入框 -->
                <view class="input-item">
                    <text class="tit">确认密码</text>
                    <input type="password" v-model="confirmPassword" placeholder="请确认密码" maxlength="20"/>
                </view>
                <!-- 手机号输入框 -->
                <view class="input-item">
                    <text class="tit">手机号</text>
                    <input type="tel" v-model="telephone" placeholder="请输入手机号" maxlength="11"/>
                </view>
                <!-- 验证码输入框 -->
                <view class="input-item">
                    <text class="tit">验证码</text>
                    <input type="text" v-model="authCode" placeholder="请输入验证码" maxlength="6"/>
                </view>
            </view>
            <!-- 注册按钮 -->
            <button class="confirm-btn" @click="register" :disabled="registering">注册</button>
            <!-- 忘记密码 -->
            <view class="forget-section" @click="toLogin">
                返回登录页
            </view>
        </view>
    </view>
</template>

<script>
    import { mapMutations } from 'vuex';
    import { memberRegister } from '@/api/member.js';

    export default {
        data() {
            return {
                username: '', // 清空用户名
                password: '', // 清空密码
                confirmPassword: '', // 添加确认密码字段并清空
                telephone: '', // 添加手机号字段并清空
                authCode: '', // 添加验证码字段并清空
                registering: false // 添加注册状态标识字段
            }
        },
        methods: {
            ...mapMutations(['login']),
            // 返回上一页
            navBack() {
                uni.navigateBack();
            },
            // 跳转到登录页面
            toLogin() {
                uni.navigateBack();
            },
            // 注册按钮点击事件处理函数
            register() {
                // 如果正在注册中，则不响应
                if (this.registering) return;

                // 校验用户名、密码、手机号和验证码
                if (!this.username.trim() || !this.password.trim() || !this.confirmPassword.trim() || !this.telephone.trim() || !this.authCode.trim()) {
                    uni.showToast({
                        title: '请输入用户名、密码、手机号和验证码',
                        icon: 'none'
                    });
                    return;
                }

                if (this.password !== this.confirmPassword) {
                    uni.showToast({
                        title: '两次输入的密码不一致',
                        icon: 'none'
                    });
                    return;
                }

                // 开始注册
                this.registering = true;
                // 调用注册接口
                memberRegister({
                    username: this.username,
                    password: this.password,
                    telephone: this.telephone,
                    authCode: this.authCode
                }).then(response => {
                    // 注册成功后的处理
                    uni.showToast({
                        title: '注册成功',
                        icon: 'success'
                    });
                    // 清空表单
                    this.username = '';
                    this.password = '';
                    this.confirmPassword = '';
                    this.telephone = '';
                    this.authCode = '';
                    // 返回登录页面
                    uni.navigateBack();
                }).catch(error => {
                    // 注册失败处理
                    uni.showToast({
                        title: '注册失败，请重试',
                        icon: 'none'
                    });
                }).finally(() => {
                    // 注册结束
                    this.registering = false;
                });
            }
        }
    }
</script>  


<style lang='scss'>
.page {
	background: #fff;
}

.container {
	padding-top: 115px;
	position: relative;
	width: 100vw;
	height: 100vh;
	overflow: hidden;
	background: #fff;
}

.wrapper {
	position: relative;
	z-index: 90;
	background: #fff;
	padding-bottom: 40upx;
}

.back-btn {
	position: absolute;
	left: 40upx;
	z-index: 9999;
	padding-top: var(--status-bar-height);
	top: 40upx;
	font-size: 40upx;
	color: $font-color-dark;
}

.left-top-sign {
	font-size: 120upx;
	color: $page-color-base;
	position: relative;
	left: -16upx;
}

.right-top-sign {
	position: absolute;
	top: 80upx;
	right: -30upx;
	z-index: 95;

	&:before,
	&:after {
		display: block;
		content: "";
		width: 400upx;
		height: 80upx;
		background: #b4f3e2;
	}

	&:before {
		transform: rotate(50deg);
		border-radius: 0 50px 0 0;
	}

	&:after {
		position: absolute;
		right: -198upx;
		top: 0;
		transform: rotate(-50deg);
		border-radius: 50px 0 0 0;
	}
}

.left-bottom-sign {
	position: absolute;
	left: -270upx;
	bottom: -320upx;
	border: 100upx solid #d0d1fd;
	border-radius: 50%;
	padding: 180upx;
}

.welcome {
	position: relative;
	left: 50upx;
	top: -90upx;
	font-size: 46upx;
	color: #555;
	text-shadow: 1px 0px 1px rgba(0, 0, 0, .3);
}

.input-content {
	padding: 0 60upx;
}

.input-item {
	display: flex;
	flex-direction: column;
	align-items: flex-start;
	justify-content: center;
	padding: 0 30upx;
	background: $page-color-light;
	height: 120upx;
	border-radius: 4px;
	margin-bottom: 50upx;

	&:last-child {
		margin-bottom: 0;
	}

	.tit {
		height: 50upx;
		line-height: 56upx;
		font-size: $font-sm+2upx;
		color: $font-color-base;
	}

	input {
		height: 60upx;
		font-size: $font-base + 2upx;
		color: $font-color-dark;
		width: 100%;
	}
}

.confirm-btn {
	width: 630upx;
	height: 76upx;
	line-height: 76upx;
	border-radius: 50px;
	margin-top: 70upx;
	background: $uni-color-primary;
	color: #fff;
	font-size: $font-lg;

	&:after {
		border-radius: 100px;
	}
}

.forget-section {
	font-size: $font-sm+2upx;
	color: $font-color-spec;
	text-align: center;
	margin-top: 40upx;
}
</style>
