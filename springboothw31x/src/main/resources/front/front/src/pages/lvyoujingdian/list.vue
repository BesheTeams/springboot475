<template>
<div>
	<div :style='{"padding":"12px","boxShadow":"0 0px 0px #ccc","margin":"30px auto 0","borderColor":"#fff","borderRadius":"0px","background":"#3086b9","borderWidth":"0","width":"calc(100% - 40px)","borderStyle":"solid"}' class="breadcrumb-preview">
		<el-breadcrumb :separator="'Ξ'" :style='{"width":"1200px","margin":"0 auto","fontSize":"14px","lineHeight":"1"}'>
			<el-breadcrumb-item>首页</el-breadcrumb-item>
			<el-breadcrumb-item v-for="(item, index) in breadcrumbItem" :key="index">{{item.name}}</el-breadcrumb-item>
		</el-breadcrumb>
	</div>
	
	<div class="list-preview" :style='{"width":"100%","padding":"0 20px","margin":"10px auto","position":"relative","background":"none"}'>
		<div class="category-1" :style='{"padding":"16px 20px 20px","margin":"0px auto 0","borderColor":"#eee","borderRadius":"0px","flexWrap":"wrap","background":"#98c6e2","borderWidth":"1px","display":"flex","width":"100%","borderStyle":"solid","height":"auto","order":"1"}'>
			<div class="item" :class="swiperIndex == '-1' ? 'active' : ''" @click="getList(1, '全部')" :plain="isPlain">全部</div>
			<div class="item" :class="swiperIndex == index ? 'active' : ''" v-for="(item, index) in fenlei" :key="index" @click="getList(1, item, 'btn' + index)" :ref="'btn' + index" plain>{{item}}</div>
		</div>
		
	
    <el-form :inline="true" :model="formSearch" class="list-form-pv" :style='{"padding":"20px 10px","margin":"0px auto 0","borderColor":"#00adb5","alignItems":"center","display":"flex","borderRadius":"0px","flexWrap":"wrap","background":"#f5f5f5","borderWidth":"0px 0 0px 0","width":"100%","borderStyle":"dashed","height":"auto","order":"2"}'>
      <el-form-item :style='{"alignItems":"center","margin":"0 4px 0 0","display":"flex"}'>
	    <div class="lable" v-if="true" :style='{"width":"auto","padding":"0 10px","lineHeight":"42px","display":"inline-block"}'>景点名称</div>
        <el-input v-model="formSearch.jingdianmingcheng" placeholder="景点名称" clearable></el-input>
      </el-form-item>
      <el-form-item :style='{"alignItems":"center","margin":"0 4px 0 0","display":"flex"}'>
		<div class="lable" v-if="true" :style='{"width":"auto","padding":"0 10px","lineHeight":"42px","display":"inline-block"}'>景点分类</div>
        <el-select v-model="formSearch.jingdianfenlei" placeholder="请选择景点分类" :clearable="true">
          <el-option v-for="(item, index) in jingdianfenleiOptions" :key="index" :label="item" :value="item"></el-option>
        </el-select>
      </el-form-item>
	  <el-button v-if=" true " :style='{"cursor":"pointer","border":"0px solid #fcbc45","padding":"0px 15px","boxShadow":"0px 0px 0px #ccc","margin":"0px 10px 0 0","color":"#fff","outline":"none","borderRadius":"4px","background":"linear-gradient(320deg, rgba(48,134,185,1) 0%, rgba(197,230,250,1) 80%, rgba(48,134,185,1) 100%),#3086b9","width":"auto","fontSize":"14px","lineHeight":"40px","height":"40px"}' type="primary" @click="getList(1, curFenlei)"><i v-if="true" :style='{"color":"#fff","margin":"0 10px 0 0","fontSize":"14px"}' class="el-icon-search"></i>查询</el-button>
	  <el-button v-if="isAuth('lvyoujingdian','新增')" :style='{"cursor":"pointer","border":"0px solid #db961f","padding":"0px 15px","boxShadow":"0px 0px 0px #f8a412","margin":"0px 10px 0 0","color":"#fff","outline":"none","borderRadius":"4px","background":"linear-gradient(320deg, rgba(61,199,196,1) 0%, rgba(152,247,245,1) 80%, rgba(61,199,196,1) 100%),rgb(61,199,196)","width":"auto","fontSize":"14px","lineHeight":"40px","height":"40px"}' type="primary" @click="add('/index/lvyoujingdianAdd')"><i v-if="true" :style='{"color":"#fff","margin":"0 10px 0 0","fontSize":"14px"}' class="el-icon-circle-plus-outline"></i>添加</el-button>
    </el-form>

	<div class="list" :style='{"width":"100%","margin":"0","background":"none","order":"3"}'>
		<!-- 样式一 -->
		
		<!-- 样式二 -->
		<div class="list2 index-pv1" :style='{"padding":"0 0px","margin":"20px 0 0","borderRadius":"0px","flexWrap":"wrap","background":"none","display":"flex","width":"100%","justifyContent":"space-between","height":"auto"}'>
			<div :style='{"border":"1px solid #eee","cursor":"pointer","padding":"10px","boxShadow":"0px 0px 4px #eee","margin":"0 0 20px","display":"flex","justifyContent":"space-between","borderRadius":"4px","flexWrap":"wrap","background":"rgba(255,255,255,1)","width":"49%","fontSize":"0","position":"relative","height":"240px"}' v-for="(item, index) in dataList" :key="index" @click="toDetail(item)" class="list-item animation-box">
				<img :style='{"cursor":"pointer","padding":"10px","borderColor":"#9dcde9","objectFit":"cover","borderRadius":"0","borderWidth":"6px","display":"inline-block","width":"48%","borderStyle":"dotted","height":"100%"}' v-if="item.jingdiantupian && item.jingdiantupian.substr(0,4)=='http'" :src="item.jingdiantupian" class="image" />
				<img :style='{"cursor":"pointer","padding":"10px","borderColor":"#9dcde9","objectFit":"cover","borderRadius":"0","borderWidth":"6px","display":"inline-block","width":"48%","borderStyle":"dotted","height":"100%"}' v-else :src="baseUrl + (item.jingdiantupian?item.jingdiantupian.split(',')[0]:'')" class="image" />
				<div class="item-info" :style='{"width":"48%","padding":"0px 10px","overflow":"hidden","display":"inline-block","height":"100%"}'>
					<div :style='{"padding":"0 10px","borderColor":"#9dcde9","margin":"0 0 10px","whiteSpace":"nowrap","overflow":"hidden","color":"#333","background":"none","borderWidth":"0 0 2px","lineHeight":"40px","fontSize":"14px","textOverflow":"ellipsis","borderStyle":"dotted"}' class="name ">{{item.jingdianmingcheng}}</div>
					<div :style='{"padding":"0 10px","borderColor":"#9dcde9","margin":"0 0 10px","whiteSpace":"nowrap","overflow":"hidden","color":"#333","background":"none","borderWidth":"0 0 2px","lineHeight":"40px","fontSize":"14px","textOverflow":"ellipsis","borderStyle":"dotted"}' class="name ">景点分类:{{item.jingdianfenlei}}</div>
					<div v-if="item.price" :style='{"padding":"10px","lineHeight":"24px","fontSize":"14px","color":"#f00","textAlign":"right","display":"block"}' class="price"><span :style='{"fontSize":"12px"}'>￥</span>{{item.price}}</div>
				</div>
			</div>
		</div>
	</div>

	
	<el-pagination
	  background
	  class="pagination"
	  :pager-count="7"
	  :page-size="pageSize"
	  :page-sizes="pageSizes"
	  prev-text="<"
	  next-text=">"
	  :hide-on-single-page="true"
	  :layout='["total","prev","pager","next","sizes","jumper"].join()'
	  :total="total"
	  :style='{"padding":"6px 10px","margin":"20px auto","whiteSpace":"nowrap","color":"#333","background":"#fff","width":"calc(100% - 40px)","fontWeight":"500","order":"4"}'
	  @current-change="curChange"
	  @prev-click="prevClick"
	  @next-click="nextClick"
	></el-pagination>

  </div>
</div>
</template>

<script>
  export default {
    //数据集合
    data() {
      return {
	    layouts: '',
		swiperIndex: -1,
        baseUrl: '',
        breadcrumbItem: [
          {
            name: '旅游景点'
          }
        ],
        formSearch: {
          jingdianmingcheng: '',
          jingdianfenlei: '',
        },
        fenlei: [],
        dataList: [],
        total: 1,
        pageSize: 16,
		pageSizes: [10,20,30,50],
        totalPage: 1,
        curFenlei: '全部',
        isPlain: false,
        indexQueryCondition: '',
	      jingdianfenleiOptions: [],
        timeRange: []
      }
    },
    created() {
      this.indexQueryCondition = this.$route.query.indexQueryCondition ? this.$route.query.indexQueryCondition : '';
      this.baseUrl = this.$config.baseUrl;
      this.$http.get('option/jingdianfenlei/jingdianfenlei').then(res => {
        if (res.data.code == 0) {
          this.jingdianfenleiOptions = res.data.data;
        }
      });
      this.getFenlei();
      this.getList(1, '全部');
    },
    //方法集合
    methods: {
      add(path) {
        this.$router.push({path: path});
      },
      getFenlei() {
        this.$http.get('option/jingdianfenlei/jingdianfenlei').then(res => {
          if (res.data.code == 0) {
            this.fenlei = res.data.data;
          }
        });
      },
      getList(page, fenlei, ref = '') {
		if(fenlei == '全部') this.swiperIndex = -1;
		for(let i=0;i<this.fenlei.length;i++) {
			if(fenlei == this.fenlei[i]) {
				this.swiperIndex = i;
				break;
			}
		}
        this.curFenlei = fenlei;
        if (this.curFenlei == '全部') {
          this.isPlain = false;
        } else {
          this.isPlain = true;
        }
        let params = {page, limit: this.pageSize};
        let searchWhere = {};
        if (this.formSearch.jingdianmingcheng != '') searchWhere.jingdianmingcheng = '%' + this.formSearch.jingdianmingcheng + '%';
        if (this.formSearch.jingdianfenlei != '') searchWhere.jingdianfenlei = this.formSearch.jingdianfenlei;
        if (this.curFenlei != '全部') searchWhere.jingdianfenlei = this.curFenlei;
        this.$http.get('lvyoujingdian/list', {params: Object.assign(params, searchWhere)}).then(res => {
          if (res.data.code == 0) {
            this.dataList = res.data.data.list;
            this.total = res.data.data.total;
            this.pageSize = res.data.data.pageSize;
            this.totalPage = res.data.data.totalPage;
			
			this.pageSizes = [this.pageSize, this.pageSize*2, this.pageSize*3, this.pageSize*5];
          }
        });
      },
      curChange(page) {
        this.getList(page,this.curFenlei);
      },
      prevClick(page) {
        this.getList(page,this.curFenlei);
      },
      nextClick(page) {
        this.getList(page,this.curFenlei);
      },
      toDetail(item) {
        this.$router.push({path: '/index/lvyoujingdianDetail', query: {detailObj: JSON.stringify(item)}});
      },
    }
  }
</script>

<style rel="stylesheet/scss" lang="scss" scoped>
	.list-preview .list-form-pv .el-input {
		width: auto;
	}

	.breadcrumb-preview .el-breadcrumb /deep/ .el-breadcrumb__separator {
		margin: 0 9px;
		color: #ccc;
		font-weight: 500;
	}
	
	.breadcrumb-preview .el-breadcrumb /deep/ .el-breadcrumb__inner a {
		color: #fff;
		display: inline-block;
	}
	
	.breadcrumb-preview .el-breadcrumb /deep/ .el-breadcrumb__inner {
		color: #fff;
		display: inline-block;
	}
	
	.category-1 .item {
		cursor: pointer;
		border: 0px solid #ffd800;
		padding: 0 10px;
		margin: 0;
		color: #666;
		font-size: 14px;
		border-color: #98c6e2 #ccc #98c6e2 #ccc;
		line-height: 40px;
		border-radius: 0px;
		box-shadow: 0px 0px 0px #ceb44a;
		background: linear-gradient(180deg, rgba(232,232,232,1) 0%, rgba(255,255,255,1) 50%, rgba(212,211,211,1) 100%),#fff;
		width: auto;
		border-width: 8px 16px 8px 16px;
		border-style: solid;
		text-align: center;
		height: 60px;
	}
	
	.category-1 .item:hover {
		cursor: pointer;
		border-radius: 0px;
		margin: 0;
		color: #fff;
		background: linear-gradient(180deg, rgba(88,157,199,1) 0%, rgba(170,208,231,1) 50%, rgba(95,166,208,1) 100%),#3086b9;
		width: auto;
		font-size: 14px;
		border-color: #98c6e2 #5fa6d0 #98c6e2 #5fa6d0;
		line-height: 40px;
		text-align: center;
	}
	
	.category-1 .item.active {
		cursor: pointer;
		border-radius: 0px;
		margin: 0 0px 0 0;
		color: #fff;
		background: linear-gradient(180deg, rgba(88,157,199,1) 0%, rgba(170,208,231,1) 50%, rgba(95,166,208,1) 100%),#3086b9;
		width: auto;
		font-size: 14px;
		border-color: #98c6e2 #5fa6d0 #98c6e2 #5fa6d0;
		border-width: 8px 16px 8px 16px;
		line-height: 40px;
		border-style: solid;
		text-align: center;
	}
	
	.category-2 .item {
		cursor: pointer;
		border-radius: 4px;
		box-shadow: 0;
		margin: 0 0 10px 0;
		color: #fff;
		background: linear-gradient(135deg, rgba(17,112,210,1) 0%, rgba(8,179,68,1) 100%);
		width: 100%;
		font-size: 14px;
		line-height: 36px;
		text-align: center;
	}
	
	.category-2 .item:hover {
		cursor: pointer;
		border: 1px solid #1170d2;
		border-radius: 4px;
		box-shadow: 0;
		margin: 0 0 10px 0;
		color: #fff;
		background: linear-gradient(135deg, rgba(8,179,68,1) 0%, rgba(17,112,210,1) 100%);
		width: 100%;
		font-size: 14px;
		line-height: 36px;
		text-align: center;
	}
	
	.category-2 .item.active {
		cursor: pointer;
		border: 1px solid #1170d2;
		border-radius: 4px;
		box-shadow: 0;
		margin: 0 0 10px 0;
		color: #fff;
		background: linear-gradient(135deg, rgba(8,179,68,1) 0%, rgba(17,112,210,1) 100%);
		width: 100%;
		font-size: 14px;
		line-height: 36px;
		text-align: center;
	}
	
	.list-form-pv .el-input /deep/ .el-input__inner {
		border: 1px solid #d2e9f6;
		border-radius: 4px;
		padding: 0 10px;
		box-shadow: inset 0px 0px 24px 0px rgba(48,134,185,.2);
		margin: 0px;
		outline: none;
		color: #333;
		width: 140px;
		font-size: 14px;
		line-height: 40px;
		height: 40px;
	}
	
	.list-form-pv .el-select /deep/ .el-input__inner {
		border: 1px solid #d2e9f6;
		border-radius: 8px;
		padding: 0 10px;
		box-shadow: inset 0px 0px 24px 0px rgba(48,134,185,.2);
		margin: 0px;
		outline: none;
		color: #666;
		width: 140px;
		font-size: 14px;
		line-height: 40px;
		height: 40px;
	}
	
	.list-form-pv .el-date-editor /deep/ .el-input__inner {
		border: 1px solid #d2e9f6;
		border-radius: 8px;
		padding: 0 30px;
		box-shadow: inset 0px 0px 24px 0px rgba(48,134,185,.2);
		margin: 0px;
		outline: none;
		color: #666;
		width: 140px;
		font-size: 14px;
		line-height: 40px;
		height: 40px;
	}
	
	.list .index-pv1 .animation-box {
		transform: rotate(0deg) scale(1) skew(0deg, 0deg) translate3d(0px, 0px, 0px);
		z-index: initial;
	}
	
	.list .index-pv1 .animation-box:hover {
		-webkit-perspective: 1000px;
		perspective: 1000px;
		transition: 0.3s;
		z-index: 1;
	}
	
	.list .index-pv1 .animation-box img {
		transform: rotate(0deg) scale(1) skew(0deg, 0deg) translate3d(0px, 0px, 0px);
	}
	
	.list .index-pv1 .animation-box img:hover {
		filter: brightness(1) ;
		transform: rotate(0deg)  scale(0.9);
		-webkit-perspective: 1000px;
		perspective: 1000px;
		transition: all 0.3s ease-in-out 0s;
	}
	
	.el-pagination /deep/ .el-pagination__total {
		margin: 0 10px 0 0;
		color: #666;
		font-weight: 400;
		display: inline-block;
		vertical-align: top;
		font-size: 13px;
		line-height: 28px;
		height: 28px;
	}
	
	.el-pagination /deep/ .btn-prev {
		border: none;
		border-radius: 2px;
		padding: 0;
		margin: 0 5px;
		color: #666;
		background: #f4f4f5;
		display: inline-block;
		vertical-align: top;
		font-size: 13px;
		line-height: 28px;
		min-width: 35px;
		height: 28px;
	}
	
	.el-pagination /deep/ .btn-next {
		border: none;
		border-radius: 2px;
		padding: 0;
		margin: 0 5px;
		color: #666;
		background: #f4f4f5;
		display: inline-block;
		vertical-align: top;
		font-size: 13px;
		line-height: 28px;
		min-width: 35px;
		height: 28px;
	}
	
	.el-pagination /deep/ .btn-prev:disabled {
		border: none;
		cursor: not-allowed;
		border-radius: 2px;
		padding: 0;
		margin: 0 5px;
		color: #C0C4CC;
		background: #f4f4f5;
		display: inline-block;
		vertical-align: top;
		font-size: 13px;
		line-height: 28px;
		height: 28px;
	}
	
	.el-pagination /deep/ .btn-next:disabled {
		border: none;
		cursor: not-allowed;
		border-radius: 2px;
		padding: 0;
		margin: 0 5px;
		color: #C0C4CC;
		background: #f4f4f5;
		display: inline-block;
		vertical-align: top;
		font-size: 13px;
		line-height: 28px;
		height: 28px;
	}
	
	.el-pagination /deep/ .el-pager {
		padding: 0;
		margin: 0;
		display: inline-block;
		vertical-align: top;
	}
	
	.el-pagination /deep/ .el-pager .number {
		cursor: pointer;
		padding: 0 4px;
		margin: 0 5px;
		color: #666;
		display: inline-block;
		vertical-align: top;
		font-size: 13px;
		line-height: 28px;
		border-radius: 2px;
		background: #f4f4f5;
		text-align: center;
		min-width: 30px;
		height: 28px;
	}
	
	.el-pagination /deep/ .el-pager .number:hover {
		cursor: pointer;
		padding: 0 4px;
		margin: 0 5px;
		color: #fff;
		display: inline-block;
		vertical-align: top;
		font-size: 13px;
		line-height: 28px;
		border-radius: 2px;
		background: #3086b9;
		text-align: center;
		min-width: 30px;
		height: 28px;
	}
	
	.el-pagination /deep/ .el-pager .number.active {
		cursor: default;
		padding: 0 4px;
		margin: 0 5px;
		color: #FFF;
		display: inline-block;
		vertical-align: top;
		font-size: 13px;
		line-height: 28px;
		border-radius: 2px;
		background: #3086b9;
		text-align: center;
		min-width: 30px;
		height: 28px;
	}
	
	.el-pagination /deep/ .el-pagination__sizes {
		display: inline-block;
		vertical-align: top;
		font-size: 13px;
		line-height: 28px;
		height: 28px;
	}
	
	.el-pagination /deep/ .el-pagination__sizes .el-input {
		margin: 0 5px;
		width: 100px;
		position: relative;
	}
	
	.el-pagination /deep/ .el-pagination__sizes .el-input .el-input__inner {
		border: 1px solid #DCDFE6;
		cursor: pointer;
		padding: 0 25px 0 8px;
		color: #606266;
		display: inline-block;
		font-size: 13px;
		line-height: 28px;
		border-radius: 3px;
		outline: 0;
		background: #FFF;
		width: 100%;
		text-align: center;
		height: 28px;
	}
	
	.el-pagination /deep/ .el-pagination__sizes .el-input span.el-input__suffix {
		top: 0;
		position: absolute;
		right: 0;
		height: 100%;
	}
	
	.el-pagination /deep/ .el-pagination__sizes .el-input .el-input__suffix .el-select__caret {
		cursor: pointer;
		color: #C0C4CC;
		width: 25px;
		font-size: 14px;
		line-height: 28px;
		text-align: center;
	}
	
	.el-pagination /deep/ .el-pagination__jump {
		margin: 0 0 0 24px;
		color: #606266;
		display: inline-block;
		vertical-align: top;
		font-size: 13px;
		line-height: 28px;
		height: 28px;
	}
	
	.el-pagination /deep/ .el-pagination__jump .el-input {
		border-radius: 3px;
		padding: 0 2px;
		margin: 0 2px;
		display: inline-block;
		width: 50px;
		font-size: 14px;
		line-height: 18px;
		position: relative;
		text-align: center;
		height: 28px;
	}
	
	.el-pagination /deep/ .el-pagination__jump .el-input .el-input__inner {
		border: 1px solid #DCDFE6;
		cursor: pointer;
		padding: 0 3px;
		color: #606266;
		display: inline-block;
		font-size: 14px;
		line-height: 28px;
		border-radius: 3px;
		outline: 0;
		background: #FFF;
		width: 100%;
		text-align: center;
		height: 28px;
	}
</style>
