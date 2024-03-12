<template>
  <div>
    <Message :message="message" v-show="message" />

    <div>
      <form id="form" @submit="createBurger">
        <div class="input">
          <label for="name" class="required">Name</label>

          <input
            type="text"
            name="name"
            id="name"
            v-model="name"
            placeholder="Type your name"
          />
        </div>

        <div class="input">
          <label for="bread" class="required">Choose bread</label>

          <select
            name="bread"
            id="bread"
            v-model="bread"
            onchange="this.style.borderColor = (this.value !== '') ? '#fcba03' : '#ccc';"
          >
            <option value="" disabled selected>Select your bread</option>
            <option v-for="bread in breads" :key="bread.id" :value="bread.type">
              {{ bread.type }}
            </option>
          </select>
        </div>

        <div class="input">
          <label for="meat" class="required"
            >Choose the meat for your Burger</label
          >

          <select
            name="meat"
            id="meat"
            v-model="meat"
            onchange="this.style.borderColor = (this.value !== '') ? '#fcba03' : '#ccc';"
          >
            <option value="" disabled selected>Select your meet</option>
            <option v-for="meat in meats" :key="meat.id" :value="meat.type">
              {{ meat.type }}
            </option>
          </select>
        </div>

        <div id="options" class="input">
          <label for="options" id="options-title">Select extras</label>

          <div class="options">
            <div
              class="checkbox"
              v-for="optionalItem in optionsData"
              :key="optionalItem.id"
              @click="addExtra(optionalItem.type)"
            >
              <input
                type="checkbox"
                name="optional"
                id="optional"
                v-model="options"
                :value="optionalItem.type"
              />
              <span>{{ optionalItem.type }}</span>
            </div>
          </div>
        </div>

        <div class="input">
          <button type="submit" class="button">Make Burger</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import Message from "./Message.vue";

export default {
  name: "BurgerForm",
  components: {
    Message,
  },
  data() {
    return {
      name: null,
      meat: null,
      meats: null,
      bread: null,
      breads: null,
      optionsData: null,
      options: [],
      message: null,
    };
  },

  methods: {
    async getIngredients() {
      const req = await fetch("http://localhost:3000/ingredients");
      const { meats, breads, options } = await req.json();

      this.meats = meats;
      this.breads = breads;
      this.optionsData = options;
    },

    addExtra(type) {
      if (this.options.includes(type)) {
        this.options = this.options.filter((item) => item !== type);
        this.options = Array.from(this.options);
        return;
      }

      this.options.push(type);
    },

    async createBurger(event) {
      event.preventDefault();

      if (!this.name || !this.meat || !this.bread) {
        this.message = "Please fill all the fields!";
        setTimeout(() => (this.message = ""), 4000);
        return;
      }

      const data = {
        name: this.name,
        meat: this.meat,
        bread: this.bread,
        options: Array.from(this.options),
        status: "Requested",
      };

      const dataJson = JSON.stringify(data);
      const req = await fetch("http://localhost:3000/burgers", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: dataJson,
      });

      const res = await req.json();

      this.message = `Request Nº. ${res.id} completed successfully!`;

      setTimeout(() => (this.message = ""), 4000);

      this.name = "";
      this.meat = "";
      this.bread = "";
      this.options = "";
    },
  },

  mounted() {
    this.getIngredients();
  },
};
</script>

<style scoped>
#form {
  max-width: 400px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.input {
  display: flex;
  flex-direction: column;
}

label {
  font-weight: bold;
  margin-bottom: 15px;
  color: #222;
  padding: 5px 10px;
  border-left: 4px solid #fcba03;
}

.required::after {
  content: "*";
  color: #e93737;
  margin-left: 2px;
}

input,
select {
  width: 100%;
  height: 40px;
  padding: 5px 10px;
  border-radius: 5px;
  border: 2px solid #ccc;
  transition: 0.3s border ease-in-out;
}

input:focus,
select:focus {
  outline: none;
  border: 2px solid #fcba03;
}

input:not(:placeholder-shown) {
  border-color: #fcba03;
}

.options {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
  justify-content: space-between;
  gap: 10px;
}

#options-title {
  width: 100%;
}

.checkbox {
  width: 48%;
  height: 40px;
  padding-inline: 10px;
  display: flex;
  align-items: center;
  justify-content: flex-start;
  gap: 10px;
  background-color: #fcba03;
  border-radius: 5px;
  cursor: pointer;
  transition: 0.3s;
}

.checkbox:hover {
  background-color: #daa515;
}

.checkbox span,
.checkbox input {
  width: auto;
}

.checkbox input {
  width: 20px;
  height: 20px;
}

.checkbox span {
  color: #222;
  font-weight: bold;
}

@media (max-width: 768px) {
  #options {
    flex-direction: column;
  }

  .checkbox {
    width: 100%;
  }
}

.button {
  width: 100%;
  height: 50px;
  background-color: #222;
  color: #fcba03;
  font-weight: bold;
  border: 2px solid #222;
  padding: 10px;
  font-size: 16px;
  margin: 0 auto;
  cursor: pointer;
  border-radius: 5px;
  transition: 0.5s;
}

.button:hover {
  background-color: transparent;
  color: #222;
}
</style>
