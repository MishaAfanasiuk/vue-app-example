<template>
  <div>
    <b-form @submit="onSubmit">
      <b-form-group
        id="input-group-1"
        label="Email address:"
        label-for="input-1"
      >
        <b-form-input
          id="input-1"
          v-model.trim="$v.email.$model"
          :state="email.length === 0 ? null : !isFormValid"
          type="email"
          placeholder="Enter email"
          required
          @input='setEmail'
        />
        <b-form-invalid-feedback v-if="!$v.email.required">
          {{ $t("components.form.emailShouldNotBeEmpty") }}
        </b-form-invalid-feedback>
        <b-form-invalid-feedback v-if="!$v.email.minLength">
          {{ `${$t("components.form.emailLengthShouldBeLess")} ${$v.email.$params.minLength.min}` }}
        </b-form-invalid-feedback>
        <b-form-invalid-feedback v-if="!$v.email.maxLength">
          {{ `${$t("components.form.emailLengthShouldBeGreater")} ${$v.email.$params.maxLength.max}` }}
        </b-form-invalid-feedback>
        <b-form-invalid-feedback v-if="!$v.email.isEmail">
          {{ $t("components.form.shouldBeEmail") }}
        </b-form-invalid-feedback>
      </b-form-group>

      <b-button type="submit" variant="primary" :disabled="isFormValid">
        {{ $t("components.form.submit") }}
      </b-button>
    </b-form>
  </div>
</template>

<script lang='ts'>
import { Component, Mixins } from 'vue-property-decorator'
import { validationMixin } from 'vuelidate'
import { required, minLength, maxLength } from 'vuelidate/lib/validators'
import { isEmail } from '@/utils/isEmail'

const validations = {
  email: {
    required,
    minLength: minLength(5),
    maxLength: maxLength(100),
    isEmail
  }
}

@Component({
  mixins: [validationMixin],
  validations
})
export default class Form extends Mixins(validationMixin) {
  private email: string = '';

  get isFormValid(): boolean | null {
    return this.email.length > 0 && this.$v.$error
  }

  onSubmit(event: Event): void {
    event.preventDefault()
    this.$router.push('/result');
  };

  setEmail(email: string): void {
    this.email = email;
    this.$v.email.$touch();
  }
}
</script>
