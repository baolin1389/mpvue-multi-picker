<template>
  <div class="container">

    <button type="default" @click="showPickerView">多级联动选择（组件）</button>
    <multiPicker ref="picker" :value="pickerValue" :data="dataArray" @onConfirm="onConfirm"></multiPicker>

    <button type="default" @click="showPickerView2">多级联动选择（v-model引用方式）</button>
    <multiPicker ref="picker2" v-model="pickerValue" :data="dataArray" @onConfirm="onConfirm"></multiPicker>
    <div style="margin-top:50rpx;">
      选中值：{{pickedValue}}
    </div>
    <div style="margin-top:50rpx;">
      选中内容：{{pickedContent}}
    </div>
  </div>
</template>

<script>
import multiPicker from '@/multiPicker'

export default {
  data () {
    return {
      dataArray: [
        {
          'value': '1',
          'label': '选项1',
          'children': []
        },
        {
          'value': '2',
          'label': '选项2',
          'children': [
            {
              'value': '21',
              'label': '选项21',
              'children': []
            },
            {
              'value': '22',
              'label': '选项22',
              'children': [
                {
                  'value': '221',
                  'label': '选项221',
                  'children': []
                },
                {
                  'value': '222',
                  'label': '选项222',
                  'children': []
                }
              ]
            },
            {
              'value': '23',
              'label': '选项23',
              'children': [
                {
                  'value': '231',
                  'label': '选项231',
                  'children': [
                    {
                      'value': '2311',
                      'label': '选项221',
                      'children': []
                    },
                    {
                      'value': '2312',
                      'label': '选项2312',
                      'children': []
                    }
                  ]
                },
                {
                  'value': '232',
                  'label': '选项232',
                  'children': []
                }
              ]
            }
          ]
        },
        {
          'value': '3',
          'label': '选项3',
          'children': []
        },
        {
          'value': '4',
          'label': '选项4',
          'children': [
            {
              'value': '41',
              'label': '选项41',
              'children': [
                {
                  'value': '411',
                  'label': '选项411',
                  'children': [
                    {
                      'value': '4111',
                      'label': '选项4111',
                      'children': [
                        {
                          'value': '41111',
                          'label': '选项41111',
                          'children': []
                        }
                      ]
                    }
                  ]
                }
              ]
            }
          ]
        }
      ],
      pickerValue: [],
      pickedContent: '', // 选中的内容
      pickedValue: []// 选中的值（v-model模式下，可以用pickerValue代替）
    }
  },

  components: {
    multiPicker
  },

  methods: {

    showPickerView () {
      this.$refs.picker.show()
    },

    showPickerView2 () {
      this.$refs.picker2.show()
    },

    onConfirm (value) {
      this.pickedValue = value

      this.pickedContent = []
      let tempDataArray = this.dataArray
      let i = 0
      while (i < value.length) {
        this.pickedContent = this.pickedContent + tempDataArray[value[i]].label + ' '
        tempDataArray = tempDataArray[value[i]].children
        i++
      }

      console.log('pickerValue:' + this.pickerValue)
      console.log('选中的值为：' + this.pickedValue)
      console.log('选中的内容为：' + this.pickedContent)
    }
  },

  created () {

  }
}
</script>

<style scoped>
</style>
