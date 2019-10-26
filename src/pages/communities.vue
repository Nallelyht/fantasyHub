<template>
  <eh-layout>
    <el-container direction="vertical">
      <div class="el-col el-col-24 el-col-xs-24">
        <h1 class="main-title">
          <p>
            <strong>Cosechamos</strong> riqueza.
          </p>
          <p>
            <strong>Conectamos</strong> personas.
          </p>
        </h1>

        <div class="main-subtitle el-col-xs-24">
          <p>
            Inversiones
            <strong>desde 50€</strong>
            en comunidades agrícolas productivas y rentables de todo el mundo sed do eiusmod
          </p>
        </div>
      </div>
      <div id="cards" v-if="cards">
        <Cards :cards="cards" :totalCards="totalCards"></Cards>
      </div>
    </el-container>
  </eh-layout>
</template>

<script>
import BaseLayout from '@/layouts/BaseLayout.vue'
import Card from '@/components/Card.vue'
import axios from 'axios'
/**
 * Here goes the "main screen" with communitie's cards
 */

export default {
  name: 'Communities',
  data () {
    return {
      cards: [],
      totalCards: []
    }
  },
  created () {
    this.getDataCards()
  },
  methods: {
    getDataCards () {
      axios.get('https://fantasyhub.herokuapp.com/communities/').then(res => {
        if (res.data) {
          this.cards = res.data
          var numCard = res.data.length
          for (let i = 0; i < numCard; i++) {
            this.totalCards.push('25 €')
          }
        }
      })
    }
  },
  components: {
    'eh-layout': BaseLayout,
    Cards: Card
  }
}
</script>

<style lang="scss" scoped>
.main-title {
  p {
    font-weight: 400;
    color: #062f4f;
    font-size: 42px;
    letter-spacing: -0.47px;
    line-height: 56px;
    margin: 0;
  }
  p:first-child {
    margin-right: 10px;
  }
}
.main-subtitle {
  width: 55%;
  font-size: 22px;
  color: #062f4f;
  line-height: 34px;
}
@media only screen and (max-width: 840px) {
  .main-title {
    p {
      font-size: 31.2px;
      letter-spacing: -0.35px;
      line-height: 40.8px;
    }
  }
  .main-subtitle {
    font-size: 17px;
    color: #062f4f;
    line-height: 25px;
    width: 320px;
  }
}
@media only screen and (min-width: 505px) {
  .main-title {
    p {
      display: inline-block;
    }
  }
}
@media only screen and (max-width: 300px) {
  .main-subtitle {
    width: 300px;
  }
}
</style>
