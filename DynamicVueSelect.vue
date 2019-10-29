<script>
import vueSelect from 'vue-select'
export default {
  name: 'DynamicVueSelect',
  extends: vueSelect,
  props: {
    perElement: {
      type: Number,
      required: false,
      default: 5
    }
  },
  data () {
    return {
      clickCount: 0
    }
  },
  methods: {
    // override
    onSearchFocus () {
      this.open = true
      if (this.clickCount === 0) {
        this.clickCount++
        this.$emit('search:focus')
      }
    },
    // override
    onAfterSelect (option) {
      const { moreAction, onClick } = option
      if (moreAction) {
        this.open = true
        if (typeof onClick === 'function') {
          this.clickCount++
          // select options 에서 ...더보기를 제거
          this.mutableOptions.pop()
          this.mutableLoading = true
          try {
            onClick(this.clickCount, this.perElement)
          } catch (error) {
            this.commonErrorHandlerV2(error)
          } finally {
            this.mutableLoading = false
            this.clearSelection()
          }
        } else {
          console.log(`등록된 click 이벤트가 없습니다. input callback: ${onClick} , type: ${typeof onClick}`)
        }
      } else {
        this.open = false
        this.$refs.search.blur()
      }
      if (this.clearSearchOnSelect) {
        this.search = ''
      }
    }
  }
}
</script>

<style>
.v-select .spinner {
  top: 4px;
}

.v-select .spinner,
.v-select .spinner:after {
  width: 3em;
  height: 3em;
}
</style>
