<template>
  <div class="post border-solid rounded-lg">
    <div
        v-if="post"
         class="post__img rounded-lg overflow-hidden"
    >
      <img :src="'/images/' + post.img + '.jpg'" alt="Post Image"/>
    </div>
    <div
        v-if="post"
        class="post__title text-xl"
    >
      {{ post.title }}
    </div>
    <div
        v-if="post"
        ref="postDescription"
        class="post__description"
        :class="{'test': conf.showMore, 'post__description-full': conf.isShowed }"
    >
      <p class="rounded-lg">
        {{ post.description }}
        <div @click="showMore" v-if="conf.showMore" class="post__showmore border border-black rounded-lg">
          <span v-if="!conf.isShowed">show more</span>
          <span v-else>hide</span>
        </div>
      </p>
    </div>
    <div
        v-if="post" class="post__date"
    >
      {{ post.date }}
    </div>
  </div>
</template>

<script>
export default {
  props: ['post'],
  data() {
    return {
      conf: {
        showMore: false,
        isShowed: false,
      }
    }
  },
  methods: {
    showMore(e) {
      const block = this.$refs.postDescription;
      this.conf.isShowed = !this.conf.isShowed;
    },
    checkHeight() {
      const block = this.$refs.postDescription;
      const blockHeight = block.offsetHeight;
      const contentHeight = block.scrollHeight;
      if (contentHeight > blockHeight) {
        block.classList.add('post__show-more');
        this.conf.showMore = true;
      }
      console.log(block);
      console.log(blockHeight);
      console.log(contentHeight);
    }
  },
  mounted: function() {
    this.checkHeight()
  }
}
</script>

<style lang="scss">
.post {
  display: grid;
  grid-template-columns: 150px 1fr;
  grid-template-rows: auto 1fr;
  gap: 14px;
  padding-right: 14px;
  position: relative;
  max-height: 150px;
  box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px;

  &__img {
    grid-row-start: 1;
    grid-row-end: 3;
    min-height: 150px;
    overflow: hidden;

    img {
      object-fit: cover;
      width: 100%;
      height: 100%;
      display: block;
    }
  }

  &__title {
    padding-top: 14px;
  }

  &__description {
    margin-bottom: 15px;
    height: calc(100% - 15px);

    &:not(.post__description-full) {
      overflow: hidden;
      p {
        height: 100%;
      }

    }



    &.test {
      position: relative;
    }

    &-full {
      p {
        position: absolute;
        background-color: white;
        box-shadow: rgba(0, 0, 0, 0.35) 0px 5px 15px;
        .post__showmore {
          bottom: -30%;
        }
      }
    }
  }

  &__date {
    font-size: 12px;
    position: absolute;
    right: 14px;
    bottom: 4px;
  }

  &__showmore {
    position: absolute;
    bottom: 0%;
    background-color: white;
    padding: 3px 14px 5px;
    left: 50%;
    transform: translateX(-50%);

    &:hover {
      cursor: pointer;
    }
  }

  &__title, &__description {

  }
}
</style>
