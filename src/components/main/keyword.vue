<template>
  <div class="keyword container" style="background-color: #fff">
    <el-form :label-position="labelPosition" label-width="120px" :model="formLabelAlign" style="margin-top: 3%; width:70%;margin-left: 24%;">
      <!-- <form class="form-horizontal">
        <div class="form-group" style="margin-bottom: 28px;">
           <label for="name" class="col-sm-2 control-label">方案名：</label> 
          <div class="col-sm-10" style="border-radius:4px;padding:0;width:80%;">
            <input type="text" class="form-control" id="name" placeholder="输入方案名" v-model="project_name">
          </div> 
        </div>
      </form> -->
      <form class="form-horizontal" style="margin-top:60px;margin-bottom: 40px;">
        <div class="form-group">
         <!--  <label for="notkw" class="col-sm-2 control-label">排除词：</label> -->
          <div class="col-sm-10 mustkwcontent" @click="focus('mustkw')" style="padding-left: 10px;border-color:#ebebeb ">
          <img src="../../assets/icon/kw_bixu.png" style="padding-right:5px;height: 18px ">
                  <el-tag
            :key="tag"
            v-for="tag in mustags"
            :closable="true"
            :close-transition="false"
            @close="musthandleClose(tag)"
          >
          {{tag}}
          </el-tag>
            <input type="text" class="form-control" id="mustkw"  placeholder="请输入必须包含关键词" @blur="mustcreate_notag" v-model="mustkw" @keyup.enter="mustcreate_notag" style="height: 42px;font-size: 16px;">
          </div> 
          <span style="font-size: 10px;margin-left: 66%;color:#999"><i style="color:red">*</i>多个词之间用“<i style="color:red">，</i>”隔开</span>    
        </div>  
      </form>

      <form class="form-horizontal" style="margin-bottom: 40px;">
        <div class="form-group">
         <!--  <label for="kw" class="col-sm-2 control-label">关键词：</label> -->
          <div class="col-sm-10 kwcontent"  @click="focus('kw')" style="padding-left: 10px;border-color:#ebebeb ">
          <img src="../../assets/icon/kw_guanjianci.png" style="width:20px;height: 18px">
                  <el-tag
            :key="tag"
            v-for="tag in tags"
            :closable="true"
            :close-transition="false"
            @close="handleClose(tag)"
          >
          {{tag}}
          </el-tag>
            <input type="text" class="form-control" id="kw" placeholder="请输入关键词" @blur="create_tag" v-model="kw" @keyup.enter="create_tag" style="height: 42px;font-size: 16px;">
          </div> 
          <span style="font-size: 10px;margin-left: 66%;color:#999"><i style="color:red">*</i>多个词之间用“<i style="color:red">，</i>”隔开</span>      
        </div>  
      </form>  

      <form class="form-horizontal" style="margin-bottom: 40px;">
        <div class="form-group">
         <!--  <label for="notkw" class="col-sm-2 control-label">排除词：</label> -->
          <div class="col-sm-10 t_notkwcontent" @click="focus('t_notkw')" style="padding-left: 10px;border-color:#ebebeb ">
          <img src="../../assets/icon/kw_not.png" >
                  <el-tag
            :key="tag"
            v-for="tag in t_notags"
            :closable="true"
            :close-transition="false"
            @close="t_nothandleClose(tag)"
          >
          {{tag}}
          </el-tag>
            <input type="text" class="form-control" id="t_notkw" placeholder="请输入标题排除词" @blur="t_notcreate_notag" v-model="t_notkw" @keyup.enter="t_notcreate_notag" style="height: 42px;font-size: 16px;">
          </div> 
          <span style="font-size: 10px;margin-left: 66%;color:#999"><i style="color:red">*</i>多个词之间用“<i style="color:red">，</i>”隔开</span>    
        </div>  
      </form>

      <form class="form-horizontal" >
        <div class="form-group">
         <!--  <label for="notkw" class="col-sm-2 control-label">排除词：</label> -->
          <div class="col-sm-10 notkwcontent" @click="focus('notkw')" style="padding-left: 10px;border-color:#ebebeb ">
          <img src="../../assets/icon/kw_not_content.png" >
                  <el-tag
            :key="tag"
            v-for="tag in notags"
            :closable="true"
            :close-transition="false"
            @close="nothandleClose(tag)"
          >
          {{tag}}
          </el-tag>
            <input type="text" class="form-control" id="notkw" placeholder="请输入排除词" @blur="notcreate_notag" v-model="notkw" @keyup.enter="notcreate_notag" style="height: 42px;font-size: 16px;">
          </div> 
          <span style="font-size: 10px;margin-left: 66%;color:#999"><i style="color:red">*</i>多个词之间用“<i style="color:red">，</i>”隔开</span>    
        </div>  
      </form>

    </el-form>
      <button type="button" class="btn  btn-md btn-inline-block " style="width:66px;margin-left: 46%;margin-top:4%;height: 34px;font-size: 18px;background-color: #00a17c;
    border-color: #00a17c;color: white;" @click="to_list">保存</button>
  </div>
</template>

<script>
export default {
  mounted :function () {
    let _this=this;
    let project_id=JSON.parse(window.sessionStorage.getItem('project_id'));
    $('.keyword').css('height',window.screen.height);
    $.ajax({
     type: "GET",
     url: 'http://192.168.1.2:8080/rs0/api/v1.1/project/'+project_id+'/keyword',
     traditional: true,
     data: {
         "method": 'get'
     },
     success: function(data){
      for(let i=0;i<data.data.keywordList.length;i++){
        if(data.data.keywordList[i].isIncluded==1){
            _this.notags.push(data.data.keywordList[i].name);
        }else if(data.data.keywordList[i].isIncluded==5){
          _this.tags.push(data.data.keywordList[i].name)
        }else if(data.data.keywordList[i].isIncluded==0){
          _this.t_notags.push(data.data.keywordList[i].name)
        }else if(data.data.keywordList[i].isIncluded==6){
          _this.mustags.push(data.data.keywordList[i].name)
        }
       } 
      }
    })
  },
  data () {
  	return {
          labelPosition: 'left',
          formLabelAlign: {
            name: '招商银行',
            region: '',
            type: ''
          },
          project_name:'',
          tags: [],
          kw:'',
          notags:[],
          notkw:'',
          t_notags:[],
          t_notkw:'',
          mustags:[],
          mustkw:''
      }
  	},
  methods: {
    to_list () {
      let _this=this;
      let project_id=JSON.parse(window.sessionStorage.getItem('project_id'));
      if(this.mustags.length==0){
        this.$message({
          message: '必须包含关键词不能为空哦',
          type: 'warning'
        });
      }else{
        $.ajax({
         type: "POST",
         url: 'http://192.168.1.2:8080/rs0/api/v1.1/project/'+project_id+'/keyword',
         traditional: true,
         data: {
        'method': 'post',
        'includeKeywords': this.tags,
        'mustIncludeKeywords':this.mustags,
        'titleExcludeKeywords':this.t_notags,
        'contentExcludeKeywords': this.notags
         },
         success: function(data){
            console.log(JSON.stringify(data));
            console.log(data.success);
            console.log(data.message);
            console.log(_this.notags);
            window.location.href='#/main/list'
            $('.el-tabs__item:eq(0)').addClass('is-active');
            $('.el-tabs__item:eq(1)').removeClass('is-active');
         }
        })
      }     
    },
    handleClose(tag) {
        this.tags.splice(this.tags.indexOf(tag), 1);
      },
    nothandleClose(tag) {
        this.notags.splice(this.notags.indexOf(tag), 1);
      }, 
      t_nothandleClose(tag) {
        this.t_notags.splice(this.t_notags.indexOf(tag), 1);
      },
     musthandleClose(tag){
      this.mustags.splice(this.mustags.indexOf(tag), 1);
     }, 
    create_tag () {
        let kw = this.kw;
        let Tag=this.mustags.concat(this.notags).concat(this.tags).concat(this.t_notags)
        if (kw) {
          if(Tag.indexOf(kw)==-1){
            kw=kw.replace(/，/ig,','); //转化逗号
              let dd=kw.split(',');
              if(dd.length>1){ //若输入逗号即添加的数组长度大于一即链接
                this.tags=this.tags.concat(dd);
                 for(var j=0;j<this.tags.length;j++){
                  if(this.tags[j]==''){
                    this.tags.splice(j,1)
                  }
                 }
              }else{
                this.tags.push(kw);
              }      
            }else{
              this.$message({
                            message: '请不要输入与排除词、关键词重复的内容',
                            type: 'warning'
                    }); 
            }
          }    
        this.kw=''
      },
    notcreate_notag () {
        let notkw = this.notkw;
        let Tag=this.mustags.concat(this.notags).concat(this.tags)
        if (notkw) {
          if(Tag.indexOf(notkw)==-1){
            notkw=notkw.replace(/，/ig,','); //转化逗号
              let dd=notkw.split(',');
              if(dd.length>1){ //若输入逗号即添加的数组长度大于一即链接
                this.notags=this.notags.concat(dd);
                 for(var j=0;j<this.notags.length;j++){
                  if(this.notags[j]==''){
                    this.notags.splice(j,1)
                  }
                 }
              }else{
                this.notags.push(notkw);
              }      
            }else{
              this.$message({
                            message: '请不要输入与排除词、关键词重复的内容',
                            type: 'warning'
                    });
            }
          }   
        this.notkw=''
      },
      t_notcreate_notag () {
        let t_notkw = this.t_notkw;
        let Tag=this.mustags.concat(this.notags).concat(this.tags)
        if (t_notkw) {
          if(Tag.indexOf(t_notkw)==-1){
            t_notkw=t_notkw.replace(/，/ig,','); //转化逗号
              let dd=t_notkw.split(',');
              if(dd.length>1){ //若输入逗号即添加的数组长度大于一即链接
                this.t_notags=this.t_notags.concat(dd);
                 for(var j=0;j<this.t_notags.length;j++){
                  if(this.t_notags[j]==''){
                    this.t_notags.splice(j,1)
                  }
                 }
              }else{
                this.t_notags.push(t_notkw);
              }      
            }else{
              this.$message({
                            message: '请不要输入与排除词、关键词重复的内容',
                            type: 'warning'
                    });
            }
          }   
        this.t_notkw=''
      },
      mustcreate_notag () {
        let mustkw = this.mustkw;
        let Tag=this.mustags.concat(this.notags).concat(this.tags).concat(this.t_notags)
        if (mustkw) {
          if(Tag.indexOf(mustkw)==-1){
            mustkw=mustkw.replace(/，/ig,','); //转化逗号
              let dd=mustkw.split(',');
              if(dd.length>1){ //若输入逗号即添加的数组长度大于一即链接
                this.mustags=this.mustags.concat(dd);
                 for(var j=0;j<this.mustags.length;j++){
                  if(this.mustags[j]==''){
                    this.mustags.splice(j,1)
                  }
                 }
              }else{
                this.mustags.push(mustkw);
              }      
            }else{
              this.$message({
                        message: '请不要输入与排除词重复的内容',
                        type: 'warning'
                    }); 
            }
          }    
        this.mustkw=''
      },
    focus (n) {
        $('#'+n+'').focus();
      },
    blur () { 
    }
  }
}
</script>

<style lang="scss" scoped>
  .keyword{
    ::-webkit-scrollbar {    width: 12px;}/* 滚动槽 */
    ::-webkit-scrollbar-track {    -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.3);    border-radius: 10px;}/* 滚动条滑块 */
    ::-webkit-scrollbar-thumb {    border-radius: 10px;    background:  #00b38a;;    -webkit-box-shadow: inset 0 0 6px rgba(0,0,0,0.5);}
    //::-webkit-scrollbar-button {display: inline-block;background: red;}
   // ::-webkit-scrollbar-thumb:window-inactive {    background: rgba(255,0,0,0.4);}
    ::-webkit-input-placeholder {/*Chrome/Safari*/
    color:#ccc;
    }
    ::-moz-input-placeholder {/*Firefox*/
    color:#ccc;
    }
    >h3{
      margin-left: 6%;
      color:#f0ad4e
    }
    >hr{
      margin: 10px 0;
    }
    .form-horizontal{
        #name{
          border: 1px solid #ccc;
          border-radius:4px;
          padding:0px 4px;
          box-shadow:none; 
                 
        }
        #kw{
          display: inline-block;
          width:30%;
          outline: none !important;
          border-width: 0;
          text-decoration: none;
          box-shadow:none;
          padding: 6px 4px;
          
        }
        .kwcontent{
          border: 1px solid #ccc;
          border-radius:4px;
          padding:0px 4px;
          width:80%;
          cursor:text;
          >.el-tag{
            margin-left: 3px;
            background: rgb(50,168,128);
          }
        }
        .notkwcontent{
          border: 1px solid #ccc;
          border-radius:4px;
          padding:0px 4px;
          width:80%;
          cursor:text;
          >.el-tag{
            margin-left: 3px;
            background: rgb(237,99,84)
          }
        }
        #notkw{
          display: inline-block;
          width:30%;
          outline: none !important;
          border-width: 0;
          text-decoration: none;
          box-shadow:none;
          padding: 6px 4px; 
        }
        .t_notkwcontent{
          border: 1px solid #ccc;
          border-radius:4px;
          padding:0px 4px;
          width:80%;
          cursor:text;
          >.el-tag{
            margin-left: 3px;
            background: rgb(237,99,84)
          }
        }
        #t_notkw{
          display: inline-block;
          width:30%;
          outline: none !important;
          border-width: 0;
          text-decoration: none;
          box-shadow:none;
          padding: 6px 4px; 
        }
        .mustkwcontent{
          border: 1px solid #ccc;
          border-radius:4px;
          padding:0px 4px;
          width:80%;
          cursor:text;
          >.el-tag{
            margin-left: 3px;
            background: rgb(50,168,128)
          }
        }
        #mustkw{
          display: inline-block;
          width:30%;
          outline: none !important;
          border-width: 0;
          text-decoration: none;
          box-shadow:none;
          padding: 6px 4px; 
        }
        .el-tag{
          padding: 0;
          background-color: rgba(239,107,63,.7);
          i{
            right:0;
          }
        }
      }
  }
</style>