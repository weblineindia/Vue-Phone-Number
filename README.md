# Vue-Phone-Number

A Vue.js Phone number component is provide to add single/multiple input Phone number field with validation.
The Phone number value is automatically validated on blur event.
You can change validation message using props.
You can also disable Phone number field using disable props.

## Table of contents

- [Browser Support](#browser-support)
- [Demo](#demo)
- [Getting started](#getting-started)
- [Usage](#usage)
- [Available Props](#available-props)
- [Methods](#methods)
- [Want to Contribute?](#want-to-contribute)
- [Need Help / Support?](#need-help)
- [Collection of Components](#collection-of-components)
- [Changelog](#changelog)
- [License](#license)
- [Keywords](#Keywords)

## Browser Support

| ![Chrome](https://raw.github.com/alrra/browser-logos/master/src/chrome/chrome_48x48.png) | ![Firefox](https://raw.github.com/alrra/browser-logos/master/src/firefox/firefox_48x48.png) | ![Safari](https://raw.github.com/alrra/browser-logos/master/src/safari/safari_48x48.png) | ![Edge](https://raw.github.com/alrra/browser-logos/master/src/edge/edge_48x48.png) | ![IE](https://raw.github.com/alrra/browser-logos/master/src/archive/internet-explorer_9-11/internet-explorer_9-11_48x48.png) |
| ---------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------- |
| 83.0 ✔                                                                                   | 77.0 ✔                                                                                      | 13.1.1 ✔                                                                                 | 83.0 ✔                                                                             | 11.9 ✔                                                                                                                       |

## Demo

[![](phone.gif)](https://github.com/weblineindia/Vue-Phone-Number/phone.gif)

## Getting started

Install the npm package:

```bash
npm install vue-weblineindia-phone-number
#OR
yarn add vue-weblineindia-phone-number
```

## Usage

Use the `<vue-weblineindia-phone-number>` component:

```vue
<template>
  <div>
    <div v-for="(phoneV, pkey) in phoneValue" :key="phoneV.key">
      <PhoneField
        :values="phoneValue"
        :index="pkey"
        :id="'Phone'"
        :name="'Phone_'"
        :onMultiplePhone="onMultiplePhone"
        :is-show-plus="phoneValue.length - 1 === pkey"
        @focus="onFocusPhone"
        @input="onChangePhone"
        @blur="onBlurPhone"
      />
    </div>
  </div>
</template>

<script>
import PhoneNumber from "vue-weblineindia-phone-number";
export default {
  components: { PhoneNumber },
  data: function () {
    return {
      phoneValue: [{ phone: "" }],
      preferredCountries: [
        { code: "IN", maxlength: 10 },
        { code: "US", maxlength: 11 },
      ],
      defaultSelectedCountry: [{ code: "IN", maxlength: 10 }],
    };
  },

  methods: {
    onFocusPhone(event, index) {
      event.target.placeholder = "";
    },
    onChangePhone(value, id, index, error) {
      this.phoneValue[index].phone = value[index].phone;
    },
    onBlurPhone(event, placeholder) {
      event.target.placeholder = placeholder;
    },
    onMultiplePhone() {
      this.phoneValue.push({
        phone: "",
      });
      this.defaultSelectedCountry.push(this.defaultSelectedCountry[0]);
    },
  },
};
</script>
```

## Available Props

| Prop                   | Type          | default                        | Description                                     |
| ---------------------- | ------------- | ------------------------------ | ----------------------------------------------- |
| maxlength              | Number        | 10                             | The phone maxlength                             |
| id                     | String        |                                | The phone id                                    |
| duplicatePhoneError    | String        | Do not enter same Phone Number | Duplicate error message for phone number        |
| onMultiplePhone        | Function      |                                | When click on plus icon on Phone number field   |
| name                   | String        | Phone                          | The Phone name.                                 |
| index                  | Number        | 0                              | The Phone index.                                |
| values                 | Array[Object] | [{phone : ''}]                 | The Phone default array                         |
| isMultiple             | Boolean       | true                           | A flag to implement multiple Phone              |
| isShowPlus             | Boolean       | false                          | A flag to show plus icon for add multiple Phone |
| placeholder            | String        | Phone                          | The Phone placeholder                           |
| disabled               | Boolean       | false                          | Disable phone field                             |
| tabindex               | Number        | 0                              | The Phone tabIndex                              |
| preferredCountries     | Array[Object] | []                             | The Phone Code peferredCountries                |
| ignoredCountries       | Boolean       | false                          | The Phone country list not showing              |
| defaultSelectedCountry | Array[Object] | [{code: "IN",maxlength: 10}]   | The Phone default country                       |

## Methods

| Name            | Description                                             |
| --------------- | ------------------------------------------------------- |
| focus           | Gets triggered when the input field receives focus.     |
| blur            | Gets triggered when the input field loses focus.        |
| change          | Gets triggered every time input got changed.            |
| onChageDropdown | Gets triggered every time phone code got changed.       |
| input           | Gets triggered every time phone code value got changed. |

## Want to Contribute?

- Created something awesome, made this code better, added some functionality, or whatever (this is the hardest part).
- [Fork it](http://help.github.com/forking/).
- Create new branch to contribute your changes.
- Commit all your changes to your branch.
- Submit a [pull request](http://help.github.com/pull-requests/).

---

## Need Help?

We also provide a free, basic support for all users who want to use this VueJS Phone Number Component in their software project. In case you want to customize this Phone Number Component to suit your development needs, then feel free to contact our [VueJS developers](https://www.weblineindia.com/hire-vuejs-developer.html).

---

## Collection of Components

We have built many other components and free resources for software development in various programming languages. Kindly click here to view our [Free Resources for Software Development](https://www.weblineindia.com/communities.html)

---

## Changelog

Detailed changes for each release are documented in [CHANGELOG.md](./CHANGELOG.md).

## License

[MIT](LICENSE)

[mit]: https://github.com/weblineindia/Vue-Phone-Number/blob/master/LICENSE

## Keywords

vue-weblineindia-phone-number,phone,phonenumber,vue components,vuejs,vuejs component
