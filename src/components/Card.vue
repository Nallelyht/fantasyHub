<template>
  <div>
    <el-row :gutter="60">
      <el-col style="padding: 30px" :xs="24" :sm="12" :md="8" :lg="6" v-for="(card, index) in cards" :key="card.id">
        <div class="card">
          <div class="card_container">
            <p class="project_location">{{card.name}}</p>
            <img :src="card.image" :alt="card.name" :title="card.location" />
          </div>
          <el-card :body-style="{ padding: '0px 10px'}">
            <div style="padding: 14px;">
              <span
                class="el-card_title"
              >Faltan {{formatPrice(card.project.goal - card.project.contributed)}} €</span>
              <el-progress
                :percentage="(card.project.contributed*100)/card.project.goal"
                :show-text="false"
              ></el-progress>
              <span class="project_title">{{card.project.name}}</span>
              <div class="investment">
                <router-link
                  :to="{name: 'detail', params: {communityID: card.id, value:value[index]}}"
                >
                  <span class="investment_title">INVERTIR</span>
                </router-link>
                <el-select id="custom-select" v-model="value[index]" :key="index">
                  <el-option
                    v-for="item in options"
                    :key="item.value"
                    :label="item.label"
                    :value="item.value"
                  >
                    <span
                      style="font-size: 12px; color: #062F4F; letter-spacing: 0.65px; text-align: center; line-height: 13.06px; font-weight: 700"
                    >{{ item.label }}</span>
                  </el-option>
                </el-select>
              </div>
              <div class="advertisement">
                <p>RECIBIRÁS + {{card.project.interest}}%</p>
                <p>
                  <small>(Interés anual en moneda local)</small>
                </p>
              </div>
            </div>
          </el-card>
        </div>
      </el-col>
    </el-row>
  </div>
</template>

<script>
export default {
  name: 'Card',
  props: ['cards', 'totalCards'],
  data () {
    return {
      options: [
        {
          value: '25',
          label: '25€'
        },
        {
          value: '50',
          label: '50 €'
        },
        {
          value: '100',
          label: '100 €'
        }
      ],
      value: this.totalCards
    }
  },
  mounted () {},
  methods: {
    formatPrice (value) {
      let val = (value / 1).toFixed(0).replace('.', ',')
      return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, '.')
    }
  },
  filters: {
    currency (value) {
      if (typeof value !== 'number') {
        return value
      }
      var formatter = new Intl.NumberFormat('es-ES', {
        style: 'currency',
        currency: 'EUR',
        minimumFractionDigits: 0
      })
      return formatter.format(value)
    }
  },
  components: {}
}
</script>

<style lang="scss" scoped>
@mixin custom_letter {
  font-size: 14px;
  color: #062f4f;
  letter-spacing: 0.65px;
  text-align: center;
  line-height: 13.06px;
  font-weight: bold;
}
.card {
  height: 100%;
  width: 100%;
  background: linear-gradient(transparent, #e6eae5);
  position: relative;
  min-height: 560px;
  &_container {
    img {
      height: 228px;
      width: 100%;
    }
    h3 {
      text-align: center;
      margin: 0;
      padding: 10px;
    }
  }
  .el-card {
    position: absolute;
    top: 45%;
    right: 6%;
    left: 6%;
    bottom: 4%;
    border: 1px solid rgba(0, 0, 0, 0.12);
    box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.5);
    min-height: 273px;
    display: flex;
    justify-content: center;
    align-items: center;
    & > div {
      display: flex;
      justify-content: center;
      align-items: center;
    }
    &_title {
      font-size: 12px;
      color: #062f4f;
      letter-spacing: 0;
      text-align: center;
      line-height: 22.4px;
      display: flex;
      justify-content: center;
    }
    .project_location {
      text-align: center;
      padding: 10%;
      font-size: 20px;
      color: #062f4f;
      letter-spacing: 0.17px;
      line-height: 24px;
    }
    .project_title {
      font-size: 16px;
      color: #062f4f;
      text-align: center;
      line-height: 24px;
      text-align: center;
      justify-content: center;
      display: flex;
      margin: 20px;
    }
    .investment {
      width: 204px;
      height: 52px;
      background: #87f96e;
      border: 1px solid rgba(6, 47, 79, 0.1);
      border-radius: 4px;
      display: flex;
      justify-content: space-around;
      align-items: center;
      flex-direction: row;
      box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.2);
      margin: auto;
      &_title {
        @include custom_letter;
      }
      .el-select {
        width: 85px;
        @include custom_letter;
        .el-input--suffix .el-input__inner {
          padding-right: 30px;
          padding-left: 10px;
          @include custom_letter;
        }
      }
    }
    .advertisement {
      @include custom_letter;
      margin-top: 20px;
      & > p {
        margin: 0;
        font-size: 12px;
      }
    }
  }
}
a {
  text-decoration: none;
}
</style>
