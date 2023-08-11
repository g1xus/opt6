<template>
  <div class="table-header">
    <div class="table-header__el" :class="{'table-header__el_active':isDraggableHeader==header}"
         v-for="header in filteredHeaders"
         :style="{width: header.width}"
         @mousedown="dndHeader(header)"

    >
      <div class="table-header__title" @mouseover="sort(header)">
        {{ header.title }}
      </div>

      <div class="table-header__limiter" v-if="header.id != filteredHeaders.length"
           @mousedown="resizeHeader($event, header)"
      ></div>

    </div>

  </div>
</template>
<script>
import {ref, computed} from 'vue'

export default {
  props: ['headers'],
  setup(props) {

    let headers = ref(props.headers)
    let isDraggableHeader = ref(null)
    const filteredHeaders = computed(() => {
      return headers.value.filter((header) => header.isShow == true)
    })
    function sort(column2) {
      if (!isDraggableHeader.value || column2 == isDraggableHeader.value) return;
      let indexColumn1 = headers.value.findIndex( column => column == isDraggableHeader.value )
      headers.value.splice(indexColumn1, 1)
      let indexColumn2 = headers.value.findIndex( column => column == column2 )
      let append = (indexColumn1 == indexColumn2) ? 1 : 0;
      headers.value.splice(indexColumn2 + append, 0, isDraggableHeader.value)
      // $emit('sort', headers)
    }


    function dndHeader(header) {
      if(header.isResizingHeader == false) {
        isDraggableHeader.value = header
        document.onmouseup = function () {
          isDraggableHeader.value = ''
        }
      }
    }


    function resizeHeader(e, item) {
      item.isResizingHeader = true
      let headerStartWidth = e.target.parentNode.offsetWidth
      let cursorStartPos = e.clientX

      function moveAt(e) {
        item.width = headerStartWidth - (cursorStartPos - e.clientX) + 'px'
      }

      document.onmousemove = function (e) {
        moveAt(e);
      };
      document.onmouseup = () => {
        document.onmousemove = null;
        item.isResizingHeader = false
      }
    }



    return {
      resizeHeader,
      isDraggableHeader,
      sort,
      dndHeader,
      filteredHeaders
    }
  }
}
</script>

<style lang="sass">
.table
  width: 100%
  font-size: 16px
  border-radius: 10px
  box-shadow: 0 5px 20px 0 rgba(0, 0, 0, 0.07)
  border: solid 1px #eeeff1

  &-header
    display: flex
    text-align: left
    font-weight: 600
    border-top: 1px solid #eeeff1
    border-bottom: 1px solid #eeeff1
    &__el
      padding: 14px 10px
      white-space: nowrap
      cursor: grab
      overflow: hidden
      user-select: none
      position: relative
      //transition: 30s
      &_active
        background-color: #eeeff1
        transform: scale(103%)
    &__title
      display: inline
    &__limiter
      position: absolute
      right: 0
      top: 0
      height: 1000px
      width: 1px
      cursor: col-resize
      background-color: #eeeff1
</style>