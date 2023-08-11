<template>
  <div v-for="item in items" class="table-row">
    <div class="table-row__placeholder" v-if="item.isTopPlaceholder"></div>
    <div class="table-row__wrapper" @mouseenter="changeProbablyDrop(item)">
      <div class="table-row__el" :class="{'table-row__el_border':header.isResizingHeader}"
           v-for="header in headers.filter((header) => header.isShow == true)"
           :style="{width: header.width}">
        <div class="table-row__toggler" v-if="header.id==1">
          <img src="@/assets/drag.svg" alt="Переместить"
               @mousedown="onMouseDown($event, $event.target, item)">
          <div class="table-row__id"> {{ item[header.name] }}</div>
        </div>

        <div class="table-row__actions"
             v-else-if="header.id==2">
          <img src="@/assets/options.svg" alt="Опции" @click="item.isActionsActive=!item.isActionsActive">
          <div class="table-row__action table__modal"
               v-if="item.isActionsActive == true"
               @click="deleteProduct(item)"
          >
            Удалить
          </div>
        </div>
        <div class="table-row__name" v-else-if="header.id==3">
          <div class="table-row__selected" @click="item.isOptionsActive=!item.isOptionsActive">{{`${item.name}  ${item.title}`}}</div>
          <div class="table-row__options table__modal" v-if="item.isOptionsActive == true">
            <div class="table-row__option" v-for="titleOption in item.titleOptions" @click="changeItemTitle(item, titleOption)">
              <b>{{ item.name }}</b> {{titleOption}}
            </div>
          </div>
        </div>
        <input class="table-row__input" v-else
                  v-model="item[header.name]" @focusout="changeIsSavableData()">
      </div>
    </div>
    <div class="table-row__placeholder" v-if="item.isBottomPlaceholder"></div>
  </div>
</template>
<script>
import {ref} from 'vue'

export default {
  props: ['headers', 'items'],
  setup(props, {emit}) {
    let items = ref(props.items)
    let itemProductId
    let probablyDrop = ref(0)
    let dragging
    function changeIsSavableData() {
      emit('changeIsSavableData', true)
    }
    function changeItemTitle(item, titleOption) {
      item.title = titleOption
      item.isOptionsActive = false
      changeIsSavableData()
    }

    function getCoords(elem) {   // кроме IE8-
      let box = elem.getBoundingClientRect();
      return {
        top: box.top + pageYOffset,
        left: box.left + pageXOffset
      };
    }

    function onMouseDown(e, element, item) {
      item.isTopPlaceholder = true
      probablyDrop.value = item.productId
      dragging = true
      let elParent = ((element.parentNode).parentNode).parentNode
      let coords = getCoords(elParent);
      let shiftX = e.pageX - coords.left;
      let shiftY = e.pageY - coords.top;

      itemProductId = item.productId

      elParent.classList.add('table-row__wrapper_active');
      element.style.display = 'none'
      moveAt(e);


      function moveAt(e) {
        elParent.style.left = (e.pageX - shiftX) + 40 + 'px';
        elParent.style.top = e.pageY - shiftY + 'px';
      }

      document.onmousemove = function (e) {
        moveAt(e);
        item.productId = probablyDrop.value
        console.log(probablyDrop.value)
      };
      document.onmouseup = function () {
        if (dragging) {
          document.onmousemove = null;
          probablyDrop.value = 0
          elParent.onmouseup = null;
          elParent.classList.remove('table-row__wrapper_active')
          element.style.display = 'block'
          dragging = false
          items.value = items.value.map(x => {
            x.isTopPlaceholder = false
            x.isBottomPlaceholder = false
            return x
          })
          items.value = items.value.sort((a, b) => a.productId - b.productId)
        }
      };
    }

    function changeProbablyDrop(item) {
      if (dragging) {
        items.value = items.value.map((x) => {
          if (x.productId == item.productId && x.productId < itemProductId) {
            x.isTopPlaceholder = true
            x.isBottomPlaceholder = false
          } else if (x.productId == item.productId && x.productId > itemProductId) {
            x.isBottomPlaceholder = true
            x.isTopPlaceholder = false
          } else {
            x.isTopPlaceholder = false
            x.isBottomPlaceholder = false
          }
          return x
        })
        probablyDrop.value = item.productId
        item.productId = itemProductId
        itemProductId = probablyDrop.value
      }
    }

    function deleteProduct(item) {
      item.isActionsActive = false
      items.value.splice(item.productId - 1, 1)
      items.value.map(function (item, i) {
        item.productId = i + 1
      })

      //Можно вставить запрос на удаление объекта с БД
    }

    return {
      items,
      onMouseDown,
      probablyDrop,
      changeProbablyDrop,
      itemProductId,
      deleteProduct,
      changeItemTitle,
      changeIsSavableData
    }
  }
}
</script>

<style lang="sass">
.table

  &-row
    width: 100%

    &__el
      height: 53px
      display: flex
      align-items: center
      padding: 5px 10px
      &_border
        border-right: 1px solid #eeeff1
    &__placeholder
      width: 100%
      height: 51px
      border-radius: 5px
      border: 1px dashed #a6b7d4

    &__wrapper
      width: 100%
      display: flex
      align-items: center

      &_active
        position: absolute
        width: 50%
        z-index: 1000

    &__toggler
      display: flex

      img
        cursor: grab

        &:active
          cursor: grabbing

    &__id
      margin-left: 5px

    &__input, &__name
      font-family: MyriadPro
      border-radius: 5px
      border: 1px solid #cccccc
      width: 100%
      padding: 10px 15px
      white-space: nowrap
      resize: none
      overflow-x: hidden
      font-size: 16px
      position: relative
      &::-webkit-scrollbar
        width: 0
      &:focus
        outline: none
    &__name
      overflow: visible
      position: relative
      cursor: pointer
    &__options
      left: 0
      width: 100%
      background-color: #fff
      z-index: 1002
      top: calc(100% + 10px)
      overflow-x: auto
    &__option
      margin-bottom: 14px
    &__selected
      overflow: hidden
    &__actions
      img
        cursor: pointer
        padding: 0 5px

    &__action

      width: 150px

      color: #ae0a0a

</style>