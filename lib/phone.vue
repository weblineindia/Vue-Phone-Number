<template>
  <div class="list-view">
    <div class="country-list">
      <select
        class="form-control option-block"
        v-model="phoneCodeValue[index]"
        v-show="!ignoredCountries"
        :name="name + index"
        v-validate="{  isUniquePhone: { valueData,phoneCodeValue,index }}"
        @change="onChageDropdown($event)"
      >
        <option v-for="item in preferredCountries" :value="item" :key="item.id">{{ item.code }}</option>
      </select>
    </div>
    <div class="phone-input">
      <input
        :id="id + index"
        v-model="valueData[index].phone"
        v-validate="{  isUniquePhone: { valueData,phoneCodeValue,index }}"
        :disabled="disabled"
        type="text"
        :placeholder="placeholder"
        :tabindex="tabindex"
        autocomplete="off"
        :maxlength="preferredCountries.length > 0 ? phoneCodeValue[index].maxlength || phoneCodeValue[index].maxlength === ''  ? phoneCodeValue[index].maxlength : phoneCodeValue[index].maxlength : maxlength"
        :name="name + index"
        :class="[
          {
            input: true,
            'form-control': true,
            'is-danger': errors.has(name + index),
            'error-text': errors.has(name + index),
          },
        ]"
        @keypress.enter.prevent
        @focus="onFocus($event, index)"
        @input="onChangeField($event)"
        @change="onChange($event)"
        @blur="onBlur($event)"
      />
      <span v-if="isMultiple" v-show="valueData[index].phone !== ''" class="add-remove">
        <span>
          <font-awesome-icon icon="minus" />
        </span>
        <span v-show="isShowPlus">
          <font-awesome-icon
            icon="plus"
            class="pointer"
            @click.prevent="onMultiplePhone(id, index)"
          />
        </span>
      </span>
    </div>
    <div v-if="errors.items.length > 0">
      <div v-for="(error, key) in errors.items" :key="key">
        <p
          v-if="errors.has(name + index)"
          :class="{
            control: true,
            'error-msg': errors.has(name + index),
            'text-align':center
          }"
        >
          <span
            v-show="(error.rule === 'isUniquePhone') && error.field === name + index"
          >{{duplicatePhoneError}}</span>
        </p>
      </div>
    </div>
  </div>
</template>
<script>
/*eslint-disable */
// For load Regex
import * as Regex from "./regex.js";
import Vuelidate from "vuelidate";
import Vue from "vue";
import * as VeeValidate from "vee-validate";
// Load font awesome
import { library } from "@fortawesome/fontawesome-svg-core";
import {
  faPlus,
  faMinus,
  faCircle,
  faClone
} from "@fortawesome/free-solid-svg-icons";
import "@fortawesome/fontawesome-svg-core/styles.css";
import { FontAwesomeIcon } from "@fortawesome/vue-fontawesome";
library.add(faPlus, faMinus, faCircle, faClone);

Vue.component("font-awesome-icon", FontAwesomeIcon);

Vue.use(Vuelidate);
Vue.use(VeeValidate, {
  events: "change|blur"
});

export default {
  data() {
    return {
      value: [{ phone: "" }],
      phoneCodeValue: this.defaultSelectedCountry
    };
  },
  props: {
    maxlength: {
      type: Number,
      default: 10
    },
    id: {
      type: String,
      default: ""
    },
    phoneFormateError: {
      type: String,
      default: "Phone is not valid"
    },
    duplicatePhoneError: {
      type: String,
      default: "Do not enter same phone number"
    },
    onMultiplePhone: {
      type: Function,
      default: () => {}
    },
    name: {
      type: String,
      default: "Phone"
    },
    index: {
      type: Number,
      default: 0
    },
    values: {
      type: Array[Object],
      default: () => [
        {
          phone: ""
        }
      ]
    },
    isMultiple: {
      type: Boolean,
      default: true
    },
    isShowPlus: {
      type: Boolean,
      default: false
    },
    placeholder: {
      type: String,
      default: "Phone Number"
    },
    disabled: {
      type: Boolean,
      default: false
    },
    tabindex: {
      type: Number,
      default: 0
    },
    preferredCountries: {
      type: Array,
      default: () => [{}]
    },
    ignoredCountries: {
      type: Boolean,
      default: false
    },
    defaultSelectedCountry: {
      type: Array,
      default: () => [
        {
          code: "IN",
          maxlength: 10
        }
      ]
    }
  },
  computed: {
    valueData: {
      get() {
        return this.values;
      },
      set(newData) {
        return (this.valueData = newData);
      }
    }
  },
  async created() {
    /**
     * for uniqueness check in phone
     */
    await this.$validator.extend("isUniquePhone", {
      getMessage: () => this.duplicatePhoneError,
      validate: (value, valueArray) => {
        return new Promise(resolve => {
          if (
            event !== undefined &&
            event.target !== undefined &&
            event.target.name !== "" &&
            event.target.name !== undefined
          ) {
            const res = event.target.name.split("_");
            if (res[1] !== isNaN && res[1] !== undefined) {
              if (value) {
                if (this.values.length > 1) {
                  const tempArray = [];
                  for (let i = 0; i <= this.values.length - 1; i++) {
                    if (parseInt(i) !== parseInt(res[1])) {
                      if (
                        this.values[i].phone ===
                        this.values[parseInt(res[1])].phone
                      ) {
                          if (
                            valueArray.phoneCodeValue[i].code ===
                            valueArray.phoneCodeValue[valueArray.index].code
                          ) {
                            tempArray.push(this.values[i]);
                          }
                      }
                    }
                  }
                  if (tempArray.length > 0) {
                    resolve({ isUniqePhoneValidation: false });
                  } else {
                    resolve({ isUniqePhoneValidation: true });
                  }
                } else {
                  resolve({ isUniqePhoneValidation: true });
                }
              }
            }
          }
        })
          .catch(function(error) {
            console.log(error);
          })
          .then(function(data) {
            if (data) {
              return data.isUniqePhoneValidation;
            } else {
              return true;
            }
          });
      }
    });
  },
  methods: {
    onFocus(event, index) {
      this.$emit("focus", event, index);
    },
    onChangeField() {
      this.valueData[this.index].phone = this.valueData[
        this.index
      ].phone.replace(Regex.ONLY_NUMBERS_REGEX, "");
      this.$validator.reset()
      this.$emit("input", this.valueData, this.id, this.index, this.errors);

    },
    onBlur() {
      this.$emit("onBlur", event, this.placeholder);
    },
    onChageDropdown() {
      this.$emit("onChangeDropdown", this.phoneCodeValue);
    },
    onChange(event){
       this.$emit("change", event);
    }
  }
};
</script>
<style  scoped>
@import "./style.css";
</style>