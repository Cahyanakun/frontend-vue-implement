<template>
  <div class="gradient-custom">
    <div class="container py-2 h-100">
      <div class="row d-flex justify-content-center align-items-center h-100">
        <div class="col col-xl-10">
          <div class="card">
            <div class="card-header">
              <h5 class="py-2 text-center">ToDo App Vue - Javascript</h5>
            </div>
            <div class="card-body p-5">
              <b-row class="my-1">
                <b-col sm="12">
                  <b-form-input
                    id="input-default"
                    v-model="filter"
                    placeholder="Find Task"
                  ></b-form-input>
                </b-col>
              </b-row>
              <b-table
                class="mt-top"
                striped
                hover
                :items="items"
                :fields="fields"
                :filter="filter"
                ref="tableData"
              >
                <template #cell(status)="data">
                  <h6 class="mb-0">
                    <span
                      v-if="data.item.status == true"
                      class="badge bg-success"
                      >Done</span
                    >
                    <span v-else class="badge bg-warning">Waiting</span>
                  </h6>
                </template>

                <template #cell(action)="data">
                  <b-button
                    class="mars"
                    v-b-tooltip.hover.left="'Done Task'"
                    variant="outline-success"
                    size="sm"
                    v-if="data.item.status == false"
                    @click="checkStatus(data)"
                  >
                    <b-icon icon="check-circle"></b-icon>
                  </b-button>

                  <b-button
                    class="mars"
                    v-b-tooltip.hover.top="'Edit Task'"
                    variant="outline-warning"
                    size="sm"
                    @click="updateTask(data)"
                  >
                    <b-icon icon="pencil"></b-icon>
                  </b-button>

                  <b-button
                    variant="outline-danger"
                    v-b-tooltip.hover.right="'Delete Task'"
                    size="sm"
                    @click="deleteTask(data.item)"
                  >
                    <b-icon icon="trash"></b-icon>
                  </b-button>
                </template>
              </b-table>
              <div class="text-end">
                <b-row class="my-1">
                  <b-col sm="11">
                    <b-form-input
                      id="input-default"
                      placeholder="Enter Task ....."
                      v-model="task"
                    ></b-form-input>
                  </b-col>
                  <b-col sm="1">
                    <b-button
                      v-if="edit == false"
                      v-b-tooltip.hover.left="'Add Task'"
                      variant="success"
                      class="px-4"
                      @click="addTask"
                    >
                      <b-icon icon="plus"></b-icon>
                    </b-button>

                    <b-button
                      v-if="edit == true"
                      v-b-tooltip.hover.left="'Edit Task'"
                      variant="warning"
                      class="px-4"
                      @click="saveEditedTask"
                    >
                      <b-icon icon="pencil"></b-icon>
                    </b-button>
                  </b-col>
                </b-row>
              </div>
            </div>
          </div>
        </div>

        <div class="col col-xl-10 my-5">
          <div class="card">
            <div class="card-header">
              <h5 class="py-2 text-center">
                Free API - Game Of Thrones Random Quotes
              </h5>
            </div>
            <div class="card-body">
              <div class="row">
                <div
                  class="col-md-6 mb-3"
                  v-for="(i, index) in model"
                  :key="index"
                >
                  <div class="cards">
                    <blockquote class="blockquote text-center text-success p-4">
                      <p>
                        {{ i.sentence }}
                      </p>
                      <footer class="blockquote-footer">
                        {{ i.character.name }},
                        <cite title="The Greek Way">{{
                          i.character.house.name
                        }}</cite>
                      </footer>
                    </blockquote>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { BIcon, BButton } from "bootstrap-vue";
export default {
  name: "HelloWorld",
  components: {
    BIcon,
    BButton,
  },
  data() {
    return {
      model: [],
      filter: "",
      task: "",
      itemEdit: {},
      edit: false,
      fields: [
        {
          key: "task",
          label: "Task",
        },
        {
          key: "status",
          label: "Status",
        },

        {
          key: "action",
        },
      ],
      items: [
        { task: "Reading Book", status: true },
        { task: "Checking Task Job", status: true },
        { task: "Daily Meet", status: false },
      ],
    };
  },
  methods: {
    addTask() {
      if (this.task == "") {
        alert("silahkan isi terlebih dahulu");
      } else {
        this.items.push({
          task: this.task,
          status: false,
        });
      }
      this.task = "";
    },

    deleteTask(index) {
      this.items.splice(
        this.items.findIndex((item) => item.task === index.task),
        1
      );
    },
    updateTask({ item = "", index = "" }) {
      this.edit = true;
      this.task = item.task;
      this.itemEdit = {
        item,
        index,
      };
    },
    checkStatus({ item = "", index = "" }) {
      this.items[index] = {
        task: item.task,
        status: true,
      };
      this.$refs.tableData.refresh();
    },
    saveEditedTask() {
      this.items[this.itemEdit.index] = {
        task: this.task,
        status: this.itemEdit.item.status,
      };
      this.$refs.tableData.refresh();
      this.edit = false;
      this.task = "";
    },
  },

  created() {
    axios
      .get(`https://api.gameofthronesquotes.xyz/v1/random/6`)
      .then((response) => {
        // JSON responses are automatically parsed.
        this.model = response.data;
        console.log(this.model);
      })
      .catch((e) => {
        this.errors.push(e);
      });
  },
};
</script>

<style>
.gradient-custom {
  background: radial-gradient(50% 123.47% at 50% 50%, #00ff94 0%, #720059 100%),
    linear-gradient(121.28deg, #669600 0%, #ff0000 100%),
    linear-gradient(360deg, #0029ff 0%, #8fff00 100%),
    radial-gradient(100% 164.72% at 100% 100%, #6100ff 0%, #00ff57 100%),
    radial-gradient(100% 148.07% at 0% 0%, #fff500 0%, #51d500 100%);
  background-blend-mode: screen, color-dodge, overlay, difference, normal;
}
.mars {
  margin-right: 5px !important;
}

.mt-top {
  margin-top: 20px !important;
}
.cards {
  /* Add shadows to create the "card" effect */
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
  transition: 0.3s;
  min-height: 160px !important;
}

/* On mouse-over, add a deeper shadow */
.cards:hover {
  box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2);
}
</style>
