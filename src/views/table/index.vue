<template>
	<cl-crud :ref="setRefs('crud')" @load="onLoad">
		<el-row type="flex">
			<cl-refresh-btn />
			<cl-add-btn />
			<cl-flex1 />
			<cl-search-key />
		</el-row>
		<el-row>
			<cl-table v-bind="table" />
		</el-row>
		<el-row type="flex">
			<cl-flex1 />
			<cl-pagination />
		</el-row>
		<cl-upsert v-model="form" v-bind="upsert" />
	</cl-crud>
</template>
<script lang="ts">
import { useRefs } from '/@/core';
import { defineComponent, reactive } from 'vue';
import { Table, Upsert } from 'cl-admin-crud-vue3/types';

export default defineComponent({
	setup() {
		const { setRefs } = useRefs();

		const form = reactive<any>({
			relevance: 1
		});

		const upsert = reactive<Upsert>({
			width: '600px',
			items: [
				{
					prop: 'name',
					label: '名称',
					span: 12,
					component: {
						name: 'el-input',
						props: { placeholder: '请填写名称' }
					},
					rules: { required: true, message: '名称不能为空' }
				},
				{
					prop: 'label',
					label: '标识',
					span: 12,
					component: {
						name: 'el-input',
						props: { placeholder: '请填写标识' }
					},
					rules: {
						required: true,
						message: '标识不能为空'
					}
				},
				{
					prop: 'remark',
					label: '备注',
					span: 24,
					component: {
						name: 'el-input',
						props: {
							placeholder: '请填写备注',
							type: 'textarea',
							rows: 4
						}
					}
				},
				{
					label: '功能权限',
					prop: 'menuIdList',
					value: [],
					component: {
						name: 'cl-role-perms'
					}
				},
				{
					label: '数据权限',
					prop: 'departmentIdList',
					value: [],
					component: {
						name: 'cl-dept-check'
					}
				}
			]
		});

		const table = reactive<Table>({
			props: {
				'row-key': 'id'
			},
			columns: [
				{ label: '类别名称', prop: 'name', align: 'center' },
				{ label: '图标', prop: 'icon', align: 'center' },
				{ label: '类别描述', prop: 'description', align: 'center' },
				{ label: '创建时间', prop: 'createTime', align: 'center' },
				{ label: '修改时间', prop: 'updateTime', align: 'center' },
				{ label: '文章数量', prop: 'article_count', align: 'center' },
				{ label: '操作', type: 'op' }
			]
		});

		function onLoad({ ctx, app }: any) {
			ctx.service({
				page() {
					return Promise.resolve({
						list: [
							{
								id: 1,
								name: '王云帆',
								createTime: '2021-08-19'
							},
							{
								id: 2,
								name: 'wangdaoo',
								createTime: '2021-08-19'
							}
						]
					});
				}
			}).done();
			app.refresh();
		}

		return {
			table,
			upsert,
			form,
			onLoad,
			setRefs
		};
	}
});
</script>
