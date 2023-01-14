# 仿网易严选小程序————简易版
- 在此声明本项目归网易所有， 只做学习所用， 请尊重原厂版权

- 本项目虽然引入了vant组件库 但是百分之九十以上还是使用的自定义组件（个人认为第一次做项目还是得自己多练练手才行） 

- 解构本项目实现功能
  - 首页功能介绍
  1. 自定义顶部导航 实现了滚动控制导航栏的功能栏变化
      交互细节1： 自定义导航数据和微信系统完全一致 数据精准   可以通过wx自身提供的api获取
      交互细节2： 功能栏显示为品牌图片时不提供跳转到搜索页面的功能   显示为搜索栏时提供跳转功能
      交互细节3： 功能栏的搜索栏并没有直接做成和主页主体的搜索栏的样式 而是做成其他页面需要的顶部搜索栏基础样式 
                目的是方便其他页面复用  主页页面可以通过对基础样式进一步修改达到和主页搜索栏一致
  2. 搜索栏轮播功能  自动轮播+跳转  跳转可以用 navgator标签来实现(小程序用该标签代替了a标签) 或者可以绑定点击事件 通过wx.navigateTo实现
      交互细节1：自动轮播时默认允许手动滑动轮播内容显然是不好的行为 所以这里使用 catchtouchmove定义一个事件来禁止该行为
  3. 图片滚动栏  在滚动栏下方 创造了一个滑块框  通过滚动图片来同步显示移动变化效果
      交互细节1： 数据驱动 跟随图片数据自动同步滑块长度  通过计算比例得到滑块滑动距离  不能把数据写死
  4. 标签栏 会有一个点击更新样式的功能 可以根据点击选中的标签的下标与当前下标是否一致来判断是否更新样式
      交互细节1： 未点击时需要显示第一个为默认选择的样式
  5. 内容区 需要实现同步标签栏的更新来用当前标签的数据驱动显示 
      交互细节1： 内容区的商品卡片点击跳转到详情页的数据，需要利用id的唯一性判断，这样才可以使得不同的商品卡片跳转至详情页的数据也是唯一的
                  实现的逻辑在w-goods-item和detail中注解了
      交互细节2： 有下拉加载更新数据的功能，采用分页的功能来实现
  6. 回到顶部按钮  通过改变scrollTop属性值来实现功能
      交互细节1： 通过设置scroll-with-animation="true"为其添加动画效果，这样回到顶部会有从当前位置逐渐到底顶部的视觉效果 提高用户体验感

  - 详情页页面功能介绍
  1. 图片横向滚动 
      交互细节1： 需要设置一个显示当前图片数的标识框  但是数字不能写死 是根据获取的数据来动态更新的
  2. 邮费信息栏
      交互细节1： 设置点击事件来显示和隐藏动画，  动画的实现可以通过设置一个遮罩层和一个显示层， 用延时透明度的变化来制造视觉效果
      交互细节2： 因为这里只简单设置了显示层的固定内容，并没有需要设置显示层的滚动，
                  所以这里的点击事件是冒泡事件，也就意味着无论是点击遮罩层还是显示层都能够实现显示和隐藏功能
                  如果需要实现显示层滚动显示内容，那么隐藏的功能则不能设置为冒泡事件
  3. 底部菜单栏  仅实现了一个添加购物车功能
      交互细节1： 当通过加入购物车的点击事件，这里需要做个反馈让用户知道成功添加了，使用wx.showToast()添加提示框 增强用户体验

  - 分类页面功能介绍
  1. 侧边导航栏  类似于首页页面的标签栏 也是通过点击更新样式 但是竖向需要考虑position:fixed 因为不能因为滚动导致侧边栏改变位置
      交互细节1： 同样默认未点击操作时选择第一项为更新样式  整个侧边栏需要固定定位，不随着页面内容滚动而改变
  2. 内容区  采用flex弹性布局  
      交互细节1： 数据驱动页面显示，所以我们需要将弹性布局的每个商品卡片的宽高固定写死，这样卡片才会自动换行
                  图片下方的文字实现水平垂直居中的方式其实很简单  
                  可以采用view标签包裹文字内容text-align: center;实现水平居中 设置line-height来实现垂直居中

  - 购物车页面功能介绍
  1. 购物车的商品卡片 可以单选 选中或取消
      交互细节1： 当商品被添加至购物车时，默认选中该商品，但是提供通过点击事件来单选选中或取消
  2. 底部菜单栏  可以实现全选或者全部取消勾选
      交互细节1： 当商品被添加至购物车时，默认选中该商品，但是提供通过点击事件统一改变商品项的单选功能
  3. 推荐商品区
      交互细节1： 该商品区的不同之处在于推荐的业务逻辑，本质上推荐区的数据实际上需要通过大数据及算法获取相应数据。
                  (此处我并没有实现该功能，仅仅通过普通数据驱动罢了)

  - 其他页面介绍
    1. 搜索页面本人是采用了Vant组件库的一些基础组件，写了一些布局及功能，总的来说并没有细致研究，就在此不多提及。
        需要的同学其实可以通过官方文档去使用，还是很方便的对某些项目开发来说。
    2. 个人账号和另一个搜索页面其实也仅仅是挂了张照片 + 一部分数据驱动的商品项，也没什么好说的。