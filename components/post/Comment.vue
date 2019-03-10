<template>
  <section class="comment-section">
    <section
      class="comment-box"
      ref="commentBox"
    >
      <div class="curve"></div>
      <div class="login-mask">
        <div class="login">
          <img
            :src="guestAvatar"
            alt="用户头像"
            v-if="guestAvatar !== ''"
          >
          <svg
            aria-hidden="true"
            v-else
            @click="githubLogin"
            class="octicon octicon-mark-github"
            height="42"
            version="1.1"
            viewBox="0 0 16 16"
            width="42"
          >
            <path
              fill-rule="evenodd"
              d="M8 0C3.58 0 0 3.58 0 8c0 3.54 2.29 6.53 5.47 7.59.4.07.55-.17.55-.38 0-.19-.01-.82-.01-1.49-2.01.37-2.53-.49-2.69-.94-.09-.23-.48-.94-.82-1.13-.28-.15-.68-.52-.01-.53.63-.01 1.08.58 1.23.82.72 1.21 1.87.87 2.33.66.07-.52.28-.87.51-1.07-1.78-.2-3.64-.89-3.64-3.95 0-.87.31-1.59.82-2.15-.08-.2-.36-1.02.08-2.12 0 0 .67-.21 2.2.82.64-.18 1.32-.27 2-.27.68 0 1.36.09 2 .27 1.53-1.04 2.2-.82 2.2-.82.44 1.1.16 1.92.08 2.12.51.56.82 1.27.82 2.15 0 3.07-1.87 3.75-3.65 3.95.29.25.54.73.54 1.48 0 1.07-.01 1.93-.01 2.2 0 .21.15.46.55.38A8.013 8.013 0 0 0 16 8c0-4.42-3.58-8-8-8z"
            ></path>
          </svg>
        </div>
      </div>
      <div class="content-area">
        <div
          class="content-area-mask"
          v-if="!isLogin"
        >
          <p class="login-tips">暂无评论权限，请使用GitHub账号登陆后发表评论。</p>
        </div>
        <textarea
          rows="6"
          v-model="newComment.content"
          ref="newComment"
          :placeholder="commentPlaceHolder"
        ></textarea>
      </div>
      <div class="comment-action">
        <button
          class="btn-comment"
          @click="addNewComment"
          v-if="isLogin"
        >评论一下</button>
      </div>
    </section>
    <section class="comment-list-section">
      <div
        class="empty-comment"
        v-if="comments.length === 0"
      >还没有评论，快来抢沙发吧！</div>
      <div
        class="comment-list-area"
        v-else
      >
        <div class="comment-list-title">最新评论</div>
        <ul class="comment-list">
          <li
            class="comment-list-item"
            v-for="(comment, index) in comments"
            :key="comment.id"
          >
            <div class="user-avater">
              <img
                class="reply-comment"
                :src="comment.avatar"
                alt="头像"
                @click="handleReplyComment(comment)"
              >
            </div>
            <div class="comment-item-info">
              <p
                class="user-name reply-comment"
                @click="handleReplyComment(comment)"
              >
                {{ comment.userName }}
              </p>
              <div class="comment-item-content">
                {{ comment.content }}
                <div
                  class="comment-item-reply"
                  v-if="comment.replyComments.length>0"
                  v-for="(reply,index) in comment.replyComments"
                >
                  <p class="comment-item-reply-user">{{reply.userName}}</p>
                  <div class="comment-item-reply-content">{{reply.content}}</div>
                  <div class="comment-item-reply-time">{{reply.createdTime}}</div>
                  <span class="floor">{{reply.number}}楼</span>
                </div>
              </div>
              <p class="comment-time">{{ comment.createdTime}}</p>
            </div>
            <span class="floor">{{ comment.number }}楼</span>
          </li>
        </ul>
      </div>
    </section>
    <Dialog v-show="showDialog" :dialogOption="dialogOption" ref="dialog"></Dialog>
  </section>
</template>

<script>
import Dialog from "~/components/post/Dialog"
export default {
  components:{
    Dialog
  },
  props: {
    id: {
      type: String,
      default: ""
    }
  },
  name: '',
  data() {
    return {
      guestAvatar: "https://avatars1.githubusercontent.com/u/20529801?v=4",
      isLogin: true,
      newComment: {
        postId: 0,
        replyId: 0,
        content: ''
      },
      replyComment: {
        replyUserName: '',
        replyContent: '',
        replyNumber: 0
      },
      guestName: '',
      guestAvatar: '',
      comments: [{
        id: 1,
        avatar: "https://avatars1.githubusercontent.com/u/20529801?v=4",
        userName: "小何",
        content: "你的文章写的真好！",
        createdTime: "2018年05月11日 星期五 晚上",
        number: "1",
        replyComments: [{
          id: 1,
          avatar: "https://avatars1.githubusercontent.com/u/20529801?v=4",
          userName: "小尹",
          content: "我同意",
          createdTime: "2018年05月12日 星期六 下午",
          number: "1",
        }, {
          id: 2,
          avatar: "https://avatars1.githubusercontent.com/u/20529801?v=4",
          userName: "小钱",
          content: "我不同意！",
          createdTime: "2018年05月12日 星期六 下午",
          number: "2",
        }]
      }],
      commentPlaceHolder: "评论一下吧",
      showDialog: false,
      dialogOption: {
        title: '提示',
        text: '好的么么哒',
        cancelButtonText: '取消',
        confirmButtonText: '确定'
      }
    }
  },
  mounted() {
    console.log(this.id);
  },
  methods: {
    githubLogin() {

    },
    addNewComment() {

    },
    handleReplyComment() {

    }
  }
}
</script>

<style scoped lang='less'>
@import "~assets/less/index";
.comment-section {
  margin: 3em auto;
  .comment-box {
    position: relative;
    padding-top: 3.5em;
    .curve {
      position: absolute;
      top: 0;
      left: 0.2em;
      width: 4em;
      height: 4em;
      border-radius: 100%;
      border: 1px solid @base-color;
      background: #fff;
      z-index: 99;
    }
    .login-mask {
      position: absolute;
      top: 0;
      left: 0;
      width: 10em;
      height: 3.5em;
      background: #fff;
      z-index: 100;
    }
    .login {
      position: absolute;
      top: 0.7em;
      left: 0.7em;
      width: 3em;
      height: 3em;
      border-radius: 100%;
      border: 1px solid @base-color;
      color: @base-color;
      z-index: 101;
      cursor: pointer;

      img {
        width: 100%;
        height: 100%;
        border-radius: 100%;
      }
    }
    .content-area {
      position: relative;
      .content-area-mask {
        position: absolute;
        top: 0;
        left: 0;
        bottom: 0;
        right: 0;
        background: rgba(255, 255, 255, 0.6);
        z-index: 999;
        .login-tips {
          position: absolute;
          top: 50%;
          left: 50%;
          transform: translate(-50%, -50%);
          color: #999;
          line-height: 1.6;
        }
      }
      textarea {
        padding: 0.8em;
        line-height: 1.5;
        width: 100%;
        resize: none;
        border: 1px solid @base-color;
        outline: none;
        border-radius: 0.4em;
        box-sizing: border-box;

        &::placeholder {
          color: #bcbcbc;
          font-size: 1em;
        }
      }
    }
    .comment-action {
      margin-top: 0.4em;
      position: relative;
      .btn-comment {
        position: absolute;
        top: 0;
        right: 0;
        padding: 0.6em 1em;
        border-radius: 0.4em;
        color: #fff;
        letter-spacing: 0.1em;
        background: @base-color;
        border: none;
        outline: none;
        cursor: pointer;
      }
    }
  }
  .comment-list-section {
    margin-top: 4.2em;
    .empty-comment {
      font-size: 0.8em;
      color: #999;
      line-height: 2.6em;
      text-align: center;
      border-radius: 0.4em;
      background: #f0f0f0;
    }
    .comment-list-title {
      font-size: 1.2em;
      line-height: 2;
      padding-left: 0;
      border-bottom: 1px solid #ddd;
    }
    .comment-list-item {
      position: relative;
      padding: 1.2em 1.25em 1.2em 5.4em;
      border-bottom: 1px solid #e7e7eb;

      .user-avater {
        position: absolute;
        top: 1.2em;
        left: 0;
        width: 4em;
        height: 4em;

        img {
          width: 100%;
          height: 100%;
        }
      }
      .comment-item-reply {
        position: relative;
        margin: 1em 0 1em 1em;
        padding: 1em;
        border: 1px dashed @base-color;
        border-radius: 0.6em;
        .comment-item-reply-user {
          font-size: 1.2em;
          font-weight: 400;
          color: #9a9a9a;
          line-height: 1.2;
          margin-bottom: 0.4em;
        }
        .comment-item-reply-time {
          margin-top: 0.5em;
          font-size: 1em;
          color: #9a9a9a;
        }

        .comment-item-reply-content {
          padding: 0.5em 0;
          line-height: 1.5;
          color: #353535;
        }
        .floor {
          top: 1em;
          right: 1em;
        }
      }
      .comment-item-info {
        .user-name {
          font-size: 1.2em;
          font-weight: 400;
          color: #9a9a9a;
          line-height: 1.2;
          margin-bottom: 0.4em;
        }
        .comment-time {
          margin-top: 0.5em;
          font-size: 1em;
          color: #9a9a9a;
        }

        .comment-item-content {
          padding: 0.5em 0;
          line-height: 1.5;
          color: #353535;
        }
      }
      .floor {
        position: absolute;
        top: 1.2em;
        right: 0;
        color: #9a9a9a;
      }
      .reply-comment {
        cursor: pointer;
      }
    }
  }
}
</style>