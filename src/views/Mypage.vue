<template>
  <div id="mypage">
    <v-row>
      <v-col cols="12">
        <tab-component :showHistory="true" :isMypage="false"></tab-component>
      </v-col>
      <v-col cols="12" class="d-flex flex-row" style="background: #1E1E1E">
        <div>
          <Avatar :email="userInfo.name" class="mb-2"/>
          <p class="text-caption grey--text text--lighten-1 mb-0">{{userInfo.belong}} / {{userInfo.role}}</p>
          <p class="text-h5 mb-0">
            {{ userInfo.name }}
            <span class="text-subtitle-1 yellow--text">(크레딧: {{userInfo.credits || 0 }})
            <i class="nes-icon coin is-small" style="position: relative; top: 3px;"></i>
            </span>
          </p>
          <p class="text-subtitle-2 grey--text mb-0">{{userInfo.email}}</p>
        </div>
      </v-col>
      <v-divider/>
      <v-col cols="12">
        <v-list dense>
          <v-list-item-group>
            <!--menu1-->
            <v-list class="pl-2">
              <v-list-item-content>
                <router-link :to="{name: 'UpdateProfilePage'}">
                  <v-list-item-icon class="float-left ma-0">
                    <v-icon v-text="'mdi-account '" class=" "></v-icon>
                    <v-list-item-title class="text-subtitle-2 float-left ml-3 "
                    >회원정보 수정
                    </v-list-item-title>
                  </v-list-item-icon>
                </router-link>
              </v-list-item-content>
            </v-list>
            <!--menu2-->
            <v-list class="pl-2">
              <v-list-item-content>
                <router-link :to="{name: 'MyHacksListPage'}" class="deep-purple&#45;&#45;text accent-1">
                  <v-list-item-icon class="float-left ma-0">
                    <v-icon v-text="'mdi-clock'" class=" "></v-icon>
                  </v-list-item-icon>
                  <v-list-item-title class="text-subtitle-2 float-left ml-3 ">참여한 해커톤 보기</v-list-item-title>
                </router-link>
              </v-list-item-content>
            </v-list>
            <!--menu3-->
            <v-list class="pl-2">
              <v-list-item-content>
                <a href="https://forms.gle/vykpMXa2sca3CoVA6" target="_blank"
                   class="deep-purple&#45;&#45;text accent-1">
                  <v-list-item-icon class="float-left ma-0">
                    <v-icon v-text="'mdi-flag'" class=" "></v-icon>
                  </v-list-item-icon>
                  <v-list-item-title class="text-subtitle-2 float-left ml-3 ">크레딧 환급 받기</v-list-item-title>
                </a>
              </v-list-item-content>
            </v-list>
            <!--menu4-->
            <v-list class="pl-2">
              <v-list-item-content @click="logout()">
                <router-link :to="{name: 'MyHacksListPage'}" class="deep-purple&#45;&#45;text accent-1">
                  <v-list-item-icon class="float-left ma-0">
                    <v-icon v-text="'mdi-exit-to-app'" class=" "></v-icon>
                  </v-list-item-icon>
                  <v-list-item-title class="text-subtitle-2 float-left ml-3 ">로그아웃</v-list-item-title>
                </router-link>
              </v-list-item-content>
            </v-list>
          </v-list-item-group>
        </v-list>
      </v-col>
      <router-link :to="{name: 'UpdateProfilePage'}">
      <p v-if="isPortfolio" class="isPortfolio pl-2 pr-5 pt-2 pb-2 text-subtitle-1 white--text">
        포트폴리오를 제출하고 프로필을 완성하세요 <span class="float-right d-inline-block deep-purple--text text--accent-1">제출하기</span>
      </p>
      </router-link>
    </v-row>
  </div>
</template>

<script>
import TabComponent from './hacks/TabComponent'
import Avatar from '../components/base/main/Avatar'

export default {
  name: 'Mypage',
  components: { Avatar, TabComponent },
  data: () => ({
    userInfo: null
  }),
  created () {
    this.userInfo = this.$store.state.userInfo
  },
  computed: {
    isPortfolio () {
      if (this.userInfo.portfolio === null || this.userInfo.portfolio === 'null') {
        return true
      } else {
        return false
      }
    }
  },
  methods: {
    getApplyList () {
      this.$http({
        method: 'GET',
        url: '/hacks/apply/'
      }).then(({ data }) => {
        console.log(data)
      })
        .catch(({ error }) => {
          console.log(error)
        })
    },
    logout () {
      localStorage.removeItem('userInfo')
      this.$router.replace('/signIn')
    }
  }
}
</script>
<style scoped lang="less">
  #mypage {
    .theme--dark.v-list.v-list--dense {
      background: transparent;
      margin: -8px;

      * {
        color: #BDBDBD;
      }
      .v-list{
        background: transparent;
        &:hover {
          background: rgba(187, 134, 252, 0.08);
          * {
            color: #B388FF;
          }
        }
      }
    }
    .isPortfolio{
      position: fixed;
      bottom: 0;
      left: 0;
      width: 100%;
      background: #121212;
    }
  }
</style>
