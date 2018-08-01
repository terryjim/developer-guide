# 全局参数配置config.js

public根目录下，设置全局参数window.TParams，主要配置后台接口地址、系统参数等。

后台接口地址尽可能以约定为主，如针对模块 _**project**_

_**\[保存\] **_操作默认为 defaultUrl/project/save

_**\[删除\] **_操作默认为 defaultUrl/project/save

_**\[查询\] **_操作默认为 defaultUrl/project/getByPage

```
const defaultUrl = "http://localhost:9000/"
const TParams = {
    defaultUrl: defaultUrl,
    defaultPageSize: 20,
    urls: {
        /* get_project_list: defaultUrl + 'project/getByPage',
        save_project: defaultUrl + 'project/save',
        del_project: defaultUrl + 'project/del', */
        get_department_buildings_list: defaultUrl + 'department/getDepartmentsAndBuildings', //视图v_building获取楼盘、楼栋、物业、项目部信息列表
        getBuildingsByProject: defaultUrl + 'building/getBuildingsByProject',  //根据楼盘id获取所辖楼栋列表
        allotBuildings: defaultUrl + 'building/allot', //绑定楼栋－－物业项目部
        save_accessControl: defaultUrl + 'accessControl/saveForm',
        get_oss_params: defaultUrl + 'oss/getParams',//获取阿里oss参数设置
        login: defaultUrl + 'auth/login',
        chgPwd: defaultUrl + 'auth/chgPwd',
        getProjectList: defaultUrl + 'project/getList',//获取楼盘列表
        getPropertyList: defaultUrl + 'property/getList',//获取物业公司列表
        getDepartmentList: defaultUrl + 'department/getList',//获取项目部列表
        initPassword: defaultUrl + 'department/initPassword'//重新初始化项目部管理员密码
    },
    manufacturers: [{ 'id': 1, 'name': '平冶' }, { 'id': 2, 'name': '智果' }]
}
window.TParams = TParams

```



