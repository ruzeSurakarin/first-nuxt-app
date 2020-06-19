<template>
  <div>
    <a-layout>
      <a-layout-header>
        <p>Log In</p>
      </a-layout-header>

      <a-layout-content>
        <div class="content-align">
          <div class="profile">
            <div class="user-icon">
              <a-icon type="setting" theme="filled" spin />
            </div>
          </div>
          <a-form :form="form" @submit="handleSubmit">
            <a-form-item
              :validate-status="userNameError() ? 'error' : ''"
              :help="userNameError() || ''"
            >
              <a-input
                v-decorator="[
                  'userName',
                  {
                    rules: [
                      { required: true, message: 'Please input your username!' }
                    ]
                  }
                ]"
                placeholder="Username"
              >
                <a-icon
                  slot="prefix"
                  type="user"
                  style="color:rgba(0,0,0,.25)"
                />
              </a-input>
            </a-form-item>

            <a-form-item
              :validate-status="passwordError() ? 'error' : ''"
              :help="passwordError() || ''"
            >
              <a-input
                v-decorator="[
                  'password',
                  {
                    rules: [
                      { required: true, message: 'Please input your Password!' }
                    ]
                  }
                ]"
                type="password"
                placeholder="Password"
              >
                <a-icon
                  slot="prefix"
                  type="lock"
                  style="color:rgba(0,0,0,.25)"
                />
              </a-input>
            </a-form-item>
            <a-form-item class="submit-btn">
              <a-button
                type="primary"
                html-type="submit"
                :disabled="hasErrors(form.getFieldsError())"
              >
                Submit
              </a-button>
            </a-form-item>
          </a-form>
        </div>
      </a-layout-content>
    </a-layout>
  </div>
</template>

<script>
function hasErrors(fieldsError) {
  return Object.keys(fieldsError).some((field) => fieldsError[field])
}

export default {
  beforeCreate() {
    // const liff = window.liff
    // liff.init(
    //   { liffId: '1654303428-MvXPxLdr' },
    //   () => {
    //     if (liff.isLoggedIn()) {
    //     //   this.getProfile()
    //     } else {
    //       liff.login()
    //     }
    //   },
    //   (err) => console.error(err.code, err.message)
    // )
  },
  data() {
    return {
      hasErrors,
      form: this.$form.createForm(this, { name: 'horizontal_login' })
    }
  },
  mounted() {
    this.$nextTick(() => {
      // To disabled submit button at the beginning.
      this.form.validateFields()
    })
  },
  methods: {
    // Only show error after a field is touched.
    userNameError() {
      const { getFieldError, isFieldTouched } = this.form
      return isFieldTouched('userName') && getFieldError('userName')
    },
    // Only show error after a field is touched.
    passwordError() {
      const { getFieldError, isFieldTouched } = this.form
      return isFieldTouched('password') && getFieldError('password')
    },
    handleSubmit(e) {
      e.preventDefault()
      this.form.validateFields((err, values) => {
        if (!err) {
          console.log('Received values of form: ', values, process.env.BASE_URL)
        }
      })
    }
  }
}
</script>

<style lang="less">
.content-align {
  max-width: 300px;
  margin: 0 auto;

  .submit-btn {
    text-align: center;
  }
}

.profile {
  width: 100%;
  font-size: 90px;
  text-align: center;
  margin: 30px 0 30px 0;
  .user-icon {
    position: relative;
    margin: 0 auto;
    width: 120px;
    height: 120px;
    background: white;
    border-radius: 50%;
    box-shadow: 0 0 8px -2px;
    i {
      top: 50%;
      left: 50%;
      position: absolute;
      transform: translate(-50%, -50%);
    }
  }
}
</style>
