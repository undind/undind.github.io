<template lang="pug">
  section.admin-section
    .admin-container
      header.admin-header
        .header__desc
          .header__avatar
            img.header__avatar-img(src="../../images/greeting/me.png" alt="")
          .header__username Сергей Антонов
          .header__panelname Панель администратора
        .admin-logout
          a(
            @click="exitLink"
          ).logout__link Выйти
</template>

<script>
import { mapActions } from "vuex";

export default {
  methods: {
    ...mapActions('user', ['logOut']),
    exitLink() {
      this.logOut();
      this.$router.replace("/login");
    }
  },
}
</script>

<style lang="postcss" scoped>
@import "../../styles/mixins.pcss";

.admin-section {
  background-image: linear-gradient(to right, #3e3e59 0%, #454573 100%);
}

.admin-header {
  display: grid;
  grid-template-columns: 1fr max-content;
  grid-template-rows: 80px;
  align-items: center;
  color: #fff;

  @include tablets {
    padding: 0 15px;
  }
}

.header__desc {
  display: flex;
  align-items: center;
}

.header__avatar {
  width: 45px;
  height: 45px;
  margin-right: 20px;
}

.header__username {
  font-size: 18px;
  font-weight: 600;
  margin-right: 20px;
}

.header__panelname {
  opacity: 0.5;
  font-size: 14px;

  @include phones {
    display: none;
  }
}

.admin-logout {
  justify-self: end; 
}

.logout__link {
  text-decoration: underline;
  opacity: 0.7;
  color: #ffffff;
  font-size: 16px;

  &:hover {
    text-decoration: none;
  }
}
</style>