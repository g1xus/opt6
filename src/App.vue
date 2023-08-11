<template>
  <div class="wrapper">
    <aside class="aside"></aside>
    <div class="app">
      <header class="header">
        <h2 class="header__title">Проведение ТО и мелкий ремонт</h2>
      </header>
      <div class="tabs">
        <div class="tabs__el">
          Общее
        </div>
        <div class="tabs__el tabs__el_active">
          Товар списания
        </div>
        <div class="tabs__el">
          Доп. расходы
        </div>
      </div>
      <div class="add-info">
        <button class="add-info__button" @click="addRow">
          <img src="@/assets/plus.svg" alt="Добавить">
          Добавить строку
        </button>
      </div>
      <main class="table">
        <TableTop :headers="headers"></TableTop>
        <div class="table-main">
          <TableHeader v-bind:headers="headers"></TableHeader>
          <TableRows :headers="headers" :items="items"></TableRows>
        </div>
        <TableCheque></TableCheque>
      </main>
      <TableMobile :headers="headers" :items="items"></TableMobile>

    </div>
  </div>
</template>

<script>
import {ref} from 'vue'
import TableRows from "@/components/TableRows.vue";
import TableHeader from "@/components/TableHeader.vue";
import TableMobile from "@/components/TableMobile.vue";
import TableCheque from "@/components/TableCheque.vue";
import TableTop from "@/components/TableTop.vue";

export default {
  name: 'App',
  components: {TableTop, TableCheque, TableRows, TableHeader, TableMobile},
  setup() {
    let items = ref([
      {
        id: 1,
        productId: 1,
        title: 'Первый',
        titleOptions: ['фр. 2-5 мм, 25кг', 'фр. 2-5 мм, 25кг (белый)', 'фр. 2-5 мм, 25кг (вайт)', 'фр. 2-5 мм, 25кг, возврат', 'фр. 2-5 мм, 1т', ],
        delete: 'Действие',
        cost: 1231,
        amount: 12,
        name: 'Мраморный щебень',
        total: 1231,
        isActionsActive: false,
        isTopPlaceholder: false,
        isBottomPlaceholder: false,
        shipCost: 1000,
        maxCapacity: 23,
        isOptionsActive: false
      },
      {
        id: 2,
        productId: 2,
        title: 'Второй',
        titleOptions: ['фр. 2-5 мм, 25кг', 'фр. 2-5 мм, 25кг (белый)', 'фр. 2-5 мм, 25кг (вайт)', 'фр. 2-5 мм, 25кг, возврат', 'фр. 2-5 мм, 1т', ],
        delete: 'Действие',
        cost: 1231,
        amount: 12,
        name: 'Мраморный щебень',
        total: 1231,
        isActionsActive: false,
        isTopPlaceholder: false,
        isBottomPlaceholder: false,
        shipCost: 1000,
        maxCapacity: 23,
        isOptionsActive: false
      },
      {
        id: 3,
        productId: 3,
        title: 'Третий',
        titleOptions: ['фр. 2-5 мм, 25кг', 'фр. 2-5 мм, 25кг (белый)', 'фр. 2-5 мм, 25кг (вайт)', 'фр. 2-5 мм, 25кг, возврат', 'фр. 2-5 мм, 1т', ],
        delete: 'Действие',
        cost: 1231,
        amount: 12,
        name: 'Мраморный щебень',
        total: 1231,
        isActionsActive: false,
        isTopPlaceholder: false,
        isBottomPlaceholder: false,
        shipCost: 1000,
        maxCapacity: 23,
        isOptionsActive: false
      },
      {
        id: 4,
        productId: 4,
        title: 'Четвёртый',
        titleOptions: ['фр. 2-5 мм, 25кг', 'фр. 2-5 мм, 25кг (белый)', 'фр. 2-5 мм, 25кг (вайт)', 'фр. 2-5 мм, 25кг, возврат', 'фр. 2-5 мм, 1т', ],
        delete: 'Действие',
        cost: 1231,
        amount: 12,
        name: 'Мраморный щебень',
        total: 1231,
        isActionsActive: false,
        isTopPlaceholder: false,
        isBottomPlaceholder: false,
        shipCost: 1000,
        maxCapacity: 23,
        isOptionsActive: false
      },

    ])
    let headers = ref([
      {
        id: 1,
        title: 'Номер',
        name: 'productId',
        width: '10%',
        isShow: true,
        isResizingHeader: false
      }, {
        id: 2,
        title: 'Действие',
        name: 'delete',
        width: '5%',
        isShow: true,
        isResizingHeader: false

      }, {
        id: 3,
        title: 'Наименование еденицы',
        name: 'title',
        width: '30%',
        isShow: true,
        isResizingHeader: false

      }, {
        id: 4,
        title: 'Цена',
        name: 'cost',
        width: '10%',
        isShow: true,
        isResizingHeader: false

      }, {
        id: 5,
        title: 'Кол-во',
        name: 'amount',
        width: '10%',
        isShow: true,
        isResizingHeader: false

      }, {
        id: 6,
        title: 'Название товара',
        name: 'name',
        width: '25%',
        isShow: true,
        isResizingHeader: false

      }, {
        id: 7,
        title: 'Итого',
        name: 'total',
        width: '10%',
        isShow: true,
        isResizingHeader: false
      }, {
        id: 8,
        title: 'Цена доставки, руб',
        name: 'shipCost',
        width: '10%',
        isShow: false,
        isResizingHeader: false
      }, {
        id: 9,
        title: 'Max грузоподъемность, кг',
        name: 'maxCapacity',
        width: '10%',
        isShow: false,
        isResizingHeader: false
      },
    ])

    function addRow() {
      items.value.push({
        id: items.value.length + 1,
        productId: items.value.length + 1,
        title: 'Наименование единицы',
        titleOptions: ['фр. 2-5 мм, 25кг', 'фр. 2-5 мм, 25кг (белый)', 'фр. 2-5 мм, 25кг (вайт)', 'фр. 2-5 мм, 25кг, возврат', 'фр. 2-5 мм, 1т', ],
        delete: 'Действие',
        cost: 0,
        amount: 0,
        name: 'Название товара',
        total: 0,
        isActionsActive: false,
        isTopPlaceholder: false,
        isBottomPlaceholder: false,
        shipCost: 1000,
        maxCapacity: 23,
        isOptionsActive: false
      })
    }

    return {
      headers,
      items,
      addRow,
    }
  }
}
</script>

<style lang="sass">
.wrapper
  display: flex
  width: 100%

.app
  width: 87%
  padding: 25px
  @media all and (max-width: 992px)
    width: 100%
    padding: 16px 10px
body
  margin: 0
  font-family: MyriadPro
  overflow-x: hidden

*
  box-sizing: border-box

.aside
  width: 13%
  height: 100vh
  background-image: radial-gradient(circle at 29% 0, #000, #1c2734 103%)
  @media all and (max-width: 992px)
    display: none

.header
  margin-bottom: 25px

  &__title
    font-size: 30px
    font-weight: 400
    margin: 0

.tabs
  display: flex
  font-weight: 600
  color: #1253a2
  margin-bottom: 25px

  &__el
    margin-right: 20px

    &_active
      color: #000

.add-info
  padding: 20px 25px
  margin-bottom: 25px
  border-radius: 10px
  box-shadow: 0 5px 20px 0 rgba(0, 0, 0, 0.07)
  border: solid 1px #eeeff1

  &__button
    font-family: MyriadPro
    font-size: 14px
    color: #fff
    padding: 10px 15px 10px 10px
    border-radius: 5px
    background-color: #2f8cff
    border: none
    cursor: pointer

    img
      display: inline-block
      margin-right: 7px

.table
  @media all and (max-width: 768px)
    display: none
  &__modal
    position: absolute
    font-size: 14px
    padding: 7px 10px
    border-radius: 5px
    box-shadow: 0 0 3px 0 #000, inset 0 1px 2px 0 rgba(255, 255, 255, 0.5)
    background: #fff
    cursor: pointer
    z-index: 1002

  &-main
    margin-bottom: 20px

  &-cheque
    width: 100%
    flex-direction: column
    align-items: end
    display: flex
  &-total, &-receipt
    padding: 15px
    width: 300px
    border-radius: 5px
    border: solid 1px #eeeff1
    background-color: #fbfcfd
    margin-right: 15px
    font-size: 14px
    @media all and (max-width: 768px)
      width: 100%
      margin-right: 0
      font-size: 16px
    &__row
      display: flex
      justify-content: space-between

  &-total
    margin-bottom: 25px

    &__amount
      font-weight: 600

  &-receipt
    margin-bottom: 5px

    &__title
      color: #8f8f8f
</style>

<style>
@font-face {
  font-family: 'MyriadPro';
  src: url('assets/fonts/MyriadPro-Regular.eot');
  src: local('Myriad Pro Regular'), local('MyriadPro-Regular'),
  url('assets/fonts/MyriadPro-Regular.eot?#iefix') format('embedded-opentype'),
  url('assets/fonts/MyriadPro-Regular.woff') format('woff'),
  url('assets/fonts/MyriadPro-Regular.ttf') format('truetype');
  font-weight: normal;
  font-style: normal;
}

@font-face {
  font-family: 'MyriadProSemi';
  src: url('assets/fonts/MyriadPro-Semibold.eot');
  src: local('Myriad Pro Semibold'), local('MyriadPro-Semibold'),
  url('assets/fonts/MyriadPro-Semibold.eot?#iefix') format('embedded-opentype'),
  url('assets/fonts/MyriadPro-Semibold.woff') format('woff'),
  url('assets/fonts/MyriadPro-Semibold.ttf') format('truetype');
  font-weight: 600;
  font-style: normal;
}
</style>