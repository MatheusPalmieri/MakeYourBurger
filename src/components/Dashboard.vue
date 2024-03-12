<template>
  <Message :message="message" v-show="message" />

  <div id="table">
    <div>
      <div id="table-heading">
        <div class="order-id">#</div>
        <div>Clients</div>
        <div>Bread</div>
        <div>Meat</div>
        <div>Options</div>
        <div>Actions</div>
      </div>
    </div>

    <div id="table-rows">
      <div class="table-row" v-for="burger in burgers" :key="burger.id">
        <div class="order-number">{{ burger.id }}</div>
        <div>{{ burger.name }}</div>
        <div>{{ burger.bread }}</div>
        <div>{{ burger.meat }}</div>

        <div>
          <ul>
            <li v-for="(optional, index) in burger.options" :key="index">
              {{ optional }}
            </li>
          </ul>
        </div>

        <div class="actions">
          <select
            name="status"
            class="status"
            @change="updateBurger($event, burger.id)"
          >
            <option
              v-for="statusItem in status"
              :key="statusItem.id"
              :value="statusItem.type"
              :selected="burger.status == statusItem.type"
            >
              {{ statusItem.type }}
            </option>
          </select>

          <button class="delete-button" @click="deleteBurger(burger.id)">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="24"
              height="24"
              viewBox="0 0 24 24"
              fill="currentColor"
              class="icon icon-tabler icons-tabler-filled icon-tabler-trash-x"
            >
              <path stroke="none" d="M0 0h24v24H0z" fill="none" />
              <path
                d="M20 6a1 1 0 0 1 .117 1.993l-.117 .007h-.081l-.919 11a3 3 0 0 1 -2.824 2.995l-.176 .005h-8c-1.598 0 -2.904 -1.249 -2.992 -2.75l-.005 -.167l-.923 -11.083h-.08a1 1 0 0 1 -.117 -1.993l.117 -.007h16zm-9.489 5.14a1 1 0 0 0 -1.218 1.567l1.292 1.293l-1.292 1.293l-.083 .094a1 1 0 0 0 1.497 1.32l1.293 -1.292l1.293 1.292l.094 .083a1 1 0 0 0 1.32 -1.497l-1.292 -1.293l1.292 -1.293l.083 -.094a1 1 0 0 0 -1.497 -1.32l-1.293 1.292l-1.293 -1.292l-.094 -.083z"
              />
              <path
                d="M14 2a2 2 0 0 1 2 2a1 1 0 0 1 -1.993 .117l-.007 -.117h-4l-.007 .117a1 1 0 0 1 -1.993 -.117a2 2 0 0 1 1.85 -1.995l.15 -.005h4z"
              />
            </svg>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Message from "./Message.vue";

export default {
  name: "Dashboard",
  components: {
    Message,
  },
  data() {
    return {
      burgers: null,
      burger_id: null,
      status: [],
      message: null,
    };
  },
  methods: {
    async getRequests() {
      const req = await fetch("http://localhost:3000/burgers");
      const data = await req.json();

      this.burgers = data;

      this.getStatus();
    },

    async getStatus() {
      const req = await fetch("http://localhost:3000/status");
      const data = await req.json();

      this.status = data;
    },

    async deleteBurger(id) {
      const req = await fetch(`http://localhost:3000/burgers/${id}`, {
        method: "DELETE",
      });
      const res = await req.json();

      this.getRequests();

      this.message = `Request Nº ${res.id} was deleted!`;

      setTimeout(() => (this.message = ""), 2000);
    },

    async updateBurger(event, id) {
      const option = event.target.value;

      const dataJson = JSON.stringify({ status: option });

      const req = await fetch(`http://localhost:3000/burgers/${id}`, {
        method: "PATCH",
        headers: { "Content-Type": "application/json" },
        body: dataJson,
      });

      const res = await req.json();

      this.message = `Request Nº ${res.id} was updated to ${res.status}!`;

      setTimeout(() => (this.message = ""), 3000);
    },
  },
  mounted() {
    this.getRequests();
  },
};
</script>

<style scoped>
#table {
  max-width: 1200px;
  margin: 0 auto;
}

#table-heading,
#table-rows,
.table-row {
  display: flex;
  flex-wrap: wrap;
}

#table-heading {
  font-weight: bold;
  padding: 12px;
  border-bottom: 3px solid #333;
}

.table-row {
  width: 100%;
  padding: 12px;
  border-bottom: 1px solid #ccc;
}

#table-heading div,
.table-row div {
  width: 19%;
}

#table-heading .order-id,
.table-row .order-number {
  width: 5%;
}

.actions {
  display: flex;
  align-items: center;
  gap: 10px;
}

select {
  height: 40px;
  padding: 12px 6px;
  border-radius: 5px;
}

.delete-button {
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #222;
  border: 2px solid #222;
  color: #fcba03;
  padding: 10px;
  border-radius: 5px;
  cursor: pointer;
  transition: 0.5s;
}

.delete-button:hover {
  background-color: transparent;
  color: #222;
}
</style>
