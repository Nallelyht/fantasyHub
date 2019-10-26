<template>
  <eh-layout>
    <el-container direction="vertical">
      <el-row type="flex" class="row-bg" justify="center" :gutter="60" v-if="community">
        <el-col :sm="8" :xs="24" :md="8" :lg="8" :xl="8" class="show-xs-only">
          <el-card :body-style="{ padding: '0px' }">
            <img
              :src="community.image"
              :alt="community.name"
              :title="community.name"
              class="show-xs-only img"
              width="100%"
            />
            <div style="padding: 14px;">
              <div class="el-card__info">
                <span>Meta:</span>
                <span>{{formatPrice(community.project.goal)}} €</span>
              </div>
              <hr />
              <div class="el-card__info">
                <span>Interés anual en moneda local:</span>
                <span>{{community.project.interest}}%</span>
              </div>
              <hr />
              <div class="el-card__info">
                <span>Faltan:</span>
                <span>{{formatPrice(community.project.goal - community.project.contributed)}} €</span>
              </div>
              <div>
                <el-progress
                  :stroke-width="16"
                  :percentage="(community.project.contributed*100)/community.project.goal"
                  :show-text="false"
                ></el-progress>
              </div>
              <div class="el-card__contributed">
                <el-button type="text" class="button" @click="outerVisible = true">CONTRIBUIR</el-button>
              </div>
            </div>
          </el-card>
        </el-col>
      </el-row>
      <el-row type="flex" class="row-bg" justify="center" :gutter="60" v-if="community">
        <el-col :sm="12" :xs="24" :md="12" :lg="12" :xl="12">
          <img
            :src="community.image"
            :alt="community.name"
            :title="community.name"
            class="hidden-xs-only img"
            width="100%"
          />
          <p class="info_title">{{community.location}} : {{community.project.name}}</p>
          <p class="info_description">{{community.project.description}}</p>
          <p class="info_title">Comunidad de {{community.location}}</p>
          <p class="info_description">{{community.description}}</p>
          <img src="../assets/images/Map.jpg" alt="map" title="map" width="100%" />
        </el-col>
        <el-col :sm="8" :xs="24" :md="8" :lg="8" :xl="8" class="hidden-xs-only">
          <el-card :body-style="{ padding: '0px' }">
            <div style="padding: 14px;">
              <div class="el-card__info">
                <span>Meta:</span>
                <span>{{formatPrice(community.project.goal)}} €</span>
              </div>
              <hr />
              <div class="el-card__info">
                <span>Interés anual en moneda local:</span>
                <span>{{community.project.interest}}%</span>
              </div>
              <hr />
              <div class="el-card__info">
                <span>Faltan:</span>
                <span>{{formatPrice(community.project.goal - community.project.contributed)}} €</span>
              </div>
              <div>
                <el-progress
                  :stroke-width="16"
                  :percentage="(community.project.contributed*100)/community.project.goal"
                  :show-text="false"
                ></el-progress>
              </div>
              <div class="el-card__contributed">
                <el-button type="text" class="button" @click="outerVisible = true">CONTRIBUIR</el-button>
              </div>
            </div>
          </el-card>
        </el-col>
      </el-row>
      <el-dialog :visible.sync="outerVisible" center custom-class="dialog">
        <p class="dialog-title">Selecciona tu método de contribución</p>
        <div class="container-radios">
          <label class="container-radios__label" for="ether">
            <span>
              <input type="radio" checked="checked" name="ether" :value="ether" v-model="radio" />
              Ether
            </span>
            <img :src="require('../assets/images/'+ether)" alt="ether_coin" width="31px" />
            <span class="checkmark"></span>
          </label>
          <label class="container-radios__label" for="card">
            <span>
              <input type="radio" name="card" v-model="radio" :value="tarjeta" />
              Tarjeta
            </span>
            <img :src="require('../assets/images/'+tarjeta)" alt="credit_card" width="31px" />
            <span class="checkmark"></span>
          </label>
        </div>
        <el-dialog :visible.sync="innerVisible" append-to-body center>
          <p class="dialog-title">
            Antes de procesar tu pago
            <br />confirma la candidad que deseas invertir
          </p>
          <div class="contributed-info">
            <div>
              <small>CANTIDAD A INVERTIR</small>
              <span>{{contributed}} €</span>
            </div>
            <div>
              <small>MÉTODO SELECCIONADO</small>
              <img
                :src="require('../assets/images/' + radio)"
                :alt="radio"
                v-if="radio"
                width="52px"
              />
            </div>
          </div>
          <div slot="footer" class="dialog-footer">
            <el-button @click="innerVisible = false" class="back">MODIFICAR</el-button>
            <el-button @click="innerVisible = true; postContributed(community.id, contributed)">OK</el-button>
          </div>
        </el-dialog>
        <div slot="footer" class="dialog-footer">
          <el-button
            type="text"
            class="dialog-footer__button"
            @click="innerVisible = true"
            :disabled="isDisabled"
          >CONTRIBUIR</el-button>
        </div>
      </el-dialog>
    </el-container>
  </eh-layout>
</template>

<script>
import BaseLayout from '@/layouts/BaseLayout.vue'
import axios from 'axios'
/**
 * Here goes the detail cards
 */
export default {
  name: 'CommunityDetail',
  data () {
    return {
      community: null,
      contributed: null,
      outerVisible: false,
      innerVisible: false,
      radio: '',
      tarjeta: 'credit_card.jpeg',
      ether: 'ether_coin.jpeg'
    }
  },
  computed: {
    isDisabled () {
      console.log(this.radio)
      if (this.radio) {
        return false
      }
      return true
    }
  },
  methods: {
    getCommunity (communityID) {
      axios
        .get('https://fantasyhub.herokuapp.com/communities/' + communityID)
        .then(res => {
          if (res.data) {
            this.community = res.data
          }
        })
    },
    postContributed (communityID, contributed) {
      axios
        .post('https://fantasyhub.herokuapp.com/communities/' + communityID, {
          amount: contributed
        })
        .then(res => {
          console.log(res)
          this.outerVisible = false
          this.innerVisible = false
          this.$router.push('/')
        })
        .catch(error => {
          console.log(error)
        })
    },
    formatPrice (value) {
      let val = (value / 1).toFixed(0).replace('.', ',')
      return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, '.')
    }
  },
  mounted () {
    const communityID = this.$route.params.communityID
    this.contributed = this.$route.params.value
    if (!this.contributed || this.contributed === '25 €') {
      this.contributed = '25'
    }
    this.getCommunity(communityID)
  },
  components: {
    'eh-layout': BaseLayout
  }
}
</script>

<style lang="scss" scoped>
@import "~/assets/scss/element-variables.scss";
$color-green: #87f96e;
$color-blue: #062f4f;
@mixin green-button {
  background: $color-green;
  border: 1px solid rgba(6, 47, 79, 0.1);
  border-radius: 4px;
  display: flex;
  justify-content: space-around;
  align-items: center;
  flex-direction: row;
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.2);
  color: $color-blue;
}
.img {
  border-radius: 6px;
}
.info {
  &_title {
    font-size: 24px;
    color: $color-blue;
    letter-spacing: -0.27px;
    line-height: 56px;
  }
  &_description {
    font-family: Helvetica;
    font-size: 16px;
    color: $color-blue;
    line-height: 28px;
  }
}
.el-card {
  hr {
    border: 1px solid #e6eae5;
  }
  &__info {
    display: flex;
    justify-content: space-between;
    font-size: 18px;
    color: $color-blue;
    line-height: 24px;
    span:last-child {
      font-weight: bold;
    }
  }
  &__contributed {
    height: 52px;
    margin: 20px auto;
    @include green-button;
    .el-button--text {
      font-size: 19.2px;
      letter-spacing: 1.58px;
      text-align: center;
      line-height: 19.2px;
      font-weight: bold;
      color: #062f4f;
    }
  }
}
.dialog {
  .container-radios {
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
    &__label {
      display: flex;
      justify-content: space-between;
      align-items: center;
      width: 30%;
      padding: 10px;
      min-width: 150px;
    }
  }
  .el-dialog__body {
    display: flex;
    justify-content: center;
    flex-direction: column;
    align-items: center;
    min-width: 300px;
  }
  &-title {
    font-size: 16px;
    color: $color-blue;
    letter-spacing: -0.19px;
    text-align: center;
    line-height: 31.2px;
    word-break: break-word;
  }
  &-footer {
    display: flex;
    justify-content: center;
    width: 70%;
    margin: 0 auto;
    button {
      @include green-button;
      width: 60%;
    }
    &__button {
      font-size: 14px;
      color: $color-blue;
      letter-spacing: 1.15px;
      text-align: center;
      line-height: 19.2px;
      font-weight: bold;
      min-width: 150px;
    }
    .back {
      background: #ffffff;
      border: 1px solid rgba(6, 47, 79, 0.75);
      border-radius: 4.8px;
      font-size: 14px;
      color: $color-blue;
      letter-spacing: 1.15px;
      text-align: center;
      line-height: 19.2px;
      margin-right: 40px;
    }
  }
}
.contributed-info {
  max-width: 260px;
  margin: 0 auto;
  div {
    display: flex;
    justify-content: space-between;
    margin-top: 20px;
    min-width: 200px;
    span {
      font-size: 22px;
      color: $color-blue;
      letter-spacing: 0.41px;
      line-height: 22.4px;
      word-break: break-word;
    }
    small {
      font-size: 12px;
      color: $color-blue;
      letter-spacing: 0.23px;
      text-align: right;
      line-height: 22.4px;
      display: flex;
      align-items: center;
      word-break: break-word;
    }
  }
}
.show-xs-only {
  display: none;
}
@media only screen and (max-width: 767px) {
  .show-xs-only {
    display: block;
  }
  .contributed-info {
    div {
      flex-direction: column;
      align-items: flex-start;
    }
  }
  .dialog-footer {
    .back {
      margin-right: 10px;
      min-width: 93px;
    }
  }
}
</style>
