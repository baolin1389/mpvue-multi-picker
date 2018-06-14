# mpvue-multi-picker

> mpvue-multi-picker

[![GitHub issues](https://img.shields.io/github/issues/baolin1389/mpvue-multi-picker.svg)](https://github.com/baolin1389/mpvue-multi-picker/issues)
[![GitHub forks](https://img.shields.io/github/forks/baolin1389/mpvue-multi-picker.svg)](https://github.com/baolin1389/mpvue-multi-picker/network)
[![GitHub stars](https://img.shields.io/github/stars/baolin1389/mpvue-multi-picker.svg)](https://github.com/baolin1389/mpvue-multi-picker/stargazers)


## Screenshots

![](https://raw.githubusercontent.com/baolin1389/mpvue-multi-picker/master/pictures/picker.gif)

## install

```bash
npm install mpvue-multi-picker --save
```

## Usage

```html
<template>
  <div class="container">
    <button type="default" @click="showPickerView">多级联动选择（v-model引用方式）</button>
    <multiPicker ref="picker" v-model="pickerValue" :data="dataArray" @onConfirm="onConfirm"></multiPicker>
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
          'children': []
        },
        {
          'value': '3',
          'label': '选项3',
          'children': []
        }
      ],
      pickerValue: []
    }
  },

  components: {
    multiPicker
  },

  methods: {
    showPickerView () {
      this.$refs.picker.show()
    },

    onConfirm (value) {
      console.log('pickerValue:' + value)
    }
  }
}
</script>
```

## props

| 参数        | 说明                | 类型     | 默认值  |
| ---------  | --------------------| -------  | ------ |
| value      | picker的值          | Array    | [0]    |
| data       | 数据源,picker的选项  | Array    | []     |


