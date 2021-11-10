<template>
  <div>
    <div class="select" :class="{ active: showListProvince }">
      <input
        type="text"
        placeholder="Chọn tỉnh thành"
        class="title"
        v-model="searchName"
        @click="handleShowProvince"
      />
      <div class="select__wrap" v-show="showListProvince">
        <div class="select__list">
          <div
            class="select__item"
            v-for="item in listProvince"
            :key="item.code"
          >
            <input
              type="checkbox"
              :id="item.code"
              :value="item"
              v-model="listSelected"
            />
            <label :for="item.code">{{ item.name }}</label>
          </div>
        </div>
        <div class="select__btn">
          <button
            class="select__btn-agree"
            :class="{ active: showListSelected }"
            @click="handleShowProvince"
            :disabled="!showListSelected"
          >
            Đồng ý
          </button>
          <button class="select__btn-cancel" @click="handleCancel">Hủy</button>
        </div>
      </div>
    </div>
    <div class="list" v-show="showListSelected">
      <div class="list__item" v-for="item in listSelected" :key="item.code">
        <span>{{ item.name }}</span>
        <ion-icon name="close-outline" @click="deleteProvince(item)"></ion-icon>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "Select",
  data() {
    return {
      provinces: [],
      showListProvince: false,
      listSelected: [],
      searchName: "",
    };
  },
  methods: {
    handleShowProvince() {
      this.showListProvince = !this.showListProvince;
      this.searchName = "";
    },
    deleteProvince(province) {
      this.listSelected = this.listSelected.filter(
        (item) => item.name !== province.name
      );
    },
    handleCancel() {
      this.showListProvince = !this.showListProvince;
      this.listSelected = [];
      this.searchName = "";
    },
  },
  computed: {
    showListSelected() {
      return this.listSelected.length > 0;
    },
    listProvince() {
      if (this.searchName) {
        return this.provinces.filter((item) => {
          return item.name.toLowerCase().match(this.searchName.toLowerCase());
        });
      } else {
        return this.provinces;
      }
    },
  },
  created() {
    axios.get("https://provinces.open-api.vn/api/").then((response) => {
      this.provinces = response.data;
    });
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
.select {
  width: 480px;
  border-radius: 4px;
  border: 1px solid #999;
  margin: 0 auto;
  text-align: left;
  position: relative;
  cursor: pointer;
  z-index: 2;
  transition: all 0.2s ease;

  &.active {
    border-color: #007bc3;
    box-shadow: 0px 0px 8px 0px #007bc352;
  }

  .title {
    display: inline-block;
    color: #999;
    font-size: 16px;
    font-weight: 400;
    line-height: 1;
    padding: 15px 10px;
    width: 100%;
    border: unset;
    border-radius: 4px;
  }

  &::after {
    content: "";
    display: block;
    border: 4px solid transparent;
    border-color: #000 transparent transparent transparent;
    position: absolute;
    top: calc(50% + 3px);
    right: 22px;
    transform: translateY(-50%);
  }

  &__wrap {
    position: absolute;
    top: calc(100% + 2px);
    left: 0;
    right: 0;
    padding-bottom: 48px;
    border-radius: 8px;
    box-shadow: 0px 1px 8px 0px #66666633;
    overflow: hidden;
    background-color: #fff;
  }

  &__list {
    margin: 8px 0;
    height: 240px;
    overflow-y: scroll;

    &::-webkit-scrollbar {
      width: 6px;
    }

    &::-webkit-scrollbar-thumb {
      background: #dcdcdc;
      border-radius: 30px;
    }

    &::-webkit-scrollbar-track {
      background: transparent;
    }
  }

  &__item {
    display: flex;
    align-items: center;
    padding-left: 19px;

    &:hover {
      background: #e7f1fd;
    }

    input {
      appearance: none;
      -webkit-appearance: none;
      width: 16px;
      height: 16px;
      border-radius: 2px;
      border: 1px solid #000;
      cursor: pointer;
      position: relative;
      transition: all 0.35s ease 0s;

      &::before {
        content: "";
        position: absolute;
        display: block;
        border: solid #fff;
        top: 1px;
        left: 4px;
        width: 4px;
        height: 8px;
        border-width: 0 1px 1px 0;
        -webkit-transform: rotate(45deg);
        -ms-transform: rotate(45deg);
        transform: rotate(45deg);
        opacity: 0;
      }

      &:checked {
        background: #45d1c9;
        border-color: #45d1c9;

        &::before {
          opacity: 1;
        }
      }
    }

    label {
      font-size: 16px;
      color: #333;
      font-weight: 400;
      line-height: 24px;
      padding: 8px 11px;
      cursor: pointer;
      width: 100%;
    }
  }

  &__btn {
    position: absolute;
    bottom: 0;
    left: 0;
    padding: 0 16px 16px;
    background: #fff;

    &-agree {
      display: inline-block;
      font-size: 16px;
      font-weight: 700;
      color: #fff;
      line-height: 24px;
      padding: 4px 25px;
      border: unset;
      background: #dcdcdc;
      border-radius: 4px;

      &.active {
        background: #007bc3;
      }
    }

    &-cancel {
      display: inline-block;
      background-color: transparent;
      border: unset;
      font-size: 16px;
      color: #007bc3;
      font-weight: 400;
      line-height: 24px;
      padding: 0 26px;
      margin-left: 11px;
    }
  }
}

.list {
  width: 480px;
  border-radius: 4px;
  border: 1px solid #999;
  margin: 8px auto 0;
  padding: 7px;
  text-align: left;
  display: flex;
  overflow-x: auto;
  scrollbar-width: none;

  &::-webkit-scrollbar {
    display: none;
  }

  &__item {
    flex-shrink: 0;
    display: inline-block;
    font-size: 16px;
    font-weight: 400;
    color: #333;
    line-height: 24px;
    height: 32px;
    background: #f8f8f8;
    border-radius: 32px;
    display: flex;
    align-items: center;
    justify-content: space-between;

    span {
      flex: 1;
      text-align: center;
      padding: 0 8px 0 12px;
    }

    ion-icon {
      font-size: 22px;
      color: #666;
      padding-right: 4px;
      cursor: pointer;

      &:hover {
        color: #000;
      }
    }
  }
}
</style>
