<template>
<div>
	<Row >
		<Col span="16">
			<Input  v-model="input" placeholder="请输入查询内容" type="text">
				<span slot="prepend">
					<Select v-model="searchBy" style="width:100px">
        				<Option v-for="item in itemSelect" :value="item.value" :key="item.value">{{ item.label }}</Option>
    				</Select>
				</span>
			</Input>
		</Col>
		<Col span="4">
			<ButtonGroup>
		        <Button icon="ios-search" type="primary" style="margin-top: 0.5px;margin-left: -0.5px;" @click="search">搜索</Button>
		        <Button style="margin-top: 0.5px;margin-left: -0.5px;" type="success" >设置索引</Button>
		    </ButtonGroup>
		</Col>
	</Row>
	<Row>
		<Col span="6">
			<Alert v-if="ifSearchSuccess" type="success" style="margin:5px;letter-spacing: 4.5px;font-weight: bold" show-icon closable>
				你总共搜索出{{availNum}}条信息
			</Alert>

		</Col>
	</Row>
	<FloatPanel v-show="ifSearchSuccess" style="float: right;margin-right: 25%">
		
	</FloatPanel>
	<ul v-for="(story,index) in showStorys">
		<li><StoryItem :storyData="story" :index="(page-1)*10+index"></StoryItem></li>
	</ul>
	<Page v-show="ifSearchSuccess" :total="availNum" show-elevator @on-change="changePageNumber" style="text-align: center;margin: 2em"></Page>
</div>
</template>
<script>
	import FloatPanel from '@/components/FloatPanel'
	import StoryItem from '@/components/StoryItem'
	export default {
		components:{
			StoryItem,
			FloatPanel
		},
		data(){
			return {
				availNum:0,
				input:'哈里',
				itemSelect: [
                    {	
                        value: 'id',
                        label: '文章ID'
                    },
                    {
                        value: 'title',
                        label: '标题'
                    },
                    {
                        value: 'content',
                        label: '内容'
                    }
                ],
                searchBy: 'title',
                ifSearchSuccess:false,
                storys:[],
                page:1
			}
		},
		computed:{
			showStorys(){
				return this.storys.slice(10*(this.page-1),10*this.page)
			}
		},
		methods:{
			search(){
				let _this = this
				this.$http.post('/servlet/search',{
					field:_this.searchBy,
					input:_this.input

				}).then(
					function(res){

						_this.storys = res.data
						_this.availNum = res.data.length
						_this.ifSearchSuccess = true
					})
			},
			changePageNumber(page){
				this.page = page
			}
		}
	}

</script>