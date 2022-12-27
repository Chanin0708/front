<template>
  <div>
    <b-card style="width: 1400px; height: auto" class="mx-auto">
      <div>
        <!-- <b-button id="show-btn"  variant="success" class="mb-3 " style="justify-content: flex-end;" @click="$bvModal.show('bv-modal-example')>เพิ่มผลไม้ </b-button> -->
        <b-button id="show-btn" @click="$bvModal.show('bv-modal-example')" variant="success" class="mb-3">เพิ่มผลไม้</b-button>

        <!-- <b-modal id="show-btn" title="Add Values and Image" hide-footer>
    <b-form @submit.stop.prevent="onSubmit">
      <b-form-group label="Name : " label-for="value1">
        <b-form-input v-model="value1" :state="validation"></b-form-input>
      </b-form-group>
      <b-form-group label="Photo" label-for="image">
        <b-form-file v-model="image" accept="image/*"></b-form-file>
      </b-form-group>
      <b-button type="submit" variant="primary" >Add</b-button>
    </b-form>
  </b-modal> -->
  <b-modal id="bv-modal-example" hide-footer style="width: 500px; height: auto">
    <template #modal-title>
      Create
    </template>
    <div class="d-block">
      <b-form @submit.stop.prevent="onSubmit">
      <b-form-group label="Name : " label-for="value1">
        <b-form-input v-model="value1" :state="validation"></b-form-input>
      </b-form-group>
      <b-form-group label="Photo" label-for="image">
        <b-form-file v-model="image" accept="image/*"></b-form-file>
      </b-form-group>
    </b-form>
    </div>
    <b-button class="mt-3" block type="submit"  @click="save" variant="primary" >save</b-button>

    <b-button class="mt-3 ml-3" block type="submit" @click="$bvModal.hide('bv-modal-example')" variant="primary" >cancel</b-button>
  </b-modal>


        <div>
          <b-form-input
            v-model="text"
            placeholder="Search here"
            style="width: 200px; height: auto"
            class="mx-auto"
          ></b-form-input>
          <!-- <div class="mt-2">Value: {{ text }}</div> -->
        </div>
      </div>
    </b-card>
    <b-card style="width: 1400px; height: auto" class="mx-auto">
      <div v-if="fruitlist.length > 0">
        <div class="pt-3 mx-auto">
          <b-table :items="fruitlist" :fields="fields">
            <template v-slot:cell(name)="data">
              {{ data.item.name }}
            </template>
          </b-table>
        </div>
      </div>
    </b-card>

  </div>
</template>
<script>


export default {

  data() {
    return {
      value1: '',
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
  mounted(){
    this.getfruit();
  },
  methods: {
    getfruit() {
      console.log("aaaaa")
      this.axios.get("http://localhost:3000" + "/getdatafruit",{
        headers: {
          'Access-Control-Allow-Origin': '*',
        }
      }).then((response) =>{
        console.log("fdsfsdf")
        console.log(response.data.data)
        this.fruitlist=response.data.data;
        console.log(this.fruitlist)
      })
    }

  },
  save() {
    console.log(name)
      this.axios
        .post("http://localhost:3000" + "/upload", {
          name: this.category_name,
          author: "",
          photo: this.photo
          
        }).then((response) =>{
      //   console.log("fdsfsdf")
        console.log(response.message)
      //   this.fruitlist=response.data.data;
      //   console.log(this.fruitlist)
      })
    },
};
</script>
