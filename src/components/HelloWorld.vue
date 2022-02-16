<template>
  <div ref="coffee">

    <!-- show in alert-->
    <div class = "alert alert-success bg-color=success" v-if="showalert_success">کالای مورد نظر با موفقیت وارد سبد خرید شما شد.</div>
    <div class="alert alert-danger bg-color-danger" v-if="showalert_danger">لطفا تعداد را درست وارد نمایید</div>

    
    <!--header-->
    <ul>
      <li><a href="#home">صفحه اصلی</a></li>
      <li><a href="#about">درباره ما</a></li>
      <li><a href="#call">تماس با ما</a></li>
      <li style="float: right">
        <a
          href="#buy"
          class="active"
          data-bs-toggle="modal"
          data-bs-target="#exampleModal"
          >نمایش صورتحساب</a
        >
      </li>
    </ul>
  </div>

  <div>
    <div class="container  text-center">
      <div class="row">
        <!--Show products-->
        <div class="col-sm-4 py-2" v-for="(item, index) in items" :key="index">
          <div class="card" style="width: 15rem">
            <img
              id="myimg1"
              class="card-img-top"
              :src="item.src"
              alt="Card image cap"
            />
            <div class="card-body">
              <h5 class="card-title">{{ item.name }}</h5>
              <p class="card-text">
                طعم و مزه ای بسیار عالی و دلنشین دارد. امیدوارم خوش تان بیاید!!!
              </p>
            </div>
            <ul class="list-group list-group-flush">
              <li class="list-group-item">نام:{{ item.name }}</li>
              <li class="list-group-item">
                قیمت:<span ref="price" id="price-1">{{ item.price }}</span>
                هزارتومان
              </li>
            </ul>
            <div class="card-body">
              <label for="">تعداد کالای موردنظر:</label>
              <input type="number" name="" id="" min="0" v-model="item.count" style="margin-top: 10px; width:40px; margin-right:5px">
              <br><br>

              <button id="mybtn1" class="bg-success">
                <a
                  href="#"
                  class="card-link text-white"
                  style="text-decoration: none"
                  data-price="40000"
                  @click="addshop(item)"
                  >اضافه به سبد خرید</a
                >
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>

<!-- modal fro Invoice list-->
  <div class="modal" tabindex="-1" role="dialog" id="exampleModal" ref="modals">
    <div class="modal-dialog" role="document">
      <div class="modal-content">
        <div class="modal-header">
          <h5 class="modal-title">صورتحساب</h5>
          <button
            type="button"
            class="close"
            data-bs-dismiss="modal"
            aria-label="Close"
          >
            <span aria-hidden="true">&times;</span>
          </button>
        </div>
        <div class="modal-body">
          <table>

            <tr class="soorathesab">
              <th>نام محصول</th>
              <th>تعداد</th>
              <th>قیمت فی</th>
              <th>جمع کل</th>
            </tr>
            

            <tr v-for="(card,idx) in cards" :key="card.id" class="soorathesab">
              <td>{{card.name}}</td>
              <td><input type="number" v-model="card.count" style="width:44px;" @change="mychange(card)"></td>
              <td>{{card.price}}</td>
              <td>{{card.total_item}}</td>
              <td><svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-x-circle-fill" viewBox="0 0 16 16" @click="remove_card(card,idx)">
                <path d="M16 8A8 8 0 1 1 0 8a8 8 0 0 1 16 0zM5.354 4.646a.5.5 0 1 0-.708.708L7.293 8l-2.647 2.646a.5.5 0 0 0 .708.708L8 8.707l2.646 2.647a.5.5 0 0 0 .708-.708L8.707 8l2.647-2.646a.5.5 0 0 0-.708-.708L8 7.293 5.354 4.646z"/>
              </svg>
              </td>
            </tr>
            
          </table>

          <br>
          <p class="text-success">
            جمع کل خرید:{{totla_buy}} <span>هزار تومان</span>
          </p>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-primary" @click="remove_all_card(card,idx)">حذف صورتحساب</button>
          <button type="button" class="btn btn-danger" data-bs-dismiss="modal">
            خارج شدن
          </button>
        </div>
      </div>
    </div>



  </div>
</template>

<script >
export default {
  data() {
    return {
      showalert_success:false,
      showalert_danger:false,
      x:[],
      condition: true,
      counter: 0,
      totla_buy: 0,
      total_item: 0,
      cards: [],
      items: [
        { name: "اسپرسو", price: 40000, src:'https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQSOSTnxxiQ5sUFhzuYO4NXo4vV4lTRi3Lr8Q&usqp=CAU', id: 0, count: parseInt(''), total_item: 0,x:[] },
        { name: "کاپوچینو", price: 60000,src:'data:image/jpeg;base64,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' ,id: 1, count: parseInt(''), total_item: 0,x:[] },
        { name: "چای", price: 10000, src:'https://cdn.borna.news/servev2/aNNYqhnWv6sw/KxuoffTghAA,/%DA%86%D8%A7%DB%8C.jpg', id: 2, count: parseInt(''), total_item: 0,x:[] },
        { name: "دوغ", price: 5000, src:'https://millohypermarket.ir/wp-content/uploads/2021/04/uukhj.png', id: 3, count: parseInt(''), total_item: parseInt(''),x:[] },
        { name: "ماءالشعیر", price: 12000, src:'https://www.etmarket.org/wp-content/uploads/2021/08/%D8%AF%D9%84%D8%B3%D8%AA%D8%B1-%DB%8C%DA%A9-%D9%84%DB%8C%D8%AA%D8%B1%DB%8C.png', id: 4, count: parseInt(''), total_item: 0,x:[] },
        { name: "آب معدنی", price: 5000, src:'https://arghavancatering.com/wp-content/uploads/2019/01/ab-madani-500cc.jpg', id: 5, count: parseInt(''), total_item: 0 ,x:[]},
      ],
    };
  },

  methods: {

    /* add a product*/
    addshop(item) {
      
      
      if(item.count != 0 && !isNaN(item.count)){

        item.x.push(item.count)
      
        this.showalert_success = true;
        setTimeout(() => {
          this.showalert_success = false;
        }, 3000);
    
        item.total_item = item.price * item.count;

        if(item.count != 0 && item.x[item.x.length - 2] == undefined ){
          this.totla_buy = this.totla_buy + (item.price * item.count)
          this.cards.push(item)
      
        }else if(item.count != item.x[item.x.length - 2]) {
          this.totla_buy = this.totla_buy + (item.total_item - ( item.price * item.x[item.x.length - 2]))
          this.cards.count.push(item.count);
        } 
                  
      }
       else{
         this.showalert_danger = true
         setTimeout(() => {
          this.showalert_danger = false;
        }, 3000);
       }
      
    },

    mychange(card){
      card.x.push(card.count)
      card.total_item = card.count * card.price
      this.totla_buy = this.totla_buy + (card.total_item - ( card.price * card.x[card.x.length - 2]))
    },

    remove_all_card(){
      if(confirm("آیا مطمثن هستید؟")){
        this.cards.splice(0,this.cards.length)
        this.totla_buy = 0

        for(var i = 0 ; i< this.items.length; i++){
          this.items[i].count = parseInt('')
          this.items[i].total_item = 0
          this.items[i].x = []
        }
      }
      
    },

    remove_card(card,idx){
      if(confirm("آیا مطمئن هستید؟")){
        card.count = 0
        this.totla_buy = this.totla_buy - card.total_item
        this.cards.splice(idx,1)
        this.items[idx].count = parseInt('')
        this.items[idx].total_item = 0
        this.items[idx].x = []
      }
      
    }
  },
};
</script>

<style>

@font-face {
  font-family: "Byekan";
  src: url("./Byekan.ttf");
}

.card {
  font-weight: lighter;
  border: 4px solid rgba(136, 113, 136, 0.56);
  border-radius: 13px 14px 13px 18px;
}

.card-link:hover {
  color: red;
  font-size: 18px;
}

.list-group-item:hover {
  color: red;
}

* {
  font-family: "Byekan";
}

ul {
  list-style-type: none;
  margin: 0;
  padding: 0;
  overflow: hidden;
  background-color: #f8f4f4;
}

li {
  float: left;
  border-right: 1px solid #bbb;
}

li a {
  display: block;
  color: rgb(211, 178, 178);
  text-align: center;
  padding: 14px 16px;
  text-decoration: none;
}
li a:hover:not(.active) {
  background-color: #111;
}

.active {
  background-color: #0e8f39;
}

#title-basket {
  grid-template-columns: auto auto auto auto;
}

.soorathesab {
  display: flex;
  
}


#mybtn1 {
  transition: all 1s;
  border: 2px solid rgb(27, 11, 15);
  border-radius: 7px;
}
#mybtn1:hover {
  transform: scale(1.25, 1.25);
}

td,th{
  min-width: 110px;
  
}


</style>