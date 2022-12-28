<template>
  <div>
    <div>
      <b-card style="width: 1400px; height: auto" class="mx-auto">
        <div class="row">

          <div class="col-12 d-flex justify-content-end">
            <b-button id="show-btn" @click="$bvModal.show('bv-modal-example')" variant="success"
              class="mb-3 mr-3">เพิ่มผลไม้</b-button>
          </div>
          <div>
            <b-modal id="bv-modal-example" hide-footer style="width: 500px; height: auto">
              <template #modal-title>
                Create
              </template>
              <div class="d-block">
                <b-form @submit.stop.prevent="onSubmit">
                  <b-form-group label="Name : " label-for="name">
                    <b-form-input v-model="name"></b-form-input>
                  </b-form-group>
                  <b-form-group label="Photo" label-for="photo">
                    <b-form-file v-model="photo" accept="image/*"></b-form-file>
                  </b-form-group>
                  <div class="d-flex justify-content-end">
                    <b-button class="mt-3 mx-2" block type="submit" variant="primary">save</b-button>
                    <b-button class="mt-3 mx-2" block @click="$bvModal.hide('bv-modal-example')"
                      variant="danger">cancel</b-button>
                  </div>
                </b-form>
              </div>
            </b-modal>
          </div>

          <div class="col-12 d-flex justify-content-end">
            <b-form-input v-model="search" placeholder="Search for names" @input.native="searchName"
              style="width: 200px; height: auto"></b-form-input>
          </div>
        </div>
      </b-card>
    </div>

    <b-card style="width: 1400px; height: auto" class="mx-auto">
      <div v-if="fruitlist.length > 0">
        <div class="pt-3 mx-auto">
          <b-table striped hover :items="fruitlist"
            :fields="[{ key: 'name', label: 'ชื่อผลไม้' }, { key: 'photo', label: '' }]">
            <template v-slot:cell(name)="data" v-if="showNameColumn">
              <div style="font-size: 90px;">{{ data.item.name }}</div>
            </template>
            <template v-slot:cell(photo)="data">
              <img :src="'data:image/jpeg;base64,' + data.item.photo" alt="fruit photo" width="200" height="200" />
            </template>
          </b-table>
        </div>
      </div>
    </b-card>

  </div>
</template>
<script>
import axios from 'axios';

export default {

  data() {
    return {
      search: '',
      data: [],
      name: '',
      image: null,
      fruitlist: [],
      fields: [
        { key: "name", label: "ชื่อผลไม้" },
        { key: "image", label: "" },
      ],
      items: [{ name: "John", age: 30, image: "../public/Red_Apple.jpg" }],
      text: "",
    };
  },
  mounted() {
    this.getfruit();
    this.save();
    this.searchName();
  },
  computed: {
    filteredData() {
      return this.data.filter((item) => item.name.toLowerCase().includes(this.search.toLowerCase()));
    }
  },
  methods: {
    searchName() {
      console.log("aaaaaa")
      axios
        .get(`http://localhost:3000/searchdatafruit/?name=${this.search}`)
        .then((response) => {
          console.log("fsdfsdfs")
          console.log(response.data.data)
          this.fruitlist = response.data.data;
        })
        .catch((error) => {
          console.log(error);
        });
    },
    onSubmit() {
      const formData = new FormData();
      formData.append('name', this.name);
      formData.append('photo', this.photo);

      axios
        .post("http://localhost:3000" + "/upload", formData)
        .then((response) => {
          // console.log(response.data);
          // Close the modal
          this.$bvModal.hide('bv-modal-example');
          // Display a notification based on the response message
          if (response.data.message === 'Please provide a value, and photo.') {
            this.$bvToast.toast('Error: ' + response.data.message, {
              title: 'Error',
              variant: 'danger',
              autoHideDelay: 0,
              toaster: 'b-toaster-top-center'
            });
          } else if (response.data.message === 'Value and photo successfully added.') {
            this.$bvToast.toast(response.data.message, {
              title: 'Success',
              variant: 'success',
              autoHideDelay: 0,
              toaster: 'b-toaster-top-center'
            });
            // Refresh the page
            location.reload();
          }
        })
        .catch((error) => {
          // console.log(error);
          // Check the status code
          if (error.response.status === 400) {
            // Display a notification that stays on the screen until the user closes it
            this.$bvToast.toast('Error: ' + error.response.data.message, {
              title: 'Error',
              variant: 'danger',
              autoHideDelay: 0,
              toaster: 'b-toaster-top-center'
            });
          } else {
            // Display a notification with a default duration
            this.$bvToast.toast('Error: ' + error, {
              title: 'Error',
              variant: 'danger',
              autoHideDelay: 3000,
              toaster: 'b-toaster-top-center'
            });
          }
        });
    },
    getfruit() {
      // console.log("aaaaa")
      this.axios.get("http://localhost:3000" + "/getdatafruit", {
        headers: {
          'Access-Control-Allow-Origin': '*',
        }
      }).then((response) => {
        // console.log("fdsfsdf")
        console.log(response.data.data)
        this.fruitlist = response.data.data;
        // console.log(this.fruitlist)
      })
    }
  }



};
</script>
<style>
.large-font {
  font-size: 60px;
}
</style>