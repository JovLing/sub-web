<template>
  <div>
    <el-row style="margin-top: 10px">
      <el-col>
        <el-card>
          <div slot="header">
            <div style="text-align:center;font-size:15px">订 阅 转 换</div>
          </div>
          <el-container>
            <el-form :model="form" label-width="80px" label-position="left" style="width: 100%">
              <el-form-item label="订阅链接:">
                <el-input
                    v-model="form.sourceSubUrl"
                    type="textarea"
                    rows="3"
                    placeholder="支持各种订阅链接或单节点链接，多个链接每行一个或用 | 分隔"
                />
              </el-form-item>
              <el-form-item label="生成类型:">
                <el-select v-model="form.clientType" style="width: 100%">
                  <el-option v-for="(v, k) 在 options.clientTypes" :key="k" :label="k" :value="v"></el-option>
                </el-select>
              </el-form-item>
              <el-form-item label="后端地址:">
                <el-select
                    v-model="form.customBackend"
                    allow-create
                    filterable
                    @change="selectChanged"
                    placeholder="可自行输入后端地址"
                    style="width: 100%"
                >
                  <el-option v-for="(v, k) 在 options.customBackend" :key="k" :label="k" :value="v"></el-option>
                </el-select>
              </el-form-item>
              <el-form-item label="短链选择:">
                <el-select
                    v-model="form.shortType"
                    allow-create
                    filterable
                    placeholder="可自行输入短链API"
                    style="width: 100%"
                >
                  <el-option v-for="(v, k) 在 options.shortTypes" :key="k" :label="k" :value="v"></el-option>
                </el-select>
              </el-form-item>
              <el-form-item label="远程配置:">
                <el-select
                    v-model="form.remoteConfig"
                    allow-create
                    filterable
                    placeholder="请选择"
                    style="width: 100%"
                >
                  <el-option-group
                      v-for="group 在 options.remoteConfig"
                      :key="group.label"
                      :label="group.label"
                  >
                    <el-option
                        v-for="item 在 group.options"
                        :key="item.value"
                        :label="item.label"
                        :value="item.value"
                    ></el-option>
                  </el-option-group>
                </el-select>
              </el-form-item>
              <el-form-item label-width="0px">
                <el-collapse>
                  <el-collapse-item>
                    <template slot="title">
                      <el-form-item label="高级功能:" style="width: 100%;">
                        <el-button
                            type="limr"
                            style="width: 100%;"
                            icon="el-icon-more-outline"
                        >点击显示/隐藏
                        </el-button>
                      </el-form-item>
                    </template>
                    <el-form-item label="包含节点:">
                      <el-input v-model="form.includeRemarks" placeholder="要保留的节点，支持正则"/>
                    </el-form-item>
                    <el-form-item label="排除节点:">
                      <el-input v-model="form.excludeRemarks" placeholder="要排除的节点，支持正则"/>
                    </el-form-item>
                    <el-form-item label="节点命名:">
                      <el-input v-model="form.rename" placeholder="举例：`a@b``1@2`，|符可用\转义"/>
                    </el-form-item>       
                    <el-form-item label="远程设备:">
                      <el-input v-model="form.devid" placeholder="用于设置QuantumultX的远程设备ID"/>
                    </el-form-item>
                    <el-form-item label="更新间隔:">
                      <el-input v-model="form.interval" placeholder="返用于设置托管配置更新间隔，单位为天"/>
                    </el-form-item>
                    <el-form-item label="订阅命名:">
                      <el-input v-model="form.filename" placeholder="返回的订阅文件名，可以在支持文件名的客户端中显示出来"/>
                    </el-form-item>
                    <el-form-item class="eldiy" label-width="0px">
                      <el-row type="flex">
                        <el-col>
                          <el-checkbox v-model="form.nodeList" label="仅输出节点信息" border></el-checkbox>
                        </el-col>
                        <el-popover placement="bottom" v-model="form.extraset">
                          <el-row :gutter="10">
                            <el-col :span="12">
                              <el-checkbox v-model="form.emoji" label="添加旗帜emoji"></el-checkbox>
                            </el-col>
                            <el-col :span="12">
                              <el-checkbox v-model="form.insert" label="插入默认节点"></el-checkbox>
                            </el-col>
                          </el-row>
                          <el-row :gutter="10">
                            <el-col :span="12">
                              <el-checkbox v-model="form.udp" label="启用 UDP"></el-checkbox>
                            </el-col>
                            <el-col :span="12"> 
                              <el-checkbox v-model="form.xudp" label="启用 XUDP"></el-checkbox>
                            </el-col>
                          </el-row>
                          <el-row :gutter="10">
                            <el-col :span="12">
                              <el-checkbox v-model="form.tfo" label="启用 TFO"></el-checkbox>
                            </el-col>
                            <el-col :span="12">
                              <el-checkbox v-model="form.sort" label="基础节点排序"></el-checkbox>
                            </el-col>
                          </el-row>
                          <el-row :gutter="10">
                            <el-col :span="12">
                              <el-checkbox v-model="form.tpl。clash。doh" label="Clash.DoH"></el-checkbox>
                            </el-col>
                            <el-col :span="12">
                              <el-checkbox v-model="form.appendType" label="插入节点类型"></el-checkbox>
                            </el-col>
                          </el-row>
                          <el-row :gutter="10">
                            <el-col :span="12">
                              <el-checkbox v-model="form.tpl。surge。doh" label="Surge.DoH"></el-checkbox>
                            </el-col>
                            <el-col :span="12">
                              <el-checkbox v-model="form.tls13" label="开启 TLS_1.3"></el-checkbox>
                            </el-col>
                          </el-row>
                          <el-row :gutter="10">
                            <el-col :span="12">
                              <el-checkbox v-model="form.expand" label="展开规则全文"></el-checkbox>
                            </el-col>
                            <el-col :span="12">
                              <el-checkbox v-model="form.new_name" label="Clash新字段"></el-checkbox>
                            </el-col>
                          </el-row>
                          <el-row :gutter="10">
                            <el-col :span="12">
                              <el-checkbox v-model="form.scv" label="跳过证书验证"></el-checkbox>
                            </el-col>
                            <el-col :span="12">
                              <el-checkbox v-model="form.fdn" label="过滤不支持节点"></el-checkbox>
                            </el-col>
                          </el-row>
                          <el-button slot="reference">更多选项</el-button>
                        </el-popover>
                      </el-row>
                    </el-form-item>
                  </el-collapse-item>
                </el-collapse>
              </el-form-item>
              <div style="margin-top: 30px"></div>
              <el-divider content-position="center">
                <el-button
                    type="zhuti"
                    @click="change">
                  <i id="rijian" class="el-icon-sunny"></i>
                  <i id="yejian" class="el-icon-moon"></i>
                </el-button>
              </el-divider>
              <el-form-item label="定制订阅:">
                <el-input class="copy-content" disabled v-model="customSubUrl">
                  <el-button
                      slot="append"
                      v-clipboard:copy="customSubUrl"
                      v-clipboard:success="onCopy"
                      ref="copy-btn"
                      icon="el-icon-document-copy"
                  >复制
                  </el-button>
                </el-input>
              </el-form-item>
              <el-form-item label="订阅短链:">
                <el-input class="copy-content" disabled v-model="customShortSubUrl">
                  <el-button
                      slot="append"
                      v-clipboard:copy="customShortSubUrl"
                      v-clipboard:success="onCopy"
                      ref="copy-btn"
                      icon="el-icon-document-copy"
                  >复制
                  </el-button>
                </el-input>
              </el-form-item>
              <el-form-item label-width="0px" style="margin-top: 40px; text-align: center">
                <el-button
                    style="width: 120px"
                    type="danger"
                    @click="makeUrl"
                    :disabled="form.sourceSubUrl。length === 0 || btnBoolean"
                >生成订阅链接
                </el-button>
                <el-button
                    style="width: 120px"
                    type="danger"
                    @click="makeShortUrl"
                    :loading="loading"
                    :disabled="customSubUrl.length === 0"
                >生成短链接
                </el-button>
              </el-form-item>
              <el-form-item label-width="0px" style="text-align: center">
                <el-button
                    style="width: 120px"
                    type="primary"
                    @click="dialogUploadConfigVisible = true"
                    icon="el-icon-upload"
                    :loading="loading"
                >进阶配置
                </el-button>
                <el-button
                    style="width: 120px"
                    type="primary"
                    @click="dialogLoadConfigVisible = true"
                    icon="el-icon-copy-document"
                    :loading="loading"
                >从URL解析
                </el-button>
              </el-form-item>
            </el-form>
          </el-container>
        </el-card>
      </el-col>
    </el-row>
    <el-dialog
        :visible.sync="dialogUploadConfigVisible"
        :show-close="false"
        :close-on-click-modal="false"
        :close-on-press-escape="false"
        width="80%"
    >
      <el-tabs v-model="activeName" type="card">
        <el-tab-pane label="远程配置上传" name="first">
          <el-link type="danger" :href="sampleConfig" style="margin-bottom: 15px" target="_blank" icon="el-icon-info">
            参考案例
          </el-link>
          <el-form label-position="left">
            <el-form-item prop="uploadConfig">
              <el-input
                  v-model="uploadConfig"
                  type="textarea"
                  :autosize="{ minRows: 15， maxRows: 15}"
                  maxlength="50000"
                  show-word-limit
              ></el-input>
            </el-form-item>
          </el-form>
          <div style="float: right">
            <el-button type="primary" @click="uploadConfig = ''; dialogUploadConfigVisible = false">取 消</el-button>
            <el-button
                type="primary"
                @click="confirmUploadConfig"
                :disabled="uploadConfig.length === 0"
            >确 定
            </el-button>
          </div>
        </el-tab-pane>
        <el-tab-pane label="JS排序节点" name="second">
          <el-link type="success" :href="scriptConfig" style="margin-bottom: 15px" target="_blank" icon="el-icon-info">
            参考案例
          </el-link>
          <el-form label-position="left">
            <el-form-item prop="uploadScript">
              <el-input
                  v-model="uploadScript"
                  placeholder="使用JavaScript对节点进行自定义排序，本功能后端接口自动模版化，JS无需以挤在一行加换行符的形式输入，注意：如果你还需要自定义上传远程配置，此操作务必在其之后进行，另外，如果你需要同时进行JS排序和筛选节点，二三栏的确定按钮只需要任意按一个即可全部提交！！"
                  type="textarea"
                  :autosize="{ minRows: 15， maxRows: 15}"
                  maxlength="50000"
                  show-word-limit
              ></el-input>
            </el-form-item>
          </el-form>
          <div style="float: right">
            <el-button type="primary" @click="uploadScript = ''; dialogUploadConfigVisible = false">取 消</el-button>
            <el-button
                type="primary"
                @click="confirmUploadScript"
                :disabled="uploadScript.length === 0"
            >确 定
            </el-button>
          </div>
        </el-tab-pane>
        <el-tab-pane label="JS筛选节点" name="third">
          <el-link type="warning" :href="filterConfig" style="margin-bottom: 15px" target="_blank" icon="el-icon-info">
            参考案例
          </el-link>
          <el-form label-position="left">
            <el-form-item prop="uploadFilter">
              <el-input
                  v-model="uploadFilter"
                  placeholder="使用JavaScript对节点进行进阶筛选，本功能后端接口自动模版化，JS无需以挤在一行加换行符的形式输入，注意：如果你还需要自定义上传远程配置，此操作务必在其之后进行，另外，如果你需要同时进行JS排序和筛选节点，二三栏的确定按钮只需要任意按一个即可全部提交！"
                  type="textarea"
                  :autosize="{ minRows: 15， maxRows: 15}"
                  maxlength="50000"
                  show-word-limit
              ></el-input>
            </el-form-item>
          </el-form>
          <div style="float: right">
            <el-button type="primary" @click="uploadFilter = ''; dialogUploadConfigVisible = false">取 消</el-button>
            <el-button
                type="primary"
                @click="confirmUploadScript"
                :disabled="uploadFilter.length === 0"
            >确 定
            </el-button>
          </div>
        </el-tab-pane>
      </el-tabs>
    </el-dialog>
    <el-dialog
        :visible.sync="dialogLoadConfigVisible"
        :show-close="false"
        :close-on-click-modal="false"
        :close-on-press-escape="false"
        width="80%"
    >
      <div slot="title">
        可以从生成的长链接中解析信息,填入页面中去
      </div>
      <el-form label-position="left">
        <el-form-item prop="uploadConfig">
          <el-input
              v-model="loadConfig"
              type="textarea"
              :autosize="{ minRows: 15， maxRows: 15}"
              maxlength="5000"
              show-word-limit
          ></el-input>
        </el-form-item>
      </el-form>
      <div slot="footer" class="dialog-footer">
        <el-button type="primary" @click="loadConfig = ''; dialogLoadConfigVisible = false">取 消</el-button>
        <el-button
            type="primary"
            @click="confirmLoadConfig"
            :disabled="loadConfig.length === 0"
        >确 定</el-button>
      </div>
    </el-dialog>
  </div>
</template>
<script>
const configScriptBackend = process.env。VUE_APP_CONFIG_UPLOAD_BACKEND + '/api.php'
const remoteConfigSample = process.env。VUE_APP_SUBCONVERTER_REMOTE_CONFIG
const scriptConfigSample = process.env。VUE_APP_SCRIPT_CONFIG
const filterConfigSample = process.env。VUE_APP_FILTER_CONFIG
const defaultBackend = process.env。VUE_APP_SUBCONVERTER_DEFAULT_BACKEND + '/sub?'
const shortUrlBackend = process.env。VUE_APP_MYURLS_DEFAULT_BACKEND + '/short'
const configUploadBackend = process.env。VUE_APP_CONFIG_UPLOAD_BACKEND + '/sub.php'
export 默认 {
  data() {
    return {
      backendVersion: ""，
      centerDialogVisible: false，
      activeName: 'first'，
      // 是否为 PC 端
      isPC: true，
      btnBoolean: false，
      options: {
        clientTypes: {
          Clash: "clash"，
          ClashR: "clashr"，
          Surge4: "surge&ver=4"，
          Surge3: "surge&ver=3"，
          Surge2: "surge&ver=2"，
          "Quantumult X": "quanx"，
          Quantumult: "quan"，
          Loon: "loon"，
          Surfboard: "surfboard"，
          Mellow: "mellow"，
          V2Ray协议: "v2ray"，
          Trojan协议: "trojan"，
          "Shadowsocks(SIP002)": "ss"，
          "Shadowsocks Android(SIP008)": "sssub"，
          ShadowsocksR: "ssr"，
          ShadowsocksD: "ssd"，
          "混合订阅（mixed）": "mixed"，
          "自动判断客户端": "auto"，
        }，
        shortTypes: {
          "肥羊增强型短链": "https://v1.mk/short"，
          "肥羊备用短链": "https://d1.mk/short"，
          "品云短链": "https://sub.cm/short"，
          "suo.yt短链": "https://suo.yt/short"，
        }，
        customBackend: {
          "专属后端": "https://api.666510.xyz/sub?"，
          "肥羊增强型后端": "https://api.v1.mk/sub?"，
          "肥羊备用后端": "https://sub.d1.mk/sub?"，
          "つつ多地防失联": "https://api.tsutsu.one/sub?"，
          "sub-web作者提供": "https://api.wcc.best/sub?"，
          "subconverter作者提供": "https://sub.xeton.dev/sub?"，
          "本地局域网后端": "http://127.0.0.1:25500/sub?"，
        }，
        backendOptions: [
          {value: "https://api.666510.xyz/sub?"}，
          {value: "https://api.v1.mk/sub?"}，
          {value: "https://sub.d1.mk/sub?"}，
          {value: "https://api.tsutsu.one/sub?"}，
          {value: "https://api.wcc.best/sub?"}，
          {value: "https://sub.xeton.dev/sub?"}，
          {value: "http://127.0.0.1:25500/sub?"}，
        ]，
        remoteConfig: [
          {
            label: "ACL4SSR友情提供"，
            options: [
              {
                label: "ACL4SSR 默认版"，
                value: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online.ini"
              }，
              {
                label: "ACL4SSR 默认版+AdblockPlus"，
                value: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online_AdblockPlus.ini"
              }，
              {
                label: "ACL4SSR 默认版（无广告拦截）"，
                value: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online_NoReject.ini"
              }，
              {
                label: "ACL4SSR 精简版"，
                value: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online_Mini.ini"
              }，
              {
                label: "ACL4SSR 精简版+AdblockPlus"，
                value: "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online_Mini_AdblockPlus.ini"
              }
            ]
          }，
          {
            label: "基础配置"，
            options: [
              {
                label: "佩奇系列【🇹🇼】"，
                value: "https://raw.githubusercontent.com/JovLing/NetworkTools/LXin/Rule/Config/Comprehensive(showTW)/%E4%BD%A9%E5%A5%87%E7%B3%BB%E5%88%97.ini"
              }，
              {
                label: "TAG环球网络【🇹🇼】"，
                value: "https://raw.githubusercontent.com/JovLing/NetworkTools/LXin/Rule/Config/Comprehensive(showTW)/TAG.ini"
              }，
              {
                label: "通用配置【🇹🇼】"，
                value: "https://raw.githubusercontent.com/JovLing/NetworkTools/LXin/Rule/Config/Comprehensive(showTW)/%E9%80%9A%E7%94%A8.ini"
              }，
              {
                label: "佩奇系列【🇨🇳替换🇹🇼】"，
                value: "https://raw.githubusercontent.com/JovLing/NetworkTools/LXin/Rule/Config/Comprehensive(CNreplacesTW)/%E4%BD%A9%E5%A5%87%E7%B3%BB%E5%88%97.ini"
              }，
              {
                label: "TAG环球网络【🇨🇳替换🇹🇼】"，
                value: "https://raw.githubusercontent.com/JovLing/NetworkTools/LXin/Rule/Config/Comprehensive(CNreplacesTW)/TAG.ini"
              }，
              {
                label: "通用配置【🇨🇳替换🇹🇼】"，
                value: "https://raw.githubusercontent.com/JovLing/NetworkTools/LXin/Rule/Config/Comprehensive(CNreplacesTW)/%E9%80%9A%E7%94%A8.ini"
              }，
              {
                label: "独享IP【VPS】"，
                value: "https://raw.githubusercontent.com/JovLing/NetworkTools/LXin/Rule/Config/VPS/ClashVPS.ini"
              }
            ]
          }
        ]
      }，
      form: {
        sourceSubUrl: ""，
        clientType: ""，
        customBackend: "https://api.666510.xyz/sub?"，
        shortType: ""，
        remoteConfig: ""，
        excludeRemarks: ""，
        includeRemarks: ""，
        filename: ""，
        rename: ""，
        devid: ""，
        interval: ""，
        emoji: true，
        nodeList: false，
        extraset: false，
        tls13: false，
        udp: true，
        xudp: false，
        tfo: false，
        sort: false，
        expand: true，
        scv: false，
        fdn: false，
        appendType: false，
        insert: false， // 是否插入默认订阅的节点，对应配置项 insert_url
        new_name: true， // 是否使用 Clash 新字段
        tpl: {
          surge: {
            doh: false // dns 查询是否使用 DoH
          }，
          clash: {
            doh: false
          }
        }
      }，
      loading: false，
      customSubUrl: ""，
      customShortSubUrl: ""，
      dialogUploadConfigVisible: false，
      loadConfig: ""，
      dialogLoadConfigVisible: false，
      uploadFilter: ""，
      uploadScript: ""，
      uploadConfig: ""，
      filterConfig: filterConfigSample,
      scriptConfig: scriptConfigSample,
      sampleConfig: remoteConfigSample
    };
  }，
  创建() {
    document.title = "在线订阅转换工具";
    this。isPC = this。$getOS()。isPc;
  }，
  mounted() {
    this。tanchuang();
    this。form。clientType = "clash";
    this。getBackendVersion();
    this。anhei();
    let lightMedia = window.matchMedia('(prefers-color-scheme: light)');
    let darkMedia = window.matchMedia('(prefers-color-scheme: dark)');
    let callback = (e) => {
      if (e.matches) {
        this。anhei();
      }
    };
    if (typeof darkMedia.addEventListener === 'function' || typeof lightMedia.addEventListener === 'function') {
      lightMedia.addEventListener('change', callback);
      darkMedia.addEventListener('change', callback);
    } //监听系统主题，自动切换！
  }，
  methods: {
    selectChanged() {
      this。getBackendVersion();
    }，
    getUrlParam() {
      let query = window.location。search。substring(1);
      let vars = query.split('&');
      for (let i = 0; i < vars.length; i++) {
          var pair = vars[i]。split('=');
          if (pair[0] == "backend") {
              return decodeURIComponent(pair[1]);
          }
      }
      return "";
    }，
    anhei() {
      const getLocalTheme = window.localStorage。getItem("localTheme");
      const lightMode = window.matchMedia && window.matchMedia('(prefers-color-scheme: light)');
      const darkMode = window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)');
      if (getLocalTheme) {
        document.getElementsByTagName('body')[0]。className = getLocalTheme;
      } //读取localstorage，优先级最高！
      else if (getLocalTheme == null || getLocalTheme == "undefined" || getLocalTheme == "") {
        if (new Date()。getHours() >= 19 || new Date()。getHours() < 7) {
          document.getElementsByTagName('body')[0]。setAttribute('class'， 'dark-mode');
        } else {
          document.getElementsByTagName('body')[0]。setAttribute('class'， 'light-mode');
        } //根据当前时间来判断，用来对付QQ等不支持媒体变量查询的浏览器
        if (lightMode && lightMode.matches) {
          document.getElementsByTagName('body')[0]。setAttribute('class'， 'light-mode');
        }
        if (darkMode && darkMode.matches) {
          document.getElementsByTagName('body')[0]。setAttribute('class'， 'dark-mode');
        } //根据窗口主题来判断当前主题！
      }
    }，
    change() {
      var zhuti = document.getElementsByTagName('body')[0]。className;
      if (zhuti === 'light-mode') {
        document.getElementsByTagName('body')[0]。setAttribute('class'， 'dark-mode');
        window.localStorage。setItem('localTheme'， 'dark-mode');
      }
      if (zhuti === 'dark-mode') {
        document.getElementsByTagName('body')[0]。setAttribute('class'， 'light-mode');
        window.localStorage。setItem('localTheme'， 'light-mode');
      }
    }，
    tanchuang() {
      this。$alert(`<div style="text-align:center;font-size:15px;color:blue"><strong></br>私人api订阅转换网站，请勿使用，否则后果自负。</br></div>`， '隐私提示'， {
        confirmButtonText: '确定'，
        dangerouslyUseHTMLString: true，
        customClass: 'msgbox'
      });
    }，
    onCopy() {
      this。$message。success("已复制");
    }，
    makeUrl() {
      if (this。form。sourceSubUrl === "" || this。form。clientType === "") {
        this。$message。error("订阅链接与客户端为必填项");
        return false;
      }
      let backend =
          this。form。customBackend === ""
              ? defaultBackend
              : this。form。customBackend;
      let sourceSub = this。form。sourceSubUrl;
      sourceSub = sourceSub.replace(/(\n|\r|\n\r)/g， "|");
      this。customSubUrl =
          backend +
          "target=" +
          this。form。clientType +
          "&url=" +
          encodeURIComponent(sourceSub) +
          "&insert=" +
          this。form。insert;
      if (this。form。remoteConfig !== "") {
        this。customSubUrl +=
            "&config=" + encodeURIComponent(this。form。remoteConfig);
      }
      if (this。form。excludeRemarks !== "") {
        this。customSubUrl +=
            "&exclude=" + encodeURIComponent(this。form。excludeRemarks);
      }
      if (this。form。includeRemarks !== "") {
        this。customSubUrl +=
            "&include=" + encodeURIComponent(this。form。includeRemarks);
      }
      if (this。form。filename !== "") {
        this。customSubUrl +=
            "&filename=" + encodeURIComponent(this。form。filename);
      }
      if (this。form。rename !== "") {
        this。customSubUrl +=
            "&rename=" + encodeURIComponent(this。form。rename);
      }
      if (this。form。interval !== "") {
        this。customSubUrl +=
            "&interval=" + encodeURIComponent(this。form。interval * 86400);
      }
      if (this。form。devid !== "") {
        this。customSubUrl +=
            "&dev_id=" + encodeURIComponent(this。form。devid);
      }
      if (this。form。appendType) {
        this。customSubUrl +=
            "&append_type=" + this。form。appendType。toString();
      }
      if (this。form。tls13) {
        this。customSubUrl +=
            "&tls13=" + this。form。tls13。toString();
      }
      if (this。form。sort) {
        this。customSubUrl +=
            "&sort=" + this。form。sort。toString();
      }
      this。customSubUrl +=
          "&emoji=" +
          this。form。emoji。toString() +
          "&list=" +
          this。form。nodeList。toString() +
          "&xudp=" +
          this。form。xudp。toString() +
          "&udp=" +
          this。form。udp。toString() +
          "&tfo=" +
          this。form。tfo。toString() +
          "&expand=" +
          this。form。expand。toString() +
          "&scv=" +
          this。form。scv。toString() +
          "&fdn=" +
          this。form。fdn。toString();
      if (this。form。tpl。surge。doh === true) {
        this。customSubUrl += "&surge.doh=true";
      }
      if (this。form。clientType === "clash") {
        if (this。form。tpl。clash。doh === true) {
          this。customSubUrl += "&clash.doh=true";
        }
        this。customSubUrl += "&new_name=" + this。form。new_name。toString();
      }
      this。$copyText(this。customSubUrl);
      this。$message。success("定制订阅已复制到剪贴板");
    }，
    makeShortUrl() {
      let duan =
          this。form。shortType === ""
              ? shortUrlBackend
              : this。form。shortType;
      this。loading = true;
      let data = new FormData();
      data.append("longUrl"， btoa(this。customSubUrl));
      this。$axios
          。post(duan, data, {
            header: {
              "Content-Type": "application/form-data; charset=utf-8"
            }
          })
          。then(res => {
            if (res.data。代码 === 1 && res.data。ShortUrl !== "") {
              this。customShortSubUrl = res.data。ShortUrl;
              this。$copyText(res.data。ShortUrl);
              this。$message。success("短链接已复制到剪贴板（IOS设备和Safari浏览器不支持自动复制API，需手动点击复制按钮）");
            } else {
              this。$message。error("短链接获取失败：" + res.data。Message);
            }
          })
          。catch(() => {
            this。$message。error("短链接获取失败");
          })
          。finally(() => {
            this。loading = false;
          });
    }，
    confirmUploadConfig() {
      this。loading = true;
      let data = new FormData();
      data.append("config"， encodeURIComponent(this。uploadConfig));
      this。$axios
          。post(configUploadBackend, data, {
            header: {
              "Content-Type": "application/form-data; charset=utf-8"
            }
          })
          。then(res => {
            if (res.data。code === 0 && res.data。data !== "") {
              this。$message。success(
                  "远程配置上传成功，配置链接已复制到剪贴板"
              );
              this。form。remoteConfig = res.data。data;
              this。$copyText(this。form。remoteConfig);
              this。dialogUploadConfigVisible = false;
            } else {
              this。$message。error("远程配置上传失败: " + res.data。msg);
            }
          })
          。catch(() => {
            this。$message。error("远程配置上传失败");
          })
          。finally(() => {
            this。loading = false;
          });
    }，
    confirmLoadConfig() {
      if (this。loadConfig。indexOf("target")=== -1){
        this。$message。error("请输入正确的订阅地址,暂不支持短链接!");
        return;
      }
      let url
      try {
        url = new URL(this。loadConfig)
      } catch (error) {
        this。$message。error("请输入正确的订阅地址!");
        return;
      }
      this。form。customBackend = url.origin + url.pathname + "?"
      let param = new URLSearchParams(url.search);
      if (param.get("target")){
        let target = param.get("target");
        if (target === 'surge' && param.get("ver")) {
          // 类型为surge,有ver
          this。form。clientType = target+"&ver="+param.get("ver");
        } else if (target === 'surge'){
          //类型为surge,没有ver
          this。form。clientType = target+"&ver=4"
        } else {
          //类型为其他
          this。form。clientType = target;
        }
      }
      if (param.get("url")){
        this。form。sourceSubUrl = param.get("url");
      }
      if (param.get("insert")){
        this。form。insert = param.get("insert") === 'true';
      }
      if (param.get("config")){
        this。form。remoteConfig = param.get("config");
      }
      if (param.get("exclude")){
        this。form。excludeRemarks = param.get("exclude");
      }
      if (param.get("include")){
        this。form。includeRemarks = param.get("include");
      }
      if (param.get("filename")){
        this。form。filename = param.get("filename");
      }
      if (param.get("rename")){
        this。form。rename = param.get("rename");
      }
      if (param.get("interval")){
        this。form。interval = Math.ceil(param.get("interval")/86400) ;
      }
      if (param.get("dev_id")){
        this。form。devid = param.get("dev_id");
      }
      if (param.get("append_type")){
        this。form。appendType = param.get("append_type") === 'true';
      }
      if (param.get("tls13")){
        this。form。tls13 = param.get("tls13");
      }
      if (param.get("xudp")){
        this。form。xudp = param.get("xudp") === 'true';
      }
      if (param.get("sort")){
        this。form。sort = param.get("sort") === 'true';
      }
      if (param.get("emoji")){
        this。form。emoji = param.get("emoji") === 'true';
      }
      if (param.get("list")){
        this。form。nodeList = param.get("list") === 'true';
      }
      if (param.get("udp")){
        this。form。udp = param.get("udp") === 'true';
      }
      if (param.get("tfo")){
        this。form。tfo = param.get("tfo") === 'true';
      }
      if (param.get("expand")){
        this。form。expand = param.get("expand") === 'true';
      }
      if (param.get("scv")){
        this。form。scv = param.get("scv") === 'true';
      }
      if (param.get("fdn")){
        this。form。fdn = param.get("fdn") === 'true';
      }
      if (param.get("surge.doh")){
        this。form。tpl。surge。doh = param.get("surge.doh") === 'true';
      }
      if (param.get("clash.doh")){
        this。form。tpl。clash。doh = param.get("clash.doh") === 'true';
      }
      if (param.get("new_name")){
        this。form。new_name = param.get("new_name") === 'true';
      }
      this。dialogLoadConfigVisible = false;
    }，
    renderPost() {
      let data = new FormData();
      data.append("target"，encodeURIComponent(this。form。clientType));
      data.append("url"，encodeURIComponent(this。form。sourceSubUrl));
      data.append("config"，encodeURIComponent(this。form。remoteConfig));
      data.append("exclude"，encodeURIComponent(this。form。excludeRemarks));
      data.append("include"，encodeURIComponent(this。form。includeRemarks));
      data.append("rename"，encodeURIComponent(this。form。rename));
      data.append("tls13"，encodeURIComponent(this。form。tls13。toString()));
      data.append("xudp"，encodeURIComponent(this。form。xudp。toString()));
      data.append("emoji"，encodeURIComponent(this。form。emoji。toString()));
      data.append("list"，encodeURIComponent(this。form。nodeList。toString()));
      data.append("udp"，encodeURIComponent(this。form。udp。toString()));
      data.append("tfo"，encodeURIComponent(this。form。tfo。toString()));
      data.append("expand"，encodeURIComponent(this。form。expand。toString()));
      data.append("scv"，encodeURIComponent(this。form。scv。toString()));
      data.append("fdn"，encodeURIComponent(this。form。fdn。toString()));
      data.append("sdoh"，encodeURIComponent(this。form。tpl。surge。doh。toString()));
      data.append("cdoh"，encodeURIComponent(this。form。tpl。clash。doh。toString()));
      data.append("newname"，encodeURIComponent(this。form。new_name。toString()));
      return data;
    }，
    confirmUploadScript() {
      if (this。form。sourceSubUrl。trim() === "") {
        this。$message。error("订阅链接不能为空");
        return false;
      }
      this。loading = true;
      let data = this。renderPost();
      data.append("sortscript"，encodeURIComponent(this。uploadScript));
      data.append("filterscript"，encodeURIComponent(this。uploadFilter));
      this。$axios
          。post(configScriptBackend,data,{
            header: {
              "Content-Type": "application/form-data; charset=utf-8"
            }
          })
          。then(res => {
            if (res.data。code === 0 && res.data。data !== "") {
              this。$message。success(
                  "自定义JS上传成功，订阅链接已复制到剪贴板（IOS设备和Safari浏览器不支持自动复制API，需手动点击复制按钮）"
              );
              this。customSubUrl = res.data。data;
              this。$copyText(res.data。data);
              this。dialogUploadConfigVisible = false;
              this。btnBoolean=true;
            } else {
              this。$message。error("自定义JS上传失败: " + res.data。msg);
            }
          })
          。catch(() => {
            this。$message。error("自定义JS上传失败");
          })
          。finally(() => {
            this。loading = false;
          })
    }，
    getBackendVersion() {
      this。$axios
          。get(
              this。form。customBackend。substring(0， this。form。customBackend。length - 5) + "/version"
          )
          。then(res => {
            this。backendVersion = res.data。replace(/backend\n$/gm， "");
            this。backendVersion = this。backendVersion。replace("subconverter"， "SubConverter");
          });
    }
  }
};
</script>
