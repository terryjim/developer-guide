# 左侧导航栏设置

左侧导航根据源码AppSidebarNav中所设属性设置

分析源码首先按是否传递以下参数来渲染导航栏，优先级1为最高，即若传递了title属性则其它属性无效

| 参数名 | 说明 | 优先级 | 用法 |
| :--- | :--- | :--- | :--- |
| title | 标题（固定标题） | 1 | title:true |
| divider | 分隔符 | 2 | 未知 |
| label |  | 3 | 未知 |
| children | 可展开折叠的下级菜单 | 4 | \[{...},{...}\] |
| 以上都没有 | 普通导航项 | 5 | {...} |

其它属性：

| 参数名参数名 | 说明 | 优先级 | 用法 |
| :--- | :--- | :--- | :--- |
| name | 显示标题 |  | name: '企业管理' |
| icon | name左侧图标，菜单缩小后会只显示图标 |  | icon:'icon-layers' |
| url | 导航地址 |  | url:'/admin' |
| wrapper | 修饰title型的name |  | 见下文 |
|  |  |  |  |

1、固定标题，只显示不做任何操作，设置title为true，在导航栏中显示name信息，通过wrapper修饰name值

```
{
      title: true,
      name: '企业管理',
      icon: 'icon-energy',
      wrapper: {            // optional wrapper object
        element: 'div',        // required valid HTML5 element tag
        attributes: { style: { fontSize: 15, color: "#8bc34a" } }        // optional valid JS object with JS API naming ex: { className: "my-class", style: { fontFamily: "Verdana" }, id: "my-id"}
      },
 }
```

2、普通导航，点击打开链接

```
{
      name: '企业列表',
      url: '/owner/companies',
      icon: 'icon-settings',
    }
```

3、展开操作，点击展开、折叠下部导航

```
{
      name: 'Base',
      url: '/base',
      icon: 'icon-puzzle',
      children: [
        {
          name: 'Breadcrumbs',
          url: '/base/breadcrumbs',
          icon: 'icon-puzzle',
        },
        {
          name: 'Cards',
          url: '/base/cards',
          icon: 'icon-puzzle',
        },
        ]
}
```

上述代码点击Base可以展开折叠Breadcrumbs、Cards导航栏

name属性可直接设置html元素，但注意在文件中要导入React

```
import React from 'react'
{
    name: <b style={{ fontSize: 15, color: "#8bc34a" }} onClick={(e)=>{ e.preventDefault();
        e.target.parentElement.parentElement.classList.toggle('open');}}>物业管家</b>,
    //由于coreui将name作字符串处理直接显示出来，自己加标签后物业管家往下移了一层导致点击物业管家时无法实现预想效果，因此原有的
    //parentElement元素open方法要移至再上一层即parentElement.parentElement上，请参考coreui源码
    icon:'icon-settings',    
    children:[
    {
      name: '物业报修',
      url: '/property/ticket',
      icon: 'icon-settings',
    },
    {
      name: '客服管理',
      url: '/property/hotlines',
      icon: 'icon-location-pin',
    },]
}
```

附AppSidebarNav源码：

```
'use strict';

exports.__esModule = true;

var _extends = Object.assign || function (target) { for (var i = 1; i < arguments.length; i++) { var source = arguments[i]; for (var key in source) { if (Object.prototype.hasOwnProperty.call(source, key)) { target[key] = source[key]; } } } return target; };

var _react = require('react');

var _react2 = _interopRequireDefault(_react);

var _reactRouterDom = require('react-router-dom');

var _reactstrap = require('reactstrap');

var _classnames = require('classnames');

var _classnames2 = _interopRequireDefault(_classnames);

var _propTypes = require('prop-types');

var _propTypes2 = _interopRequireDefault(_propTypes);

var _reactPerfectScrollbar = require('react-perfect-scrollbar');

var _reactPerfectScrollbar2 = _interopRequireDefault(_reactPerfectScrollbar);

require('react-perfect-scrollbar/dist/css/styles.css');

function _interopRequireDefault(obj) { return obj && obj.__esModule ? obj : { default: obj }; }

function _objectWithoutProperties(obj, keys) { var target = {}; for (var i in obj) { if (keys.indexOf(i) >= 0) continue; if (!Object.prototype.hasOwnProperty.call(obj, i)) continue; target[i] = obj[i]; } return target; }

function _classCallCheck(instance, Constructor) { if (!(instance instanceof Constructor)) { throw new TypeError("Cannot call a class as a function"); } }

function _possibleConstructorReturn(self, call) { if (!self) { throw new ReferenceError("this hasn't been initialised - super() hasn't been called"); } return call && (typeof call === "object" || typeof call === "function") ? call : self; }

function _inherits(subClass, superClass) { if (typeof superClass !== "function" && superClass !== null) { throw new TypeError("Super expression must either be null or a function, not " + typeof superClass); } subClass.prototype = Object.create(superClass && superClass.prototype, { constructor: { value: subClass, enumerable: false, writable: true, configurable: true } }); if (superClass) Object.setPrototypeOf ? Object.setPrototypeOf(subClass, superClass) : subClass.__proto__ = superClass; }

var propTypes = {
  children: _propTypes2.default.node,
  className: _propTypes2.default.string,
  navConfig: _propTypes2.default.any,
  navFunc: _propTypes2.default.oneOfType([_propTypes2.default.func, _propTypes2.default.string]),
  isOpen: _propTypes2.default.bool,
  staticContext: _propTypes2.default.any,
  tag: _propTypes2.default.oneOfType([_propTypes2.default.func, _propTypes2.default.string])
};

var defaultProps = {
  tag: 'nav',
  navConfig: {
    items: [{
      name: 'Dashboard',
      url: '/dashboard',
      icon: 'icon-speedometer',
      badge: { variant: 'info', text: 'NEW' }
    }]
  },
  isOpen: false
};

var AppSidebarNav = function (_Component) {
  _inherits(AppSidebarNav, _Component);

  function AppSidebarNav(props) {
    _classCallCheck(this, AppSidebarNav);

    var _this = _possibleConstructorReturn(this, _Component.call(this, props));

    _this.handleClick = _this.handleClick.bind(_this);
    _this.activeRoute = _this.activeRoute.bind(_this);
    _this.hideMobile = _this.hideMobile.bind(_this);
    return _this;
  }

  AppSidebarNav.prototype.handleClick = function handleClick(e) {
    e.preventDefault();
    e.target.parentElement.classList.toggle('open');
  };

  AppSidebarNav.prototype.activeRoute = function activeRoute(routeName, props) {
    return props.location.pathname.indexOf(routeName) > -1 ? 'nav-item nav-dropdown open' : 'nav-item nav-dropdown';
  };

  AppSidebarNav.prototype.hideMobile = function hideMobile() {
    if (document.body.classList.contains('sidebar-show')) {
      document.body.classList.toggle('sidebar-show');
    }
  };

  // nav list


  AppSidebarNav.prototype.navList = function navList(items) {
    var _this2 = this;

    return items.map(function (item, index) {
      return _this2.navType(item, index);
    });
  };

  // nav type


  AppSidebarNav.prototype.navType = function navType(item, idx) {
    return item.title ? this.navTitle(item, idx) : item.divider ? this.navDivider(item, idx) : item.label ? this.navLabel(item, idx) : item.children ? this.navDropdown(item, idx) : this.navItem(item, idx);
  };

  // nav list section title


  AppSidebarNav.prototype.navTitle = function navTitle(title, key) {
    var classes = (0, _classnames2.default)('nav-title', title.class);
    return _react2.default.createElement(
      'li',
      { key: key, className: classes },
      this.navWrapper(title),
      ' '
    );
  };

  // simple wrapper for nav-title item


  AppSidebarNav.prototype.navWrapper = function navWrapper(item) {
    return item.wrapper && item.wrapper.element ? _react2.default.createElement(item.wrapper.element, item.wrapper.attributes, item.name) : item.name;
  };

  // nav list divider


  AppSidebarNav.prototype.navDivider = function navDivider(divider, key) {
    var classes = (0, _classnames2.default)('divider', divider.class);
    return _react2.default.createElement('li', { key: key, className: classes });
  };

  // nav label with nav link


  AppSidebarNav.prototype.navLabel = function navLabel(item, key) {
    var classes = {
      item: (0, _classnames2.default)('hidden-cn', item.class),
      link: (0, _classnames2.default)('nav-label', item.class ? item.class : ''),
      icon: (0, _classnames2.default)('nav-icon', !item.icon ? 'fa fa-circle' : item.icon, item.label.variant ? 'text-' + item.label.variant : '', item.label.class ? item.label.class : '')
    };
    return this.navLink(item, key, classes);
  };

  // nav dropdown


  AppSidebarNav.prototype.navDropdown = function navDropdown(item, key) {
    var classIcon = (0, _classnames2.default)('nav-icon', item.icon);
    return _react2.default.createElement(
      'li',
      { key: key, className: this.activeRoute(item.url, this.props) },
      _react2.default.createElement(
        'a',
        { className: 'nav-link nav-dropdown-toggle', href: '#', onClick: this.handleClick },
        _react2.default.createElement('i', { className: classIcon }),
        item.name
      ),
      _react2.default.createElement(
        'ul',
        { className: 'nav-dropdown-items' },
        this.navList(item.children)
      )
    );
  };

  // nav item with nav link


  AppSidebarNav.prototype.navItem = function navItem(item, key) {
    var classes = {
      item: (0, _classnames2.default)(item.class),
      link: (0, _classnames2.default)('nav-link', item.variant ? 'nav-link-' + item.variant : ''),
      icon: (0, _classnames2.default)('nav-icon', item.icon)
    };
    return this.navLink(item, key, classes);
  };

  // nav link


  AppSidebarNav.prototype.navLink = function navLink(item, key, classes) {
    var url = item.url ? item.url : '';
    return _react2.default.createElement(
      _reactstrap.NavItem,
      { key: key, className: classes.item },
      this.isExternal(url) ? _react2.default.createElement(
        _reactstrap.NavLink,
        { href: url, className: classes.link, active: true },
        _react2.default.createElement('i', { className: classes.icon }),
        item.name,
        this.navBadge(item.badge)
      ) : _react2.default.createElement(
        _reactRouterDom.NavLink,
        { to: url, className: classes.link, activeClassName: 'active', onClick: this.hideMobile },
        _react2.default.createElement('i', { className: classes.icon }),
        item.name,
        this.navBadge(item.badge)
      )
    );
  };

  // badge addon to NavItem


  AppSidebarNav.prototype.navBadge = function navBadge(badge) {
    if (badge) {
      var classes = (0, _classnames2.default)(badge.class);
      return _react2.default.createElement(
        _reactstrap.Badge,
        { className: classes, color: badge.variant },
        badge.text
      );
    }
    return null;
  };

  AppSidebarNav.prototype.isExternal = function isExternal(url) {
    var link = url ? url.substring(0, 4) : '';
    return link === 'http';
  };

  AppSidebarNav.prototype.render = function render() {
    var _props = this.props,
        className = _props.className,
        children = _props.children,
        navConfig = _props.navConfig,
        attributes = _objectWithoutProperties(_props, ['className', 'children', 'navConfig']);

    delete attributes.isOpen;
    delete attributes.staticContext;
    delete attributes.Tag;

    var navClasses = (0, _classnames2.default)(className, 'sidebar-nav');

    // sidebar-nav root
    return _react2.default.createElement(
      _reactPerfectScrollbar2.default,
      _extends({ className: navClasses }, attributes, { option: { suppressScrollX: true } }),
      _react2.default.createElement(
        _reactstrap.Nav,
        null,
        children || this.navList(navConfig.items)
      )
    );
  };

  return AppSidebarNav;
}(_react.Component);

AppSidebarNav.propTypes = process.env.NODE_ENV !== "production" ? propTypes : {};
AppSidebarNav.defaultProps = defaultProps;

exports.default = AppSidebarNav;
module.exports = exports['default'];
```



