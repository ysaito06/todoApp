<template>
  <v-card class="mx-auto" max-width="500">
    <v-toolbar color="pink" dark>
      <v-app-bar-nav-icon></v-app-bar-nav-icon>

      <v-toolbar-title>TODO</v-toolbar-title>

      <v-spacer></v-spacer>

      <v-btn icon>
        <v-icon>mdi-magnify</v-icon>
      </v-btn>

      <v-btn icon>
        <v-icon>mdi-checkbox-marked-circle</v-icon>
      </v-btn>
    </v-toolbar>

    <v-list v-for="item in items" :key="item.id" cols="12">
      <v-list-item-group>
        <v-list-item>
          <v-list-item-action>
            <v-checkbox
              :input-value="item.done"
              @click="isDone(item.id)"
            ></v-checkbox>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>{{ item.id }}</v-list-item-title>
          </v-list-item-content>
          <v-list-item-content>
            <v-list-item-title>{{ item.task }}</v-list-item-title>
          </v-list-item-content>
          <!-- アイテムの削除 -->
          <v-list-item-action>
            <v-btn @click.stop="deleteTask(item.id)" icon>
              <v-icon> mdi-backspace </v-icon></v-btn
            >
          </v-list-item-action>
        </v-list-item>
      </v-list-item-group>
    </v-list>
    <!-- アイテムの追加 -->
    <v-col cols="12" class="d-flex">
      <v-list-item-content>
        <v-form ref="add_form">
          <v-text-field
            v-model="todo"
            label="AddTodo"
            :rules="[required]"
          ></v-text-field>
        </v-form>
      </v-list-item-content>

      <v-list-item-action>
        <v-btn @click.stop="addTask(todo)" icon>
          <v-icon> mdi-plus-circle </v-icon>
        </v-btn>
      </v-list-item-action>
    </v-col>
  </v-card>
</template>
<script>
export default {
  data() {
    return {
      items: [],
      length: 0,
      required: (value) => !!value || "必ず入力してください", // 入力必須の制約
      todo: undefined,
    };
  },
  methods: {
    isDone(id) {
      const obj = this.items.find((e) => {
        e.id === id;
      });
      obj.done = !obj.done;
    },
    addTask(e) {
      if (this.$refs.add_form.validate()) {
        this.items.push({
          id: this.length,
          task: e,
          done: false,
        });
        this.length++;
        this.todo = undefined;
        this.$refs.add_form.reset();
      } else {
        return;
      }
    },
    deleteTask(id) {
      this.items = this.items.filter((e) => {
        return e.id !== id;
      });
    },
  },
};
</script>
