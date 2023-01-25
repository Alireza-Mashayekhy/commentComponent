<template>
  <div class="comments">
    <div class="header d-flex col-12">
      <!-- a gray circle if image doesn't exist -->
      <img
        v-if="user.avatar"
        class="userAvatar"
        :src="user.avatar"
        width="20"
        height="20"
      />
      <div v-else class="userNoAvatar"></div>
      <form action="#" class="col px-0">
        <input type="text" class="col-12" placeholder="Start a discussion" />
      </form>
    </div>
    <div class="commentsList">
      <div class="comment d-flex" v-for="comment in comments" :key="comment.id">
        <div class="d-flex flex-column align-items-center">
          <img class="commentAvatar" :src="comment.user.avatar" />
          <!-- line is for who has reply -->
          <div class="line" v-if="comment.replies[0]"></div>
        </div>
        <div class="texts">
          <div class="d-flex align-items-center">
            <div class="name">{{ comment.user.name }}</div>
            <div class="time">{{ timeCheck(comment.date) }}</div>
          </div>
          <p class="text">
            {{ comment.text }}
          </p>
          <div class="d-flex align-items-center">
            <button
              class="like"
              :class="{ liked: comment.iLikedIt, dontLiked: !comment.iLikedIt }"
              @click="
                if (comment.iLikedIt) {
                  comment.likes -= 1;
                } else {
                  comment.likes += 1;
                }
                comment.iLikedIt = !comment.iLikedIt;
              "
            >
              <img v-if="comment.iLikedIt" src="/icons/whiteLike.svg" alt="" />
              <img v-else src="/icons/like.svg" alt="" />
              {{ comment.likes }}
            </button>
            <div
              class="toReply"
              @click="
                closeAllInputs();
                comment.replyInput = true;
              "
            >
              Reply
            </div>
          </div>
          <div class="replies" v-if="comment.replies[0]">
            <div
              class="reply d-flex"
              v-for="reply in comment.replies"
              :key="reply.id"
            >
              <div class="d-flex flex-column align-items-center">
                <img
                  v-if="reply.user.avatar"
                  class="commentAvatar"
                  :src="reply.user.avatar"
                />
                <div v-else class="userNoAvatar"></div>
              </div>
              <div class="texts">
                <div class="d-flex align-items-center">
                  <div class="name">{{ reply.user.name }}</div>
                  <div class="time">{{ timeCheck(reply.date) }}</div>
                </div>
                <p class="text">
                  {{ reply.text }}
                </p>
                <div class="d-flex align-items-center">
                  <button
                    class="like"
                    :class="{
                      liked: reply.iLikedIt,
                      dontLiked: !reply.iLikedIt,
                    }"
                    @click="
                      if (reply.iLikedIt) {
                        reply.likes -= 1;
                      } else {
                        reply.likes += 1;
                      }
                      reply.iLikedIt = !reply.iLikedIt;
                    "
                  >
                    <img
                      v-if="reply.iLikedIt"
                      src="/icons/whiteLike.svg"
                      alt=""
                    />
                    <img v-else src="/icons/like.svg" alt="" />
                    {{ reply.likes }}
                  </button>
                </div>
              </div>
            </div>
          </div>
          <div class="sendReply d-flex col-12 px-0" v-if="comment.replyInput">
            <!-- a gray circle if image doesn't exist -->
            <img
              v-if="user.avatar"
              class="userAvatar"
              :src="user.avatar"
              width="20"
              height="20"
            />
            <div v-else class="userNoAvatar"></div>
            <form action="#" class="col px-0">
              <input
                class="col-12"
                type="text"
                v-model="reply"
                placeholder="Reply"
              />
            </form>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
export default Vue.extend({
  data() {
    return {
      reply: "",
      comments: [
        {
          id: 3,
          date: 1672576574000,
          user: {
            name: "Bessie Cooper",
            avatar:
              "https://www.godaddy.com/garage/wp-content/uploads/judith-kallos-BW-NEW-150x150.jpg",
          },
          text: "I think for our second campaign we can try to target a different audience. How does it sound for you?",
          likes: 2,
          iLikedIt: false,
          replyInput: false,
          replies: [
            {
              id: 5,
              date: 1672581014000,
              user: {
                name: "Marvin McKinney",
                avatar:
                  "https://www.gravatar.com/avatar/205e460b479e2e5b48aec07710c08d50",
              },
              text: "Yes, that sounds good! I can think about this tomorrow. Then do we plan to start that campaign?",
              likes: 3,
              iLikedIt: true,
            },
            {
              id: 6,
              date: 1672581614000,
              user: {
                name: "Bessie Cooper",
                avatar:
                  "https://www.godaddy.com/garage/wp-content/uploads/judith-kallos-BW-NEW-150x150.jpg",
              },
              text: "We plan to run the campaign on Friday - as far as I know. Do you think you will get this done by Thursday @Marvin?",
              likes: 0,
              iLikedIt: false,
            },
          ],
        },
        {
          id: 2,
          date: 1672232414000,
          user: {
            name: "Marvin McKinney",
            avatar:
              "https://www.gravatar.com/avatar/205e460b479e2e5b48aec07710c08d50",
          },
          text: "The first campaign went smoothly. Please make sure to see all attachments with the results to understand the flow.",
          likes: 2,
          iLikedIt: false,
          replyInput: false,
          replies: [],
        },
        {
          id: 1,
          date: 1671886814000,
          user: {
            name: "Savannah Nguyen",
          },
          text: "We have just published the first campaign. Let's see the results in the 5 days and we will iterate on this.",
          likes: 50,
          iLikedIt: true,
          replyInput: false,
          replies: [],
        },
      ],
      user: {
        name: "",
        avatar: "",
      },
    };
  },
  methods: {
    timeCheck(time: number): string {
      let myTime: number = 0;
      let stringTime: string = "";
      // change to second
      myTime = time / 1000;
      if (myTime > 60) {
        // change to min
        myTime /= 60;
        if (myTime > 60) {
          // change to hour
          myTime /= 60;
          if (myTime > 24) {
            // change to day
            myTime /= 24;
            if (myTime > 30) {
              // change to month
              myTime /= 30;
              if (myTime > 12) {
                // change to year
                myTime /= 12;
                stringTime = myTime.toString().split(".")[0] + "year ago";
                return stringTime;
              } else {
                stringTime = myTime.toString().split(".")[0] + "month ago";
                return stringTime;
              }
            } else {
              stringTime = myTime.toString().split(".")[0] + "day ago";
              return stringTime;
            }
          } else {
            stringTime = myTime.toString().split(".")[0] + "h ago";
            return stringTime;
          }
        } else {
          stringTime = myTime.toString().split(".")[0] + "min ago";
          return stringTime;
        }
      } else {
        stringTime = myTime.toString().split(".")[0] + "second ago";
        return stringTime;
      }
    },
    closeAllInputs() {
      for (let i = 0; i < this.comments.length; i++) {
        this.comments[i].replyInput = false;
        this.reply = "";
      }
    },
  },
});
</script>
