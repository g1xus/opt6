<template>
  <div class="table-top" id="tableTop">
    <div class="table-top__save" @click="saveData">Сохранить изменения</div>
    <img src="@/assets/settings.svg" alt="Настройки"
         @click="settingsActiveStage == 0 ? settingsActiveStage = 1 : settingsActiveStage = 0">
    <div class="table-top__settings table__modal"
         :class="{'table-top__settings_active': settingsActiveStage == 2}" v-if="settingsActiveStage != 0">
      <div class="table-top__header" v-if="settingsActiveStage == 1" @click="settingsActiveStage = 2">
        Отображение столбцов
        <img src="@/assets/arrow.svg" alt="Открыть меню">
      </div>
      <div class="table-top__header" v-else-if="settingsActiveStage == 2" @click="settingsActiveStage = 1">
        <img src="@/assets/arrow.svg" alt="Открыть меню">
        Отображение столбцов
      </div>

      <div class="table-top__filter" v-for="header in headers" @click="header.isShow = !header.isShow"
           v-if="settingsActiveStage == 2">
        <div class="table-top__checkbox" :class="{'table-top__checkbox_active': header.isShow == true}"></div>
        {{ header.title }}
      </div>
    </div>
  </div>
</template>
<script>
import {ref} from 'vue'
export default {
  props: ['headers', 'items'],
  setup(props) {
    const headers = props.headers
    let settingsActiveStage = ref(0)
    function saveData() {
      console.log('save' )
      props.items.forEach((item) => {
        console.log(item)
      })
    }
    return {
      settingsActiveStage, headers, saveData
    }
  }
}
</script>

<style scoped lang="sass">
.table-top
  display: flex
  justify-content: end
  padding: 9px 10px
  position: relative

  img
    cursor: pointer

  &__settings
    top: 100%
    z-index: 1001
    display: flex
    flex-direction: column
    justify-content: center

    &_active
      .table-top__header
        margin-bottom: 14px
        font-weight: 600
      img
        transform: rotate(180deg)
        margin-left: 0 !important
        margin-right: 11px

    img
      display: inline-block
      margin-left: 11px

  &__filter
    display: flex
    padding-bottom: 16px
    user-select: none

    &:last-child
      margin-bottom: 0

  &__checkbox
    width: 13px
    height: 13px
    border-radius: 3px
    border: solid 1px #a6b7d4
    margin-right: 4px

    &_active
      position: relative

      &:after
        content: ''
        position: absolute
        width: 100%
        height: 100%
        background-color: #3261ff
        background-image: url('../assets/checkmark.svg')
        background-size: 6px 7px
        background-position: center
        background-repeat: no-repeat
  &__save
    font-size: 12px
    color: #a6b7d4
    margin-right: 10px
    cursor: pointer
</style>
