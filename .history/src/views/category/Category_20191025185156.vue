<template>
  <div class="category">
    <left-list v-bind:list="list" @listItemClick="listItemClick" :currentIndex="currentIndex"></left-list>
    <div>
      <top :top="categorydata[currentIndex].top" class="abc" v-if="categorydata[currentIndex].top != 0"/>
    </div>
  </div>
</template>

<script>


  import leftList from './children/leftList'
  import top from './children/top'

  import {getCategory,getSubcategory,getCategoryDetail} from "network/category"

  export default {
    name: 'Category',
    data() {
      return {
        list: [],
        currentIndex: 0,
        categorydata: [],
      }
    },
    components: {
    leftList,
    top
    },
    created() {
      // getCategory().then(res => {
      //  console.log(res)
      //   this.list = res.data.category.list
      //   for (let i = 0; i < this.list.length; i++) {
      //     this.categorydata[i] = {
      //       top: [],
      //       bottom: {
      //         new: [],
      //         pop: [],
      //         sell: []
      //       }
      //     }
      //   }
      //   return getSubcategory(this.list[this.currentIndex].maitKey)
      // }).then(res => {
      //       console.log(res);
      //     this.subcategory[this.currentIndex] = res.data.list})
      //    if (this.list) {
          //  getSubcategory(this.list[this.currentIndex].maitKey).then(res => {
          //   // console.log(res);
          // this.subcategory[this.currentIndex] = res.data.list
      // })
      //    }
      this._getCategory()
    },
      // getSubcategory()
    methods: {
      _getCategory() {
         getCategory().then( res=> {
           console.log(res)
           this.list = res.data.category.list
           for (let i = 0; i < this.list.length; i++) {
             this.categorydata[i] = {
               top: [],
               bottom: {
                 new: [],
                 pop: [],
                 sell: []
               }
              }
            }
            this._getSubcategory(0)
          })
          
      },
      _getSubcategory(index) {
        this.currentIndex = index
        let maitKey = this.list[index].maitKey
        getSubcategory(maitKey).then(res => {
          console.log(res);
          this.categorydata[this.currentIndex].top = res.data.list
        })
          this._getCategoryDetail('new')
          this._getCategoryDetail('sell')
          this._getCategoryDetail('pop')
      },
      _getCategoryDetail(type) {
        let miniWallkey = this.list[this.currentIndex].miniWallkey
        getCategoryDetail(miniWallkey, type).then(res => {
          console.log(res);
          
          this.categorydata[this.currentIndex].bottom[type] = res
        })
      },
      listItemClick(index) {
        // this.currentIndex = index
        // getSubcategory(this.list[index].maitKey).then(res => {
        //   console.log(res);
        // this.subcategory[index] = res.data.list
        this._getSubcategory(index)
      }
    },
}

</script>

<style scoped>
.category {
  display: flex;
  width: 100vw;
}
.abc {
  display: flex;
  flex-wrap: wrap;
  width: 75vw;
  justify-content: flex-start;
}
</style>
