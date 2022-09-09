<template>
  <div id="app">
    <div class="container">
      <h1 class="title">Layout calculation</h1>
      <section class="calculator">
        <form class="calc_form">
          <LabelApp
            v-for="(ipt, idx) in dataInputs"
            :key="idx"
            :title="ipt.title"
          >
            <InputText
              ref="input"
              :price="ipt.price"
              :placeholder="ipt.price"
              v-model="ipt.countBlock"
              @input="total(idx)"
            ></InputText>
          </LabelApp>
          <!-- Adaptive -->
          <label class="calc_label calc_label_adaptive"
            >Aдаптив
            <input
              class="calc_input_adaptive"
              type="checkbox"
              v-model="isChecked"
            />
            <span class="check-style"></span>
          </label>
        </form>
      </section>
      <button @click="iteratingArray()" class="btn_total">Рассчитать</button>
      <div class="out">{{ priceInputsTotal }}</div>
      <div class="out">{{ valueInputsTotal }}</div>
      <button @click="reset()" class="btn_total btn_reset">Сбросить</button>
    </div>
  </div>
</template>

<script>
import LabelApp from './components/LabelApp.vue'
import InputText from './components/InputText.vue'
export default {
  name: 'App',
  components: {
    InputText,
    LabelApp
  },
  data () {
    return {
      currentInput: {
        totalPrice: 0,
        value: 0
      },
      isChecked: true,
      adaptive: 0,
      dataInputs: [
        { title: 'Простой блок', price: 400, countBlock: null, total: {} },
        { title: 'Сложный блок', price: 800, countBlock: null, total: {} },
        { title: 'Слайдер', price: 800, countBlock: null, total: {} },
        { title: 'Аккордеон', price: 400, countBlock: null, total: {} },
        { title: 'Меню', price: 800, countBlock: null, total: {} },
        { title: 'Попап', price: 600, countBlock: null, total: {} },
        { title: 'Поиск', price: 400, countBlock: null, total: {} },
        { title: 'Селект', price: 400, countBlock: null, total: {} },
        {
          title: 'Форма простая 3 инпута',
          price: 400,
          countBlock: null,
          total: {}
        },
        {
          title: 'Форма сложная свыше 3х',
          price: 800,
          countBlock: null,
          total: {}
        },
        { title: 'Видеоблок', price: 600, countBlock: null, total: {} },
        { title: 'Таблица', price: 600, countBlock: null, total: {} }
      ],
      totalPriceArray: [],
      totalcountBlockArray: []
    }
  },

  methods: {
    totalPrice (idx) {
      this.currentInput.totalPrice =
        this.dataInputs[idx].price * this.dataInputs[idx].countBlock
    },
    totalValue (idx) {
      this.currentInput.value = this.dataInputs[idx].countBlock
    },

    total (idx) {
      this.totalPrice(idx)
      this.totalValue(idx)
      const total = {
        price: 0,
        value: 0
      }
      total.price = this.currentInput.totalPrice
      total.value = +this.currentInput.value

      this.dataInputs[idx].total = Object.assign(total)
    },

    iteratingArray () {
      for (const item of this.dataInputs) {
        if (item.total.price !== undefined) {
          this.totalPriceArray.push(item.total.price)
          this.totalcountBlockArray.push(item.total.value)
        }
      }
      this.isAdaptive()
    },

    reset () {
      const refs = this.$refs.input
      refs.forEach((ref) => {
        ref.$el.value = ''
      })
      for (const item of this.dataInputs) {
        item.total.price = 0
        item.total.value = 0
      }

      this.totalPriceArray = []
      this.totalcountBlockArray = []
    },
    isAdaptive () {
      if (this.isChecked) {
        this.adaptive = this.valueInputsTotal * 200
        return this.adaptive
      }
    }

  },
  computed: {
    priceInputsTotal () {
      let result = 0
      if (this.totalPriceArray.length) {
        result = this.totalPriceArray.reduce((acc, item) => acc + item, 0)
      }
      if (this.totalPriceArray.length && this.isChecked) {
        result = this.totalPriceArray.reduce((acc, item) => acc + item, 0) + this.adaptive
      }
      return result
    },
    valueInputsTotal () {
      const result = this.totalcountBlockArray.reduce(
        (acc, item) => acc + item,
        0
      )
      return result
    }

  }
}
</script>

<style lang="scss"></style>
