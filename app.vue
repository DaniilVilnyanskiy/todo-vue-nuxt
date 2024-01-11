<template>
  <div class="container mx-auto pt-4">
    <ul class="list">
      <li v-if="posts" v-for="(item, index) in posts">
        <Post :post="item" />
        <Button
            :title="'Add review'"
            :addClass="'mt-2 ml-auto'"
            @buttonClick="handleReviewClick(item)"
        />
        <div v-if="item.isCreateReview">
          <form
              class="form"
              action=""
              @submit="submit($event, inputValue[index], item)"
          >
            <label>
              <input class="border border-black rounded-lg" type="text" v-model="inputValue[index]">
            </label>
            <Button :title="'submit'"/>
          </form>
        </div>

        <div v-if="item.reviews && item.reviews.length > 0" class="reviews">

          <div class="reviews__title">Reviews</div>

          <div v-for="(review, reviewIndex) in item.reviews" class="reviews__item">
            <div class="reviews__item-head">
              <span>Text {{reviewIndex}}: {{ review.title }}</span>
              <form
                  class="form"
                  action=""
                  @submit="submit($event, item.subInputValue[reviewIndex], review, true)"
              >
                <label>
                  <input class="border border-black rounded-lg" type="text" v-model="item.subInputValue[reviewIndex]">
                </label>
                <Button :title="'Add subreview'" :addClass="'btn-sm'"/>
              </form>
            </div>
            <div class="reviews__item-sublist">
              <div v-if="review.subReviews"
                   v-for="(subReview, subIndex) in review.subReviews"
              >
                Sub text {{subIndex}}: {{ subReview }}
              </div>
            </div>
          </div>

        </div>
      </li>
    </ul>

  </div>
</template>
<script>
import Button from "./components/Button";
import Post from "./components/Post";

export default {
  components: {Post,Button},
  data() {
    return {
      posts: [
        {
          title: 'Post first',
          img: 'post-img-1',
          description: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit. Ab, cum?',
          isCreateReview: false,
          reviews: [],
          subInputValue: []
        },
        {
          title: 'Post second',
          img: 'post-img-1',
          description: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit. Magni, nobis quas! ' +
              'At delectus dignissimos magnam quam qui reiciendis vitae. Distinctio earum eum labore nesciunt nostrum!' +
              'Lorem ipsum dolor sit amet, consectetur adipisicing elit. Magni, nobis quas! ' +
              'At delectus dignissimos magnam quam qui reiciendis vitae. Distinctio earum eum labore nesciunt nostrum!',
          date: '14.12.2023',
          isCreateReview: false,
          reviews: [],
          subInputValue: []
        }
      ],
      inputValue: []
    }
  },
  methods: {
    handleReviewClick(item) {
      item.isCreateReview = true;
    },
    submit(e, inputValue, item, subLevel = false) {
      e.preventDefault();

      if (!subLevel) {
        item.reviews.push({
          title: inputValue,
          subReviews: [],
        });
      } else {
        item.subReviews.push(inputValue);
      }

      item.isCreateReview = false;
    }
  }
}

</script>

<style lang="scss">
.list {
  display: flex;
  flex-direction: column;
  gap: 20px;

  li {
    display: flex;
    flex-direction: column;
  }
}
.reviews {
  padding: 14px 14px 14px 164px;

  &__title {
    margin-bottom: 10px;
  }
  &__item {
    border-top: 1px solid black;
    padding: 10px 0 10px 0;
    &-head {
      display: flex;
      flex-direction: column;
      form {
        margin-left: auto;
      }
    }
    &-sublist {
      padding-left: 150px;
      margin-top: 15px;
      div {
        border-top: 1px solid gray;
      }
    }
  }
}

.form {
  display: flex;
  flex-direction: column;
  gap: 8px;
  width: max-content;
}

* {
  box-sizing: border-box;
}
</style>
