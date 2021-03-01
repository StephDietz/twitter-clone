<template>
  <div class="flex container h-screen w-full overflow-visible yes-scrollbar">
    <!-- side nav -->
    <div
      class="lg:w-1/5 border-r border-lighter px-2 lg:px-6 py-2 flex flex-col justify-between"
    >
      <div>
        <button
          class="h-12 w-12 hover:bg-lightblue text-3xl rounded-full text-blue ml-1"
        >
          <i class="fab fa-twitter"></i>
        </button>
        <div>
          <button
            v-for="tab in tabs"
            v-bind:key="tab.id"
            @click="id = tab.id"
            :class="`focus:outline-none hover:text-blue flex items-center text-center py-3 px-3 hover:bg-lightblue rounded-full h-12 w-12 lg:h-auto lg:w-auto mr-auto mb-3 md:ml-0 mt-2 ${
              id === tab.id ? 'text-blue' : ''
            }`"
          >
            <i :class="`${tab.icon} text-2xl mr-4 text-left`"></i>
            <p class="text-lg font-semibold text-left hidden lg:inline-block">
              {{ tab.title }}
            </p>
          </button>
        </div>
        <button
          class="text-white bg-blue rounded-full font-semibold focus:outline-none w-12 h-12 lg:h-auto lg:w-full p-3 lg:px-2 lg:ml-0 hover:bg-darkblue"
        >
          <p class="hidden lg:block">Tweet</p>
          <i class="fas fa-plus lg:hidden"></i>
        </button>
      </div>
      <div class="lg:w-full relative">
        <button
          @click="dropdown = true"
          class="flex items-center h-auto w-full hover:bg-lightblue rounded-full p-2 focus:outline-none"
        >
          <img
            src="profile.png"
            class="w-10 h-10 rounded-full border border-lighter lg:ml-0"
          />
          <div class="hidden lg:block ml-4">
            <p class="text-sm font-bold leading-tight">Steph Dietz</p>
            <p class="text-sm leading-tight">@SaaSyEth</p>
          </div>
          <i class="hidden lg:block fas fa-ellipsis-h ml-auto text-lg"></i>
        </button>
        <div
          v-if="dropdown === true"
          class="absolute bottom-0 left-0 w-64 rounded-lg shadow-md border-lightest bg-white mb-16"
        >
          <button
            @click="dropdown = false"
            class="flex items-center w-full hover:bg-lightest p-2 focus:outline-none"
          >
            <img
              src="profile.png"
              class="w-10 h-10 rounded-full border border-lighter"
            />
            <div class="ml-4">
              <p class="text-sm font-bold leading-tight">Steph Dietz</p>
              <p class="text-sm leading-tight">@SaaSyEth</p>
            </div>
            <i class="fas fa-check ml-auto text-blue"></i>
          </button>
          <button
            @click="dropdown = false"
            class="w-full text-left hover:bg-lightest border-t border-lighter p-3 test-sm focus:outline-none"
          >
            Add an existing account
          </button>
          <button
            @click="dropdown = false"
            class="w-full text-left hover:bg-lightest border-t border-lighter p-3 test-sm focus:outline-none"
          >
            Log out @SaaSyEth
          </button>
        </div>
      </div>
    </div>
    <!-- tweets -->
    <div
      class="no-scrollbar w-full md:w-1/2 h-full md:overflow-y-scroll overflow-auto"
    >
      <div
        class="bg-white px-5 py-3 border-b border-lighter flex items-center justify-between sticky inset-x-0 top-0 homebar"
      >
        <h1 class="bg-white text-xl font-bold mt-1">Home</h1>
        <i class="far fa-star text-xl text-blue"></i>
      </div>
      <div class="px-5 py-3 border-b-8 border-lighter flex">
        <div class="flex-none">
          <img
            src="profile.png"
            class="flex-none w-12 h-12 rounded-full border border-lighter"
          />
        </div>
        <form v-on:submit.prevent="addNewTweet" class="w-full px-4 relative">
          <textarea
            v-model="tweet.content"
            placeholder="What's happening?"
            class="mt-3 pb-3 w-full focus:outline-none"
          />
          <div class="flex items-center">
            <i class="text-lg text-blue mr-4 far fa-image"></i>
            <i class="text-lg text-blue mr-4 fas fa-film"></i>
            <i class="text-lg text-blue mr-4 far fa-chart-bar"></i>
            <i class="text-lg text-blue mr-4 far fa-smile"></i>
          </div>
          <button
            type="submit"
            class="h-10 px-4 text-white font-semibold bg-blue hover:bg-darkblue focus:outline-none rounded-full absolute bottom-0 right-0"
          >
            Tweet
          </button>
        </form>
      </div>
      <div class="flex flex-col-reverse">
        <div
          v-for="tweet in tweets"
          v-bind:key="tweet.id"
          class="w-full p-4 border-b hover:bg-lighter flex"
        >
          <div class="flex-none mr-4">
            <img src="profile.png" class="h-12 w-12 rounded-full flex-none" />
          </div>
          <div class="w-full">
            <div class="flex items-center w-full">
              <p class="font-semibold">Steph Dietz</p>
              <p class="text-sm text-dark ml-2">@SaaSyEth</p>
              <p class="text-sm text-dark ml-2">Just Now</p>
              <i class="fas fa-ellipsis-h text-dark ml-auto"></i>
            </div>
            <p class="py-2">
              {{ tweet.content }}
            </p>
            <div class="flex items-center justify-between w-full">
              <div class="flex items-center text-sm text-dark">
                <i class="far fa-comment mr-3"></i>
                <p>0</p>
              </div>
              <div class="flex items-center text-sm text-dark">
                <i class="fas fa-retweet mr-3"></i>
                <p>0</p>
              </div>
              <div class="flex items-center text-sm text-dark">
                <i class="fas fa-heart mr-3"></i>
                <p>0</p>
              </div>
              <div class="flex items-center text-sm text-dark">
                <i class="fas fa-share mr-3"></i>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div
        v-for="follow in following"
        v-bind:key="follow.id"
        class="w-full p-4 border-b hover:bg-lighter flex"
      >
        <div class="flex-none mr-4">
          <img
            :src="`${follow.src}`"
            class="h-12 w-12 rounded-full flex-none"
          />
        </div>
        <div class="w-full">
          <div class="flex items-center w-full">
            <p class="font-semibold">{{ follow.name }}</p>
            <p class="text-sm text-dark ml-2">{{ follow.handle }}</p>
            <p class="text-sm text-dark ml-2">Just Now</p>
            <i class="fas fa-ellipsis-h text-dark ml-auto"></i>
          </div>
          <p class="py-2">
            {{ follow.tweet }}
          </p>
          <div class="flex items-center justify-between w-full">
            <div class="flex items-center text-sm text-dark">
              <i class="far fa-comment mr-3"></i>
              <p>0</p>
            </div>
            <div class="flex items-center text-sm text-dark">
              <i class="fas fa-retweet mr-3"></i>
              <p>0</p>
            </div>
            <div class="flex items-center text-sm text-dark">
              <i class="fas fa-heart mr-3"></i>
              <p>0</p>
            </div>
            <div class="flex items-center text-sm text-dark">
              <i class="fas fa-share mr-3"></i>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- trending -->
    <div
      class="bg-white md:block hidden w-1/3 h-full border-l border-lighter py-0 px-0 md:overflow-y-scroll overflow-auto no-scrollbar"
    >
      <form class="bg-white sticky inset-x-0 top-0 searchbar w-full pt-0 px-2">
        <input
          class="pl-12 rounded-full w-full py-2 mt-2 bg-lighter text-sm mb-3.5 focus:outline-none"
          placeholder="Search Twitter"
        />
        <i
          class="fas fa-search absolute left-0 top-0 mt-5 ml-6 text-sm text-light"
        ></i>
      </form>
      <div class="px-6">
        <div class="w-full rounded-lg bg-lightest">
          <div class="flex items-center justify-between p-3">
            <p class="text-lg font-bold">What's happening</p>
          </div>
          <button
            v-for="trend in trending"
            v-bind:key="trend.id"
            class="focus:outline-none w-full flex justify-between hover:bg-lighter p-3 border-t border-lighter"
          >
            <div>
              <p class="text-xs text-left leading-tight text-dark">
                {{ trend.top }}
              </p>
              <p class="font-semibold text-sm text-left leading-tight">
                {{ trend.title }}
              </p>
              <p class="text-left text-sm leading-tight text-dark">
                {{ trend.bottom }}
              </p>
            </div>
            <i class="fas fa-ellipsis-h text-lg text-dark"></i>
          </button>
          <button
            class="focus:outline-none p-3 w-full hover:bg-lighter text-left text-blue border-t border-lighter"
          >
            Show More
          </button>
        </div>
      </div>
      <div class="px-6">
        <div class="w-full rounded-lg bg-lightest my-4">
          <div class="p-3">
            <p class="text-lg font-bold">Who to Follow</p>
          </div>
          <button
            v-for="friend in friends"
            v-bind:key="friend.id"
            class="focus:outline-none w-full flex hover:bg-lighter p-3 border-t border-lighter"
          >
            <img
              :src="`${friend.src}`"
              class="w-12 h-12 rounded-full border border-lighter"
            />
            <div class="hidden lg:block ml-4">
              <p class="text-sm font-bold leading-tight">{{ friend.name }}</p>
              <p class="text-sm leading-tight">{{ friend.handle }}</p>
            </div>
            <button
              class="focus:outline-none ml-auto text-sm text-blue py-1 px-4 rounded-full border-2 border-blue"
            >
              Follow
            </button>
          </button>
          <button
            class="focus:outline-none p-3 w-full hover:bg-lighter text-left text-blue border-t border-lighter"
          >
            Show More
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  components: {},
  data() {
    return {
      tabs: [
        { icon: "fas fa-home", title: "Home", id: "home" },
        { icon: "fas fa-hashtag", title: "Explore", id: "explore" },
        { icon: "far fa-bell", title: "Notifications", id: "notifications" },
        { icon: "far fa-envelope", title: "Messages", id: "messages" },
        { icon: "far fa-bookmark", title: "Bookmarks", id: "bookmarks" },
        { icon: "fas fa-list", title: "Lists", id: "lists" },
        { icon: "far fa-user", title: "Profile", id: "profile" },
        { icon: "fas fa-ellipsis-h", title: "More", id: "more" },
      ],
      id: "home",
      dropdown: false,
      trending: [
        {
          top: "Trending in TX",
          title: "Gigi",
          bottom: "Trending with: Rip Gigi",
        },
        { top: "Music", title: "We Won", bottom: "135K Tweets" },
        { top: "Pop", title: "Blue Ivy", bottom: "40k tweets" },
        { top: "Trending in US", title: "Denim Day", bottom: "40k tweets" },
        { top: "Trending", title: "When Beyonce", bottom: "25.4k tweets" },
        { top: "Trending", title: "When Beyonce", bottom: "25.4k tweets" },
        { top: "Trending", title: "When Beyonce", bottom: "25.4k tweets" },
        { top: "Trending", title: "When Beyonce", bottom: "25.4k tweets" },
      ],
      friends: [
        { src: "elon.jpg", name: "Elon Musk", handle: "@teslaBoy" },
        { src: "monk.jpg", name: "Adrian Monk", handle: "@detective:)" },
        { src: "kevin.jpg", name: "Kevin Hart", handle: "@miniRock" },
      ],
      following: [
        {
          src: "elon.jpg",
          name: "Elon Musk",
          handle: "@teslaBoy",
          tweet: "Should I just quarantine on mars??",
        },
        {
          src: "kevin.jpg",
          name: "Kevin Hart",
          handle: "@miniRock",
          tweet: "Should me and the rock do another sub-par movie together????",
        },
        {
          src: "elon.jpg",
          name: "Elon Musk",
          handle: "@teslaBoy",
          tweet: "Haha just made a flame thrower. Shld I sell them?",
        },
        {
          src: "elon.jpg",
          name: "Elon Musk",
          handle: "@teslaBoy",
          tweet: "Just did something crazyyyyyyy",
        },
      ],
      tweets: [{ content: "It is so nice outside!" }],
      tweet: { content: "" },
    };
  },
  methods: {
    addNewTweet() {
      let newTweet = {
        content: this.tweet.content,
      };
      this.tweets.push(newTweet);
    },
  },
};
</script>

<style scoped>
.no-scrollbar::-webkit-scrollbar {
  display: none;
}

.yes-scrollbar::-webkit-scrollbar {
  display: inline;
}

.no-scrollbar {
  -ms-overflow-style: none;
  scrollbar-width: none;
}

.yes-scrollbar {
  -ms-overlow-style: inline;
  scrollbar-width: initial;
}

.homebar {
  z-index: 9999;
}

.searchbar {
  z-index: 9999;
}
</style>
