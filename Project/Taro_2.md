# TARO build 命令后都会发生什么

> 上篇文章已经分析了通过 taro-cli 创建项目的过程，本章主要分析已经完成的项目如何通过 taro 编译成微信小程序项目

### 执行 build 生成目标文件夹，及准备编译条件
在通过 taro init 指令生成了 taro 项目后，项目 src 目录文件中就已经包含了基本的项目示例代码，通过编译即可在在微信开发者工具中执行。
```
npm run build:weapp           // 项目编译生成微信小程序项目文件

// package.json

{
  scripts: {
    "build:weapp": "taro build --type weapp"  // 执行 taro build 命令 type 类型指定为 weapp
  }
}
```
回到 taro-cli 脚手架项目中，build 执行的过程会告诉我们，taro 编译时具体做了什么，实际上 taro 也许没有我们想象中那样神奇。

```
// @tarojs/cli/bin/taro-build
// build 指令参数定义
program
  .option('--type [typeName]', 'Build type, weapp/swan/alipay/tt/h5/quickapp/rn/qq/jd')
  .parse(process.argv)

// 判断是否存在配置文件，若不存在则退出
// 此配置文件即为项目中 config 文件夹下的配置文件，指定项目编译源文件及输出的目标文件

if (!fs.existsSync(projectConfPath)) {
  console.log(chalk.red(`找不到项目配置文件${PROJECT_CONFIG}，请确定当前目录是Taro项目根目录!`))
  process.exit(1)
}

// 将 node 执行环境的 env 修改为执行指令时传入的 type 
process.env.TARO_ENV = type
// 当前路经下的配置文件 根据node 环境 返回 dev 配置或者 pro 配置 的相应 config 文件
const projectConf = require(projectConfPath)(_.merge)

// 执行 build，传入 appPath 即项目路径，及编译参数
// appPath 为当前 node 执行的文件夹地址，即当前项目的文件夹地址
build(appPath, {
  type,
  watch,
  port: typeof port === 'string' ? port: undefined,
  release: !!release,
  page, component
})

build 函数执行，此时函数的参数分别为：
type: weapp, watch: false, port: undefined, release: false, page: null, component: null

// @tarojs/cli/dist/build
bfunction build(appPath, buildConfig) {
  // 返回一个 Promise 对象指针
  return __awaiter(this, void 0, void 0, function* () {
    const { type, watch, platform, port, release, page, component, uiIndex } = buildConfig;
    // 和build 文件一致，取的 config/index.js 文件并合并 配置的 dev或这 pro 
    const configDir = require(path.join(appPath, constants_1.PROJECT_CONFIG))(_.merge);
    // 输出文件地址
    const outputPath = path.join(appPath, configDir.outputRoot || config_1.default.OUTPUT_DIR);
    // 创建输出文件夹
    if (!fs.existsSync(outputPath)) {
        fs.ensureDirSync(outputPath);
    }
    switch (type) {
      case "weapp" /* WEAPP */:
        buildForWeapp(appPath, { watch, page, component });
        break;
  })

  buildForWeapp(appPath, { watch, page, component });
  // 其中参数为 {'执行弄的环境文件夹', {false, false, false}}

  function buildForWeapp(appPath, buildConfig) {
    require('./mini').build(appPath, Object.assign({
      adapter: "weapp" /* WEAPP */
    }, buildConfig));
  }
  // mini/build 的参数为
  {
    appPath,
    adapter: "weapp",
    watch: 0,
    page: 0,
    component: 0
  }

}
```
以上则已经从执行 npm run build:weapp 指令，解析出了源文件夹，目标文件夹，并在执行时清空了目标文件夹，用以实现目标文件的重新编译。

### 开始构建输出文件夹配置
build ...
```
// @tarojs/cli/dist/mini/index.js
function build(appPath, { watch, adapter = "weapp" /* WEAPP */, envHasBeenSet = false, port, release, page, component }) {
    return __awaiter(this, void 0, void 0, function* () {
        // 构建路径 获取源路径，目标路径，入口文件路径，入口文件文件名，npm 路径
        // setBuildData 在 @tarojs/cli/dist/mini/helper.js 中定义
        const buildData = envHasBeenSet ? helper_1.getBuildData() : helper_1.setBuildData(appPath, adapter); 
        // false
        const isQuickApp = adapter === "quickapp" /* QUICKAPP */;
        let quickappJSON;
        // 检查版本号
        yield util_1.checkCliAndFrameworkVersion(appPath, adapter);
        // 设置环境变量 TARO_ENV = weapp
        process.env.TARO_ENV = adapter;
        if (!envHasBeenSet) {
            helper_1.setIsProduction(process.env.NODE_ENV === 'production' || !watch);
        }
        // 创建目标文件夹
        fs.ensureDirSync(buildData.outputDir);
        if (!isQuickApp) {
             // 将 appPath 目录下的 project.config.json 修改 miniprogramRoot 后写到  dist 目录下 project.config.json
            buildProjectConfig();
            // 如果为百度小程序，则写百度的配置文件 .frameworkinfo
            yield buildFrameworkInfo();
        }
        else {
            quickappJSON = readQuickAppManifest();
            helper_1.setQuickappManifest(quickappJSON);
        }
        // 拷贝 copy 配置中的文件到 目标文件下
        if (!isQuickApp) {
            util_1.copyFiles(appPath, buildData.projectConfig.copy);
        }

        // 编译入口文件
        const appConfig = yield entry_1.buildEntry();
        // 配置小程序的配置文件
        helper_1.setAppConfig(appConfig);

        // 编译 page 文件
        yield page_1.buildPages();
        // 如果传入了watch 参数，就观察文件变化
        if (watch) {
            watch_1.watchFiles();
        }
    });
}

```

### 编译入口文件及页面
buildEntry 
```
// @tarojs/cli/dist/mini/entry.js

// buildEntry()
function buildEntry() {
    return __awaiter(this, void 0, void 0, function* () {
        const { appPath, buildAdapter, constantsReplaceList, entryFilePath, sourceDir, outputDir, entryFileName, sourceDirName, outputDirName, projectConfig, outputFilesTypes, isProduction, jsxAttributeNameReplace } = helper_1.getBuildData();
        const weappConf = projectConfig.weapp || { appOutput: true };
        const appOutput = typeof weappConf.appOutput === 'boolean' ? weappConf.appOutput : true;
        // 读取入口文件并转化为 string
        const entryFileCode = fs.readFileSync(entryFilePath).toString();
        const outputEntryFilePath = path.join(outputDir, entryFileName);
        // 输出 “编译 入口文件 src/app.js"
        util_1.printLog("compile" /* COMPILE */, '入口文件', `${sourceDirName}/${entryFileName}`);
        // {
        //     code: appOutput.js,
        //     sourcePath: src/app.js,
        //     sourceDir: app.js,
        //     outputPath: dist,
        //     isApp: true,
        //     isTyped: none,
        //     adapter: weapp,
        //     constantsReplaceList: weapp,
        //     jsxAttributeNameReplace: 0
        // }


        try {
            const transformResult = wxTransformer({
                code: entryFileCode,
                sourcePath: entryFilePath,
                sourceDir,
                outputPath: outputEntryFilePath,
                isApp: true,
                // 标识是否为 typescript
                isTyped: constants_1.REG_TYPESCRIPT.test(entryFilePath),
                adapter: buildAdapter,
                env: constantsReplaceList,
                jsxAttributeNameReplace
            });
            // app.js的template忽略
            // 
            const res = astProcess_1.parseAst(constants_1.PARSE_AST_TYPE.ENTRY, transformResult.ast, [], entryFilePath, outputEntryFilePath);
            let resCode = res.code;
            if (buildAdapter !== "quickapp" /* QUICKAPP */) {
                resCode = yield compileScript_1.compileScriptFile(resCode, entryFilePath, outputEntryFilePath, buildAdapter);
                if (isProduction) {
                    resCode = util_1.uglifyJS(resCode, entryFilePath, appPath, projectConfig.plugins.uglify);
                }
            }
            // 处理res.configObj 中的tabBar配置
            const tabBar = res.configObj.tabBar;
            if (tabBar && typeof tabBar === 'object' && !util_1.isEmptyObject(tabBar)) {
                const { list: listConfig, iconPath: pathConfig, selectedIconPath: selectedPathConfig } = constants_1.CONFIG_MAP[buildAdapter];
                const list = tabBar[listConfig] || [];
                let tabBarIcons = [];
                list.forEach(item => {
                    item[pathConfig] && tabBarIcons.push(item[pathConfig]);
                    item[selectedPathConfig] && tabBarIcons.push(item[selectedPathConfig]);
                });
                tabBarIcons = tabBarIcons.map(item => path.resolve(sourceDir, item));
                if (tabBarIcons && tabBarIcons.length) {
                    res.mediaFiles = res.mediaFiles.concat(tabBarIcons);
                }
            }
            if (buildAdapter === "quickapp" /* QUICKAPP */) {
                // 生成 快应用 ux 文件
                const styleRelativePath = util_1.promoteRelativePath(path.relative(outputEntryFilePath, path.join(outputDir, `app${outputFilesTypes.STYLE}`)));
                const uxTxt = util_1.generateQuickAppUx({
                    script: resCode,
                    style: styleRelativePath
                });
                fs.writeFileSync(path.join(outputDir, `app${outputFilesTypes.TEMPL}`), uxTxt);
                util_1.printLog("generate" /* GENERATE */, '入口文件', `${outputDirName}/app${outputFilesTypes.TEMPL}`);
            }
            else {
                if (res.configObj.workers) {
                    buildWorkers(res.configObj.workers);
                }
                if (res.configObj.tabBar && res.configObj.tabBar.custom) {
                    yield buildCustomTabbar();
                }
                // 完成 app.json 及 app.js 的编译
                if (appOutput) {
                    fs.writeFileSync(path.join(outputDir, 'app.json'), JSON.stringify(res.configObj, null, 2));
                    util_1.printLog("generate" /* GENERATE */, '入口配置', `${outputDirName}/app.json`);
                    fs.writeFileSync(path.join(outputDir, 'app.js'), resCode);
                    util_1.printLog("generate" /* GENERATE */, '入口文件', `${outputDirName}/app.js`);
                }
            }
            const dependencyTree = helper_1.getDependencyTree();
            const fileDep = dependencyTree.get(entryFilePath) || {
                style: [],
                script: [],
                json: [],
                media: []
            };
            // 编译依赖的脚本文件
            if (util_1.isDifferentArray(fileDep['script'], res.scriptFiles)) {
                yield compileScript_1.compileDepScripts(res.scriptFiles, buildAdapter !== "quickapp" /* QUICKAPP */);
            }
            // 编译样式文件
            if (util_1.isDifferentArray(fileDep['style'], res.styleFiles) && appOutput) {
                yield compileStyle_1.compileDepStyles(path.join(outputDir, `app${outputFilesTypes.STYLE}`), res.styleFiles);
                util_1.printLog("generate" /* GENERATE */, '入口样式', `${outputDirName}/app${outputFilesTypes.STYLE}`);
            }
            // 拷贝依赖文件
            if (util_1.isDifferentArray(fileDep['json'], res.jsonFiles)) {
                helper_1.copyFilesFromSrcToOutput(res.jsonFiles);
            }
            if (util_1.isDifferentArray(fileDep['media'], res.mediaFiles)) {
                helper_1.copyFilesFromSrcToOutput(res.mediaFiles);
            }
            fileDep['style'] = res.styleFiles;
            fileDep['script'] = res.scriptFiles;
            fileDep['json'] = res.jsonFiles;
            fileDep['media'] = res.mediaFiles;
            dependencyTree.set(entryFilePath, fileDep);
            return res.configObj;
        }
        
    });
}
```
buildEntry 过程中，首先通过 babel-core 将入口文件转化为 ast 树，ast 进行依赖分析，将 taro、taro-weapp 复制到 dist/npm 文件夹中。 
分析入口文件的配置 config，将 config 写到 dist 的配置文件中。   
通过元素遍历将 Nav 指令转化为微信指令。  
将 jsx 语法转化为 js 语法。  

通过 parse 将 app.js 输出到 dist。  
编译 app.json 及 app.wxss 文件。 

#### 编译其他页面
其他页面编译逻辑跟入口页逻辑一致，通过对 ast 树，对页面依赖，页面配置进行分析遍历，然后编译出页面

### 总结
执行 build 脚本，执行逻辑如下：  
- 定义源码文件、文件路径，出口文件、文件路径
- 创建 dist 文件夹，并将项目的 project.config.json 文件写到 dist 文件夹下，创建 npm 目录用来存放 taro、taro-component。
- 通过 ast 分析入口页面依赖，遍历所有节点，将节点 react 指令转化为 微信小程序指令；将 config wxss 节点单独写到 dist 文件夹下
- 通过 ast 分析 page 页面依赖，遍历节点，将编译完成后的页面放到 daist 的 Page 目录下  

以上完成了页面编译的基本流程，后面将 ast 的具体实现来进行分析。 









