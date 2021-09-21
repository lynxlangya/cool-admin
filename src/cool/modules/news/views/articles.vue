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
						:style="{ borderRadius: '10px', width: '80px', height: '80px' }"
					/>
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
			items: []
		});

		// 表格配置
		const table = reactive<Table>({
			columns: [
				{ type: 'selection' },
				{ label: '文章标题', prop: 'title', align: 'center' },
				{ label: '封面', prop: 'coverImage', width: 120 },
				{ label: '作者', prop: 'author', width: 80 },
				{ label: '文章标签', prop: 'tags', align: 'center', width: 100 },
				{ label: '创建时间', prop: 'createTime', sortable: true, align: 'center' },
				{ label: '修改时间', prop: 'updateTime', align: 'center' },
				{ label: '文章类别', prop: 'categories', align: 'center' },
				{ label: '阅读数量', prop: 'readingsNumber', align: 'right', width: 80 },
				{ label: '操作', type: 'op' }
			]
		});

		// crud 加载
		function onLoad({ ctx, app }: CrudLoad) {
			// 绑定 service
			ctx.service(service.NewsArticlesService).done();
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
