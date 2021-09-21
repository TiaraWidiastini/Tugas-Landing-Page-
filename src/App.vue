<template>
 <nav id="header" class="w-full  top-10 py-1 bg-purple-400 shadow-lg border-b border-black-300">
        <div class="w-full flex items-center justify-between mt-0 px-6 py-2">
            <label for="menu-toggle" class="cursor-pointer md:hidden block">
                      <svg class="fill-current text-blue-600" xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 20 20">
                         <title>menu</title>
                         <path d="M0 3h20v2H0V3zm0 6h20v2H0V9zm0 6h20v2H0v-2z"></path>
                      </svg>
                   </label>
            <input class="hidden" type="checkbox" id="menu-toggle">

            <div class="hidden md:flex md:items-center md:w-auto w-full order-3 md:order-1" id="menu">
                <nav>
                    <ul class="md:flex items-center justify-between text-base text-black pt-4 md:pt-0">
                        <li><a class="inline-block no-underline hover:text-white font-medium text-lg py-2 px-4 lg:-ml-2" href="#">Home</a></li>
                        <li><a class="inline-block no-underline hover:text-white font-medium text-lg py-2 px-4 lg:-ml-2" href="#">Products</a></li>
                        <li><a class="inline-block no-underline hover:text-white font-medium text-lg py-2 px-4 lg:-ml-2" href="#">About</a></li>
                    </ul>
                </nav>
            </div>

            <div class="order-2 md:order-3 flex flex-wrap items-center justify-end mr-0 md:mr-4" id="nav-content">
                <div class="auth flex items-center w-full md:w-full">
                    <button class="bg-red-200 text-black  p-2 rounded border border-gray-300 mr-4 hover:bg-gray-100 hover:text-gray-700"><b>Sign in</b></button>
                    <button class="bg-red-200 text-black  p-2 rounded border border-gray-300 mr-4 hover:bg-gray-100 hover:text-gray-700"><b>Sign up</b></button>
                </div>
            </div>
        </div>
    </nav>
    <div id="app">
        <!-- Container -->
        <div class="mx-auto">
            <div class="flex justify-center px-6 my-12">
                <!-- Row -->
                <div class="w-full xl:w-3/4 lg:w-11/12 flex">
                    <!-- Col -->
                    <div class="w-full h-auto p-6 border  bg-purple-500 hidden lg:block lg:w-5/12 bg-cover rounded-l-lg">
                        <h3 class="pt-4 text-2xl mb-3 text-center"><b>Produk</b></h3>
                        <form v-on:submit.prevent="addNewProduct" class="p-4 bg-purple-400 rounded">
                            <div class="grid grid-cols-4 gap-4 p-2">
                                <div class="md:col-span-1 col-span-4">Kode Barang</div>
                                <div class="md:col-span-3 col-span-4">
                                    <input type="text" v-model="inputP.code" class="border border-transparant rounded">
                                </div>
                            </div>
                            <div class="grid grid-cols-4 gap-4 p-2">
                                <div class="md:col-span-1 col-span-4">Nama Barang</div>
                                <div class="md:col-span-3 col-span-4">
                                    <input type="text" v-model="inputP.name" class="border border-transparant rounded">
                                </div>
                            </div>
                            <div class="grid grid-cols-4 gap-4 p-2">
                                <div class="md:col-span-1 col-span-4">Type Barang</div>
                                <div class="md:col-span-3 col-span-4">
                                    <select v-model="inputP.typeID" class="border border-transparant rounded" id="">
                                        <option v-for="item in items" :value="item.id" :key="item.id">
                                                {{item.type}}
                                        </option>
                                    </select>
                                </div>
                            </div>
                            <div class="grid grid-cols-4 gap-4 p-2">
                                <div class="md:col-span-1 col-span-4">Harga Barang</div>
                                <div class="md:col-span-3 col-span-4">
                                    <input type="number" v-model="inputP.price" class="border border-transparant rounded">
                                </div>
                            </div> 
                            <div class="grid grid-cols-4 gap-4 p-2">
                                <button class="py-3 px-3 text-black rounded-lg bg-red-200 shadow-lg block md:inline-block"><b>Save</b></button>
                            </div>
                            <div class="grid grid-cols-4 gap-4 p-2">
                                {{ message }}
                            </div>
                        </form>
                    </div>
                    <!-- Col -->
                    <div class="w-full lg:w-7/12 bg-purple-400 p-7 rounded-lg lg:rounded-l-none bg-purple-400">
                        <h3 class="pt-4 text-2xl text-center"><b>Product List</b></h3>
                        <table class="w-full">
                            <thead>
                                <tr class="text-md font-semibold tracking-wide text-left text-gray-900 bg-gray-100 uppercase border-b border-gray-600">
                                    <th>kode</th>
                                    <th>Nama</th>
                                    <th>Type</th>
                                    <th>harga</th>
                                    <th>quantity</th>
                                    <th>total harga</th>
                                    <th>Aksi</th>
                                </tr>
                            </thead>
                            <tbody>
                                <tr v-for="(item, index) in products" :key="index" class="text-gray-700">
                                    <td class="px-4 py-3 border">
                                        {{ item.code }}
                                    </td>
                                    <td class="px-4 py-3 border">
                                        {{ item.name }}
                                    </td>
                                    <td class="px-4 py-3 border">
                                        {{getType(item.typeID)}}
                                    </td>
                                    <td class="px-4 py-3 border">
                                        {{ item.price }}
                                    </td>
                                    <td class="border text-center px-6 py-2 w-28 flex items-center justify-center">
                                    <button class="px-2 py-1 rounded-full bg-red-600 text-white hover:bg-red-500" @click="handleMinButton(item.code)" type="button">-</button>
                                    <span class="mx-2">
                                        {{ item.quantity }}
                                    </span>
                                    <button class="px-2 py-1 rounded-full bg-blue-600 text-white hover:bg-blue-500" @click="handlePlusButton(item.code)" type="button">+</button>
                                    </td>
                                     <td class="border text-center px-6 py-2">{{ totalHarga(item.code) }}</td>
                                    <td class="px-4 py-3 border">
                                        <button v-on:click="products.splice(index, 1)">Remove</button>
                                        <button v-on:click="editProduct(index, item)">Edit</button>
                                    </td>
                                </tr>
                            </tbody>
                        </table>

                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      inputP: {
        quantity: 1,
      },
      message: "",
      items: [
        {
          id: 1,
          type: "Baju",
        },
        {
          id: 2,
          type: "Celana",
        },
      ],
      products: [],
      productID: 1,
      ind: null,
    };
  },
  methods: {
    addNewProduct: function () {
      if (this.ind != null) {
        Object.assign(this.products[this.ind], {
          code: this.inputP.code,
          name: this.inputP.name,
          typeID: this.inputP.typeID,
          price: this.inputP.price,
          quantity: this.inputP.quantity,
        });
      } else {
        this.products.push({
          code: this.inputP.code,
          name: this.inputP.name,
          typeID: this.inputP.typeID,
          price: this.inputP.price,
          quantity: this.inputP.quantity,
        });
      }
      this.inputP.code = "";
      this.inputP.name = "";
      this.inputP.typeID = "";
      this.inputP.price = "";
      this.ind = null;
      this.inputP.quantity = 1;
    },

    getItem: function (id) {
      let res = this.products.filter((e) => {
        if (e.code == id) return e;
      });
      return res;
    },

    editProduct: function (ind, p) {
      this.ind = ind;
      this.inputP.code = p.code;
      this.inputP.name = p.name;
      this.inputP.typeID = p.typeID;
      this.inputP.price = p.price;
      this.inputP.quantity = p.quantity;
    },

    handlePlusButton: function (id) {
      this.getItem(id)[0].quantity++;
    },

    handleMinButton: function (id) {
      if (this.getItem(id)[0].quantity != 1) this.getItem(id)[0].quantity--;
    },

    getType: function (id) {
      var m = this.items.filter(function (elem) {
        if (elem.id == id) return elem;
      });
      return m[0].type;
    },
    totalHarga: function (id) {
      let res = this.getItem(id);
      return res[0].quantity * res[0].price;
    },
  },
};
</script>