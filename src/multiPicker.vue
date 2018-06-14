<template>
  <div class="page">
    <div :class="{'pickerMask':visible}" @click="maskClick" catchtouchmove="true"></div>
    <div class="weui-picker" :class="{'weui_picker_view_show':visible}">
      <div class="weui-picker__hd">
        <div href="javascript:;" class="weui-picker__action" @click="pickerCancel">取消</div>
        <div href="javascript:;" class="weui-picker__action" @click="pickerConfirm">确定</div>
      </div>
      <picker-view indicator-style="height: 40px;" :value="pickerValue" class="weui_picker_view" @change="pickerChange">
        <picker-view-column>
          <div class="picker-item" v-for="(item,i) in dataArray" :key="i">
            {{item.label}}
          </div>
        </picker-view-column>
        <picker-view-column v-if="pickerValue.length > 1">
          <div class="picker-item" v-for="(item,i) in dataArray[pickerValue[0]].children" :key="i">
            {{item.label}}
          </div>
        </picker-view-column>
        <picker-view-column v-if="pickerValue.length > 2">
          <div class="picker-item" v-for="(item,i) in dataArray[pickerValue[0]].children[pickerValue[1]].children" :key="i">
            {{item.label}}
          </div>
        </picker-view-column>
        <picker-view-column v-if="pickerValue.length > 3">
          <div class="picker-item" v-for="(item,i) in dataArray[pickerValue[0]].children[pickerValue[1]].children[pickerValue[2]].children" :key="i">
            {{item.label}}
          </div>
        </picker-view-column>
        <picker-view-column v-if="pickerValue.length > 4">
          <div class="picker-item" v-for="(item,i) in dataArray[pickerValue[0]].children[pickerValue[1]].children[pickerValue[2]].children[pickerValue[2]].children" :key="i">
            {{item.label}}
          </div>
        </picker-view-column>
      </picker-view>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    // picker的值，数组 ，默认[0]
    value: {
      type: Array,
      default: [0]
    },
    // 数据源
    data: {
      type: Array,
      default: []
    }
  },
  data () {
    return {
      pickerValue: [0],
      visible: false,
      dataArray: []
    }
  },

  watch: {
    data: function (newValue, oldValue) {
      this.dataArray = newValue
      this.initPicker()
    }
  },
  mounted () {
    this.initPicker()
  },
  created () {
    this.pickerValue = this.value
    this.dataArray = this.data
  },
  methods: {
    // 显示picker
    show () {
      this.visible = true
    },
    // 隐藏
    hide () {
      this.visible = false
    },
    // 滚动事件
    pickerChange (e) {
      let value = e.mp.detail.value

      let column = this.getChangedColumn(value)
      this.pickerValue = value

      let array = this.dataArray[this.pickerValue[0]]
      let i = 0
      while (i < column) {
        i++
        array = array.children[this.pickerValue[i]]
      }

      let newValue = this.pickerValue.slice(0, column + 1)

      let j = column
      while (array.children.length > 0) {
        newValue[j + 1] = 0
        array = array.children[0]
        j++
      }
      this.pickerValue = newValue

      // console.log('选中的值为：' + this.dataArray[this.pickerValue[0]].label + '-' + this.dataArray[this.pickerValue[0]].children[this.pickerValue[1]].label)
      // console.log('pickerValue：' + this.pickerValue)
    },
    // 点击确认
    pickerConfirm () {
      // console.log('选中的值为：' + this.dataArray[this.pickerValue[0]].label + '-' + this.dataArray[this.pickerValue[0]].children[this.pickerValue[1]].label)
      // console.log('pickerValue：' + this.pickerValue)
      this.hide()

      this.$emit('input', this.pickerValue)

      this.$emit('onConfirm', this.pickerValue)
    },
    // 点击取消
    pickerCancel () {
      this.hide()
    },
    // 遮罩层点击
    maskClick () {
      this.hide()
    },
    // 初始化picker
    initPicker () {
      // 修正pickerValue
      let i = 0
      let array = this.dataArray
      while (array.length > 0) {
        if (this.pickerValue.length > i) {
          if (array[this.pickerValue[i]]) {
            array = array[this.pickerValue[i]].children
          } else {
            array = array[0].children
            this.pickerValue[i] = 0
          }
        } else {
          array = array[0].children
          this.pickerValue.push(0)
        }
        i++
      }
    },
    // 获取修改的列
    getChangedColumn (value) {
      let i = 0
      for (i = 0; i < value.length && i < this.pickerValue.length; i++) {
        if (value[i] !== this.pickerValue[i]) {
          return i
        }
      }
      return i
    }
  }
}
</script>


<style scoped>
page {
  margin-top: 100px;
  position: relative;
}
.weui-picker {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  transition: all 0.3s ease;
  transform: translateY(100%);
  z-index: 3000;
}
.weui_picker_view {
  position: relative;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 238px;
  background-color: rgba(255, 255, 255, 1);
}
.weui-picker__hd {
  display: flex;
  padding: 9px 15px;
  background-color: #fff;
  position: relative;
  text-align: center;
  font-size: 17px;
}
.weui-picker__action {
  display: block;
  flex: 1;
  color: #1aad19;
  cursor: pointer;
  font-size: 32rpx;
}
.weui-picker__action:first-child {
  text-align: left;
  color: #888;
}
.weui-picker__action:last-child {
  text-align: right;
}
.weui-picker__hd:after {
  content: " ";
  position: absolute;
  left: 0;
  bottom: 0;
  right: 0;
  height: 1px;
  border-bottom: 1px solid #e5e5e5;
  color: #e5e5e5;
  transform-origin: 0 100%;
  transform: scaleY(0.5);
}
.weui_picker_view_show {
  transform: translateY(0);
}
.picker-item {
  text-align: center;
  line-height: 40px;
  font-size: 24rpx;
}
.pickerMask {
  position: fixed;
  z-index: 1000;
  top: 0;
  right: 0;
  left: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.6);
}
</style>
