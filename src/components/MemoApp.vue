<template>
  <div class="memo-app">
    <memo-form @addMemo="addMemo"/>
    <ul class="memo-list">
      <memo-item v-for="memo in memos"
                 :key="memo.id"
                 :memo="memo"
                 @deleteMemo="deleteMemo"
                 @updateMemo="updateMemo"
      />
    </ul>
  </div>
</template>

<script>
import MemoForm from "@/components/MemoForm";
import MemoItem from "@/components/MemoItem";
import axios from 'axios';

const memoAPICore = axios.create({
  baseURL: 'http://localhost:8000/api/memos'
});

export default {
  name: "MemoApp",
  components: {MemoItem, MemoForm},
  data() {
    return {
      memos: [],
    }
  },
  setup() {
  },
  created() {
    // 만약 기존에 추가된 localstorage에 데이터가 있다면 create 훅에서 localStorage의 데이터를 컴포넌트 내의 memos에 넣어주고,
    // 그렇지 않다면 그대로 빈 배열로 초기화한다.
    //this.memos = localStorage.memos ? JSON.parse(localStorage.memos) : [];

    // axios 객체의 get 메소드를 이용하여 데이터를 받아온다.
    memoAPICore.get('/')
        .then(res => {
          this.memos = res.data;
        })
  },
  mounted() {
  },
  unmounted() {
  },
  methods: {
    addMemo(payload) {
      // MemoForm dㅔ서 올려 받은 데이터를 먼저 컴포넌트 내부 데이터에 추가한다.
      this.memos.push(payload);
      // 내부 데이터를 문자열로 변환 후, 로컬 스토리지에 저장한다.
      this.storeMemo();
    },
    storeMemo() {
      const memoToString = JSON.stringify(this.memos);
      localStorage.setItem('memos', memoToString);
    },
    deleteMemo(id) {
      // 자식 컴포넌트에서 인자로 전달해주는 id에 해당하는 메모 데이터의 인덱스를 찾는다.
      const targetIndex = this.memos.findIndex(v => v.id === id);
      // 찾은 인덱스 번호에 해당하는 데이터를 삭제한다.
      this.memos.splice(targetIndex, 1);
      // 삭제된 후의 데이터를 다시 로컬스토리지에 마찬가지로 저장한다.
      this.storeMemo;

    },
    updateMemo(payload) {
      // 수정된 메모를 저장한다.
      const {id, content} = payload;
      const targetIndex = this.memos.findIndex(v => v.id === id);
      const targetMemo = this.memos[targetIndex];
      this.memos.splice(targetIndex, 1, {...targetMemo, content});
      this.storeMemo();
    }
  }
}
</script>

<style scoped>
.memo-list {
  padding: 20px 0;
  margin: 0;
}
</style>