<template>
  <div class="container-fluid mb-5">
    <nav class="navbar navbar-dark bg-dark align-items-baseline">
      <div class="container-fluid">
        <a
          href="#"
          @click.prevent="isWatchListVisible = false"
          class="navbar-brand fs-3"
          style="color: rgb(138, 138, 14); text-decoration: none"
          >All Books</a
        >
        <div class="d-flex align-items-baseline text-light">
          <p>
            [ {{ booksListCount() }} ]
            {{ booksListCount() == 1 ? "Book" : "Books" }} with total price : [
            {{ handleCurrency(getTotalPrice()) }} ]
          </p>
          <button
            class="btn btn-success mx-2"
            @click="isWatchListVisible = true"
          >
            Show List
          </button>
        </div>
      </div>
    </nav>

    <div class="container-fluid mt-3" v-if="isWatchListVisible == false">
      <div class="row d-flex justify-content-around text-center">
        <div
          v-for="book in books"
          :key="book.id"
          style="width: 15rem; margin: 0.2rem"
          :class="[
            'card p-0 mb-3 border border-1',
            book.pages < 50 ? 'border-danger' : 'border-success',
          ]"
        >
          <img
            :src="book.image"
            style="max-height: 350px"
            class="card-img-top"
          />
          <div
            class="
              card-footer
              fs-6
              font-weight-bold
              border border-bottom-2
              text-center
            "
          >
            {{ book.name }}
          </div>
          <div class="card-body py-0">
            <div class="book-content">
              <div class="row d-flex justify-content-around">
                <div class="col-12">
                  <span>Category </span> :
                  {{ book.category }}
                </div>
                <div class="col-12">
                  <span>Author </span> :
                  {{ book.author }}
                </div>
                <div class="col-12">
                  <span>Pages </span> :
                  <span :class="book.pages < 50 ? 'less' : 'more'">{{
                    book.pages
                  }}</span>
                </div>
                <div class="col-12">
                  <span>Price </span> :
                  {{ handleCurrency(book.price.value) }}
                </div>
                <div class="col-12">
                  <span>ISBN </span> :
                  {{ book.ISBN }}
                </div>
              </div>
            </div>
          </div>
          <div class="card-footer">
            <button
              class="btn btn-dark"
              :disabled="checkExist(book.id)"
              @click="addToList(book)"
            >
              Add to List
            </button>
          </div>
        </div>
      </div>
    </div>

    <div class="container mt-3" v-if="isWatchListVisible == true">
      <div class="alert" role="alert" v-if="booksList.length == 0">
        <div
          class="
            content-wrapper
            d-flex
            justify-content-center
            align-items-center
            bg-white
          "
        >
          <img
            src="https://cdn.dribbble.com/users/888330/screenshots/2653750/media/b7459526aeb0786638a2cf16951955b1.png"
            alt="there is no data to show"
          />
        </div>
      </div>

      <div class="watchList" v-if="booksList.length > 0">
        <div class="table-responsive">
          <table
            class="table table-hover table-bordered table-striped text-center"
          >
            <thead>
              <tr>
                <th>ID</th>
                <th>Book Name</th>
                <th>Category</th>
                <th>Author</th>
                <th>Pages</th>
                <th>Price</th>
                <th>Actions</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="item in booksList">
                <td>{{ item.book.id }}</td>
                <td>{{ item.book.name }}</td>
                <td>{{ item.book.category }}</td>
                <td>{{ item.book.author }}</td>
                <td>{{ item.book.pages }}</td>
                <td>{{ handleCurrency(item.book.price.value) }}</td>
                <td>
                  <button
                    class="btn btn-danger"
                    type="button"
                    @click="removeFromList(item)"
                  >
                    Remove
                  </button>
                </td>
              </tr>
            </tbody>
            <tfoot>
              <tr>
                <th colspan="3">Total Price:</th>
                <td colspan="3">{{ handleCurrency(getTotalPrice()) }}</td>
                <td>
                  <button class="btn btn-primary" @click="checkout">
                    checkout
                  </button>
                </td>
              </tr>
            </tfoot>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import booksData from "./books";
export default {
  data: () => ({
    books: booksData,
    booksList: [],
    isWatchListVisible: false,
  }),
  methods: {
    addToList(book) {
      this.booksList.push({
        book: book,
      });
    },

    getTotalPrice() {
      let total = 0;
      for (item of this.booksList) {
        total += item.book.price.value;
      }
      return total;
    },

    booksListCount() {
      return this.booksList.length;
    },

    handleCurrency(value) {
      return new Intl.NumberFormat("en-US", {
        style: "currency",
        currency: "USD",
      }).format(value);
    },

    checkExist(bookID) {
      return this.booksList.some((item) => item.book.id == bookID);
    },

    removeFromList(book) {
      this.booksList.splice(
        this.booksList.findIndex((item) => item.book.id == book.book.id),
        1
      );
    },

    checkout() {
      let conf = window.confirm("Are you sure you want to checkout?");
      if (conf) {
        this.booksList = [];
      }
    },
  },
};
</script>

<style scoped>
.bord {
  border: 2px solid red;
}

.less {
  color: red;
}

.more {
  color: green;
}

.card-body span {
  font-weight: bolder;
}
</style>
