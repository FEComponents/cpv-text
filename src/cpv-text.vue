<template>
  <FormItem
    :label="childProp.name"
    :prop="childProp.prop"
    :rules="ruleValidate"
  >
    <template v-if="modalType !== 'readonly'">
      <Input
        v-model.trim="formData[curProp]"
        :type="type"
        :placeholder="childProp.placeholder"
        :maxlength="childProp.maxlength"
        clearable
      />
    </template>
    <template v-else>
      {{ propObj }}
    </template>
  </FormItem>
</template>

<script>
export default {
  /**
   * type = text、password、textarea、url、email、date、number、tel
   */
  name: 'CPVtext',
  props: {
    childProp: {
      type: Object
    },
    formData: {
      type: Object
    },
    modalType: {
      type: String
    }
  },
  data() {
    return {}
  },
  computed: {
    propObj() {
      let childProp = this.childProp
      let formData = this.formData
      return formData[(childProp?.textOption?.prop)]
        ? formData[childProp.textOption.prop]
        : formData[childProp.prop]
    },
    type() {
      let type = this.childProp.type
      return type ? type : 'text'
    },
    curProp() {
      let curProp = this.childProp
      if (curProp.isChildProp) {
        return this.childProp.textOption.prop
      } else {
        return curProp.prop
      }
    },
    ruleValidate() {
      let childProp = this.childProp
      if (childProp.required) {
        if (childProp.regexp) {
          return [
            {
              required: true,
              pattern: childProp.regexp,
              message: '请输入' + childProp.name,
              trigger: 'blur'
            }
          ]
        } else {
          return [
            {
              required: true,
              message: '请输入' + childProp.name,
              trigger: 'blur'
            }
          ]
        }
      } else {
        return []
      }
    }
  }
}
</script>
