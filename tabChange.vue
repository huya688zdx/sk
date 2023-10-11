<template>
  <div class="tab-change">
    <div
      v-for="item in changedOrderList"
      :key="item.value"
      class="tab-item"
      :class="{ active: item.value === activeIndex }"
      @click="tabChange(item.value)"
    >
      <el-badge
        class="item"
        :class="{ 'special-item': item.num < 10 }"
        :value="item.num > 99 ? '99+' : item.num"
        :hidden="!([2, 3, 4].includes(item.value) && item.num)"
      >
        {{ item.label }}
      </el-badge>
    </div>
  </div>
</template>

<script >



import { Vue, Component, Prop, Watch } from 'vue-property-decorator'
import { getOrderDetailPage } from '@/api/order'

@Component({
  name: 'TabChange'
})
export default class extends Vue {
  @Prop({ default: '' }) orderStatics: any
  @Prop({ default: '' }) defaultActivity: any
  private activeIndex: number = this.defaultActivity || 0

  @Watch('defaultActivity')
  private onChange(val) {
    this.activeIndex = Number(val)
  }

  get changedOrderList() {
    return [
      {
        label: '全部订单',
        value: 0
      },
      {
        label: '待接单',
        value: 2,
        num: this.orderStatics.toBeConfirmed
      },
      {
        label: '待派送',
        value: 3,
        num: this.orderStatics.confirmed
      },
      {
        label: '派送中',
        value: 4,
        num: this.orderStatics.deliveryInProgress
      },
      {
        label: '已完成',
        value: 5
      },
      {
        label: '已取消',
        value: 6
      }
    ]
  }

  private tabChange(activeIndex) {
    this.activeIndex = activeIndex
    this.$emit('tabChange', activeIndex)
  }
}

</script>

<style lang="scss" scoped>
/* Define your component styles here */
</style>
