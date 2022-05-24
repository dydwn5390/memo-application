<template>
  <li class="memo-item">
    <!-- 등록된 props의 값을 각 DOM에 위치시킨다 -->
    <strong>{{ memo.title }}</strong>
    <p @dblclick="handleDblclick">
      <!-- template 태그를 이용하여 제목 텍스트를 감싸준다. -->
      <template v-if="!isEditing"> {{ memo.content }}</template>
      <!-- 수정 필드를 위한 태그를 추가해준다. -->
      <input v-else type="text"
             ref="content"
             :value="memo.content"
             @blur="handleBlur"
             @keydown.enter="updateMemo"/>
    </p>
    <button type="button" @click="deleteMemo"><i class="fas fa-times"></i></button>
  </li>
</template>

<script>
export default {
  name: "MemoItem",
  components: {},
  // 부모에게 내려받은 props 를 등록한다
  props: {
    memo: {
      type: Object
    }
  },
  data() {
    return {
      isEditing: false
    }
  },
  setup() {
  },
  created() {
  },
  mounted() {
  },
  unmounted() {
  },
  methods: {
    deleteMemo() {
      // 부모로부터 props로 내려받은 memo의 id를 부모 컴포넌트의 사용자 정의 이벤트인 deleteMemo 함수의 파라미터로 전달해준다.
      const id = this.memo.id;
      this.$emit('deleteMemo', id);
    },
    handleDblclick() {
      this.isEditing = true;
      // content에 foucs 이벤트를 추가한다.
      this.$nextTick(() => {
        this.$refs.content.focus();
      })
    },
    updateMemo(e) {
      const id = this.memo.id;
      const content = e.target.value.trim(0);
      if (content.length <= 0) {
        return false;
      }
      this.$emit('updateMemo', {id, content});
      this.isEditing = false;
    },
    handleBlur() {
      this.isEditing = false;
    }
  },
}
</script>

<style scoped>
.memo-item {
  overflow: hidden;
  position: relative;
  margin-bottom: 20px;
  padding: 24px;
  box-shadow: 0 4px 10px -4px rgba(0, 0, 0, 0.2);
  background-color: #fff;
  list-style: none;
}

.memo-item button {
  background: none;
  position: absolute;
  right: 20px;
  top: 20px;
  font-size: 20px;
  color: #e5e5e5;
  border: 0;
}

.memo-item strong {
  display: block;
  margin-bottom: 12px;
  font-size: 18px;
  font-weight: normal;
  word-break: break-all;
}

.memo-item p {
  margin: 0;
  font-size: 14px;
  line-height: 22px;
  color: #666;
}

.memo-item p input[type="text"] {
  box-sizing: border-box;
  width: 100%;
  font-size: inherit;
  border: 1px solid #999;
}

</style>