
<template>
    
    <div id="mainapp" class="container">
         <!--nav-->
         <nav class="navbar navbar-expand-lg bg-light">
            <div class="container-fluid">
              <a class="navbar-brand" href="#" @click.prevent="iscartvisible=false">
                BooK List
              </a>
              <div class="d-flex align-items-baseline justify-content-between">
                <p class="mx-2 mb-0 fw-bold">[ {{wishlist.list.length}} ] 
                  <template v-if="wishlist.list.length==1">Book</template> 
                  <template v-else>Books</template>
                   in your wish list [{{getTolalPrice()}}]
                  </p>
                <button class="btn btn-outline-primary ms-2" @click="iscartvisible=true"> <i class="bi bi-book me-2"></i>wish list</button>
              </div>
            </div>
          </nav>
         <!-- end of nav-->


         <!--book list -->
           <div class=" row m-auto text-center mt-4" v-if="iscartvisible==false">
            <div class="card border m-auto" style="width:23rem;" v-for="book in books">
                <h4 class="card-title mt-4" :title="book.author">{{book.name}}</h4><br>
                <img class="w-100" style="width: 200px; height: 350px;" :src="book.image"  >
                <div class="card-body row" style="width:23rem;">
                    <h5 class="">Author:{{book.author}}</h5><br><br>

                    <div class="col-7">
                    <h5 class="">Category:{{book.Category}}</h5>
                      <h5 :class="[book.pages>=200?'more':'less']">pages:{{book.pages}}</h5>
                    </div>
                    <div class="col-5">
                      <h5 class="">ISBN: {{book.ISBN}}</h5>
                      <h5 class="">Price: {{formatCurrency(book.price)}}</h5>
                    </div>
                    <button href="#" class="btn btn-primary" @click="addBtn(book) ,addElementTolist(book)" :disabled="book.addedToWishList" >Add To List</button>  
                                </div>
              </div>
           </div>
         <!-- end of book list -->

         <!-- wish list -->
            <div class="row text center" v-if="iscartvisible==true" >
            <h3 class="text-danger text-center mt-4" v-if="wishlist.list.length==0" > Your wish list is empty</h3>
            <img src="../assets/sadbook.png" class="w-50"  v-if="wishlist.list.length==0">
            <div class="container" v-else>
              <table class="table table-striped text-center" >
                <thead>
                  <tr>
                    <th>ID</th>
                    <th>Name</th>
                    <th>Price</th>
                    <th>Action</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="element in wishlist.list">
                    <td>{{element.book.id}}</td>
                    <td>{{element.book.name}}</td>
                    <td>{{formatCurrency(element.book.price)}}</td>
                    <td>
                      <button class="btn btn-outline-danger" @click="removeElement(element)">
                        <i class="fas fa-trash"></i> Delete
                      </button>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
         </div>
         <!--end of  wish list -->
         


        </div>
  </template>
  
  <script >
import  books  from "../BooksData";
export default {
     data:()=>({
            iscartvisible:false,
            books:books,
            wishlist:{
              list:[]
            }
           }),
           methods: {
            addBtn(book) {
                book.addedToWishList = true;
              },
              formatCurrency(value){
               return  Intl.NumberFormat("ar-SA",{
                  style:'currency',
                  currency:'SAR',
                  minimumFractionDigits:0
                }).format(value);
              },
              removeElement(element) {
                const index = this.wishlist.list.findIndex(e => e.book.id === element.book.id);
                if (index !== -1) {
                  this.wishlist.list.splice(index, 1);
                  element.book.addedToWishList = false;
                }
              },
              
              getTolalPrice() {
                let total = 0;
                for (let i = 0; i < this.wishlist.list.length; i++) {
                  total += this.wishlist.list[i].book.price;
                }
                return this.formatCurrency(total);
              },


              addElementTolist(book){
                this.wishlist.list.push({
                  book:book
                });
            }
           }
  };
  </script>