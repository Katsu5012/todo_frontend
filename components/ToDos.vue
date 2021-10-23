<template>
  <div>
    <div id="status">
      <span v-show="status.moving">移動中</span>
      <span v-show="status.fixed">そのアイテムは操作できません</span>
    </div>
    <div>
      <h3>完了（{{ completeToDos.length }}）</h3>
      <draggable
        v-model="completeToDos"
        group="myGroup"
        :options="options"
        @choose="onChoose"
        @start="onStart"
        @clone="onClone"
        @add="onAdd"
        @remove="onRemove"
        @update="onUpdate"
        @sort="onSort"
        @filter="onFilter"
        @end="onEnd"
        v-for="item in completeToDos"
        :key="item.todo_id"
      >
        <p>
          {{ item.contents }}
        </p>
      </draggable>
    </div>
    <div>
      <h3>未完了({{ inCompleteToDos.length }}）</h3>
      <draggable
        v-model="inCompleteToDos"
        group="myGroup"
        :options="options"
        @choose="onChoose"
        @start="onStart"
        @clone="onClone"
        @add="onAdd"
        @remove="onRemove"
        @update="onUpdate"
        @sort="onSort"
        @filter="onFilter"
        @end="onEnd"
        v-for="item in inCompleteToDos"
        :key="item.todo_id"
      >
        <p>{{item.contents}}</p>
      </draggable>
    </div>
  </div>
</template>
  
  <script>
import draggable from "vuedraggable";
import Vue from "vue";

export default Vue.extend({
  props: ["inCompleteToDos", "completeToDos"],
  components: { draggable },

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
    };
  },

  computed: {
    isFixed() {
      return (fixed) => {
        return {
          fixed: fixed === true,
        };
      };
    },
  },

  methods: {
    // フィルタされている要素を選択した時（filterイベント）
    onFilter() {
      console.log("onFilter");
      this.status.fixed = true;
      setTimeout(() => {
        this.status.fixed = false;
      }, 1000);
    },
    // 選択された時（chooseイベント）
    onChoose(e) {
      console.log("onChoose");
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
    onAdd() {
      console.log("onAdd");
    },
    // リストから要素が除去された時（removeイベント）
    onRemove() {
      console.log("onRemove");
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
  },
  mounted() {
    console.log(this.inCompleteToDos);
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
.item:hover {
  cursor: grab;
}
.item:active {
  cursor: grabbing;
}
.sortable-chosen {
  background-color: #42b983;
}
</style>