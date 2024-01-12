<template>
  <div id="app">
    <form action="" @submit.prevent="submit($event)">
      <p v-for="(item, index) in elems" :key="index">
        <input
          type="text"
          @blur="handleBlur(index)"
          :class="{
            /* you can replace this with a function if you like */
            'form-group--error': $v.elems.$each[index].$dirty && $v.elems.$each[index].$error /* these is the state class for error */,
            'form-group--success': $v.elems.$each[index].$dirty && !$v.elems.$each[index].$error /* these is the state class for success */,
          }"
          v-model="item.value"
        />
        <button @click="remove(index)" type="button">delete</button>
      </p>
      <br />
      <div>
        <button type="button" @click="add" class="btn-primary edit">Add</button>
        &nbsp;
        <button type="submit" class="btn-primary edit">Submit</button>
      </div>
    </form>
  </div>
</template>

<script>
/**
 * This example uses vuelidate for validation
 * http://vuelidate.js.org/
 *
 * REQUIREMENTS:
 *
 * 1. Implement the "add" and "delete" functionality
 * 2. Make the text input fields required by adding
 * validation rules for the `value` attribute of each elements
 * 3. On submit, vaidate the form and show an alert
 * with the message "Success" (if the form is valid)
 * or "Form not valid"
 * 4. Make the text input elements reflect their state (error/succes)
 * when the user tries to submit the form
 *
 * BONUS POINT A:
 * Make the text input elements change their state
 * under the following conditions:
 *    a) when the field is changed the first time
 *    b) on each keystroke afterwards
 * This means that if you start typing something in the first input
 * the field should not turn green as soon as I type the first letter
 * but it should turn green when I move the cursor to another field (blur)
 * However, if I return to tht same first input, on each keystroke
 * the status should be updated.
 *
 * Example:
 * 1. Type "abc" in the first field -> Nothing happens
 * 2. Move the cursor to the second field -> First field turns green
 * 3. Move the cursor back to the first field -> Nothing happens with the second field
 * 4. Hit Delete until the first field is empty -> As soon as the first field is empty, the field turns red
 *
 * BONUS POINT B:
 * Require that the first element in the list to be exactly equal to string "abc"
 *
 *
 *
 */

import { required } from "vuelidate/lib/validators";
export default {
  name: "App",
  data() {
    return {
      elems: [
        {
          value: "",
        },
        {
          value: "",
        },
      ],
    };
  },
  validations() {
    return {
      elems: {
        $each: {
          value: {
            required
          }
        }
      },
    };
  },
  methods: {
    submit: function () {
      this.$v.elems.$touch();
      if (this.$v.elems.$error) {
        alert('Form not valid');
      } else {
        alert('Success');
      }
    },
    add: function () {
      this.elems.push({value: ""});
    },
    remove: function (index) {
      this.elems.splice(index, 1);
    },
    handleBlur(index) {
      if (this.elems[index].value === "") {
        return;
      }
      this.$v.elems.$each[index].$touch()
    }
  },
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.form-group--error {
  border: 3px solid red;
}
.form-group--success {
  border: 3px solid green;
}
.pair {
  display: flex;
  grid-gap: 16px;
}
</style>
