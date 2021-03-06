<template lang="pug">
  .new-reviews
    .my-reviews__header
      .my-reviews__title {{ title }}
    form.my-reviews__form(@submit.prevent="onSubmit")
      .my-reviews__edit
        avatar-upload(
          v-model="reviewData.photo"
          :error-message="validation.firstError('reviewData.photo')"
        )
      .my-reviews__content
        .my-reviews__row
          simple-input(
            label="Имя автора"
            v-model="reviewData.author"
            :error-message="validation.firstError('reviewData.author')"
          ).my-reviews__block
          simple-input(
            label="Титул автора"
            v-model="reviewData.occ"
            :error-message="validation.firstError('reviewData.occ')"
          ).my-reviews__block
        .my-works__row
          simple-textarea(
            label="Отзыв"
            v-model="reviewData.text"
            :error-message="validation.firstError('reviewData.text')"
          )
        .my-reviews__content-buttons
          button(
            :disabled="isSending"
            @click.prevent="$emit('reset')"
          ).app-button__reset Отмена
          button(
            type="submit"
            :disabled="isSending"
          ).app-button Сохранить
</template>

<script>
import SimpleVueValidation, { Validator } from 'simple-vue-validator';

const emptyReviewData = {
  author: '',
  occ: '',
  text: '',
  photo: null,
};

export default {
  components: {
    AvatarUpload: () => import("components/AvatarUpload.vue"),
    SimpleInput: () => import("components/SimpleInput.vue"),
    SimpleTextarea: () => import("components/SimpleTextarea.vue"),
  },
  props: {
    currentReview: {
      type: Object,
      default: null,
    },
    isSending: {
      type: Boolean,
      default: false
    }
  },
  watch: {
    currentReview() {
      this.updateReviewData();
      this.validation.reset();
    },
  },
  mixins: [SimpleVueValidation.mixin],
  data() {
    return {
      reviewData: emptyReviewData,
    };
  },
  validators: {
    'reviewData.photo': (value) => {
      return Validator.value(value).required('Загрузите фото');
    },
    'reviewData.author': (value) => {
      return Validator.value(value).required('Введите автора');
    },
    'reviewData.occ': (value) => {
      return Validator.value(value).required('Введите титул');
    },
    'reviewData.text': (value) => {
      return Validator.value(value).required('Введите текст отзыва');
    },
  },
  computed: {
    title() {
      return this.currentReview ? "Редактирование отзыва" : "Новый отзыв";
    },
  },
  methods: {
    updateReviewData() {
      this.reviewData = this.currentReview ? { ...this.currentReview } : { ...emptyReviewData } ;
    },
    onSubmit() {
      this.$validate().then((success) => {
        if (success) {
          const eventType = this.currentReview ? 'update' : 'create';
          this.$emit(eventType, this.reviewData);
          this.validation.reset();
        }
      });
    },
  },
  created() {
    this.updateReviewData();
  },
}
</script>

<style lang="postcss" scoped>
@import "../../styles/mixins.pcss";

.my-reviews__header {
  border-bottom: 1px solid rgba(#1f232d, 0.15);
  padding-bottom: 20px;
  padding-left: 15px;

  @include phones {
    padding-top: 20px;
  }
}

.my-reviews__title {
  font-size: 18px;
  font-weight: 700;
}

.my-reviews__form {
  display: grid;
  grid-template-columns: max-content 60%;
  padding: 50px 8px 0;
  grid-gap: 30px;

  @include tablets {
    grid-template-columns: max-content 1fr;
    padding: 20px 15px;
  }

  @include phones {
    grid-template-columns: 1fr;
  }
}

.my-reviews__edit {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;
  align-self: start;
}

.my-reviews__edit-link  {
  background: none;
  color: $admin-color;
  font-weight: 600;
  margin-top: 30px;
  transition: opacity 0.3s;

  &:hover {
    opacity: 0.85;
  }
}

.my-reviews__row {
  margin-bottom: 30px;
  display: flex;

  @include tablets {
    flex-direction: column;
  }
}

.my-reviews__block {
  flex: 1;
  display: block;
  margin-right: 30px;

  @include tablets {
    margin-right: 0;
    margin-bottom: 30px;
    width: 70%;
  }

  @include phones {
    width: 100%;
  }

  &:last-child {
    margin-right: 0;
  }
}

.my-reviews__content-buttons {
  margin-top: 60px;
  display: flex;
  justify-content: flex-end;

  @include phones {
    justify-content: center;
    margin-top: 30px;
  }
}

.app-button__reset {
  border: 0;
  background: none;
  color: $admin-color;
  font-weight: 600;
  margin-right: 60px;
  transition: opacity 0.3s;

  @include phones {
    margin-right: 10px;
  }

  &:hover {
    opacity: 0.85;
  }

  &:disabled {
    opacity: 0.2;
    cursor: default;
  }
}

.app-button {
  color: #fff;
  font-weight: 700;
  text-transform: uppercase;
  border-radius: 25px;
  padding: 20px 45px;
  background-image: linear-gradient(to right, #006aed 0%, #3f35cb 100%);
  transition: opacity 0.3s;

  &:hover {
    opacity: 0.85;
  }

  &:disabled {
    opacity: 0.2;
    cursor: default;
  }
}
</style>