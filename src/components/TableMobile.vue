<template>
  <main class="products">
    <div class="products-el" v-for="item in items">
      <div class="products-el__row" v-for="header in headers.filter((header) => header.id != 1 && header.isShow == true)">
        <div class="products-el__header">
          {{ header.title }}
        </div>
        <div v-if="header.id == 2">
          <img src="@/assets/options.svg" alt="Действие" @click="item.isActionsActive=!item.isActionsActive">
          <div class="table-row__option table__modal"
               v-if="item.isActionsActive == true"
               @click="deleteProduct(item)"
          >
            Удалить</div>
        </div>
        <div class="table-row__name" v-else-if="header.id==3">
          <div class="table-row__selected" @click="item.isOptionsActive=!item.isOptionsActive">{{`${item.name}  ${item.title}`}}</div>
          <div class="table-row__options table__modal" v-if="item.isOptionsActive == true">
            <div class="table-row__option" v-for="titleOption in item.titleOptions" @click="item.title = titleOption, item.isActionsActive = false">
              <b>{{ item.name }}</b> {{titleOption}}
            </div>
          </div>
        </div>
        <input v-model="item[header.name]" v-else class="products-el__value">
      </div>
    </div>
  </main>
  <TableCheque></TableCheque>
</template>

<script>
import {ref} from 'vue'
import TableCheque from "@/components/TableCheque.vue";
export default {
  components: {TableCheque},
  props: ['items', 'headers'],
  setup(props) {
    let items = ref(props.items)
    function deleteProduct(item) {
      item.isActionsActive = false
      items.value.splice(item.productId - 1, 1)
      items.value.map(function (item, i) {
        item.productId = i + 1
      })
    }
    return {
      deleteProduct
    }
  }

}
</script>

<style lang="sass" scoped>
.table-cheque
  @media all and (min-width: 769px)
    display: none
.products
  display: none
  @media all and (max-width: 768px)
    display: block
  &-el
    margin-bottom: 5px
    padding: 15px
    border-radius: 10px
    box-shadow: 0 5px 20px 0 rgba(0, 0, 0, 0.07)
    border: solid 1px #eeeff1
    &__row
      margin-bottom: 15px
    &__header
      font-size: 10px
      color: #8f8f8f
      margin-bottom: 5px
    &__value
      font-family: MyriadPro
      font-size: 16px
      width: 100%
      padding: 10px 15px
      border-radius: 5px
      border: solid 1px #ccc
</style>