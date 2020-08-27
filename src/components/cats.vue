<template>
  <div class="first-page container-fluid">
    <div class="title-page">
      <h2>Ты сегодня покормил кота?</h2>
    </div>
    <div class="products">
      <div
        class="product-container"
        v-for="(product,index) in products"
        :key="index"
        :data-index="index"
        @click="buyProduct"
      >
        <div class="product-wrapper" :class="product.class">
          <div class="left-corner"></div>
          <div class="product">
            <h5 class="product__slogan">Сказочное заморское яство</h5>
            <h2 class="product__name">
              <span class="product__mainname">Нямушка</span>
              <span class="product__adding">{{product.name}}</span>
            </h2>
            <div class="product__promo">
              <p>{{product.promo.portions}}</p>
              <p>{{product.promo.mouses}}</p>
              <p>{{product.promo.isGood}}</p>
            </div>
            <div class="product__weight">
              <span class="product__kg">{{product.weight}}</span>
              кг
            </div>
          </div>
        </div>
        <div class="product__description">
          {{product.description}}
          <a v-if="product.productBuy.exist">{{product.productBuy.text}}</a>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      products: [
        {
          name: "с фуа-гра",
          count: 5,
          promo: {
            portions: "10 порций",
            mouses: "мышь в подарок",
            isGood: "",
          },
          weight: "0,5",
          description: "Чего сидишь? Порадуй котэ,",
          productBuy: { exist: true, text: "купи" },
          class: "product-wrapper_blue",
          chosen: false,
        },
        {
          name: "с рыбой",
          count: 3,
          promo: {
            portions: "40 порций",
            mouses: "2 мыши в подарок",
            isGood: "",
          },
          weight: "2",
          description: "Головы щучьи с чесноком да свежайшая сёмгушка.",
          productBuy: { exist: false, text: "купи" },
          class: "product-wrapper_pink",
          chosen: false,
        },
        {
          name: "с курой",
          count: 0,
          promo: {
            portions: "100 порций",
            mouses: "5 мышей в подарок",
            isGood: "заказчик доволен",
          },
          weight: "5",
          description: "Извините, товар закончился",
          productBuy: { exist: false, text: "купи" },
          class: "product-wrapper_disabled",
          chosen: false,
        },
      ],
      shopBasket: [],
    };
  },
  methods: {
    buyProduct() {
      if (
        !event.target.closest(".product-container") ||
        event.target.closest(".product-wrapper_disabled")
      )
        return;
      let chosenProduct = this.products[
        event.target.closest(".product-container").dataset.index
      ];
      if (this.shopBasket.includes(chosenProduct.name)) {
        this.returnProduct(chosenProduct);
      } else {
        this.shopBasket.push(chosenProduct.name);
        chosenProduct.count--; //Убираем количество товара
        chosenProduct.chosen = !chosenProduct.chosen;
        event.target
          .closest(".product-container")
          .querySelector(".product")
          .classList.add("product_chosen");
      }
    },
    returnProduct(chosenProduct) {
      this.shopBasket.splice(this.shopBasket.indexOf(chosenProduct.name), 1); //Удаляем товар из корзины
      chosenProduct.count++; //Добавляем количество товар обратно в магазин
      chosenProduct.chosen = !chosenProduct.chosen;
      event.target
        .closest(".product-container")
        .querySelector(".product")
        .classList.remove("product_chosen");
    },
  },
};
</script>

<style lang="scss">
@font-face {
  font-family: "Exo 2", sans-serif;
  src: url("/assets/Exo2.0-Thin.otf");
}
$blue: rgb(0, 153, 204);
$pink: #cc0066;
$green: rgb(27, 177, 27);
$disabled-color: #cccccc;
* {
  box-sizing: border-box;
}
.first-page {
  font-family: "Exo 2", sans-serif;
  min-height: 100vh;
  background: url("../assets/Pattern.png");
  display: flex;
  flex-wrap: wrap;
}
.title-page {
  width: 100%;
  text-align: center;
  font-size: 36px;
  color: white;
}
.products {
  width: 100%;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-around;
}
.product-container {
  margin: 3em 0;
  width: 25%;
  cursor: pointer;
}
.product-wrapper {
  clip-path: polygon(50px 0, 100% 0, 100% 100%, 0 100%, 0 50px);
  position: relative;
  height: 485px;
  background: linear-gradient(135deg, rgba(255, 255, 255, 0) 30px, white 30px);
  background-repeat: no-repeat;

  border: 5px solid white;
  border-radius: 10px;
  .left-corner {
    height: 5px;
    width: 98px;
    background: white;
    position: absolute;
    left: -51px;
    top: -6px;
    transform-origin: right;
    transform: rotate(-45deg);
  }
}
.product {
  position: relative;
  height: 100%;
  width: 100%;
  padding: 5% 0 0 15%;
  background: url("../assets/Photo.png");
  background-position: 25% 170%;
  background-repeat: no-repeat;
}
.product__slogan {
  font-size: 16px;
  font-weight: 300;
}
.product__name {
  display: flex;
  flex-wrap: wrap;
  font-size: 48px;
  line-height: 1em;
  font-family: "Trebuchet MS", "Lucida Sans Unicode", "Lucida Grande",
    "Lucida Sans", Arial, sans-serif;
}
.product__adding {
  width: 100%;
  font-size: 24px;
  line-height: 1em;
}
.product__promo {
  margin-top: 1em;
  font-size: 14px;
  line-height: 1.2em;
  font-weight: 300;
  p {
    line-height: 1.2em;
    margin: 0 !important;
  }
}
.product__weight {
  position: absolute;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  align-items: center;
  right: 0;
  bottom: 0;
  font-size: 21px;
  height: 80px;
  width: 80px;
  border-radius: 100%;
  color: white;

  line-height: 1em;
  transform: translate(-30%, -30%);
}
.product__kg {
  display: flex;
  justify-content: center;
  width: 100%;
  font-size: 42px;
  line-height: 1em;
}
.product__description {
  color: white;
  text-align: center;
}
.product-wrapper_blue {
  border-color: $blue;
  .left-corner {
    background-color: $blue;
  }
  .product__weight {
    background: $blue;
  }
}
.product-wrapper_pink {
  border-color: $pink;
  .left-corner {
    background-color: $pink;
  }
  .product__weight {
    background: $pink;
  }
}
.product-wrapper_green {
  border-color: $green;
  .left-corner {
    background-color: $green;
  }
  .product__weight {
    background: $green;
  }
}
.product_chosen {
  opacity: 0.5;
}
.product-wrapper_disabled {
  cursor: initial;
  .product {
    opacity: 0.5;
  }
  border-color: $disabled-color;
  .left-corner {
    background-color: $disabled-color;
  }
  .product__weight {
    background: $disabled-color;
  }
}

@media screen and(max-width: 992px) {
  .products {
    justify-items: start;
  }
  .product-container {
    width: 35%;
  }
}
@media screen and(max-width: 768px) {
  .product-container {
    width: 45%;
  }
}

@media screen and(max-width: 650px) {
  .product-container {
    width: 65%;
  }
}
@media screen and(max-width: 425px) {
  .product-container {
    width: 80%;
  }
}
@media screen and(max-width: 375px) {
  .product-container {
    width: 90%;
  }
}
</style>
