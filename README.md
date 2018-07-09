
## 使用方法:
### 1.Application 的 onCreate()方法 初始化：

``` java

          /**
           * 初始化日志系统
           * context :    上下文
           * isDebug :    是不是Debug模式,true:崩溃后显示自定义崩溃页面 ;false:关闭应用,不跳转奔溃页面(默认)
           * CrashCallBack : 回调执行
           */
          MCrashMonitor.init(this, true, new MCrashCallBack() {
              @Override
              public void onCrash(File file) {
                  //可以在这里保存标识，下次再次进入把日志发送给服务器
                  Log.i(TAG, "CrashMonitor回调:" + file.getAbsolutePath());
              }
          });

```

### 2.动态设置其它额外信息到日志中（如用户手机号，wifi状态等信息）：

``` java

        String extraInfo = "用户手机号码：00000000" +
                "\n用户网络环境：wifi";
        MCrashMonitor.setCrashLogExtraInfo(extraInfo);

```

### 3.其它相关方法介绍：

``` java

        /**
         * Created by maning on 2017/4/20.
         * 主类
         */
        public class MCrashMonitor {

            /**
             * 初始化
             *
             * @param context 上下文
             */
            public static void init(Context context) {
                MCrashHandler crashHandler = MCrashHandler.getInstance();
                crashHandler.init(context);
            }

            /**
             * 初始化
             *
             * @param context 上下文
             * @param isDebug 是否处于debug状态
             */
            public static void init(Context context, boolean isDebug) {
                MCrashHandler crashHandler = MCrashHandler.getInstance();
                crashHandler.init(context, isDebug);
            }

            /**
             * 初始化
             *
             * @param context        上下文
             * @param crashCallBacks 回调
             */
            public static void init(Context context, MCrashCallBack crashCallBacks) {
                MCrashHandler crashHandler = MCrashHandler.getInstance();
                crashHandler.init(context, crashCallBacks);
            }

            /**
             * 初始化
             *
             * @param context        上下文
             * @param isDebug        是否处于debug状态
             * @param crashCallBacks 回调
             */
            public static void init(Context context, boolean isDebug, MCrashCallBack crashCallBacks) {
                MCrashHandler crashHandler = MCrashHandler.getInstance();
                crashHandler.init(context, isDebug, crashCallBacks);
            }

            /**
             * 日志列表页面
             *
             * @param context
             */
            public static void startCrashListPage(Context context) {
                Intent intent = new Intent(context.getApplicationContext(), CrashListActivity.class);
                intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
                context.getApplicationContext().startActivity(intent);
            }

            /**
             * 打开奔溃展示页面
             *
             * @param context
             */
            public static void startCrashShowPage(Context context) {
                Intent intent = new Intent(context.getApplicationContext(), CrashShowActivity.class);
                intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
                context.getApplicationContext().startActivity(intent);
            }

            /**
             * 获取日志的路径
             *
             * @param context
             * @return
             */
            public static String getCrashLogFilesPath(Context context) {
                return MFileUtils.getCrashLogPath(context);
            }

            /**
             * 设置额外的日志内容，当发生崩溃的时候会写入当前内容到文件开头
             * 例如L用户手机号码，Token , 网络环境等定制化东西
             *
             * @param content 内容
             */
            public static void setCrashLogExtraInfo(String content) {
                if (!TextUtils.isEmpty(content)) {
                    MCrashHandler crashHandler = MCrashHandler.getInstance();
                    crashHandler.setExtraContent(content);
                }
            }

        }

```