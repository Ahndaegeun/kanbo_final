<template>
  <div v-if="board.열렸니" class="comment-input-div">
    <div>
      <ul>
        <li class="comment-li" v-for="(item, index) in board.댓글" :key="index">
          <div class="content-wrapper">
            <div class="info-wrapper">
              <span class="name">{{item.name}}</span>            
              <span class="date">{{item.date}}</span>            
            </div>
            <textarea @keyup="update" class="text-area" :value="item.content" :readonly="item.isModify"></textarea>
          </div>
          <div>
            <i @click="this.changeCommentsIsOpen(item)" v-if="item.isOpen==false" class="fas fa-ellipsis-h"></i>
            
            <!-- 여기서 토큰 값을 비교해서 본인의 게시글일 경우 수정,삭제 아이콘을 보여주고
                  본인의 글이 아니면 신고 버튼을 보여준다. v-if를 통해서
                  <div>
                    <i>신고</i>
                  </div>
            -->
            
            <div class="box" v-if="item.isOpen==true && item.isFinish == false">
              <i @click="toUpdate(item); this.changeIsFinish(item)" class="fas fa-edit"></i>
              <i @click="deleteComment" class="far fa-trash-alt"></i>
              <i @click="this.changeCommentsIsOpen(item)" class="fas fa-long-arrow-alt-left"></i>
            </div>

            <div id="finish-id" v-if="item.isOpen == true && item.isFinish == true" 
                  @click="toUpdate(item); this.changeIsFinish(item)">Finish</div>
          </div>
        </li>  
      </ul>
      <button v-if="board.댓글수 - board.댓글.length !== 0"
              id="more-btn"
              @click="this.extraComments(board)">더 보기</button>
    </div>
  </div>
</template>
<script>
import { mapActions, mapMutations } from 'vuex'

export default {
  name: 'BoardComment',
  data() {
    return {
      numberOfComments : this.board.댓글수,
      updateContent : '',
    }
  },

  props: {
    board: Object
  },

  computed : {
        ...mapActions({
          getComments : 'projectBoard/getComments',
        }),
  },

  methods: {
    ...mapActions({
      extraComments: 'projectBoard/extraComments'
    }),

    ...mapMutations({
      changeCommentsIsOpen : 'projectBoard/changeCommentsIsOpen',
      changeCommentsIsUpdate : 'projectBoard/changeCommentsIsUpdate',
      changeIsFinish : 'projectBoard/changeIsFinish',
      changeIsModify : 'projectBoard/changeIsModify'
    }),
    
    //댓글 삭제
    deleteComment(item){
      this.axios.delete('', null, { params : { idx : item.idx}})
                .then(e => {
                    console.log(e)
                  })
    },
    //댓글 수정 컨트롤러에 보내줌
    updateComment(item){
      this.axios.put('',null, { params : {idx : item.idx, content : this.updateContent}}).then(e => {
        console.log(e)
      })
    },
    toUpdate(item){
      this.changeIsModify(item)
    },
    //keyup될 때마다 새로쓰는 댓글 내용 지역변수에 저장
    update(e){
      this.updateContent = e.target.value
    },
  },
}
</script>

<style>
.comment-input-div {
  padding-right: 20px;
  padding-left: 20px;
}

.name-span {
  margin-right: 10px;
}

.comment-li {
  margin-top: 20px;
  display: flex;
  width: fit-content;
  align-items: center;
}

.content-wrapper {
  background-color: #414556;
  padding: 10px;
  margin-right: 10px;
  font-size: 14px;
  border-radius: 5px;
}

.info-wrapper {
  margin-bottom: 5px;
}

.info-wrapper .date {
  font-size: 10px;
  margin-left: 10px;
}

.content-detail {
  margin-left: 10px;
}

.box {
  display: flex;
  flex-direction: column;
  font-size: 12px;
  gap:8px;
}

i {
  cursor: pointer;
}

.comment-div {
    display: flex;
    justify-content: right;
    padding-right: 20px;
    padding-bottom: 20px;
}
#comment-btn {
  color: black;
}
textarea {
  background-color: #414556;
  color: #fff;
  height: auto;
  resize: none;
  outline: none;
  border: none;
  margin : 0;
  padding: 0;
  overflow: hidden;
}
#finish-id {
  cursor: pointer;
}
</style>