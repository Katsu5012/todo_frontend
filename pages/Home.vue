<template>
  <v-app>
    <AfterLoginHeader />
    <div>
      <v-main style="background: #ddd8d8; height: auto">
        <v-row>
          <v-col class="mt-5 ml-10" cols="4">
            <v-row class="flex-column">
              <v-col>
                <v-text-field
                  v-model="dateRangeText"
                  label="Date range"
                  prepend-icon="mdi-calendar"
                  readonly
                ></v-text-field>
              </v-col>
              <v-col>
                <v-date-picker v-model="dates" range></v-date-picker>
              </v-col>
              <v-btn class="mt-3">取得</v-btn>
            </v-row>
          </v-col>
          <v-col>
            <v-row>
              <v-col>
                <div>
                  <h3>完了（{{ completeToDos.length }}）</h3>
                  <draggable
                    v-model="completeToDos"
                    group="myGroup"
                    :options="options"
                    @choose="onChoose(true)"
                    @start="onStart"
                    @clone="onClone"
                    @add="onAdd"
                    @remove="onRemove(true)"
                    @update="onUpdate"
                    @sort="onSort"
                    @filter="onFilter"
                    @end="onEnd"
                  >
                    <div v-for="item in completeToDos" :key="item.todo_id">
                      <v-col class="white mt-3 mr-10 ml-10">
                        <p>{{item.todo_id}}</p>
                        <p>やること:{{ item.contents }}</p>
                        <p>期限:{{ item.deadline }}</p>
                      </v-col>
                    </div>
                  </draggable>
                </div>
              </v-col>
              <v-col>
                <div>
                  <h3>未完了({{ inCompleteToDos.length }}）</h3>
                  <draggable
                    v-model="inCompleteToDos"
                    group="myGroup"
                    :options="options"
                    @choose="onChoose(false)"
                    @start="onStart"
                    @clone="onClone"
                    @add="onAdd"
                    @remove="onRemove(false)"
                    @update="onUpdate"
                    @sort="onSort"
                    @filter="onFilter"
                    @end="onEnd"
                  >
                    <div v-for="item in inCompleteToDos" :key="item.todo_id">
                      <v-col class="white mt-3 mr-20 ml-10">
                        <p>{{item.todo_id}}</p>
                        <p>やること:{{ item.contents }}</p>
                        <p>期限:{{ item.deadline }}</p>
                      </v-col>
                    </div>
                  </draggable>
                </div>
              </v-col>
            </v-row>
          </v-col>
        </v-row>
      </v-main>
    </div>
  </v-app>
</template>
<script lang='ts'>
import Vue from "vue";
import draggable from "vuedraggable";
import ToDos from "../components/ToDos.vue";
function dumpObj(obj: any): string {
  return JSON.stringify(obj, null, 2);
}
type User = {
  user_id: Number;
  user_name: String;
};
type ToDo = {
  todo_id: Number;
  contents: String;
  completion: Boolean;
  deadline: String;
  fixed: Boolean;
};
export default Vue.extend({
  components: {
    draggable,
    ToDos,
  },
  data() {
    return {
      options: {
        group: "myGroup",
        animation: 200,
        filter: ".fixed",
      },

      status: {
        moving: false,
        fixed: false,
      },
      inCompleteToDos: [
        {
          todo_id: 11,
          contents: "nuxtやる",
          completion: true,
          deadline: "2021-10-11",
          fixed: false,
        },
        {
          todo_id: 2,
          contents: "aws勉強する",
          completion: false,
          deadline: "2021-10-20",
          fixed: false,
        },
      ] as ToDo[],
      dates: [] as String[],
      completeToDos: [
        {
          todo_id: 3,
          contents: "laravel勉強する",
          completion: true,
          deadline: "2021-10-19",
          fixed: false,
        },
      ] as ToDo[],
      beforeArray: [] as ToDo[],
    };
  },
  computed: {
    dateRangeText(): String {
      return this.dates.join(" ~ ");
    },
    formattedItems1(): string {
      return dumpObj(this.completeToDos);
    },
    formattedItems2(): string {
      return dumpObj(this.inCompleteToDos);
    },
  },
  created() {
    const sixDaysAgo: Date = this.getDay();
    sixDaysAgo.setDate(sixDaysAgo.getDate() - 6);
    const today: Date = new Date();

    console.log(today);
  },
  methods: {
    onFilter() {
      console.log("onFilter");
      this.status.fixed = true;
      setTimeout(() => {
        this.status.fixed = false;
      }, 1000);
    },
    // 選択された時（chooseイベント）
    onChoose(data: Boolean): void {
      console.log("onChoose");
      this.beforeArray = data ? this.completeToDos : this.inCompleteToDos;
      console.log(data);
      console.log(this.beforeArray);
    },
    // 動作が始まった時（startイベント）
    onStart() {
      console.log("onStart");
      this.status.moving = true;
    },
    // 動作が開始され要素のコピーが行われた時（cloneイベント）
    onClone() {
      console.log("onClone");
    },
    // リストの更新が行われた時（updateイベント）
    onUpdate() {
      console.log("onUpdate");
    },
    // リストに要素が加えられた時（addイベント）
    onAdd(e) {
      console.log("onAdd");
    },
    // リストから要素が除去された時（removeイベント）
    onRemove(data: Boolean) {
      console.log("onRemove");
      console.log("kkkk");
      this.getArrayDifference(data);
    },
    // 並び替えが行われた時（sortイベント）
    onSort() {
      console.log("onSort");
    },
    // 動作が終わった時（endイベント）
    onEnd(e) {
      console.log("onEnd");
      this.status.moving = false;
    },
    getArrayDifference(data: Boolean) {
      const afterArray = data ? this.completeToDos : this.inCompleteToDos;
      console.log(afterArray);

      const b = this.beforeArray.map((item) => item["todo_id"]);
      console.log(b);
      const a = afterArray.map((item) => item["todo_id"]);
      console.log(a);
      let difference=a.filter((i)=>{return b.indexOf(i) == -1})
      console.log(difference)
    },
    getDay(): Date {
      return new Date();
    },
    dateToZeroPaddingString(date: Date): String {
      return `${("0000" + date.getFullYear()).slice(-4)}-${(
        "00" +
        (date.getMonth() + 1)
      ).slice(-2)}-${("00" + date.getDate()).slice(-2)}`;
    },
    resetCalenderDate(): void {
      this.dates = [];
    },
  },
});
</script>
<style scoped>
#status {
  min-height: 25px;
  border: 1px solid #42b983;
}
.item {
  display: inline-block;
  margin: 10px;
  padding: 10px;
  border: 1px solid #7f7f7f;
  border-radius: 10px;
  background-color: #ffffff;
}
</style>