<template>
	<el-main class="bg">
		<el-form ref="form" :model="form" status-icon label-width="120px" v-if="is_view()">
			<el-col v-if="user_group === '管理员' || $check_field('get','fishing_point_name') || $check_field('add','fishing_point_name') || $check_field('set','fishing_point_name')" :xs="24" :sm="12" :lg="8">
				<el-form-item label="钓点名称" prop="fishing_point_name">
					<el-input id="fishing_point_name" v-model="form['fishing_point_name']" placeholder="请输入钓点名称"
							  v-if="user_group === '管理员' || (form['fishing_point_information_id'] && $check_field('set','fishing_point_name')) || (!form['fishing_point_information_id'] && $check_field('add','fishing_point_name'))" :disabled="disabledObj['fishing_point_name_isDisabled']"></el-input>
					<div v-else-if="$check_field('get','fishing_point_name')">{{form['fishing_point_name']}}</div>
				</el-form-item>
			</el-col>
			<el-col v-if="user_group === '管理员' || $check_field('get','fishing_point_position') || $check_field('add','fishing_point_position') || $check_field('set','fishing_point_position')" :xs="24" :sm="12" :lg="8">
				<el-form-item label="钓点位置" prop="fishing_point_position">
					<el-input id="fishing_point_position" v-model="form['fishing_point_position']" placeholder="请输入钓点位置"
							  v-if="user_group === '管理员' || (form['fishing_point_information_id'] && $check_field('set','fishing_point_position')) || (!form['fishing_point_information_id'] && $check_field('add','fishing_point_position'))" :disabled="disabledObj['fishing_point_position_isDisabled']"></el-input>
					<div v-else-if="$check_field('get','fishing_point_position')">{{form['fishing_point_position']}}</div>
				</el-form-item>
			</el-col>
			<el-col v-if="user_group === '管理员' || $check_field('get','species_of_fish') || $check_field('add','species_of_fish') || $check_field('set','species_of_fish')" :xs="24" :sm="12" :lg="8">
				<el-form-item label="鱼的种类" prop="species_of_fish">
					<el-input id="species_of_fish" v-model="form['species_of_fish']" placeholder="请输入鱼的种类"
							  v-if="user_group === '管理员' || (form['fishing_point_information_id'] && $check_field('set','species_of_fish')) || (!form['fishing_point_information_id'] && $check_field('add','species_of_fish'))" :disabled="disabledObj['species_of_fish_isDisabled']"></el-input>
					<div v-else-if="$check_field('get','species_of_fish')">{{form['species_of_fish']}}</div>
				</el-form-item>
			</el-col>
			<el-col v-if="user_group === '管理员' || $check_field('get','fishing_point_picture') || $check_field('add','fishing_point_picture') || $check_field('set','fishing_point_picture')" :xs="24" :sm="12" :lg="8">
				<el-form-item label="钓点图片" prop="fishing_point_picture">
					<el-upload :disabled="disabledObj['fishing_point_picture_isDisabled']" id="fishing_point_picture" class="avatar-uploader" drag
						accept="image/gif, image/jpeg, image/png, image/jpg" action="" :http-request="upload_fishing_point_picture"
						:show-file-list="false" v-if="user_group === '管理员' || (form['fishing_point_information_id'] && $check_field('set','fishing_point_picture')) || (!form['fishing_point_information_id'] && $check_field('add','fishing_point_picture'))">
						<img v-if="form['fishing_point_picture']" :src="$fullUrl(form['fishing_point_picture'])" class="avatar">
						<i v-else class="el-icon-plus avatar-uploader-icon"></i>
					</el-upload>
					<el-image v-else-if="$check_field('get','fishing_point_picture')" style="width: 100px; height: 100px"
						:src="$fullUrl(form['fishing_point_picture'])" :preview-src-list="[$fullUrl(form['fishing_point_picture'])]">
						<div slot="error" class="image-slot">
							<img src="../../../public/img/error.png" style="width: 90px; height: 90px" />
						</div>
					</el-image>
				</el-form-item>
			</el-col>
			<el-col v-if="user_group === '管理员' || $check_field('get','introduction_to_fishing_points') || $check_field('add','introduction_to_fishing_points') || $check_field('set','introduction_to_fishing_points')" :xs="24" :sm="12" :lg="8">
				<el-form-item label="钓点介绍" prop="introduction_to_fishing_points">
					<el-input type="textarea" id="introduction_to_fishing_points" v-model="form['introduction_to_fishing_points']" placeholder="请输入钓点介绍"
						v-if="user_group === '管理员' || (form['fishing_point_information_id'] && $check_field('set','introduction_to_fishing_points')) || (!form['fishing_point_information_id'] && $check_field('add','introduction_to_fishing_points'))" :disabled="disabledObj['introduction_to_fishing_points_isDisabled']"></el-input>
					<div v-else-if="$check_field('get','introduction_to_fishing_points')">{{form['introduction_to_fishing_points']}}</div>
				</el-form-item>
			</el-col>
			<el-col :xs="24" :sm="12" :lg="8">
				<el-form-item>
					<el-button type="primary" @click="submit()">提交</el-button>
					<el-button @click="cancel()">取消</el-button>
				</el-form-item>
			</el-col>

		</el-form>
	</el-main>
</template>

<script>
	import mixin from "@/mixins/page.js";

	export default {
		mixins: [mixin],
		data() {
			return {
				field: "fishing_point_information_id",
				url_add: "~/api/fishing_point_information/add?",
				url_set: "~/api/fishing_point_information/set?",
				url_get_obj: "~/api/fishing_point_information/get_obj?",
				url_upload: "~/api/fishing_point_information/upload?",

				query: {
					"fishing_point_information_id": 0,
				},

				form: {
					"fishing_point_name":'', // 钓点名称
					"fishing_point_position":'', // 钓点位置
					"species_of_fish":'', // 鱼的种类
					"fishing_point_picture":'', // 钓点图片
					"introduction_to_fishing_points":'', // 钓点介绍
					"fishing_point_information_id": 0, // ID

				},
				disabledObj:{
					"fishing_point_name_isDisabled": false,
					"fishing_point_position_isDisabled": false,
					"species_of_fish_isDisabled": false,
					"fishing_point_picture_isDisabled": false,
					"introduction_to_fishing_points_isDisabled": false,
				},

			}
		},
		methods: {
			/**
			 * 上传钓点图片
			 * @param {Object} param图片参数
			 */
			upload_fishing_point_picture(param){
				this.uploadFile(param.file, "fishing_point_picture");
			},

			/**
			 * 获取对象之前
			 * @param {Object} param
			 */
			get_obj_before(param) {
				var form = "";
				if(this.form && form){
					Object.keys(this.form).forEach(key => {
						Object.keys(form).forEach(dbKey => {
							// if(dbKey === "charging_standard"){
							// 	this.form['charging_rules'] = form[dbKey];
							// 	this.disabledObj['charging_rules_isDisabled'] = true;
							// };
							if(key === dbKey){
								this.disabledObj[key+'_isDisabled'] = true;
							}
						})
					})
				}
				$.db.del("form");
				return param;
			},

			/**
			 * 获取对象之后
			 * @param {Object} json
			 * @param {Object} func
			 */
			get_obj_after(json, func){

			},

			is_view(){
				var bl = this.user_group == "管理员";

				if(!bl){
					bl = this.$check_action('/fishing_point_information/table','add');
					console.log(bl ? "你有表格添加权限视作有添加权限" : "你没有表格添加权限");
				}
				if(!bl){
					bl = this.$check_action('/fishing_point_information/table','set');
					console.log(bl ? "你有表格添加权限视作有修改权限" : "你没有表格修改权限");
				}
				if(!bl){
					bl = this.$check_action('/fishing_point_information/view','add');
					console.log(bl ? "你有视图添加权限视作有添加权限" : "你没有视图添加权限");
				}
				if(!bl){
					bl = this.$check_action('/fishing_point_information/view','set');
					console.log(bl ? "你有视图修改权限视作有修改权限" : "你没有视图修改权限");
				}
				if(!bl){
					bl = this.$check_action('/fishing_point_information/view','get');
					console.log(bl ? "你有视图查询权限视作有查询权限" : "你没有视图查询权限");
				}

				console.log(bl ? "具有当前页面的查看权，请注意这不代表你有字段的查看权" : "无权查看当前页，请注意即便有字段查询权限没有页面查询权限也不行");

				return bl;
			},
			/**
			 * 上传文件
			 * @param {Object} param
			 */
			uploadimg(param) {
				this.uploadFile(param.file, "avatar");
			},

		},
		created() {
		}
	}
</script>

<style>
	.avatar-uploader .el-upload {
		border: 1px dashed #d9d9d9;
		border-radius: 6px;
		cursor: pointer;
		position: relative;
		overflow: hidden;
	}

	.avatar-uploader .el-upload:hover {
		border-color: #409EFF;
	}

	.avatar-uploader-icon {
		font-size: 28px;
		color: #8c939d;
		width: 178px;
		height: 178px;
		line-height: 178px;
		text-align: center;
	}

	.avatar {
		width: 178px;
		height: 178px;
		display: block;
	}
</style>
