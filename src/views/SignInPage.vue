<template>
  <div id="signIn">
      <v-row>
        <!--title-->
        <v-col cols="12">
          <h1 class="text-h3 white--text mt-12" v-html="randomTitle"></h1>
        </v-col>
        <!--아이디 입력-->
        <v-col cols="12">
          <v-text-field
            v-model="email"
            label="이메일"
            filled
            hint="이메일을 입력하세요"
            color="deep-purple accent-1"
          ></v-text-field>

        </v-col>
        <!--비밀번호 입력-->
        <v-col cols="12">
          <v-text-field
            type="password"
            v-model="password"
            label="비밀번호"
            hint="비밀번호를 입력하세요"
            filled
            color="deep-purple accent-1"
          ></v-text-field>
        </v-col>
        <v-col cols="12">
          <v-btn block
                 @click="singIn"
          class="deep-purple accent-1">로그인
          </v-btn>
          <v-btn block outlined
                 class="mt-3"
                 color="deep-purple accent-1"
                 @click="singUp"
          >회원가입</v-btn>
        </v-col>
        <v-col cols="12">
          <v-alert type="error" v-if="nonUser">
            회원가입 후 이용해주세요.
          </v-alert>
        </v-col>
        <div class="signin_img">
          <img :src="randomImg" :alt="randomTitle">
        </div>
      </v-row>
  </div>
</template>

<script>
export default {
  name: 'SignInPage',
  data: () => ({
    email: null,
    password: null,
    nonUser: false,
    randomNumber: Math.floor(Math.random() * 3),
    userId: null
  }),
  computed: {
    randomTitle () {
      // const title = ['누구나 해커톤을<br>쉽고 재밌게 👾', '누구나 즐기는️<br>온라인 해커톤 🎮', '가슴 뛰는 해커톤들이<br>기다리고 있어요! 💻']
      // return title[this.randomNumber]
      // return title[0]
      return '쉽게 즐기는<br>온라인 해커톤🎮'
    },
    randomImg () {
      // this.randomNumber + 1
      return require('../assets/images/illust/illust_signin_1.svg')
    }

  },
  methods: {
    singIn () {
      this.$http({
        method: 'POST',
        url: '/accounts/sign-in/',
        data: {
          email: this.email,
          password: this.password
        }
      }).then((res) => {
        if (res.status === 200) {
          const data = res.data
          // console.log(data)
          this.nonUser = false
          this.userId = data.id
          /* 토큰정보 넣기 & 저장 */
          document.cookie = `accessToken=${data.token}`
          localStorage.setItem('token', data.token)
          delete data.token
          localStorage.setItem('userInfo', JSON.stringify(data))
          this.$store.commit('setUserInfo', data)
          this.$router.push('/hacks/list')
        } else {
          this.nonUser = true
        }
      })
        .catch(({ error }) => {
          this.nonUser = true
          console.log(error)
        })
        .then(() => {
          this.getAccountsProfile()
        })
    },
    getAccountsProfile () {
      this.$http.get(`/accounts/profile/${this.userId}`)
        .then(({ data }) => {
          // console.log(data)
          let userInfo = JSON.parse(localStorage.getItem('userInfo'))
          userInfo = { ...userInfo, ...data }
          localStorage.setItem('userInfo', JSON.stringify(userInfo))
          this.$store.commit('setUserInfo', userInfo)
        })
        .catch(error => {
          console.log(error)
        })
    },
    singUp () {
      this.$router.push('/signUp')
    }
  }
}
</script>

<style lang="less" scoped>
 #signIn {
   .v-text-field > .v-input__control > .v-input__slot:after {
     color: #BB86FC;
   }
   .signin_img{
     position: relative;
     bottom: 0;
     left: 50%;
     transform: translateX(-50%);
     img{
       display: block;
     }
   }
 }
</style>
