<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GitHub工具库系统</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/limonte-sweetalert2/11.6.16/sweetalert2.min.css">
    <style>
        :root {
            --primary-color: #1a73e8;
            --secondary-color: #4caf50;
            --danger-color: #e74c3c;
            --dark-color: #0d1117;
            --light-color: #f0f6fc;
            --border-color: #30363d;
            --success-color: #2ecc71;
            --warning-color: #f1c40f;
            --github-color: #4078c0;
            --gray-color: #8b949e;
            --sidebar-width: 260px;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: #0d1117;
            color: #f0f6fc;
            min-height: 100vh;
            display: flex;
            overflow-x: hidden;
        }
        
        /* 侧边栏导航 */
        .sidebar {
            width: var(--sidebar-width);
            height: 100vh;
            background: rgba(13, 17, 23, 0.95);
            border-right: 1px solid var(--border-color);
            position: fixed;
            top: 0;
            left: 0;
            padding: 20px 0;
            box-shadow: 5px 0 15px rgba(0, 0, 0, 0.4);
            z-index: 100;
            transition: transform 0.3s ease;
            display: flex;
            flex-direction: column;
        }
        
        .app-header {
            padding: 0 20px 20px;
            border-bottom: 1px solid var(--border-color);
            display: flex;
            flex-direction: column;
        }
        
        .app-title {
            font-size: 1.8rem;
            display: flex;
            align-items: center;
            gap: 12px;
            color: #58a6ff;
            padding: 15px 0;
        }
        
        .app-title i {
            font-size: 1.6rem;
        }
        
        .app-description {
            margin-top: 5px;
            font-size: 0.95rem;
            color: var(--gray-color);
            line-height: 1.6;
        }
        
        .tool-manager {
            padding: 15px 20px;
            margin-bottom: 10px;
        }
        
        .btn {
            background: linear-gradient(135deg, var(--primary-color), #3d85e7);
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 6px;
            cursor: pointer;
            font-weight: 600;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 0.95rem;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            width: 100%;
            justify-content: center;
            margin-bottom: 10px;
        }
        
        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 12px rgba(0, 0, 0, 0.3);
        }
        
        .btn-add-tool {
            background: linear-gradient(135deg, var(--success-color), #27ae60);
        }
        
        .btn-manage {
            background: linear-gradient(135deg, var(--warning-color), #e67e22);
        }
        
        .btn-github {
            background: linear-gradient(135deg, var(--github-color), #2a4e8a);
        }
        
        .tab-list {
            list-style: none;
            padding: 20px 0;
            flex-grow: 1;
            overflow-y: auto;
        }
        
        .tab-item {
            padding: 12px 25px;
            display: flex;
            align-items: center;
            gap: 15px;
            cursor: pointer;
            transition: all 0.2s;
            border-left: 3px solid transparent;
            position: relative;
        }
        
        .tab-item:hover {
            background: rgba(110, 118, 129, 0.1);
        }
        
        .tab-item.active {
            background: rgba(56, 139, 253, 0.15);
            border-left-color: #58a6ff;
            color: #58a6ff;
        }
        
        .tab-item i {
            width: 22px;
            text-align: center;
            font-size: 1.1rem;
        }
        
        .tab-name {
            font-size: 0.95rem;
            font-weight: 500;
            flex: 1;
            overflow: hidden;
            text-overflow: ellipsis;
            white-space: nowrap;
        }
        
        .tab-item.active .tab-name {
            font-weight: 600;
        }
        
        .tab-badge {
            font-size: 0.75rem;
            background: var(--dark-color);
            padding: 2px 8px;
            border-radius: 10px;
            border: 1px solid var(--border-color);
        }
        
        .footer-info {
            padding: 20px;
            border-top: 1px solid var(--border-color);
            font-size: 0.85rem;
            color: var(--gray-color);
            line-height: 1.6;
        }
        
        .footer-info a {
            color: #58a6ff;
            text-decoration: none;
        }
        
        /* 主内容区 */
        .content-area {
            flex: 1;
            margin-left: var(--sidebar-width);
            min-height: 100vh;
            padding: 20px;
        }
        
        .tool-container {
            display: none;
            min-height: calc(100vh - 40px);
        }
        
        .tool-container.active {
            display: block;
            animation: fadeIn 0.5s ease;
        }
        
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        
        /* 设置页面样式 */
        .settings-card {
            background: rgba(22, 27, 34, 0.85);
            border-radius: 12px;
            padding: 25px;
            border: 1px solid var(--border-color);
            margin-bottom: 20px;
        }
        
        .settings-header {
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 1px solid var(--border-color);
        }
        
        .settings-title {
            font-size: 1.4rem;
            color: #58a6ff;
            font-weight: 600;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .github-repo-info {
            display: flex;
            align-items: center;
            gap: 15px;
            margin-top: 10px;
            padding: 15px;
            background: rgba(13, 17, 23, 0.5);
            border-radius: 8px;
            border: 1px solid var(--border-color);
        }
        
        .github-icon {
            font-size: 2.5rem;
            color: white;
        }
        
        .github-details {
            flex-grow: 1;
        }
        
        .repo-name {
            font-size: 1.2rem;
            font-weight: 600;
            color: #58a6ff;
        }
        
        .repo-path {
            font-size: 0.9rem;
            color: var(--gray-color);
            margin-top: 5px;
        }
        
        .connection-status {
            padding: 5px 15px;
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: 500;
            display: inline-flex;
            align-items: center;
            gap: 5px;
        }
        
        .status-connected {
            background: rgba(46, 204, 113, 0.2);
            color: #2ecc71;
        }
        
        .status-disconnected {
            background: rgba(231, 76, 60, 0.2);
            color: #e74c3c;
        }
        
        .file-browser {
            margin-top: 30px;
        }
        
        .file-list {
            margin-top: 20px;
            max-height: 300px;
            overflow-y: auto;
        }
        
        .file-item {
            padding: 12px 15px;
            display: flex;
            align-items: center;
            gap: 15px;
            border-bottom: 1px solid var(--border-color);
            transition: all 0.3s;
        }
        
        .file-item:hover {
            background: rgba(88, 166, 255, 0.1);
        }
        
        .file-icon {
            font-size: 1.2rem;
            color: #58a6ff;
        }
        
        .file-name {
            flex-grow: 1;
            font-weight: 500;
        }
        
        .file-size {
            font-size: 0.85rem;
            color: var(--gray-color);
        }
        
        .file-path {
            font-size: 0.85rem;
            color: var(--gray-color);
            margin-left: 15px;
        }
        
        .file-actions {
            display: flex;
            gap: 8px;
            margin-left: 15px;
        }
        
        .file-action {
            background: transparent;
            border: 1px solid var(--border-color);
            color: var(--gray-color);
            width: 32px;
            height: 32px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
        }
        
        .file-action:hover {
            color: #58a6ff;
            border-color: #58a6ff;
        }
        
        /* 模态框样式 */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            z-index: 1000;
            align-items: center;
            justify-content: center;
        }
        
        .modal.active {
            display: flex;
        }
        
        .modal-content {
            background: #161b22;
            width: 90%;
            max-width: 600px;
            border-radius: 12px;
            border: 1px solid var(--border-color);
            padding: 30px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.5);
            max-height: 90vh;
            overflow-y: auto;
        }
        
        .modal-header {
            margin-bottom: 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .modal-title {
            font-size: 1.5rem;
            color: #58a6ff;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .close-btn {
            background: transparent;
            border: none;
            color: var(--gray-color);
            font-size: 1.5rem;
            cursor: pointer;
        }
        
        .form-group {
            margin-bottom: 20px;
        }
        
        .form-group label {
            display: block;
            margin-bottom: 8px;
            font-weight: 500;
        }
        
        .form-control {
            width: 100%;
            background: rgba(22, 27, 34, 0.8);
            border: 1px solid var(--border-color);
            color: #f0f6fc;
            padding: 12px;
            border-radius: 6px;
            font-size: 1rem;
        }
        
        .form-control:focus {
            outline: none;
            border-color: #58a6ff;
            box-shadow: 0 0 0 3px rgba(88, 166, 255, 0.2);
        }
        
        textarea.form-control {
            min-height: 150px;
            resize: vertical;
            font-family: monospace;
        }
        
        .file-upload-area {
            border: 2px dashed #58a6ff;
            border-radius: 8px;
            padding: 30px;
            text-align: center;
            background: rgba(88, 166, 255, 0.1);
            margin-bottom: 15px;
            cursor: pointer;
            transition: all 0.3s ease;
        }
        
        .file-upload-area:hover {
            background: rgba(88, 166, 255, 0.2);
            border-color: #2ecc71;
        }
        
        .file-upload-area i {
            font-size: 3rem;
            color: #58a6ff;
            margin-bottom: 15px;
        }
        
        .modal-footer {
            margin-top: 30px;
            display: flex;
            gap: 10px;
            justify-content: flex-end;
        }
        
        .btn-primary {
            background: var(--primary-color);
            color: white;
            border: none;
            padding: 10px 20px;
            border-radius: 6px;
            cursor: pointer;
            font-weight: 600;
        }
        
        .btn-secondary {
            background: transparent;
            color: var(--gray-color);
            border: 1px solid var(--border-color);
            padding: 10px 20px;
            border-radius: 6px;
            cursor: pointer;
            font-weight: 600;
        }
        
        .btn-primary:hover {
            background: #3d94f3;
        }
        
        .btn-secondary:hover {
            background: rgba(110, 118, 129, 0.1);
        }
        
        /* 移动适配 */
        .mobile-menu-btn {
            display: none;
            position: fixed;
            top: 20px;
            right: 20px;
            background: rgba(56, 139, 253, 0.2);
            border: 1px solid #30363d;
            color: #58a6ff;
            font-size: 1.5rem;
            width: 50px;
            height: 50px;
            border-radius: 8px;
            z-index: 200;
            cursor: pointer;
            justify-content: center;
            align-items: center;
        }
        
        @media (max-width: 900px) {
            .sidebar {
                transform: translateX(-100%);
            }
            
            .sidebar.active {
                transform: translateX(0);
            }
            
            .content-area {
                margin-left: 0;
            }
            
            .mobile-menu-btn {
                display: flex;
            }
        }
    </style>
</head>
<body>
    <!-- 移动端菜单按钮 -->
    <div class="mobile-menu-btn" id="menuBtn">
        <i class="fas fa-bars"></i>
    </div>
    
    <!-- 左侧导航 -->
    <div class="sidebar" id="sidebar">
        <div class="app-header">
            <div class="app-title">
                <i class="fab fa-github"></i>
                GitHub工具库
            </div>
            <p class="app-description">工具脚本存储在您的GitHub仓库</p>
        </div>
        
        <div class="tool-manager">
            <button class="btn btn-add-tool" id="addToolBtn">
                <i class="fas fa-plus-circle"></i>
                添加新工具
            </button>
            <button class="btn btn-manage" id="manageToolsBtn">
                <i class="fas fa-cog"></i>
                工具管理
            </button>
            <button class="btn btn-github" id="repoSettingsBtn">
                <i class="fab fa-github"></i>
                GitHub仓库设置
            </button>
        </div>
        
        <ul class="tab-list" id="tabList">
            <li class="tab-item active" data-tool="repo-manager">
                <i class="fas fa-cogs"></i>
                <span class="tab-name">仓库管理器</span>
            </li>
            <li class="tab-item" data-tool="text-analyzer">
                <i class="fas fa-chart-bar"></i>
                <span class="tab-name">文本分析工具</span>
            </li>
            <li class="tab-item" data-tool="excel-extractor">
                <i class="fas fa-file-excel"></i>
                <span class="tab-name">Excel提取工具</span>
            </li>
        </ul>
        
        <div class="footer-info">
            <p>© 2023 GitHub工具库系统</p>
            <p>工具脚本存储在您的GitHub仓库</p>
            <p>使用Personal Access Token进行认证</p>
        </div>
    </div>
    
    <!-- 主内容区 -->
    <div class="content-area">
        <!-- 仓库管理器 -->
        <div class="tool-container active" data-tool="repo-manager">
            <div class="settings-card">
                <div class="settings-header">
                    <h2 class="settings-title">
                        <i class="fab fa-github"></i>
                        GitHub仓库配置
                    </h2>
                </div>
                
                <div class="github-repo-info">
                    <i class="fab fa-github github-icon"></i>
                    <div class="github-details">
                        <div class="repo-name" id="repoName">未选择仓库</div>
                        <div class="repo-path" id="repoPath">选择或创建新的GitHub仓库来存储工具</div>
                    </div>
                    <div class="connection-status status-disconnected" id="repoStatus">
                        <i class="fas fa-times-circle"></i>
                        未连接
                    </div>
                </div>
                
                <div class="form-group">
                    <label for="accessToken">GitHub个人访问令牌 (PAT)</label>
                    <input type="password" id="accessToken" class="form-control" placeholder="输入GitHub Personal Access Token">
                    <small style="color: #8b949e; display: block; margin-top: 5px;">需要repo权限来访问您的仓库</small>
                </div>
                
                <div class="form-group">
                    <label for="username">GitHub用户名</label>
                    <input type="text" id="username" class="form-control" placeholder="输入GitHub用户名">
                </div>
                
                <div class="form-group">
                    <label for="repoNameInput">仓库名称</label>
                    <div style="display: flex; gap: 10px;">
                        <input type="text" id="repoNameInput" class="form-control" placeholder="输入仓库名称">
                        <button class="btn" id="connectRepoBtn">
                            <i class="fas fa-link"></i> 连接
                        </button>
                    </div>
                    <small style="color: #8b949e; display: block; margin-top: 5px;">仓库不存在时将自动创建</small>
                </div>
            </div>
            
            <div class="settings-card">
                <div class="settings-header">
                    <h2 class="settings-title">
                        <i class="fas fa-folder"></i>
                        工具脚本管理器
                    </h2>
                </div>
                
                <div class="file-browser">
                    <div style="display: flex; gap: 10px;">
                        <button class="btn btn-primary" id="refreshBtn">
                            <i class="fas fa-sync-alt"></i> 刷新文件列表
                        </button>
                        <button class="btn btn-success" id="createToolBtn">
                            <i class="fas fa-plus-circle"></i> 创建新工具
                        </button>
                    </div>
                    
                    <div class="file-list" id="fileList">
                        <!-- 文件列表将由JavaScript动态生成 -->
                    </div>
                </div>
            </div>
        </div>
        
        <!-- 文本分析工具 -->
        <div class="tool-container" data-tool="text-analyzer">
            <div class="settings-card">
                <div class="settings-header">
                    <h2 class="settings-title">
                        <i class="fas fa-chart-bar"></i>
                        文本分析工具
                    </h2>
                </div>
                <p>此工具脚本存储在您的GitHub仓库: <span id="toolLocationText">/tools/text-analyzer.html</span></p>
                
                <div class="form-group">
                    <label for="textInput">输入要分析的文本</label>
                    <textarea id="textInput" class="form-control" rows="6">文本分析工具可以帮助您快速分析文本内容，提取关键信息。此工具已存储在您的GitHub仓库中，可以随时进行更新和调用。</textarea>
                </div>
                
                <button class="btn btn-primary" id="analyzeTextBtn">
                    <i class="fas fa-chart-pie"></i> 分析文本
                </button>
                
                <div class="results" style="margin-top: 30px;">
                    <div class="stats-row" style="display: flex; gap: 15px; margin-top: 20px;">
                        <div class="stat-card" style="background: rgba(22, 27, 34, 0.8); border-radius: 10px; padding: 20px; text-align: center; flex: 1;">
                            <h3 style="color: #58a6ff; margin-bottom: 10px;">字符数</h3>
                            <div style="font-size: 2rem; font-weight: bold; color: #2ecc71;">128</div>
                        </div>
                        <div class="stat-card" style="background: rgba(22, 27, 34, 0.8); border-radius: 10px; padding: 20px; text-align: center; flex: 1;">
                            <h3 style="color: #58a6ff; margin-bottom: 10px;">词语数</h3>
                            <div style="font-size: 2rem; font-weight: bold; color: #2ecc71;">24</div>
                        </div>
                    </div>
                    
                    <div style="margin-top: 30px; background: rgba(22, 27, 34, 0.8); border-radius: 10px; padding: 20px;">
                        <h3 style="color: #58a6ff; margin-bottom: 15px;">关键词提取</h3>
                        <div style="display: flex; flex-wrap: wrap; gap: 10px;">
                            <span style="background: rgba(88, 166, 255, 0.2); padding: 5px 15px; border-radius: 20px;">文本分析</span>
                            <span style="background: rgba(88, 166, 255, 0.2); padding: 5px 15px; border-radius: 20px;">关键信息</span>
                            <span style="background: rgba(88, 166, 255, 0.2); padding: 5px 15px; border-radius: 20px;">GitHub仓库</span>
                            <span style="background: rgba(88, 166, 255, 0.2); padding: 5px 15px; border-radius: 20px;">更新</span>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- Excel提取工具 -->
        <div class="tool-container" data-tool="excel-extractor">
            <div class="settings-card">
                <div class="settings-header">
                    <h2 class="settings-title">
                        <i class="fas fa-file-excel"></i>
                        Excel字段提取工具
                    </h2>
                </div>
                <p>此工具脚本存储在您的GitHub仓库: <span id="excelToolLocation">/tools/excel-extractor.html</span></p>
                
                <div class="form-group">
                    <div class="file-upload-area">
                        <i class="fas fa-cloud-upload-alt"></i>
                        <h3>点击或拖拽Excel文件到此处</h3>
                        <p>支持 .xlsx 和 .xls 格式</p>
                        <input type="file" id="excelFileInput" accept=".xlsx, .xls" style="display: none;">
                    </div>
                </div>
                
                <div class="form-group">
                    <label for="excelOutput">提取结果</label>
                    <textarea id="excelOutput" class="form-control" rows="8" readonly>{"title": "示例报告.xlsx", "sheets": 3, "fields": ["ID", "Name", "Date", "Value"], "extracted": 4}</textarea>
                </div>
            </div>
        </div>
    </div>
    
    <!-- 添加工具模态框 -->
    <div class="modal" id="toolModal">
        <div class="modal-content">
            <div class="modal-header">
                <h2 class="modal-title">
                    <i class="fas fa-file-upload"></i>
                    上传新工具到GitHub仓库
                </h2>
                <button class="close-btn" id="closeToolModal">&times;</button>
            </div>
            
            <div class="form-group">
                <label for="toolName">工具名称</label>
                <input type="text" id="toolName" class="form-control" placeholder="输入工具名称">
            </div>
            
            <div class="form-group">
                <label for="toolDescription">工具描述</label>
                <textarea id="toolDescription" class="form-control" placeholder="输入工具描述"></textarea>
            </div>
            
            <div class="form-group">
                <label for="toolFolder">存储路径</label>
                <div style="display: flex; gap: 10px; align-items: center;">
                    <span style="color: var(--gray-color);">/tools/</span>
                    <input type="text" id="toolFolder" class="form-control" value="my-tool.html">
                </div>
            </div>
            
            <div class="form-group">
                <label>上传方式</label>
                <div class="file-upload-area" id="htmlUploadArea">
                    <i class="fas fa-file-upload"></i>
                    <h3>上传HTML文件</h3>
                    <p>选择工具HTML文件上传</p>
                    <input type="file" id="htmlFileInput" accept=".html" style="display: none;">
                </div>
                
                <div style="text-align: center; margin: 15px 0; color: var(--gray-color);">
                    或
                </div>
                
                <label for="htmlCode">手动输入HTML代码</label>
                <textarea id="htmlCode" class="form-control" placeholder="在此粘贴工具HTML代码..."></textarea>
            </div>
            
            <div class="modal-footer">
                <button class="btn-secondary" id="cancelBtn">取消</button>
                <button class="btn-primary" id="saveToolBtn">提交到GitHub</button>
            </div>
        </div>
    </div>
    
    <script src="https://cdnjs.cloudflare.com/ajax/libs/limonte-sweetalert2/11.6.16/sweetalert2.min.js"></script>
    <script>
        // 模拟GitHub存储
        const githubStorage = {
            connected: false,
            repoName: '',
            username: '',
            tools: [
                { name: "文本分析工具", path: "tools/text-analyzer.html", content: "..." },
                { name: "Excel提取工具", path: "tools/excel-extractor.html", content: "..." }
            ],
            
            // 连接GitHub仓库
            connect: function(username, repoName, token) {
                return new Promise((resolve) => {
                    setTimeout(() => {
                        this.connected = true;
                        this.repoName = repoName;
                        this.username = username;
                        resolve();
                    }, 1000);
                });
            },
            
            // 创建文件
            createFile: function(path, content) {
                return new Promise((resolve) => {
                    setTimeout(() => {
                        this.tools.push({
                            name: path.split('/').pop().replace('.html', ''),
                            path: path,
                            content: content
                        });
                        resolve();
                    }, 800);
                });
            },
            
            // 获取文件列表
            getFiles: function() {
                return new Promise((resolve) => {
                    setTimeout(() => {
                        resolve([...this.tools]);
                    }, 500);
                });
            }
        };

        // 页面控制器
        const AppController = {
            init: function() {
                this.initEventListeners();
                this.initUI();
            },
            
            initEventListeners: function() {
                // 移动端菜单按钮
                document.getElementById('menuBtn').addEventListener('click', () => {
                    document.getElementById('sidebar').classList.toggle('active');
                });
                
                // 标签切换
                document.querySelectorAll('.tab-item').forEach(item => {
                    item.addEventListener('click', (e) => {
                        const toolId = e.currentTarget.dataset.tool;
                        this.activateTool(toolId);
                    });
                });
                
                // 添加工具按钮
                document.getElementById('addToolBtn').addEventListener('click', () => {
                    this.showModal('toolModal');
                });
                
                // 仓库连接按钮
                document.getElementById('connectRepoBtn').addEventListener('click', () => {
                    this.connectGithubRepo();
                });
                
                // 上传文件处理
                document.getElementById('htmlUploadArea').addEventListener('click', () => {
                    document.getElementById('htmlFileInput').click();
                });
                
                document.getElementById('htmlFileInput').addEventListener('change', (e) => {
                    this.handleFileUpload(e.target.files[0]);
                });
                
                // 保存工具
                document.getElementById('saveToolBtn').addEventListener('click', () => {
                    this.saveTool();
                });
                
                // 取消按钮
                document.getElementById('cancelBtn').addEventListener('click', () => {
                    this.closeModal('toolModal');
                });
                
                // 关闭模态框
                document.getElementById('closeToolModal').addEventListener('click', () => {
                    this.closeModal('toolModal');
                });
                
                // 刷新文件列表
                document.getElementById('refreshBtn').addEventListener('click', () => {
                    this.loadFileList();
                });
                
                // 创建新工具按钮
                document.getElementById('createToolBtn').addEventListener('click', () => {
                    this.showModal('toolModal');
                });
                
                // 分析文本按钮
                document.getElementById('analyzeTextBtn').addEventListener('click', () => {
                    this.analyzeText();
                });
            },
            
            initUI: function() {
                // 初始化UI状态
                if(githubStorage.connected) {
                    document.getElementById('repoName').textContent = githubStorage.repoName;
                    document.getElementById('repoPath').textContent = `${githubStorage.username}/${githubStorage.repoName}`;
                    document.getElementById('repoStatus').className = 'connection-status status-connected';
                    document.getElementById('repoStatus').innerHTML = '<i class="fas fa-check-circle"></i> 已连接';
                }
                
                // 加载文件列表
                this.loadFileList();
            },
            
            showModal: function(modalId) {
                document.getElementById(modalId).classList.add('active');
            },
            
            closeModal: function(modalId) {
                document.getElementById(modalId).classList.remove('active');
            },
            
            handleFileUpload: function(file) {
                if (!file) return;
                
                const reader = new FileReader();
                reader.onload = (e) => {
                    document.getElementById('htmlCode').value = e.target.result;
                };
                reader.readAsText(file);
            },
            
            connectGithubRepo: function() {
                const token = document.getElementById('accessToken').value;
                const username = document.getElementById('username').value;
                const repoName = document.getElementById('repoNameInput').value;
                
                if (!token || !username || !repoName) {
                    Swal.fire({
                        icon: 'error',
                        title: '缺少信息',
                        text: '请填写所有必填字段'
                    });
                    return;
                }
                
                const connectBtn = document.getElementById('connectRepoBtn');
                const originalText = connectBtn.innerHTML;
                connectBtn.innerHTML = '<i class="fas fa-spinner fa-spin"></i> 连接中...';
                connectBtn.disabled = true;
                
                // 模拟GitHub连接
                githubStorage.connect(username, repoName, token)
                    .then(() => {
                        document.getElementById('repoName').textContent = repoName;
                        document.getElementById('repoPath').textContent = `${username}/${repoName}`;
                        document.getElementById('repoStatus').className = 'connection-status status-connected';
                        document.getElementById('repoStatus').innerHTML = '<i class="fas fa-check-circle"></i> 已连接';
                        
                        this.loadFileList();
                        
                        Swal.fire({
                            icon: 'success',
                            title: '连接成功',
                            text: `已成功连接到仓库 ${username}/${repoName}`
                        });
                    })
                    .catch(error => {
                        Swal.fire({
                            icon: 'error',
                            title: '连接失败',
                            text: error.message || '无法连接到GitHub，请检查您的信息'
                        });
                    })
                    .finally(() => {
                        connectBtn.innerHTML = originalText;
                        connectBtn.disabled = false;
                    });
            },
            
            loadFileList: function() {
                const fileList = document.getElementById('fileList');
                fileList.innerHTML = '<div class="file-item"><i class="fas fa-spinner fa-spin"></i> 加载中...</div>';
                
                // 模拟获取文件列表
                githubStorage.getFiles()
                    .then(files => {
                        if (files.length === 0) {
                            fileList.innerHTML = '<div class="file-item" style="justify-content: center; color: var(--gray-color);">没有文件</div>';
                            return;
                        }
                        
                        fileList.innerHTML = '';
                        
                        files.forEach(file => {
                            const fileItem = document.createElement('div');
                            fileItem.className = 'file-item';
                            fileItem.innerHTML = `
                                <i class="fas fa-file-code file-icon"></i>
                                <div class="file-name">${file.name}</div>
                                <div class="file-path">${file.path}</div>
                                <div class="file-actions">
                                    <button class="file-action" title="运行工具">
                                        <i class="fas fa-play"></i>
                                    </button>
                                    <button class="file-action" title="编辑工具">
                                        <i class="fas fa-edit"></i>
                                    </button>
                                    <button class="file-action" title="删除工具">
                                        <i class="fas fa-trash"></i>
                                    </button>
                                </div>
                            `;
                            fileList.appendChild(fileItem);
                        });
                    })
                    .catch(error => {
                        fileList.innerHTML = `<div class="file-item" style="justify-content: center; color: var(--danger-color);">${error.message || '加载失败'}</div>`;
                    });
            },
            
            saveTool: function() {
                const toolName = document.getElementById('toolName').value;
                const toolDescription = document.getElementById('toolDescription').value;
                const toolPath = document.getElementById('toolFolder').value;
                const htmlCode = document.getElementById('htmlCode').value;
                
                if (!toolName || !htmlCode || !toolPath) {
                    Swal.fire({
                        icon: 'error',
                        title: '缺少信息',
                        text: '请填写工具名称、路径和内容'
                    });
                    return;
                }
                
                const saveBtn = document.getElementById('saveToolBtn');
                const originalText = saveBtn.innerHTML;
                saveBtn.innerHTML = '<i class="fas fa-spinner fa-spin"></i> 上传中...';
                saveBtn.disabled = true;
                
                const fullPath = `tools/${toolPath}`;
                
                // 模拟上传到GitHub
                githubStorage.createFile(fullPath, htmlCode)
                    .then(() => {
                        this.closeModal('toolModal');
                        this.loadFileList();
                        
                        Swal.fire({
                            icon: 'success',
                            title: '工具已保存',
                            text: `${toolName} 已成功上传到GitHub仓库`
                        });
                    })
                    .catch(error => {
                        Swal.fire({
                            icon: 'error',
                            title: '保存失败',
                            text: error.message || '上传工具时发生错误'
                        });
                    })
                    .finally(() => {
                        saveBtn.innerHTML = originalText;
                        saveBtn.disabled = false;
                    });
            },
            
            activateTool: function(toolId) {
                // 更新tab激活状态
                document.querySelectorAll('.tab-item').forEach(tab => {
                    tab.classList.remove('active');
                    if (tab.dataset.tool === toolId) {
                        tab.classList.add('active');
                    }
                });
                
                // 更新内容区域
                document.querySelectorAll('.tool-container').forEach(container => {
                    container.classList.remove('active');
                    if (container.dataset.tool === toolId) {
                        container.classList.add('active');
                    }
                });
            },
            
            analyzeText: function() {
                const text = document.getElementById('textInput').value;
                
                // 显示处理状态
                const button = document.getElementById('analyzeTextBtn');
                const originalText = button.innerHTML;
                button.innerHTML = '<i class="fas fa-spinner fa-spin"></i> 分析中...';
                button.disabled = true;
                
                // 模拟文本分析
                setTimeout(() => {
                    // 这里应该是文本分析逻辑
                    button.innerHTML = originalText;
                    button.disabled = false;
                    
                    Swal.fire({
                        icon: 'success',
                        title: '分析完成',
                        text: '文本分析已完成，结果如下所示'
                    });
                }, 1500);
            }
        };
        
        // 初始化应用
        document.addEventListener('DOMContentLoaded', function() {
            AppController.init();
        });
    </script>
</body>
</html>