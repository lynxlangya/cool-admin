<template>
	<cl-crud :ref="setRefs('crud')" @load="onLoad">
		<el-row type="flex" align="middle">
			<!-- 刷新按钮 -->
			<cl-refresh-btn />
			<!-- 新增按钮 -->
			<cl-add-btn />
			<!-- 删除按钮 -->
			<cl-multi-delete-btn />
			<cl-flex1 />
			<!-- 关键字搜索 -->
			<cl-search-key />
		</el-row>

		<el-row>
			<!-- 数据表格 -->
			<cl-table :ref="setRefs('table')" v-bind="table">
				<template #column-coverImage="{ scope }">
					<el-image
						:src="scope.row.coverImage"
						:preview-src-list="[scope.row.coverImage]"
						:style="{ borderRadius: '10px' }"
					/>
				</template>
				<template #column-icon="{ scope }">
					<icon-svg :name="scope.row.icon" size="16px" style="margin-top: 5px" />
				</template>
			</cl-table>
		</el-row>

		<el-row type="flex">
			<cl-flex1 />
			<!-- 分页控件 -->
			<cl-pagination />
		</el-row>

		<!-- 新增、编辑 -->
		<cl-upsert :ref="setRefs('upsert')" v-bind="upsert" />
	</cl-crud>
</template>

<script lang="ts">
import { defineComponent, inject, reactive } from 'vue';
import { CrudLoad, Upsert, Table } from 'cl-admin-crud-vue3/types';
import { useRefs } from '/@/core';

export default defineComponent({
	setup() {
		const { refs, setRefs } = useRefs();
		// 请求服务
		const service = inject<any>('service');

		// 新增、编辑配置
		const upsert = reactive<Upsert>({
			items: [
				{
					prop: 'name',
					label: '名称',
					span: 12,
					component: {
						name: 'el-input',
						props: { placeholder: '请填写名称', clearable: true }
					},
					rules: { required: true, message: '名称不能为空' }
				},
				{
					prop: 'description',
					label: '描述',
					span: 12,
					component: {
						name: 'el-input',
						props: {
							placeholder: '请填写描述',
							type: 'textarea',
							showWordLimit: true
						}
					},
					rules: { required: true, message: '描述不能为空' }
				},
				{
					prop: 'icon',
					label: '图标',
					span: 12,
					component: {
						name: 'cl-menu-icons'
					},
					rules: { required: true, message: '图标不能为空' }
				},
				{
					prop: 'sort',
					label: '排序',
					span: 12,
					component: {
						name: 'el-input-number',
						props: {
							placeholder: '请填写排序号',
							min: 0,
							max: 99,
							'controls-position': 'right'
						}
					}
				},
				{
					prop: 'coverImage',
					label: '封面',
					span: 12,
					component: {
						name: 'cl-upload'
						// props: {
						// 	drag: true
						// }
					},
					rules: { required: true, message: '请上传封面' }
				}
			]
		});

		// 表格配置
		const table = reactive<Table>({
			columns: [
				{ type: 'selection' },
				{ label: '封面', prop: 'coverImage', width: 80 },
				{ label: '类别名称', prop: 'name', align: 'center' },
				{ label: '图标', prop: 'icon', width: 80 },
				{ label: '类别描述', prop: 'description', align: 'center' },
				{ label: '创建时间', prop: 'createTime', sortable: true, align: 'center' },
				{ label: '修改时间', prop: 'updateTime', align: 'center' },
				{ label: '排序', prop: 'sort', align: 'center' },
				{ label: '文章数量', prop: 'article_count', align: 'center' },
				{ label: '操作', type: 'op' }
			]
		});

		// crud 加载
		function onLoad({ ctx, app }: CrudLoad) {
			// 绑定 service
			console.log(service);
			console.log(`%c -----`, 'color: #4CAF50; font-weight: bold');
			ctx.service(service.newsCategoriesService).done();
			app.refresh();
		}

		return {
			refs,
			setRefs,
			upsert,
			table,
			onLoad
		};
	}
});
</script>
