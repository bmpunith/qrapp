
<template>
  <div class="container">

  <div class="card o-hidden border-0 shadow-lg">
      <div class="card-body p-0">
          <!-- Nested Row within Card Body -->
          <div class="row">
              <div class="col-lg-12">
                  <div class="p-5">
                      <div class="text-center">
                          <h1 class="h4 text-gray-900 mb-4">Generate QR Code</h1>
                      </div>
                      <form class="user" @submit.prevent="onSubmit">
                          <div class="form-group">
                              <input type="text" class="form-control form-control-user" v-model="form.data"
                                  placeholder="Data">
                          </div>
                          <div class="form-group" style="display: flex; justify-content: center;">
                            <button class="btn btn-primary btn-user btn-block" style="width: 250px">
                                Generate
                            </button>
                          </div>
                      </form>
                      <img :src="qrCodeUrl">
                      <hr>
                      <a v-if="qrCodeUrl" :href="qrCodeUrl" download="qr_code.png">Download QR Code</a>
                  </div>
              </div>
          </div>
      </div>
  </div>

  </div>
</template>

<script>
import axios from 'axios';
export default {
  data() {
    return {
      form: {
        data: ""
      },
      selectedText: "",
      responseData: "",
      qrCodeUrl: ""
    };
  },
  methods: {
    async onSubmit() {
      console.log("Data ", this.form)
      try {
        const response = await axios.get('https://api.qrserver.com/v1/create-qr-code/', {
          params: {
            data: this.form.data,
            size: '100x100'
          },
          responseType: 'blob' // Set the response type to blob
        });
        // Convert the blob to a data URL
        this.qrCodeUrl = URL.createObjectURL(response.data);
        console.log(this.qrCodeUrl)
      } catch (error) {
        console.error('Error fetching data:', error);
      }
    }
  }
};
</script>

<style scoped>
h1 {
  font-weight: 500;
  font-size: 2.6rem;
  position: relative;
  top: -10px;
}

h3 {
  font-size: 1.2rem;
}

.greetings h1,
.greetings h3 {
  text-align: center;
}

@media (min-width: 1024px) {
  .greetings h1,
  .greetings h3 {
    text-align: left;
  }
}

.container {
  width: 1140px; /* Limit the maximum width of input boxes */
}
</style>
