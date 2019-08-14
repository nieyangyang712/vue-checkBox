<template>
  <div>
    <Card>
      <div>
        已选中的权限:{{selectedPermissions}}
      </div>
      <Tree :data="data5" :render="renderContent" class="permission-tree-box"></Tree>
    </Card>
  </div>
</template>

<script>
export default {
  name: "rbac_role_permission_page",
  data() {
    return {
      selectedPermissions: ["p1_view", "p1_export"],
      data5: [
        {
          title: "parent 1",
          expand: true,
          permissions: [
            { value: "p1_view", text: "浏览" },
            { value: "p1_create", text: "创建" },
            { value: "p1_update", text: "更新" },
            { value: "p1_remove", text: "删除" },
            { value: "p1_export", text: "导出" }
          ],
          children: [
            {
              title: "child 1-1",
              expand: true,
              permissions: [
                { value: "p1_1_view", text: "浏览" },
                { value: "p1_1_create", text: "创建" },
                { value: "p1_1_update", text: "更新" },
                { value: "p1_1_remove", text: "删除" },
                { value: "p1_1_export", text: "导出" }
              ],
              children: [
                {
                  title: "leaf 1-1-1",
                  expand: true
                },
                {
                  title: "leaf 1-1-2",
                  expand: true
                }
              ]
            },
            {
              title: "child 1-2",
              expand: true,
              children: [
                {
                  title: "leaf 1-2-1",
                  expand: true
                },
                {
                  title: "leaf 1-2-1",
                  expand: true
                }
              ]
            }
          ]
        }
      ],
      buttonProps: {
        type: "default",
        size: "small"
      }
    };
  },
  methods: {
    renderContent(h, { root, node, data }) {
      return h(
        "span",
        {
          style: {
            display: "inline-block",
            width: "100%"
          },
          class: "permission-tree-node"
        },
        [
          h("span", [
            h("Icon", {
              props: {
                type: "ios-paper-outline"
              },
              style: {
                marginRight: "8px"
              }
            }),
            h("span", data.title)
          ]),
          h(
            "span",
            {
              style: {
                display: "inline-block",
                float: "right",
                marginRight: "32px"
              }
            },
            [
              h(
                "CheckboxGroup",
                {
                  props: {
                    value: this.selectedPermissions
                  },
                  on: {
                    "on-change": event => {
                      this.selectedPermissions = event;
                    }
                  }
                },
                (data.permissions || []).map(obj => {
                  return h(
                    "Checkbox",
                    {
                      props: {
                        label: obj.value
                      }
                    },
                    obj.text
                  );
                })
              )
            ]
          )
        ]
      );
    },
    append(data) {
      const children = data.children || [];
      children.push({
        title: "appended node",
        expand: true
      });
      this.$set(data, "children", children);
    },
    remove(root, node, data) {
      const parentKey = root.find(el => el === node).parent;
      const parent = root.find(el => el.nodeKey === parentKey).node;
      const index = parent.children.indexOf(data);
      parent.children.splice(index, 1);
    }
  },
  mounted() {}
};
</script>
<style>
.permission-tree-box ul.ivu-tree-children > li {
  margin: 0;
}
.permission-tree-node {
  padding: 10px 5px 10px 0;
  border-bottom: 1px solid #f6f6f6;
}
.permission-tree-node:hover {
  background-color: #f7f7f7;
}
</style>