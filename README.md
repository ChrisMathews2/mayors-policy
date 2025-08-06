# mayors-policy
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mayor's Policy Dashboard</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
            background: linear-gradient(135deg, #0f172a 0%, #1e293b 100%);
            color: #e0e0e0;
            min-height: 100vh;
            overflow-x: hidden;
        }

        /* Header */
        header {
            background: rgba(15, 23, 42, 0.95);
            backdrop-filter: blur(20px);
            border-bottom: 1px solid rgba(59, 130, 246, 0.3);
            padding: 1.5rem 0;
            position: sticky;
            top: 0;
            z-index: 100;
            box-shadow: 0 4px 30px rgba(0, 0, 0, 0.3);
        }

        .header-content {
            max-width: 1400px;
            margin: 0 auto;
            padding: 0 2rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        h1 {
            font-size: 2.5rem;
            font-weight: 700;
            background: linear-gradient(135deg, #3b82f6 0%, #60a5fa 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 0 0 30px rgba(59, 130, 246, 0.5);
        }

        /* Search and Filter */
        .controls {
            max-width: 1400px;
            margin: 2rem auto;
            padding: 0 2rem;
            display: flex;
            gap: 1rem;
            flex-wrap: wrap;
        }

        .search-box {
            flex: 1;
            min-width: 300px;
            position: relative;
        }

        .search-box input {
            width: 100%;
            padding: 1rem 3rem 1rem 1.5rem;
            background: rgba(30, 41, 59, 0.8);
            border: 2px solid rgba(59, 130, 246, 0.3);
            border-radius: 12px;
            color: #e0e0e0;
            font-size: 1rem;
            transition: all 0.3s ease;
        }

        .search-box input:focus {
            outline: none;
            border-color: #3b82f6;
            box-shadow: 0 0 20px rgba(59, 130, 246, 0.4);
            transform: translateY(-2px);
        }

        .search-icon {
            position: absolute;
            right: 1rem;
            top: 50%;
            transform: translateY(-50%);
            color: #64748b;
        }

        .filter-buttons {
            display: flex;
            gap: 0.5rem;
            flex-wrap: wrap;
        }

        .filter-btn {
            padding: 0.75rem 1.5rem;
            background: rgba(30, 41, 59, 0.8);
            border: 2px solid rgba(59, 130, 246, 0.3);
            border-radius: 8px;
            color: #94a3b8;
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 0.9rem;
            font-weight: 500;
        }

        .filter-btn:hover {
            background: rgba(59, 130, 246, 0.2);
            border-color: #3b82f6;
            transform: translateY(-2px);
            box-shadow: 0 4px 15px rgba(59, 130, 246, 0.3);
        }

        .filter-btn.active {
            background: #3b82f6;
            border-color: #3b82f6;
            color: white;
            box-shadow: 0 0 20px rgba(59, 130, 246, 0.5);
        }

        /* Add Policy Button */
        .add-policy-btn {
            padding: 0.75rem 2rem;
            background: linear-gradient(135deg, #3b82f6 0%, #2563eb 100%);
            border: none;
            border-radius: 8px;
            color: white;
            cursor: pointer;
            font-size: 1rem;
            font-weight: 600;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(59, 130, 246, 0.3);
        }

        .add-policy-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 25px rgba(59, 130, 246, 0.5);
        }

        /* Policy Grid */
        .policy-grid {
            max-width: 1400px;
            margin: 2rem auto;
            padding: 0 2rem;
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
            gap: 2rem;
        }

        .policy-card {
            background: rgba(30, 41, 59, 0.8);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(59, 130, 246, 0.2);
            border-radius: 16px;
            padding: 2rem;
            transition: all 0.3s ease;
            cursor: pointer;
            position: relative;
            overflow: hidden;
        }

        .policy-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 4px;
            background: linear-gradient(90deg, #3b82f6, #60a5fa);
            transform: scaleX(0);
            transition: transform 0.3s ease;
        }

        .policy-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 40px rgba(59, 130, 246, 0.3);
            border-color: #3b82f6;
        }

        .policy-card:hover::before {
            transform: scaleX(1);
        }

        .policy-card:hover .policy-delete-btn {
            opacity: 1;
        }

        .policy-delete-btn {
            position: absolute;
            top: 1rem;
            right: 1rem;
            background: rgba(239, 68, 68, 0.2);
            border: 1px solid rgba(239, 68, 68, 0.3);
            color: #f87171;
            padding: 0.5rem;
            border-radius: 8px;
            cursor: pointer;
            opacity: 0;
            transition: all 0.3s ease;
            font-size: 1.2rem;
            line-height: 1;
            width: 2rem;
            height: 2rem;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .policy-delete-btn:hover {
            background: rgba(239, 68, 68, 0.3);
            border-color: #ef4444;
            transform: scale(1.1);
        }

        .policy-category {
            display: inline-block;
            padding: 0.25rem 0.75rem;
            background: rgba(59, 130, 246, 0.2);
            color: #60a5fa;
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: 600;
            margin-bottom: 1rem;
        }

        .policy-title {
            font-size: 1.5rem;
            font-weight: 600;
            margin-bottom: 1rem;
            color: #f1f5f9;
        }

        .policy-description {
            color: #94a3b8;
            line-height: 1.6;
            margin-bottom: 1.5rem;
        }

        .policy-meta {
            display: flex;
            justify-content: space-between;
            align-items: center;
            color: #64748b;
            font-size: 0.9rem;
        }

        .policy-status {
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .status-dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            background: #10b981;
            animation: pulse 2s ease-in-out infinite;
        }

        @keyframes pulse {
            0%, 100% { opacity: 1; transform: scale(1); }
            50% { opacity: 0.7; transform: scale(1.2); }
        }

        /* Modal */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, 0.8);
            backdrop-filter: blur(5px);
            z-index: 1000;
            align-items: center;
            justify-content: center;
            padding: 2rem;
            overflow-y: auto;
        }

        .modal-content {
            background: #1e293b;
            border-radius: 20px;
            padding: 3rem;
            max-width: 600px;
            width: 100%;
            max-height: 90vh;
            overflow-y: auto;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5);
            border: 1px solid rgba(59, 130, 246, 0.3);
            margin: auto;
        }

        .modal h2 {
            font-size: 2rem;
            margin-bottom: 2rem;
            color: #f1f5f9;
        }

        .form-group {
            margin-bottom: 1.5rem;
        }

        .form-group label {
            display: block;
            margin-bottom: 0.5rem;
            color: #94a3b8;
            font-weight: 500;
        }

        .form-group input,
        .form-group select,
        .form-group textarea {
            width: 100%;
            padding: 0.75rem 1rem;
            background: rgba(15, 23, 42, 0.8);
            border: 2px solid rgba(59, 130, 246, 0.3);
            border-radius: 8px;
            color: #e0e0e0;
            font-size: 1rem;
            transition: all 0.3s ease;
        }

        .form-group input:focus,
        .form-group select:focus,
        .form-group textarea:focus {
            outline: none;
            border-color: #3b82f6;
            box-shadow: 0 0 10px rgba(59, 130, 246, 0.3);
        }

        .form-group textarea {
            min-height: 120px;
            resize: vertical;
        }

        .form-buttons {
            display: flex;
            gap: 1rem;
            justify-content: flex-end;
            margin-top: 2rem;
        }

        .btn {
            padding: 0.75rem 2rem;
            border: none;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .btn-primary {
            background: linear-gradient(135deg, #3b82f6 0%, #2563eb 100%);
            color: white;
        }

        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 20px rgba(59, 130, 246, 0.4);
        }

        .btn-secondary {
            background: rgba(100, 116, 139, 0.2);
            color: #94a3b8;
            border: 2px solid rgba(100, 116, 139, 0.3);
        }

        .btn-secondary:hover {
            background: rgba(100, 116, 139, 0.3);
            border-color: #64748b;
        }

        .btn-danger {
            background: rgba(239, 68, 68, 0.2);
            color: #f87171;
            border: 2px solid rgba(239, 68, 68, 0.3);
        }

        .btn-danger:hover {
            background: rgba(239, 68, 68, 0.3);
            border-color: #ef4444;
            transform: translateY(-2px);
            box-shadow: 0 4px 20px rgba(239, 68, 68, 0.4);
        }

        /* Empty State */
        .empty-state {
            text-align: center;
            padding: 4rem 2rem;
            color: #64748b;
        }

        .empty-state svg {
            width: 100px;
            height: 100px;
            margin-bottom: 2rem;
            opacity: 0.5;
        }

        /* Document Vault Styles */
        .document-item {
            display: flex;
            align-items: center;
            gap: 1rem;
            padding: 1rem;
            background: rgba(30, 41, 59, 0.5);
            border: 1px solid rgba(59, 130, 246, 0.2);
            border-radius: 8px;
            transition: all 0.3s ease;
        }

        .document-item:hover {
            background: rgba(30, 41, 59, 0.8);
            border-color: #3b82f6;
            transform: translateX(5px);
        }

        .document-icon {
            font-size: 2rem;
            width: 3rem;
            text-align: center;
        }

        .document-info {
            flex: 1;
        }

        .document-name {
            font-weight: 600;
            color: #f1f5f9;
            margin-bottom: 0.25rem;
        }

        .document-meta {
            font-size: 0.875rem;
            color: #64748b;
        }

        .document-category-badge {
            display: inline-block;
            padding: 0.25rem 0.75rem;
            border-radius: 12px;
            font-size: 0.75rem;
            font-weight: 600;
            margin-right: 0.5rem;
        }

        .document-actions {
            display: flex;
            gap: 0.5rem;
        }

        .doc-btn {
            padding: 0.5rem 1rem;
            background: rgba(59, 130, 246, 0.2);
            border: 1px solid rgba(59, 130, 246, 0.3);
            border-radius: 6px;
            color: #60a5fa;
            cursor: pointer;
            font-size: 0.875rem;
            transition: all 0.3s ease;
        }

        .doc-btn:hover {
            background: rgba(59, 130, 246, 0.3);
            border-color: #3b82f6;
        }

        .doc-btn.danger {
            background: rgba(239, 68, 68, 0.2);
            border-color: rgba(239, 68, 68, 0.3);
            color: #f87171;
        }

        .doc-btn.danger:hover {
            background: rgba(239, 68, 68, 0.3);
            border-color: #ef4444;
        }

        .vault-filters {
            display: flex;
            gap: 0.5rem;
            flex-wrap: wrap;
        }

        .doc-category-tag {
            display: inline-flex;
            align-items: center;
            gap: 0.5rem;
            padding: 0.5rem 1rem;
            background: rgba(59, 130, 246, 0.2);
            border: 1px solid rgba(59, 130, 246, 0.3);
            border-radius: 20px;
            font-size: 0.875rem;
            margin-bottom: 0.5rem;
        }

        .doc-category-tag button {
            background: none;
            border: none;
            color: #f87171;
            cursor: pointer;
            padding: 0 0.25rem;
            font-size: 1rem;
        }

        /* Upload Progress */
        .upload-progress {
            display: none;
            margin-top: 1rem;
            padding: 1rem;
            background: rgba(59, 130, 246, 0.1);
            border: 1px solid rgba(59, 130, 246, 0.3);
            border-radius: 8px;
        }

        .progress-bar {
            width: 100%;
            height: 8px;
            background: rgba(59, 130, 246, 0.2);
            border-radius: 4px;
            overflow: hidden;
            margin-top: 0.5rem;
        }

        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #3b82f6, #60a5fa);
            transition: width 0.3s ease;
            width: 0%;
        }

        /* Drag and Drop */
        .file-upload-area {
            border: 2px dashed rgba(59, 130, 246, 0.5);
            border-radius: 12px;
            padding: 2rem;
            text-align: center;
            transition: all 0.3s ease;
            margin-bottom: 1rem;
        }

        .file-upload-area.drag-over {
            background: rgba(59, 130, 246, 0.1);
            border-color: #3b82f6;
        }

        .file-upload-area p {
            color: #94a3b8;
            margin-bottom: 1rem;
        }

        .file-input-label {
            display: inline-block;
            padding: 0.75rem 1.5rem;
            background: rgba(59, 130, 246, 0.2);
            border: 1px solid rgba(59, 130, 246, 0.3);
            border-radius: 8px;
            color: #60a5fa;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .file-input-label:hover {
            background: rgba(59, 130, 246, 0.3);
            border-color: #3b82f6;
        }

        /* Links Styles */
        .link-item {
            display: flex;
            align-items: center;
            gap: 0.5rem;
            padding: 0.5rem;
            background: rgba(30, 41, 59, 0.5);
            border: 1px solid rgba(59, 130, 246, 0.2);
            border-radius: 6px;
        }

        .link-item:hover {
            background: rgba(30, 41, 59, 0.8);
            border-color: rgba(59, 130, 246, 0.4);
        }

        .link-icon {
            font-size: 1.2rem;
            width: 1.5rem;
            text-align: center;
        }

        .link-info {
            flex: 1;
        }

        .link-title {
            font-weight: 500;
            color: #60a5fa;
            cursor: pointer;
            text-decoration: none;
        }

        .link-title:hover {
            text-decoration: underline;
        }

        .link-url {
            font-size: 0.75rem;
            color: #64748b;
        }

        .link-remove {
            background: none;
            border: none;
            color: #f87171;
            cursor: pointer;
            padding: 0.25rem;
            font-size: 1rem;
            opacity: 0.7;
            transition: opacity 0.2s;
        }

        .link-remove:hover {
            opacity: 1;
        }

        .policy-links-section {
            margin-top: 1rem;
            padding-top: 1rem;
            border-top: 1px solid rgba(59, 130, 246, 0.2);
        }

        .policy-links-title {
            font-size: 0.875rem;
            font-weight: 600;
            color: #94a3b8;
            margin-bottom: 0.5rem;
        }

        .policy-link-badge {
            display: inline-flex;
            align-items: center;
            gap: 0.25rem;
            padding: 0.25rem 0.5rem;
            background: rgba(59, 130, 246, 0.1);
            border: 1px solid rgba(59, 130, 246, 0.2);
            border-radius: 4px;
            font-size: 0.75rem;
            color: #60a5fa;
            text-decoration: none;
            transition: all 0.2s;
            margin-right: 0.5rem;
            margin-bottom: 0.5rem;
        }

        .policy-link-badge:hover {
            background: rgba(59, 130, 246, 0.2);
            border-color: rgba(59, 130, 246, 0.4);
            transform: translateY(-1px);
        }

        /* Responsive */
        @media (max-width: 768px) {
            h1 {
                font-size: 2rem;
            }
            
            .controls {
                flex-direction: column;
            }
            
            .search-box {
                min-width: 100%;
            }
            
            .policy-grid {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <header>
        <div class="header-content">
            <h1>Mayor's Policy Dashboard</h1>
            <div style="display: flex; gap: 1rem;">
                <button class="add-policy-btn" onclick="openVaultModal()">📁 Document Vault</button>
                <button class="add-policy-btn" onclick="openCategoryModal()">⚙️ Manage Categories</button>
                <button class="add-policy-btn" onclick="openModal()">+ Add New Policy</button>
            </div>
        </div>
    </header>

    <div class="controls">
        <div class="search-box">
            <input type="text" id="searchInput" placeholder="Search policies..." onkeyup="filterPolicies()">
            <span class="search-icon">🔍</span>
        </div>
        <div class="filter-buttons">
            <button class="filter-btn active" onclick="setFilter('all')">All</button>
            <div id="categoryFilters">
                <!-- Category filters will be dynamically added here -->
            </div>
        </div>
    </div>

    <div class="policy-grid" id="policyGrid">
        <!-- Policies will be dynamically added here -->
    </div>

    <div class="empty-state" id="emptyState" style="display: none;">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2"/>
        </svg>
        <h3>No policies found</h3>
        <p>Add your first policy to get started!</p>
    </div>

    <!-- Modal for adding/editing policies -->
    <div class="modal" id="policyModal">
        <div class="modal-content">
            <h2 id="modalTitle">Add New Policy</h2>
            <form id="policyForm">
                <div class="form-group">
                    <label for="policyTitle">Policy Title</label>
                    <input type="text" id="policyTitle" required>
                </div>
                <div class="form-group">
                    <label for="policyCategory">Category</label>
                    <select id="policyCategory" required>
                        <!-- Categories will be dynamically populated -->
                    </select>
                </div>
                <div class="form-group">
                    <label for="policyDescription">Description</label>
                    <textarea id="policyDescription" required></textarea>
                </div>
                <div class="form-group">
                    <label for="policyDetails">Full Details</label>
                    <textarea id="policyDetails" rows="6"></textarea>
                </div>
                
                <!-- Links Section -->
                <div class="form-group">
                    <label>Related Links & Documents</label>
                    <div style="display: flex; gap: 1rem; margin-bottom: 1rem;">
                        <input type="text" id="linkTitle" placeholder="Link title" style="flex: 1;">
                        <input type="text" id="linkUrl" placeholder="URL or path (e.g., /docs/page.html, page.html, file://…)" style="flex: 2;">
                        <button type="button" class="btn btn-primary" onclick="addLink()">Add Link</button>
                    </div>
                    <div style="display: flex; gap: 1rem; margin-bottom: 1rem;">
                        <select id="documentSelect" style="flex: 1;">
                            <option value="">Select a document from vault...</option>
                        </select>
                        <button type="button" class="btn btn-primary" onclick="addDocumentLink()">Add Document</button>
                    </div>
                    <div id="linksList" style="display: flex; flex-direction: column; gap: 0.5rem; max-height: 200px; overflow-y: auto;">
                        <!-- Links will be displayed here -->
                    </div>
                </div>
                
                <div class="form-buttons">
                    <button type="button" class="btn btn-secondary" onclick="closeModal()">Cancel</button>
                    <button type="button" class="btn btn-danger" id="deletePolicyBtn" style="display: none;" onclick="deletePolicyFromModal()">Delete Policy</button>
                    <button type="submit" class="btn btn-primary">Save Policy</button>
                </div>
            </form>
        </div>
    </div>

    <!-- Modal for managing categories -->
    <div class="modal" id="categoryModal">
        <div class="modal-content">
            <h2>Manage Policy Categories</h2>
            <div class="form-group">
                <label>Add New Category</label>
                <div style="display: flex; gap: 1rem;">
                    <input type="text" id="newCategoryName" placeholder="Category name">
                    <input type="color" id="newCategoryColor" value="#3b82f6" style="width: 80px; cursor: pointer;">
                    <button class="btn btn-primary" onclick="addCategory()">Add</button>
                </div>
            </div>
            <div class="form-group">
                <label>Existing Categories</label>
                <div id="categoryList" style="display: flex; flex-direction: column; gap: 0.5rem; margin-top: 1rem;">
                    <!-- Categories will be listed here -->
                </div>
            </div>
            <div class="form-buttons">
                <button type="button" class="btn btn-secondary" onclick="closeCategoryModal()">Close</button>
            </div>
        </div>
    </div>

    <!-- Modal for document vault -->
    <div class="modal" id="vaultModal">
        <div class="modal-content" style="max-width: 900px;">
            <h2>Document Vault</h2>
            <div class="form-group">
                <label>Upload Document</label>
                
                <!-- Drag and Drop Area -->
                <div class="file-upload-area" id="dropZone">
                    <p>📤 Drag and drop files here or click to browse</p>
                    <label for="fileInput" class="file-input-label">Choose Files</label>
                    <input type="file" id="fileInput" accept="*/*" multiple style="display: none;">
                </div>
                
                <div style="display: flex; gap: 1rem; margin-top: 1rem;">
                    <select id="fileCategory" style="flex: 1;">
                        <option value="">Select Category...</option>
                    </select>
                    <button class="btn btn-primary" onclick="uploadDocuments()">Upload Selected Files</button>
                </div>
                
                <div class="upload-progress" id="uploadProgress">
                    <span id="uploadStatus">Uploading...</span>
                    <div class="progress-bar">
                        <div class="progress-fill" id="progressFill"></div>
                    </div>
                </div>
                
                <small style="color: #64748b; display: block; margin-top: 0.5rem;">
                    Supports all file types: PDF, Word, Excel, HTML, images, videos, and more. Maximum file size: 50MB per file.
                </small>
            </div>
            
            <div class="form-group">
                <label>Manage Document Categories</label>
                <div style="display: flex; gap: 1rem; margin-bottom: 1rem;">
                    <input type="text" id="newDocCategoryName" placeholder="New category name" style="flex: 1;">
                    <input type="color" id="newDocCategoryColor" value="#3b82f6" style="width: 60px; cursor: pointer;">
                    <button class="btn btn-primary" onclick="addDocumentCategory()">Add Category</button>
                </div>
                <div id="docCategoryList" style="display: flex; gap: 0.5rem; flex-wrap: wrap;">
                    <!-- Document categories will be listed here -->
                </div>
            </div>

            <div class="form-group">
                <label>Documents (<span id="documentCount">0</span>)</label>
                <div class="vault-filters" style="margin-bottom: 1rem;">
                    <button class="filter-btn active" onclick="filterDocuments('all')">All Files</button>
                    <div id="docCategoryFilters" style="display: inline-flex; gap: 0.5rem;">
                        <!-- Category filters will be added here -->
                    </div>
                </div>
                <div id="documentList" style="display: grid; gap: 1rem; max-height: 400px; overflow-y: auto;">
                    <!-- Documents will be listed here -->
                </div>
            </div>
            <div class="form-buttons">
                <button type="button" class="btn btn-secondary" onclick="closeVaultModal()">Close</button>
            </div>
        </div>
    </div>

    <script>
        // Initialize localStorage data
        function initializeStorage() {
            // Initialize policies if not exists
            if (!localStorage.getItem('mayorPolicies')) {
                const defaultPolicies = [
                    {
                        id: 1,
                        title: "Green Energy Initiative",
                        category: "environment",
                        description: "Transition to 100% renewable energy sources for all municipal buildings by 2030.",
                        details: "This comprehensive initiative includes solar panel installation, wind energy partnerships, and energy efficiency upgrades.",
                        date: "2024-01-15",
                        status: "Active"
                    },
                    {
                        id: 2,
                        title: "Affordable Housing Program",
                        category: "social",
                        description: "Create 5,000 new affordable housing units over the next 5 years.",
                        details: "Partnership with developers to ensure 20% of new constructions are affordable units.",
                        date: "2024-02-01",
                        status: "Active"
                    },
                    {
                        id: 3,
                        title: "Smart City Infrastructure",
                        category: "infrastructure",
                        description: "Implement IoT sensors and smart traffic management systems citywide.",
                        details: "Phase 1 focuses on downtown area with expansion to residential districts in Phase 2.",
                        date: "2024-03-10",
                        status: "Planning"
                    }
                ];
                localStorage.setItem('mayorPolicies', JSON.stringify(defaultPolicies));
            }
            
            // Initialize categories if not exists
            if (!localStorage.getItem('mayorCategories')) {
                const defaultCategories = [
                    { id: 'economic', name: 'Economic', color: '#3b82f6' },
                    { id: 'social', name: 'Social', color: '#10b981' },
                    { id: 'infrastructure', name: 'Infrastructure', color: '#f59e0b' },
                    { id: 'environment', name: 'Environment', color: '#84cc16' },
                    { id: 'education', name: 'Education', color: '#8b5cf6' }
                ];
                localStorage.setItem('mayorCategories', JSON.stringify(defaultCategories));
            }
            
            // Initialize document categories if not exists
            if (!localStorage.getItem('mayorDocCategories')) {
                const defaultDocCategories = [
                    { id: 'policies', name: 'Policies', color: '#3b82f6' },
                    { id: 'reports', name: 'Reports', color: '#10b981' },
                    { id: 'presentations', name: 'Presentations', color: '#f59e0b' },
                    { id: 'legal', name: 'Legal', color: '#ef4444' },
                    { id: 'budgets', name: 'Budgets', color: '#8b5cf6' }
                ];
                localStorage.setItem('mayorDocCategories', JSON.stringify(defaultDocCategories));
            }
            
            // Initialize documents if not exists
            if (!localStorage.getItem('mayorDocuments')) {
                localStorage.setItem('mayorDocuments', JSON.stringify([]));
            }
        }

        // Load data from localStorage
        let categories = JSON.parse(localStorage.getItem('mayorCategories') || '[]');
        let policies = JSON.parse(localStorage.getItem('mayorPolicies') || '[]');
        let documentCategories = JSON.parse(localStorage.getItem('mayorDocCategories') || '[]');
        let documents = JSON.parse(localStorage.getItem('mayorDocuments') || '[]');
        
        let currentFilter = 'all';
        let currentDocFilter = 'all';
        let editingPolicyId = null;
        let selectedFiles = [];
        let policyLinks = [];

        // Save functions
        function saveCategories() {
            localStorage.setItem('mayorCategories', JSON.stringify(categories));
        }

        function savePolicies() {
            localStorage.setItem('mayorPolicies', JSON.stringify(policies));
        }

        function saveDocumentCategories() {
            localStorage.setItem('mayorDocCategories', JSON.stringify(documentCategories));
        }

        function saveDocuments() {
            localStorage.setItem('mayorDocuments', JSON.stringify(documents));
        }

        // Initialize the dashboard
        function init() {
            initializeStorage();
            categories = JSON.parse(localStorage.getItem('mayorCategories') || '[]');
            policies = JSON.parse(localStorage.getItem('mayorPolicies') || '[]');
            documentCategories = JSON.parse(localStorage.getItem('mayorDocCategories') || '[]');
            documents = JSON.parse(localStorage.getItem('mayorDocuments') || '[]');
            
            updateCategoryFilters();
            updateCategorySelect();
            renderPolicies();
            setupDragAndDrop();
        }

        // Setup drag and drop functionality
        function setupDragAndDrop() {
            const dropZone = document.getElementById('dropZone');
            const fileInput = document.getElementById('fileInput');
            
            // Prevent default drag behaviors
            ['dragenter', 'dragover', 'dragleave', 'drop'].forEach(eventName => {
                dropZone.addEventListener(eventName, preventDefaults, false);
                document.body.addEventListener(eventName, preventDefaults, false);
            });
            
            // Highlight drop zone when item is dragged over it
            ['dragenter', 'dragover'].forEach(eventName => {
                dropZone.addEventListener(eventName, highlight, false);
            });
            
            ['dragleave', 'drop'].forEach(eventName => {
                dropZone.addEventListener(eventName, unhighlight, false);
            });
            
            // Handle dropped files
            dropZone.addEventListener('drop', handleDrop, false);
            
            // Handle file input change
            fileInput.addEventListener('change', handleFileSelect);
            
            function preventDefaults(e) {
                e.preventDefault();
                e.stopPropagation();
            }
            
            function highlight(e) {
                dropZone.classList.add('drag-over');
            }
            
            function unhighlight(e) {
                dropZone.classList.remove('drag-over');
            }
            
            function handleDrop(e) {
                const dt = e.dataTransfer;
                const files = dt.files;
                handleFiles(files);
            }
            
            function handleFileSelect(e) {
                const files = e.target.files;
                handleFiles(files);
            }
            
            function handleFiles(files) {
                selectedFiles = Array.from(files);
                updateFileList();
            }
        }

        function updateFileList() {
            const dropZone = document.getElementById('dropZone');
            if (selectedFiles.length > 0) {
                const fileNames = selectedFiles.map(f => f.name).join(', ');
                dropZone.innerHTML = `
                    <p>📄 Selected ${selectedFiles.length} file(s)</p>
                    <p style="font-size: 0.875rem; color: #94a3b8;">${fileNames}</p>
                    <label for="fileInput" class="file-input-label">Choose Different Files</label>
                    <input type="file" id="fileInput" accept="*/*" multiple style="display: none;">
                `;
                // Re-attach event listener
                document.getElementById('fileInput').addEventListener('change', function(e) {
                    selectedFiles = Array.from(e.target.files);
                    updateFileList();
                });
            }
        }

        // Update category filter buttons
        function updateCategoryFilters() {
            const container = document.getElementById('categoryFilters');
            container.innerHTML = '';
            
            categories.forEach(cat => {
                const btn = document.createElement('button');
                btn.className = 'filter-btn';
                btn.textContent = cat.name;
                btn.onclick = () => setFilter(cat.id);
                btn.style.setProperty('--category-color', cat.color);
                container.appendChild(btn);
            });
        }

        // Update category select dropdown
        function updateCategorySelect() {
            const select = document.getElementById('policyCategory');
            select.innerHTML = '';
            
            categories.forEach(cat => {
                const option = document.createElement('option');
                option.value = cat.id;
                option.textContent = cat.name;
                select.appendChild(option);
            });
        }

        // Get category by ID
        function getCategoryById(id) {
            return categories.find(cat => cat.id === id) || categories[0];
        }

        // Render policies
        function renderPolicies() {
            const grid = document.getElementById('policyGrid');
            const emptyState = document.getElementById('emptyState');
            const searchTerm = document.getElementById('searchInput').value.toLowerCase();
            
            let filteredPolicies = policies.filter(policy => {
                const matchesFilter = currentFilter === 'all' || policy.category === currentFilter;
                const matchesSearch = policy.title.toLowerCase().includes(searchTerm) || 
                                    policy.description.toLowerCase().includes(searchTerm);
                return matchesFilter && matchesSearch;
            });

            grid.innerHTML = '';
            
            if (filteredPolicies.length === 0) {
                emptyState.style.display = 'block';
            } else {
                emptyState.style.display = 'none';
                filteredPolicies.forEach(policy => {
                    const card = createPolicyCard(policy);
                    grid.appendChild(card);
                });
            }
        }

        // Create policy card element
        function createPolicyCard(policy) {
            const card = document.createElement('div');
            card.className = 'policy-card';
            
            const category = getCategoryById(policy.category);
            
            // Generate links HTML
            let linksHtml = '';
            if (policy.links && policy.links.length > 0) {
                linksHtml = `
                    <div class="policy-links-section">
                        <div class="policy-links-title">Related Links</div>
                        <div style="display: flex; flex-wrap: wrap;">
                            ${policy.links.map(link => `
                                <a href="${link.url}" target="_blank" class="policy-link-badge" title="${link.url}">
                                    <span>${link.icon}</span>
                                    <span>${link.title}</span>
                                </a>
                            `).join('')}
                        </div>
                    </div>
                `;
            }
            
            card.innerHTML = `
                <button class="policy-delete-btn" onclick="deletePolicy(event, ${policy.id})" title="Delete policy">×</button>
                <span class="policy-category" style="background: ${category.color}20; color: ${category.color};">${category.name}</span>
                <h3 class="policy-title">${policy.title}</h3>
                <p class="policy-description">${policy.description}</p>
                ${linksHtml}
                <div class="policy-meta">
                    <span>${new Date(policy.date).toLocaleDateString()}</span>
                    <div class="policy-status">
                        <span class="status-dot"></span>
                        <span>${policy.status}</span>
                    </div>
                </div>
            `;
            
            // Add click event to the card, but not the delete button
            card.onclick = (e) => {
                if (!e.target.classList.contains('policy-delete-btn')) {
                    viewPolicy(policy.id);
                }
            };
            
            return card;
        }

        // Delete policy function
        function deletePolicy(event, id) {
            event.stopPropagation(); // Prevent card click event
            
            const policy = policies.find(p => p.id === id);
            if (policy && confirm(`Are you sure you want to delete the policy "${policy.title}"?`)) {
                policies = policies.filter(p => p.id !== id);
                savePolicies();
                renderPolicies();
            }
        }

        // Filter policies
        function setFilter(filter) {
            currentFilter = filter;
            
            // Update active button
            document.querySelectorAll('.filter-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            event.target.classList.add('active');
            
            renderPolicies();
        }

        // Search policies
        function filterPolicies() {
            renderPolicies();
        }

        // Modal functions
        function openModal() {
            document.getElementById('policyModal').style.display = 'flex';
            document.getElementById('modalTitle').textContent = 'Add New Policy';
            document.getElementById('policyForm').reset();
            document.getElementById('deletePolicyBtn').style.display = 'none';
            editingPolicyId = null;
            policyLinks = [];
            updateLinksList();
            updateDocumentSelect();
        }

        function closeModal() {
            document.getElementById('policyModal').style.display = 'none';
            editingPolicyId = null;
            policyLinks = [];
        }

        function viewPolicy(id) {
            const policy = policies.find(p => p.id === id);
            if (policy) {
                editingPolicyId = id;
                document.getElementById('modalTitle').textContent = 'Edit Policy';
                document.getElementById('policyTitle').value = policy.title;
                document.getElementById('policyCategory').value = policy.category;
                document.getElementById('policyDescription').value = policy.description;
                document.getElementById('policyDetails').value = policy.details || '';
                document.getElementById('deletePolicyBtn').style.display = 'inline-block';
                
                // Load existing links
                policyLinks = policy.links ? [...policy.links] : [];
                updateLinksList();
                updateDocumentSelect();
                
                document.getElementById('policyModal').style.display = 'flex';
            }
        }

        function deletePolicyFromModal() {
            if (editingPolicyId) {
                const policy = policies.find(p => p.id === editingPolicyId);
                if (policy && confirm(`Are you sure you want to delete the policy "${policy.title}"?`)) {
                    policies = policies.filter(p => p.id !== editingPolicyId);
                    savePolicies();
                    renderPolicies();
                    closeModal();
                }
            }
        }

        function openCategoryModal() {
            document.getElementById('categoryModal').style.display = 'flex';
            updateCategoryList();
        }

        function closeCategoryModal() {
            document.getElementById('categoryModal').style.display = 'none';
        }

        function openVaultModal() {
            document.getElementById('vaultModal').style.display = 'flex';
            updateDocumentCategorySelect();
            updateDocumentCategoryList();
            updateDocumentCategoryFilters();
            renderDocuments();
            updateDocumentCount();
        }

        function closeVaultModal() {
            document.getElementById('vaultModal').style.display = 'none';
            selectedFiles = [];
            resetUploadArea();
        }

        function resetUploadArea() {
            const dropZone = document.getElementById('dropZone');
            dropZone.innerHTML = `
                <p>📤 Drag and drop files here or click to browse</p>
                <label for="fileInput" class="file-input-label">Choose Files</label>
                <input type="file" id="fileInput" accept="*/*" multiple style="display: none;">
            `;
            setupDragAndDrop();
            document.getElementById('uploadProgress').style.display = 'none';
        }

        // Category management
        function addCategory() {
            const name = document.getElementById('newCategoryName').value.trim();
            const color = document.getElementById('newCategoryColor').value;
            
            if (name) {
                const id = name.toLowerCase().replace(/\s+/g, '-');
                
                // Check if category already exists
                if (!categories.find(cat => cat.id === id)) {
                    categories.push({ id, name, color });
                    saveCategories();
                    document.getElementById('newCategoryName').value = '';
                    updateCategoryFilters();
                    updateCategorySelect();
                    updateCategoryList();
                } else {
                    alert('Category already exists!');
                }
            }
        }

        function updateCategoryList() {
            const list = document.getElementById('categoryList');
            list.innerHTML = '';
            
            categories.forEach(cat => {
                const item = document.createElement('div');
                item.style.cssText = 'display: flex; align-items: center; gap: 1rem; padding: 0.75rem; background: rgba(30, 41, 59, 0.5); border-radius: 8px;';
                
                item.innerHTML = `
                    <input type="color" value="${cat.color}" onchange="updateCategoryColor('${cat.id}', this.value)" style="width: 40px; height: 40px; cursor: pointer; border: none; border-radius: 4px;">
                    <span style="flex: 1;">${cat.name}</span>
                    <button class="btn btn-secondary" style="padding: 0.5rem 1rem;" onclick="deleteCategory('${cat.id}')">Delete</button>
                `;
                
                list.appendChild(item);
            });
        }

        function updateCategoryColor(id, color) {
            const category = categories.find(cat => cat.id === id);
            if (category) {
                category.color = color;
                saveCategories();
                renderPolicies();
            }
        }

        function deleteCategory(id) {
            // Check if category is in use
            const inUse = policies.some(policy => policy.category === id);
            
            if (inUse) {
                alert('Cannot delete this category as it is being used by existing policies.');
                return;
            }
            
            if (confirm('Are you sure you want to delete this category?')) {
                categories = categories.filter(cat => cat.id !== id);
                saveCategories();
                updateCategoryFilters();
                updateCategorySelect();
                updateCategoryList();
            }
        }

        // Document Vault Functions
        function updateDocumentCategorySelect() {
            const select = document.getElementById('fileCategory');
            select.innerHTML = '<option value="">Select Category...</option>';
            
            documentCategories.forEach(cat => {
                const option = document.createElement('option');
                option.value = cat.id;
                option.textContent = cat.name;
                select.appendChild(option);
            });
        }

        function updateDocumentCategoryList() {
            const list = document.getElementById('docCategoryList');
            list.innerHTML = '';
            
            documentCategories.forEach(cat => {
                const tag = document.createElement('div');
                tag.className = 'doc-category-tag';
                tag.style.backgroundColor = cat.color + '20';
                tag.style.borderColor = cat.color + '60';
                tag.style.color = cat.color;
                
                tag.innerHTML = `
                    <span>${cat.name}</span>
                    <button onclick="deleteDocumentCategory('${cat.id}')" title="Delete category">×</button>
                `;
                
                list.appendChild(tag);
            });
        }

        function updateDocumentCategoryFilters() {
            const container = document.getElementById('docCategoryFilters');
            container.innerHTML = '';
            
            documentCategories.forEach(cat => {
                const btn = document.createElement('button');
                btn.className = 'filter-btn';
                btn.textContent = cat.name;
                btn.onclick = (e) => filterDocuments(cat.id, e);
                btn.style.setProperty('--category-color', cat.color);
                container.appendChild(btn);
            });
        }

        function updateDocumentCount() {
            document.getElementById('documentCount').textContent = documents.length;
        }

        function addDocumentCategory() {
            const name = document.getElementById('newDocCategoryName').value.trim();
            const color = document.getElementById('newDocCategoryColor').value;
            
            if (name) {
                const id = name.toLowerCase().replace(/\s+/g, '-');
                
                if (!documentCategories.find(cat => cat.id === id)) {
                    documentCategories.push({ id, name, color });
                    saveDocumentCategories();
                    document.getElementById('newDocCategoryName').value = '';
                    updateDocumentCategorySelect();
                    updateDocumentCategoryList();
                    updateDocumentCategoryFilters();
                } else {
                    alert('Document category already exists!');
                }
            }
        }

        function deleteDocumentCategory(id) {
            const inUse = documents.some(doc => doc.category === id);
            
            if (inUse) {
                alert('Cannot delete this category as it contains documents.');
                return;
            }
            
            if (confirm('Are you sure you want to delete this category?')) {
                documentCategories = documentCategories.filter(cat => cat.id !== id);
                saveDocumentCategories();
                updateDocumentCategorySelect();
                updateDocumentCategoryList();
                updateDocumentCategoryFilters();
                
                // Reset filter if we were filtering by this category
                if (currentDocFilter === id) {
                    currentDocFilter = 'all';
                    renderDocuments();
                }
            }
        }

        function getDocumentCategoryById(id) {
            return documentCategories.find(cat => cat.id === id);
        }

        function getFileIcon(filename) {
            const ext = filename.split('.').pop().toLowerCase();
            const icons = {
                pdf: '📄',
                doc: '📝',
                docx: '📝',
                xls: '📊',
                xlsx: '📊',
                ppt: '📽️',
                pptx: '📽️',
                html: '🌐',
                htm: '🌐',
                jpg: '🖼️',
                jpeg: '🖼️',
                png: '🖼️',
                gif: '🖼️',
                svg: '🖼️',
                mp4: '🎥',
                mp3: '🎵',
                zip: '📦',
                rar: '📦',
                txt: '📋',
                csv: '📊',
                json: '{}',
                xml: '</>',
                js: '📜',
                css: '🎨'
            };
            return icons[ext] || '📎';
        }

        function getFileType(filename) {
            const ext = filename.split('.').pop().toLowerCase();
            if (['pdf'].includes(ext)) return 'pdf';
            if (['doc', 'docx', 'txt', 'rtf'].includes(ext)) return 'doc';
            if (['html', 'htm'].includes(ext)) return 'html';
            if (['jpg', 'jpeg', 'png', 'gif', 'svg', 'bmp'].includes(ext)) return 'image';
            return 'other';
        }

        function formatFileSize(bytes) {
            if (bytes === 0) return '0 Bytes';
            const k = 1024;
            const sizes = ['Bytes', 'KB', 'MB', 'GB'];
            const i = Math.floor(Math.log(bytes) / Math.log(k));
            return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + ' ' + sizes[i];
        }

        async function uploadDocuments() {
            const categorySelect = document.getElementById('fileCategory');
            const categoryId = categorySelect.value;
            
            if (selectedFiles.length === 0) {
                alert('Please select files to upload.');
                return;
            }
            
            if (!categoryId) {
                alert('Please select a category for the documents.');
                return;
            }
            
            const uploadProgress = document.getElementById('uploadProgress');
            const progressFill = document.getElementById('progressFill');
            const uploadStatus = document.getElementById('uploadStatus');
            
            uploadProgress.style.display = 'block';
            
            let uploadedCount = 0;
            const totalFiles = selectedFiles.length;
            
            for (const file of selectedFiles) {
                // Check file size (50MB limit)
                if (file.size > 50 * 1024 * 1024) {
                    alert(`File "${file.name}" exceeds 50MB limit and will be skipped.`);
                    continue;
                }
                
                uploadStatus.textContent = `Uploading ${uploadedCount + 1} of ${totalFiles}: ${file.name}`;
                
                try {
                    const content = await readFileAsBase64(file);
                    
                    const doc = {
                        id: Date.now() + Math.random(), // Ensure unique ID
                        name: file.name,
                        size: file.size,
                        type: getFileType(file.name),
                        category: categoryId,
                        uploadDate: new Date().toISOString(),
                        content: content
                    };
                    
                    documents.push(doc);
                    uploadedCount++;
                    
                    // Update progress
                    const progress = (uploadedCount / totalFiles) * 100;
                    progressFill.style.width = progress + '%';
                    
                } catch (error) {
                    console.error(`Error uploading ${file.name}:`, error);
                    alert(`Failed to upload ${file.name}`);
                }
            }
            
            saveDocuments();
            renderDocuments();
            updateDocumentCount();
            
            uploadStatus.textContent = `Upload complete! ${uploadedCount} of ${totalFiles} files uploaded.`;
            
            setTimeout(() => {
                selectedFiles = [];
                categorySelect.value = '';
                resetUploadArea();
            }, 2000);
        }

        function readFileAsBase64(file) {
            return new Promise((resolve, reject) => {
                const reader = new FileReader();
                reader.onload = (e) => resolve(e.target.result);
                reader.onerror = reject;
                reader.readAsDataURL(file);
            });
        }

        function filterDocuments(filter, event) {
            currentDocFilter = filter;
            
            // Update active button
            document.querySelectorAll('.vault-filters .filter-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            
            if (event && event.target) {
                event.target.classList.add('active');
            } else {
                // For programmatic calls, find and activate the right button
                document.querySelector('.vault-filters .filter-btn').classList.add('active');
            }
            
            renderDocuments();
        }

        function renderDocuments() {
            const list = document.getElementById('documentList');
            list.innerHTML = '';
            
            let filteredDocs = documents.filter(doc => {
                if (currentDocFilter === 'all') return true;
                return doc.category === currentDocFilter;
            });
            
            if (filteredDocs.length === 0) {
                list.innerHTML = '<p style="text-align: center; color: #64748b; padding: 2rem;">No documents found in this category.</p>';
                return;
            }
            
            // Sort by upload date (newest first)
            filteredDocs.sort((a, b) => new Date(b.uploadDate) - new Date(a.uploadDate));
            
            filteredDocs.forEach(doc => {
                const item = document.createElement('div');
                item.className = 'document-item';
                
                const category = getDocumentCategoryById(doc.category);
                const categoryBadge = category ? 
                    `<span class="document-category-badge" style="background: ${category.color}20; color: ${category.color};">${category.name}</span>` : 
                    '';
                
                item.innerHTML = `
                    <div class="document-icon">${getFileIcon(doc.name)}</div>
                    <div class="document-info">
                        <div class="document-name">${doc.name}</div>
                        <div class="document-meta">
                            ${categoryBadge}
                            ${formatFileSize(doc.size)} • Uploaded ${new Date(doc.uploadDate).toLocaleDateString()}
                        </div>
                    </div>
                    <div class="document-actions">
                        <button class="doc-btn" onclick="viewDocument(${doc.id})">View</button>
                        <button class="doc-btn" onclick="downloadDocument(${doc.id})">Download</button>
                        <button class="doc-btn danger" onclick="deleteDocument(${doc.id})">Delete</button>
                    </div>
                `;
                
                list.appendChild(item);
            });
        }

        function viewDocument(id) {
            const doc = documents.find(d => d.id === id);
            if (doc) {
                const fileType = doc.name.split('.').pop().toLowerCase();
                
                if (fileType === 'html' || fileType === 'htm') {
                    // For HTML files, extract the base64 content and decode it
                    const base64Content = doc.content.split(',')[1];
                    const decodedContent = atob(base64Content);
                    
                    // Open in new window and write the HTML content
                    const win = window.open('', '_blank');
                    win.document.open();
                    win.document.write(decodedContent);
                    win.document.close();
                } else if (['jpg', 'jpeg', 'png', 'gif', 'svg', 'bmp'].includes(fileType)) {
                    // For images, open in new window with img tag
                    const win = window.open('', '_blank');
                    win.document.write(`
                        <html>
                        <head><title>${doc.name}</title></head>
                        <body style="margin: 0; display: flex; justify-content: center; align-items: center; min-height: 100vh; background: #000;">
                            <img src="${doc.content}" style="max-width: 100%; max-height: 100vh; object-fit: contain;">
                        </body>
                        </html>
                    `);
                } else if (fileType === 'txt') {
                    // For text files, decode and display
                    const base64Content = doc.content.split(',')[1];
                    const decodedContent = atob(base64Content);
                    const win = window.open('', '_blank');
                    win.document.write(`
                        <html>
                        <head><title>${doc.name}</title></head>
                        <body style="font-family: monospace; white-space: pre-wrap; padding: 20px;">
                            ${decodedContent.replace(/</g, '&lt;').replace(/>/g, '&gt;')}
                        </body>
                        </html>
                    `);
                } else {
                    // For other files, open in new tab
                    window.open(doc.content, '_blank');
                }
            }
        }

        function downloadDocument(id) {
            const doc = documents.find(d => d.id === id);
            if (doc) {
                const a = document.createElement('a');
                a.href = doc.content;
                a.download = doc.name;
                document.body.appendChild(a);
                a.click();
                document.body.removeChild(a);
            }
        }

        function deleteDocument(id) {
            const doc = documents.find(d => d.id === id);
            if (doc && confirm(`Are you sure you want to delete "${doc.name}"?`)) {
                documents = documents.filter(d => d.id !== id);
                saveDocuments();
                renderDocuments();
                updateDocumentCount();
            }
        }

        // Link Management Functions
        function updateDocumentSelect() {
            const select = document.getElementById('documentSelect');
            select.innerHTML = '<option value="">Select a document from vault...</option>';
            
            // Group documents by category
            const docsByCategory = {};
            documents.forEach(doc => {
                if (!docsByCategory[doc.category]) {
                    docsByCategory[doc.category] = [];
                }
                docsByCategory[doc.category].push(doc);
            });
            
            // Add documents grouped by category
            Object.keys(docsByCategory).forEach(catId => {
                const category = getDocumentCategoryById(catId);
                if (category && docsByCategory[catId].length > 0) {
                    const optgroup = document.createElement('optgroup');
                    optgroup.label = category.name;
                    
                    docsByCategory[catId].forEach(doc => {
                        const option = document.createElement('option');
                        option.value = doc.id;
                        option.textContent = doc.name;
                        option.dataset.docName = doc.name;
                        option.dataset.docType = doc.type;
                        optgroup.appendChild(option);
                    });
                    
                    select.appendChild(optgroup);
                }
            });
        }

        function addLink() {
            const titleInput = document.getElementById('linkTitle');
            const urlInput = document.getElementById('linkUrl');
            
            const title = titleInput.value.trim();
            const url = urlInput.value.trim();
            
            if (!title || !url) {
                alert('Please enter both title and URL');
                return;
            }
            
            // Allow http(s), file://, data:, mailto:, tel:, and relative paths
            (function() {
                const ok = /^(https?:\/\/|file:\/\/|data:|mailto:|tel:)/i.test(url)
                         || url.startsWith('/') || url.startsWith('./') || url.startsWith('../')
                         || /^[\w\-./#?%=&]+$/.test(url);
                if (!ok) {
                    alert('Please enter a URL or path (http(s), file://, or a relative path like /docs/page.html).');
                    return;
                }
            })();

            
            policyLinks.push({
                id: Date.now(),
                title: title,
                url: url,
                type: 'external',
                icon: '🔗'
            });
            
            titleInput.value = '';
            urlInput.value = '';
            updateLinksList();
        }

        function addDocumentLink() {
            const select = document.getElementById('documentSelect');
            const selectedOption = select.options[select.selectedIndex];
            
            if (!select.value) {
                alert('Please select a document');
                return;
            }
            
            const doc = documents.find(d => d.id == select.value);
            if (!doc) return;
            
            // Check if already added
            if (policyLinks.find(link => link.docId === doc.id)) {
                alert('This document is already linked to this policy');
                return;
            }
            
            policyLinks.push({
                id: Date.now(),
                title: doc.name,
                url: '#', // Will be handled by viewLinkedDocument
                type: 'document',
                docId: doc.id,
                icon: getFileIcon(doc.name)
            });
            
            select.value = '';
            updateLinksList();
        }

        function removeLink(linkId) {
            policyLinks = policyLinks.filter(link => link.id !== linkId);
            updateLinksList();
        }

        function updateLinksList() {
            const container = document.getElementById('linksList');
            container.innerHTML = '';
            
            if (policyLinks.length === 0) {
                container.innerHTML = '<p style="color: #64748b; text-align: center; padding: 1rem;">No links added yet</p>';
                return;
            }
            
            policyLinks.forEach(link => {
                const item = document.createElement('div');
                item.className = 'link-item';
                
                if (link.type === 'external') {
                    item.innerHTML = `
                        <span class="link-icon">${link.icon}</span>
                        <div class="link-info">
                            <a href="${link.url}" target="_blank" class="link-title">${link.title}</a>
                            <div class="link-url">${link.url}</div>
                        </div>
                        <button class="link-remove" onclick="removeLink(${link.id})">×</button>
                    `;
                } else {
                    item.innerHTML = `
                        <span class="link-icon">${link.icon}</span>
                        <div class="link-info">
                            <a href="javascript:void(0)" onclick="viewLinkedDocument(${link.docId})" class="link-title">${link.title}</a>
                            <div class="link-url">Document from vault</div>
                        </div>
                        <button class="link-remove" onclick="removeLink(${link.id})">×</button>
                    `;
                }
                
                container.appendChild(item);
            });
        }

        function viewLinkedDocument(docId) {
            // Find the document and view it
            viewDocument(docId);
        }

        // Update form submission to include links
        document.getElementById('policyForm').addEventListener('submit', function(e) {
            e.preventDefault();
            
            const formData = {
                title: document.getElementById('policyTitle').value,
                category: document.getElementById('policyCategory').value,
                description: document.getElementById('policyDescription').value,
                details: document.getElementById('policyDetails').value,
                date: new Date().toISOString().split('T')[0],
                status: 'Active',
                links: policyLinks
            };
            
            if (editingPolicyId) {
                // Update existing policy
                const index = policies.findIndex(p => p.id === editingPolicyId);
                policies[index] = { ...policies[index], ...formData };
            } else {
                // Add new policy
                formData.id = Date.now();
                policies.push(formData);
            }
            
            savePolicies();
            renderPolicies();
            closeModal();
        });

        // Close modal when clicking outside
        document.getElementById('policyModal').addEventListener('click', function(e) {
            if (e.target === this) {
                closeModal();
            }
        });

        document.getElementById('categoryModal').addEventListener('click', function(e) {
            if (e.target === this) {
                closeCategoryModal();
            }
        });

        document.getElementById('vaultModal').addEventListener('click', function(e) {
            if (e.target === this) {
                closeVaultModal();
            }
        });

        // Handle enter key for adding categories
        document.getElementById('newCategoryName').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                addCategory();
            }
        });

        document.getElementById('newDocCategoryName').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                addDocumentCategory();
            }
        });

        // Initialize on load
        init();
    </script>
    <html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mayor's Policy Dashboard</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
            background: linear-gradient(135deg, #0f172a 0%, #1e293b 100%);
            color: #e0e0e0;
            min-height: 100vh;
            overflow-x: hidden;
        }

        /* Header */
        header {
            background: rgba(15, 23, 42, 0.95);
            backdrop-filter: blur(20px);
            border-bottom: 1px solid rgba(59, 130, 246, 0.3);
            padding: 1.5rem 0;
            position: sticky;
            top: 0;
            z-index: 100;
            box-shadow: 0 4px 30px rgba(0, 0, 0, 0.3);
        }

        .header-content {
            max-width: 1400px;
            margin: 0 auto;
            padding: 0 2rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        h1 {
            font-size: 2.5rem;
            font-weight: 700;
            background: linear-gradient(135deg, #3b82f6 0%, #60a5fa 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-shadow: 0 0 30px rgba(59, 130, 246, 0.5);
        }

        /* Search and Filter */
        .controls {
            max-width: 1400px;
            margin: 2rem auto;
            padding: 0 2rem;
            display: flex;
            gap: 1rem;
            flex-wrap: wrap;
        }

        .search-box {
            flex: 1;
            min-width: 300px;
            position: relative;
        }

        .search-box input {
            width: 100%;
            padding: 1rem 3rem 1rem 1.5rem;
            background: rgba(30, 41, 59, 0.8);
            border: 2px solid rgba(59, 130, 246, 0.3);
            border-radius: 12px;
            color: #e0e0e0;
            font-size: 1rem;
            transition: all 0.3s ease;
        }

        .search-box input:focus {
            outline: none;
            border-color: #3b82f6;
            box-shadow: 0 0 20px rgba(59, 130, 246, 0.4);
            transform: translateY(-2px);
        }

        .search-icon {
            position: absolute;
            right: 1rem;
            top: 50%;
            transform: translateY(-50%);
            color: #64748b;
        }

        .filter-buttons {
            display: flex;
            gap: 0.5rem;
            flex-wrap: wrap;
        }

        .filter-btn {
            padding: 0.75rem 1.5rem;
            background: rgba(30, 41, 59, 0.8);
            border: 2px solid rgba(59, 130, 246, 0.3);
            border-radius: 8px;
            color: #94a3b8;
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 0.9rem;
            font-weight: 500;
        }

        .filter-btn:hover {
            background: rgba(59, 130, 246, 0.2);
            border-color: #3b82f6;
            transform: translateY(-2px);
            box-shadow: 0 4px 15px rgba(59, 130, 246, 0.3);
        }

        .filter-btn.active {
            background: #3b82f6;
            border-color: #3b82f6;
            color: white;
            box-shadow: 0 0 20px rgba(59, 130, 246, 0.5);
        }

        /* Add Policy Button */
        .add-policy-btn {
            padding: 0.75rem 2rem;
            background: linear-gradient(135deg, #3b82f6 0%, #2563eb 100%);
            border: none;
            border-radius: 8px;
            color: white;
            cursor: pointer;
            font-size: 1rem;
            font-weight: 600;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(59, 130, 246, 0.3);
        }

        .add-policy-btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 25px rgba(59, 130, 246, 0.5);
        }

        /* Policy Grid */
        .policy-grid {
            max-width: 1400px;
            margin: 2rem auto;
            padding: 0 2rem;
            display: grid;
            grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
            gap: 2rem;
        }

        .policy-card {
            background: rgba(30, 41, 59, 0.8);
            backdrop-filter: blur(10px);
            border: 1px solid rgba(59, 130, 246, 0.2);
            border-radius: 16px;
            padding: 2rem;
            transition: all 0.3s ease;
            cursor: pointer;
            position: relative;
            overflow: hidden;
        }

        .policy-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 4px;
            background: linear-gradient(90deg, #3b82f6, #60a5fa);
            transform: scaleX(0);
            transition: transform 0.3s ease;
        }

        .policy-card:hover {
            transform: translateY(-5px);
            box-shadow: 0 10px 40px rgba(59, 130, 246, 0.3);
            border-color: #3b82f6;
        }

        .policy-card:hover::before {
            transform: scaleX(1);
        }

        .policy-card:hover .policy-delete-btn {
            opacity: 1;
        }

        .policy-delete-btn {
            position: absolute;
            top: 1rem;
            right: 1rem;
            background: rgba(239, 68, 68, 0.2);
            border: 1px solid rgba(239, 68, 68, 0.3);
            color: #f87171;
            padding: 0.5rem;
            border-radius: 8px;
            cursor: pointer;
            opacity: 0;
            transition: all 0.3s ease;
            font-size: 1.2rem;
            line-height: 1;
            width: 2rem;
            height: 2rem;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .policy-delete-btn:hover {
            background: rgba(239, 68, 68, 0.3);
            border-color: #ef4444;
            transform: scale(1.1);
        }

        .policy-category {
            display: inline-block;
            padding: 0.25rem 0.75rem;
            background: rgba(59, 130, 246, 0.2);
            color: #60a5fa;
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: 600;
            margin-bottom: 1rem;
        }

        .policy-title {
            font-size: 1.5rem;
            font-weight: 600;
            margin-bottom: 1rem;
            color: #f1f5f9;
        }

        .policy-description {
            color: #94a3b8;
            line-height: 1.6;
            margin-bottom: 1.5rem;
        }

        .policy-meta {
            display: flex;
            justify-content: space-between;
            align-items: center;
            color: #64748b;
            font-size: 0.9rem;
        }

        .policy-status {
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .status-dot {
            width: 8px;
            height: 8px;
            border-radius: 50%;
            background: #10b981;
            animation: pulse 2s ease-in-out infinite;
        }

        @keyframes pulse {
            0%, 100% { opacity: 1; transform: scale(1); }
            50% { opacity: 0.7; transform: scale(1.2); }
        }

        /* Modal */
        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, 0.8);
            backdrop-filter: blur(5px);
            z-index: 1000;
            align-items: center;
            justify-content: center;
            padding: 2rem;
            overflow-y: auto;
        }

        .modal-content {
            background: #1e293b;
            border-radius: 20px;
            padding: 3rem;
            max-width: 600px;
            width: 100%;
            max-height: 90vh;
            overflow-y: auto;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.5);
            border: 1px solid rgba(59, 130, 246, 0.3);
            margin: auto;
        }

        .modal h2 {
            font-size: 2rem;
            margin-bottom: 2rem;
            color: #f1f5f9;
        }

        .form-group {
            margin-bottom: 1.5rem;
        }

        .form-group label {
            display: block;
            margin-bottom: 0.5rem;
            color: #94a3b8;
            font-weight: 500;
        }

        .form-group input,
        .form-group select,
        .form-group textarea {
            width: 100%;
            padding: 0.75rem 1rem;
            background: rgba(15, 23, 42, 0.8);
            border: 2px solid rgba(59, 130, 246, 0.3);
            border-radius: 8px;
            color: #e0e0e0;
            font-size: 1rem;
            transition: all 0.3s ease;
        }

        .form-group input:focus,
        .form-group select:focus,
        .form-group textarea:focus {
            outline: none;
            border-color: #3b82f6;
            box-shadow: 0 0 10px rgba(59, 130, 246, 0.3);
        }

        .form-group textarea {
            min-height: 120px;
            resize: vertical;
        }

        .form-buttons {
            display: flex;
            gap: 1rem;
            justify-content: flex-end;
            margin-top: 2rem;
        }

        .btn {
            padding: 0.75rem 2rem;
            border: none;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .btn-primary {
            background: linear-gradient(135deg, #3b82f6 0%, #2563eb 100%);
            color: white;
        }

        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 20px rgba(59, 130, 246, 0.4);
        }

        .btn-secondary {
            background: rgba(100, 116, 139, 0.2);
            color: #94a3b8;
            border: 2px solid rgba(100, 116, 139, 0.3);
        }

        .btn-secondary:hover {
            background: rgba(100, 116, 139, 0.3);
            border-color: #64748b;
        }

        .btn-danger {
            background: rgba(239, 68, 68, 0.2);
            color: #f87171;
            border: 2px solid rgba(239, 68, 68, 0.3);
        }

        .btn-danger:hover {
            background: rgba(239, 68, 68, 0.3);
            border-color: #ef4444;
            transform: translateY(-2px);
            box-shadow: 0 4px 20px rgba(239, 68, 68, 0.4);
        }

        /* Empty State */
        .empty-state {
            text-align: center;
            padding: 4rem 2rem;
            color: #64748b;
        }

        .empty-state svg {
            width: 100px;
            height: 100px;
            margin-bottom: 2rem;
            opacity: 0.5;
        }

        /* Document Vault Styles */
        .document-item {
            display: flex;
            align-items: center;
            gap: 1rem;
            padding: 1rem;
            background: rgba(30, 41, 59, 0.5);
            border: 1px solid rgba(59, 130, 246, 0.2);
            border-radius: 8px;
            transition: all 0.3s ease;
        }

        .document-item:hover {
            background: rgba(30, 41, 59, 0.8);
            border-color: #3b82f6;
            transform: translateX(5px);
        }

        .document-icon {
            font-size: 2rem;
            width: 3rem;
            text-align: center;
        }

        .document-info {
            flex: 1;
        }

        .document-name {
            font-weight: 600;
            color: #f1f5f9;
            margin-bottom: 0.25rem;
        }

        .document-meta {
            font-size: 0.875rem;
            color: #64748b;
        }

        .document-category-badge {
            display: inline-block;
            padding: 0.25rem 0.75rem;
            border-radius: 12px;
            font-size: 0.75rem;
            font-weight: 600;
            margin-right: 0.5rem;
        }

        .document-actions {
            display: flex;
            gap: 0.5rem;
        }

        .doc-btn {
            padding: 0.5rem 1rem;
            background: rgba(59, 130, 246, 0.2);
            border: 1px solid rgba(59, 130, 246, 0.3);
            border-radius: 6px;
            color: #60a5fa;
            cursor: pointer;
            font-size: 0.875rem;
            transition: all 0.3s ease;
        }

        .doc-btn:hover {
            background: rgba(59, 130, 246, 0.3);
            border-color: #3b82f6;
        }

        .doc-btn.danger {
            background: rgba(239, 68, 68, 0.2);
            border-color: rgba(239, 68, 68, 0.3);
            color: #f87171;
        }

        .doc-btn.danger:hover {
            background: rgba(239, 68, 68, 0.3);
            border-color: #ef4444;
        }

        .vault-filters {
            display: flex;
            gap: 0.5rem;
            flex-wrap: wrap;
        }

        .doc-category-tag {
            display: inline-flex;
            align-items: center;
            gap: 0.5rem;
            padding: 0.5rem 1rem;
            background: rgba(59, 130, 246, 0.2);
            border: 1px solid rgba(59, 130, 246, 0.3);
            border-radius: 20px;
            font-size: 0.875rem;
            margin-bottom: 0.5rem;
        }

        .doc-category-tag button {
            background: none;
            border: none;
            color: #f87171;
            cursor: pointer;
            padding: 0 0.25rem;
            font-size: 1rem;
        }

        /* Upload Progress */
        .upload-progress {
            display: none;
            margin-top: 1rem;
            padding: 1rem;
            background: rgba(59, 130, 246, 0.1);
            border: 1px solid rgba(59, 130, 246, 0.3);
            border-radius: 8px;
        }

        .progress-bar {
            width: 100%;
            height: 8px;
            background: rgba(59, 130, 246, 0.2);
            border-radius: 4px;
            overflow: hidden;
            margin-top: 0.5rem;
        }

        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #3b82f6, #60a5fa);
            transition: width 0.3s ease;
            width: 0%;
        }

        /* Drag and Drop */
        .file-upload-area {
            border: 2px dashed rgba(59, 130, 246, 0.5);
            border-radius: 12px;
            padding: 2rem;
            text-align: center;
            transition: all 0.3s ease;
            margin-bottom: 1rem;
        }

        .file-upload-area.drag-over {
            background: rgba(59, 130, 246, 0.1);
            border-color: #3b82f6;
        }

        .file-upload-area p {
            color: #94a3b8;
            margin-bottom: 1rem;
        }

        .file-input-label {
            display: inline-block;
            padding: 0.75rem 1.5rem;
            background: rgba(59, 130, 246, 0.2);
            border: 1px solid rgba(59, 130, 246, 0.3);
            border-radius: 8px;
            color: #60a5fa;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .file-input-label:hover {
            background: rgba(59, 130, 246, 0.3);
            border-color: #3b82f6;
        }

        /* Links Styles */
        .link-item {
            display: flex;
            align-items: center;
            gap: 0.5rem;
            padding: 0.5rem;
            background: rgba(30, 41, 59, 0.5);
            border: 1px solid rgba(59, 130, 246, 0.2);
            border-radius: 6px;
        }

        .link-item:hover {
            background: rgba(30, 41, 59, 0.8);
            border-color: rgba(59, 130, 246, 0.4);
        }

        .link-icon {
            font-size: 1.2rem;
            width: 1.5rem;
            text-align: center;
        }

        .link-info {
            flex: 1;
        }

        .link-title {
            font-weight: 500;
            color: #60a5fa;
            cursor: pointer;
            text-decoration: none;
        }

        .link-title:hover {
            text-decoration: underline;
        }

        .link-url {
            font-size: 0.75rem;
            color: #64748b;
        }

        .link-remove {
            background: none;
            border: none;
            color: #f87171;
            cursor: pointer;
            padding: 0.25rem;
            font-size: 1rem;
            opacity: 0.7;
            transition: opacity 0.2s;
        }

        .link-remove:hover {
            opacity: 1;
        }

        .policy-links-section {
            margin-top: 1rem;
            padding-top: 1rem;
            border-top: 1px solid rgba(59, 130, 246, 0.2);
        }

        .policy-links-title {
            font-size: 0.875rem;
            font-weight: 600;
            color: #94a3b8;
            margin-bottom: 0.5rem;
        }

        .policy-link-badge {
            display: inline-flex;
            align-items: center;
            gap: 0.25rem;
            padding: 0.25rem 0.5rem;
            background: rgba(59, 130, 246, 0.1);
            border: 1px solid rgba(59, 130, 246, 0.2);
            border-radius: 4px;
            font-size: 0.75rem;
            color: #60a5fa;
            text-decoration: none;
            transition: all 0.2s;
            margin-right: 0.5rem;
            margin-bottom: 0.5rem;
        }

        .policy-link-badge:hover {
            background: rgba(59, 130, 246, 0.2);
            border-color: rgba(59, 130, 246, 0.4);
            transform: translateY(-1px);
        }

        /* Responsive */
        @media (max-width: 768px) {
            h1 {
                font-size: 2rem;
            }
            
            .controls {
                flex-direction: column;
            }
            
            .search-box {
                min-width: 100%;
            }
            
            .policy-grid {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <header>
        <div class="header-content">
            <h1>Mayor's Policy Dashboard</h1>
            <div style="display: flex; gap: 1rem;">
                <button class="add-policy-btn" onclick="openVaultModal()">📁 Document Vault</button>
                <button class="add-policy-btn" onclick="openCategoryModal()">⚙️ Manage Categories</button>
                <button class="add-policy-btn" onclick="openModal()">+ Add New Policy</button>
            </div>
        </div>
    </header>

    <div class="controls">
        <div class="search-box">
            <input type="text" id="searchInput" placeholder="Search policies..." onkeyup="filterPolicies()">
            <span class="search-icon">🔍</span>
        </div>
        <div class="filter-buttons">
            <button class="filter-btn active" onclick="setFilter('all')">All</button>
            <div id="categoryFilters">
                <!-- Category filters will be dynamically added here -->
            </div>
        </div>
    </div>

    <div class="policy-grid" id="policyGrid">
        <!-- Policies will be dynamically added here -->
    </div>

    <div class="empty-state" id="emptyState" style="display: none;">
        <svg viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
            <path d="M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2"/>
        </svg>
        <h3>No policies found</h3>
        <p>Add your first policy to get started!</p>
    </div>

    <!-- Modal for adding/editing policies -->
    <div class="modal" id="policyModal">
        <div class="modal-content">
            <h2 id="modalTitle">Add New Policy</h2>
            <form id="policyForm">
                <div class="form-group">
                    <label for="policyTitle">Policy Title</label>
                    <input type="text" id="policyTitle" required>
                </div>
                <div class="form-group">
                    <label for="policyCategory">Category</label>
                    <select id="policyCategory" required>
                        <!-- Categories will be dynamically populated -->
                    </select>
                </div>
                <div class="form-group">
                    <label for="policyDescription">Description</label>
                    <textarea id="policyDescription" required></textarea>
                </div>
                <div class="form-group">
                    <label for="policyDetails">Full Details</label>
                    <textarea id="policyDetails" rows="6"></textarea>
                </div>
                
                <!-- Links Section -->
                <div class="form-group">
                    <label>Related Links & Documents</label>
                    <div style="display: flex; gap: 1rem; margin-bottom: 1rem;">
                        <input type="text" id="linkTitle" placeholder="Link title" style="flex: 1;">
                        <input type="text" id="linkUrl" placeholder="URL or path (e.g., /docs/page.html, page.html, file://…)" style="flex: 2;">
                        <button type="button" class="btn btn-primary" onclick="addLink()">Add Link</button>
                    </div>
                    <div style="display: flex; gap: 1rem; margin-bottom: 1rem;">
                        <select id="documentSelect" style="flex: 1;">
                            <option value="">Select a document from vault...</option>
                        </select>
                        <button type="button" class="btn btn-primary" onclick="addDocumentLink()">Add Document</button>
                    </div>
                    <div id="linksList" style="display: flex; flex-direction: column; gap: 0.5rem; max-height: 200px; overflow-y: auto;">
                        <!-- Links will be displayed here -->
                    </div>
                </div>
                
                <div class="form-buttons">
                    <button type="button" class="btn btn-secondary" onclick="closeModal()">Cancel</button>
                    <button type="button" class="btn btn-danger" id="deletePolicyBtn" style="display: none;" onclick="deletePolicyFromModal()">Delete Policy</button>
                    <button type="submit" class="btn btn-primary">Save Policy</button>
                </div>
            </form>
        </div>
    </div>

    <!-- Modal for managing categories -->
    <div class="modal" id="categoryModal">
        <div class="modal-content">
            <h2>Manage Policy Categories</h2>
            <div class="form-group">
                <label>Add New Category</label>
                <div style="display: flex; gap: 1rem;">
                    <input type="text" id="newCategoryName" placeholder="Category name">
                    <input type="color" id="newCategoryColor" value="#3b82f6" style="width: 80px; cursor: pointer;">
                    <button class="btn btn-primary" onclick="addCategory()">Add</button>
                </div>
            </div>
            <div class="form-group">
                <label>Existing Categories</label>
                <div id="categoryList" style="display: flex; flex-direction: column; gap: 0.5rem; margin-top: 1rem;">
                    <!-- Categories will be listed here -->
                </div>
            </div>
            <div class="form-buttons">
                <button type="button" class="btn btn-secondary" onclick="closeCategoryModal()">Close</button>
            </div>
        </div>
    </div>

    <!-- Modal for document vault -->
    <div class="modal" id="vaultModal">
        <div class="modal-content" style="max-width: 900px;">
            <h2>Document Vault</h2>
            <div class="form-group">
                <label>Upload Document</label>
                
                <!-- Drag and Drop Area -->
                <div class="file-upload-area" id="dropZone">
                    <p>📤 Drag and drop files here or click to browse</p>
                    <label for="fileInput" class="file-input-label">Choose Files</label>
                    <input type="file" id="fileInput" accept="*/*" multiple style="display: none;">
                </div>
                
                <div style="display: flex; gap: 1rem; margin-top: 1rem;">
                    <select id="fileCategory" style="flex: 1;">
                        <option value="">Select Category...</option>
                    </select>
                    <button class="btn btn-primary" onclick="uploadDocuments()">Upload Selected Files</button>
                </div>
                
                <div class="upload-progress" id="uploadProgress">
                    <span id="uploadStatus">Uploading...</span>
                    <div class="progress-bar">
                        <div class="progress-fill" id="progressFill"></div>
                    </div>
                </div>
                
                <small style="color: #64748b; display: block; margin-top: 0.5rem;">
                    Supports all file types: PDF, Word, Excel, HTML, images, videos, and more. Maximum file size: 50MB per file.
                </small>
            </div>
            
            <div class="form-group">
                <label>Manage Document Categories</label>
                <div style="display: flex; gap: 1rem; margin-bottom: 1rem;">
                    <input type="text" id="newDocCategoryName" placeholder="New category name" style="flex: 1;">
                    <input type="color" id="newDocCategoryColor" value="#3b82f6" style="width: 60px; cursor: pointer;">
                    <button class="btn btn-primary" onclick="addDocumentCategory()">Add Category</button>
                </div>
                <div id="docCategoryList" style="display: flex; gap: 0.5rem; flex-wrap: wrap;">
                    <!-- Document categories will be listed here -->
                </div>
            </div>

            <div class="form-group">
                <label>Documents (<span id="documentCount">0</span>)</label>
                <div class="vault-filters" style="margin-bottom: 1rem;">
                    <button class="filter-btn active" onclick="filterDocuments('all')">All Files</button>
                    <div id="docCategoryFilters" style="display: inline-flex; gap: 0.5rem;">
                        <!-- Category filters will be added here -->
                    </div>
                </div>
                <div id="documentList" style="display: grid; gap: 1rem; max-height: 400px; overflow-y: auto;">
                    <!-- Documents will be listed here -->
                </div>
            </div>
            <div class="form-buttons">
                <button type="button" class="btn btn-secondary" onclick="closeVaultModal()">Close</button>
            </div>
        </div>
    </div>

    <script>
        // Initialize localStorage data
        function initializeStorage() {
            // Initialize policies if not exists
            if (!localStorage.getItem('mayorPolicies')) {
                const defaultPolicies = [
                    {
                        id: 1,
                        title: "Green Energy Initiative",
                        category: "environment",
                        description: "Transition to 100% renewable energy sources for all municipal buildings by 2030.",
                        details: "This comprehensive initiative includes solar panel installation, wind energy partnerships, and energy efficiency upgrades.",
                        date: "2024-01-15",
                        status: "Active"
                    },
                    {
                        id: 2,
                        title: "Affordable Housing Program",
                        category: "social",
                        description: "Create 5,000 new affordable housing units over the next 5 years.",
                        details: "Partnership with developers to ensure 20% of new constructions are affordable units.",
                        date: "2024-02-01",
                        status: "Active"
                    },
                    {
                        id: 3,
                        title: "Smart City Infrastructure",
                        category: "infrastructure",
                        description: "Implement IoT sensors and smart traffic management systems citywide.",
                        details: "Phase 1 focuses on downtown area with expansion to residential districts in Phase 2.",
                        date: "2024-03-10",
                        status: "Planning"
                    }
                ];
                localStorage.setItem('mayorPolicies', JSON.stringify(defaultPolicies));
            }
            
            // Initialize categories if not exists
            if (!localStorage.getItem('mayorCategories')) {
                const defaultCategories = [
                    { id: 'economic', name: 'Economic', color: '#3b82f6' },
                    { id: 'social', name: 'Social', color: '#10b981' },
                    { id: 'infrastructure', name: 'Infrastructure', color: '#f59e0b' },
                    { id: 'environment', name: 'Environment', color: '#84cc16' },
                    { id: 'education', name: 'Education', color: '#8b5cf6' }
                ];
                localStorage.setItem('mayorCategories', JSON.stringify(defaultCategories));
            }
            
            // Initialize document categories if not exists
            if (!localStorage.getItem('mayorDocCategories')) {
                const defaultDocCategories = [
                    { id: 'policies', name: 'Policies', color: '#3b82f6' },
                    { id: 'reports', name: 'Reports', color: '#10b981' },
                    { id: 'presentations', name: 'Presentations', color: '#f59e0b' },
                    { id: 'legal', name: 'Legal', color: '#ef4444' },
                    { id: 'budgets', name: 'Budgets', color: '#8b5cf6' }
                ];
                localStorage.setItem('mayorDocCategories', JSON.stringify(defaultDocCategories));
            }
            
            // Initialize documents if not exists
            if (!localStorage.getItem('mayorDocuments')) {
                localStorage.setItem('mayorDocuments', JSON.stringify([]));
            }
        }

        // Load data from localStorage
        let categories = JSON.parse(localStorage.getItem('mayorCategories') || '[]');
        let policies = JSON.parse(localStorage.getItem('mayorPolicies') || '[]');
        let documentCategories = JSON.parse(localStorage.getItem('mayorDocCategories') || '[]');
        let documents = JSON.parse(localStorage.getItem('mayorDocuments') || '[]');
        
        let currentFilter = 'all';
        let currentDocFilter = 'all';
        let editingPolicyId = null;
        let selectedFiles = [];
        let policyLinks = [];

        // Save functions
        function saveCategories() {
            localStorage.setItem('mayorCategories', JSON.stringify(categories));
        }

        function savePolicies() {
            localStorage.setItem('mayorPolicies', JSON.stringify(policies));
        }

        function saveDocumentCategories() {
            localStorage.setItem('mayorDocCategories', JSON.stringify(documentCategories));
        }

        function saveDocuments() {
            localStorage.setItem('mayorDocuments', JSON.stringify(documents));
        }

        // Initialize the dashboard
        function init() {
            initializeStorage();
            categories = JSON.parse(localStorage.getItem('mayorCategories') || '[]');
            policies = JSON.parse(localStorage.getItem('mayorPolicies') || '[]');
            documentCategories = JSON.parse(localStorage.getItem('mayorDocCategories') || '[]');
            documents = JSON.parse(localStorage.getItem('mayorDocuments') || '[]');
            
            updateCategoryFilters();
            updateCategorySelect();
            renderPolicies();
            setupDragAndDrop();
        }

        // Setup drag and drop functionality
        function setupDragAndDrop() {
            const dropZone = document.getElementById('dropZone');
            const fileInput = document.getElementById('fileInput');
            
            // Prevent default drag behaviors
            ['dragenter', 'dragover', 'dragleave', 'drop'].forEach(eventName => {
                dropZone.addEventListener(eventName, preventDefaults, false);
                document.body.addEventListener(eventName, preventDefaults, false);
            });
            
            // Highlight drop zone when item is dragged over it
            ['dragenter', 'dragover'].forEach(eventName => {
                dropZone.addEventListener(eventName, highlight, false);
            });
            
            ['dragleave', 'drop'].forEach(eventName => {
                dropZone.addEventListener(eventName, unhighlight, false);
            });
            
            // Handle dropped files
            dropZone.addEventListener('drop', handleDrop, false);
            
            // Handle file input change
            fileInput.addEventListener('change', handleFileSelect);
            
            function preventDefaults(e) {
                e.preventDefault();
                e.stopPropagation();
            }
            
            function highlight(e) {
                dropZone.classList.add('drag-over');
            }
            
            function unhighlight(e) {
                dropZone.classList.remove('drag-over');
            }
            
            function handleDrop(e) {
                const dt = e.dataTransfer;
                const files = dt.files;
                handleFiles(files);
            }
            
            function handleFileSelect(e) {
                const files = e.target.files;
                handleFiles(files);
            }
            
            function handleFiles(files) {
                selectedFiles = Array.from(files);
                updateFileList();
            }
        }

        function updateFileList() {
            const dropZone = document.getElementById('dropZone');
            if (selectedFiles.length > 0) {
                const fileNames = selectedFiles.map(f => f.name).join(', ');
                dropZone.innerHTML = `
                    <p>📄 Selected ${selectedFiles.length} file(s)</p>
                    <p style="font-size: 0.875rem; color: #94a3b8;">${fileNames}</p>
                    <label for="fileInput" class="file-input-label">Choose Different Files</label>
                    <input type="file" id="fileInput" accept="*/*" multiple style="display: none;">
                `;
                // Re-attach event listener
                document.getElementById('fileInput').addEventListener('change', function(e) {
                    selectedFiles = Array.from(e.target.files);
                    updateFileList();
                });
            }
        }

        // Update category filter buttons
        function updateCategoryFilters() {
            const container = document.getElementById('categoryFilters');
            container.innerHTML = '';
            
            categories.forEach(cat => {
                const btn = document.createElement('button');
                btn.className = 'filter-btn';
                btn.textContent = cat.name;
                btn.onclick = () => setFilter(cat.id);
                btn.style.setProperty('--category-color', cat.color);
                container.appendChild(btn);
            });
        }

        // Update category select dropdown
        function updateCategorySelect() {
            const select = document.getElementById('policyCategory');
            select.innerHTML = '';
            
            categories.forEach(cat => {
                const option = document.createElement('option');
                option.value = cat.id;
                option.textContent = cat.name;
                select.appendChild(option);
            });
        }

        // Get category by ID
        function getCategoryById(id) {
            return categories.find(cat => cat.id === id) || categories[0];
        }

        // Render policies
        function renderPolicies() {
            const grid = document.getElementById('policyGrid');
            const emptyState = document.getElementById('emptyState');
            const searchTerm = document.getElementById('searchInput').value.toLowerCase();
            
            let filteredPolicies = policies.filter(policy => {
                const matchesFilter = currentFilter === 'all' || policy.category === currentFilter;
                const matchesSearch = policy.title.toLowerCase().includes(searchTerm) || 
                                    policy.description.toLowerCase().includes(searchTerm);
                return matchesFilter && matchesSearch;
            });

            grid.innerHTML = '';
            
            if (filteredPolicies.length === 0) {
                emptyState.style.display = 'block';
            } else {
                emptyState.style.display = 'none';
                filteredPolicies.forEach(policy => {
                    const card = createPolicyCard(policy);
                    grid.appendChild(card);
                });
            }
        }

        // Create policy card element
        function createPolicyCard(policy) {
            const card = document.createElement('div');
            card.className = 'policy-card';
            
            const category = getCategoryById(policy.category);
            
            // Generate links HTML
            let linksHtml = '';
            if (policy.links && policy.links.length > 0) {
                linksHtml = `
                    <div class="policy-links-section">
                        <div class="policy-links-title">Related Links</div>
                        <div style="display: flex; flex-wrap: wrap;">
                            ${policy.links.map(link => `
                                <a href="${link.url}" target="_blank" class="policy-link-badge" title="${link.url}">
                                    <span>${link.icon}</span>
                                    <span>${link.title}</span>
                                </a>
                            `).join('')}
                        </div>
                    </div>
                `;
            }
            
            card.innerHTML = `
                <button class="policy-delete-btn" onclick="deletePolicy(event, ${policy.id})" title="Delete policy">×</button>
                <span class="policy-category" style="background: ${category.color}20; color: ${category.color};">${category.name}</span>
                <h3 class="policy-title">${policy.title}</h3>
                <p class="policy-description">${policy.description}</p>
                ${linksHtml}
                <div class="policy-meta">
                    <span>${new Date(policy.date).toLocaleDateString()}</span>
                    <div class="policy-status">
                        <span class="status-dot"></span>
                        <span>${policy.status}</span>
                    </div>
                </div>
            `;
            
            // Add click event to the card, but not the delete button
            card.onclick = (e) => {
                if (!e.target.classList.contains('policy-delete-btn')) {
                    viewPolicy(policy.id);
                }
            };
            
            return card;
        }

        // Delete policy function
        function deletePolicy(event, id) {
            event.stopPropagation(); // Prevent card click event
            
            const policy = policies.find(p => p.id === id);
            if (policy && confirm(`Are you sure you want to delete the policy "${policy.title}"?`)) {
                policies = policies.filter(p => p.id !== id);
                savePolicies();
                renderPolicies();
            }
        }

        // Filter policies
        function setFilter(filter) {
            currentFilter = filter;
            
            // Update active button
            document.querySelectorAll('.filter-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            event.target.classList.add('active');
            
            renderPolicies();
        }

        // Search policies
        function filterPolicies() {
            renderPolicies();
        }

        // Modal functions
        function openModal() {
            document.getElementById('policyModal').style.display = 'flex';
            document.getElementById('modalTitle').textContent = 'Add New Policy';
            document.getElementById('policyForm').reset();
            document.getElementById('deletePolicyBtn').style.display = 'none';
            editingPolicyId = null;
            policyLinks = [];
            updateLinksList();
            updateDocumentSelect();
        }

        function closeModal() {
            document.getElementById('policyModal').style.display = 'none';
            editingPolicyId = null;
            policyLinks = [];
        }

        function viewPolicy(id) {
            const policy = policies.find(p => p.id === id);
            if (policy) {
                editingPolicyId = id;
                document.getElementById('modalTitle').textContent = 'Edit Policy';
                document.getElementById('policyTitle').value = policy.title;
                document.getElementById('policyCategory').value = policy.category;
                document.getElementById('policyDescription').value = policy.description;
                document.getElementById('policyDetails').value = policy.details || '';
                document.getElementById('deletePolicyBtn').style.display = 'inline-block';
                
                // Load existing links
                policyLinks = policy.links ? [...policy.links] : [];
                updateLinksList();
                updateDocumentSelect();
                
                document.getElementById('policyModal').style.display = 'flex';
            }
        }

        function deletePolicyFromModal() {
            if (editingPolicyId) {
                const policy = policies.find(p => p.id === editingPolicyId);
                if (policy && confirm(`Are you sure you want to delete the policy "${policy.title}"?`)) {
                    policies = policies.filter(p => p.id !== editingPolicyId);
                    savePolicies();
                    renderPolicies();
                    closeModal();
                }
            }
        }

        function openCategoryModal() {
            document.getElementById('categoryModal').style.display = 'flex';
            updateCategoryList();
        }

        function closeCategoryModal() {
            document.getElementById('categoryModal').style.display = 'none';
        }

        function openVaultModal() {
            document.getElementById('vaultModal').style.display = 'flex';
            updateDocumentCategorySelect();
            updateDocumentCategoryList();
            updateDocumentCategoryFilters();
            renderDocuments();
            updateDocumentCount();
        }

        function closeVaultModal() {
            document.getElementById('vaultModal').style.display = 'none';
            selectedFiles = [];
            resetUploadArea();
        }

        function resetUploadArea() {
            const dropZone = document.getElementById('dropZone');
            dropZone.innerHTML = `
                <p>📤 Drag and drop files here or click to browse</p>
                <label for="fileInput" class="file-input-label">Choose Files</label>
                <input type="file" id="fileInput" accept="*/*" multiple style="display: none;">
            `;
            setupDragAndDrop();
            document.getElementById('uploadProgress').style.display = 'none';
        }

        // Category management
        function addCategory() {
            const name = document.getElementById('newCategoryName').value.trim();
            const color = document.getElementById('newCategoryColor').value;
            
            if (name) {
                const id = name.toLowerCase().replace(/\s+/g, '-');
                
                // Check if category already exists
                if (!categories.find(cat => cat.id === id)) {
                    categories.push({ id, name, color });
                    saveCategories();
                    document.getElementById('newCategoryName').value = '';
                    updateCategoryFilters();
                    updateCategorySelect();
                    updateCategoryList();
                } else {
                    alert('Category already exists!');
                }
            }
        }

        function updateCategoryList() {
            const list = document.getElementById('categoryList');
            list.innerHTML = '';
            
            categories.forEach(cat => {
                const item = document.createElement('div');
                item.style.cssText = 'display: flex; align-items: center; gap: 1rem; padding: 0.75rem; background: rgba(30, 41, 59, 0.5); border-radius: 8px;';
                
                item.innerHTML = `
                    <input type="color" value="${cat.color}" onchange="updateCategoryColor('${cat.id}', this.value)" style="width: 40px; height: 40px; cursor: pointer; border: none; border-radius: 4px;">
                    <span style="flex: 1;">${cat.name}</span>
                    <button class="btn btn-secondary" style="padding: 0.5rem 1rem;" onclick="deleteCategory('${cat.id}')">Delete</button>
                `;
                
                list.appendChild(item);
            });
        }

        function updateCategoryColor(id, color) {
            const category = categories.find(cat => cat.id === id);
            if (category) {
                category.color = color;
                saveCategories();
                renderPolicies();
            }
        }

        function deleteCategory(id) {
            // Check if category is in use
            const inUse = policies.some(policy => policy.category === id);
            
            if (inUse) {
                alert('Cannot delete this category as it is being used by existing policies.');
                return;
            }
            
            if (confirm('Are you sure you want to delete this category?')) {
                categories = categories.filter(cat => cat.id !== id);
                saveCategories();
                updateCategoryFilters();
                updateCategorySelect();
                updateCategoryList();
            }
        }

        // Document Vault Functions
        function updateDocumentCategorySelect() {
            const select = document.getElementById('fileCategory');
            select.innerHTML = '<option value="">Select Category...</option>';
            
            documentCategories.forEach(cat => {
                const option = document.createElement('option');
                option.value = cat.id;
                option.textContent = cat.name;
                select.appendChild(option);
            });
        }

        function updateDocumentCategoryList() {
            const list = document.getElementById('docCategoryList');
            list.innerHTML = '';
            
            documentCategories.forEach(cat => {
                const tag = document.createElement('div');
                tag.className = 'doc-category-tag';
                tag.style.backgroundColor = cat.color + '20';
                tag.style.borderColor = cat.color + '60';
                tag.style.color = cat.color;
                
                tag.innerHTML = `
                    <span>${cat.name}</span>
                    <button onclick="deleteDocumentCategory('${cat.id}')" title="Delete category">×</button>
                `;
                
                list.appendChild(tag);
            });
        }

        function updateDocumentCategoryFilters() {
            const container = document.getElementById('docCategoryFilters');
            container.innerHTML = '';
            
            documentCategories.forEach(cat => {
                const btn = document.createElement('button');
                btn.className = 'filter-btn';
                btn.textContent = cat.name;
                btn.onclick = (e) => filterDocuments(cat.id, e);
                btn.style.setProperty('--category-color', cat.color);
                container.appendChild(btn);
            });
        }

        function updateDocumentCount() {
            document.getElementById('documentCount').textContent = documents.length;
        }

        function addDocumentCategory() {
            const name = document.getElementById('newDocCategoryName').value.trim();
            const color = document.getElementById('newDocCategoryColor').value;
            
            if (name) {
                const id = name.toLowerCase().replace(/\s+/g, '-');
                
                if (!documentCategories.find(cat => cat.id === id)) {
                    documentCategories.push({ id, name, color });
                    saveDocumentCategories();
                    document.getElementById('newDocCategoryName').value = '';
                    updateDocumentCategorySelect();
                    updateDocumentCategoryList();
                    updateDocumentCategoryFilters();
                } else {
                    alert('Document category already exists!');
                }
            }
        }

        function deleteDocumentCategory(id) {
            const inUse = documents.some(doc => doc.category === id);
            
            if (inUse) {
                alert('Cannot delete this category as it contains documents.');
                return;
            }
            
            if (confirm('Are you sure you want to delete this category?')) {
                documentCategories = documentCategories.filter(cat => cat.id !== id);
                saveDocumentCategories();
                updateDocumentCategorySelect();
                updateDocumentCategoryList();
                updateDocumentCategoryFilters();
                
                // Reset filter if we were filtering by this category
                if (currentDocFilter === id) {
                    currentDocFilter = 'all';
                    renderDocuments();
                }
            }
        }

        function getDocumentCategoryById(id) {
            return documentCategories.find(cat => cat.id === id);
        }

        function getFileIcon(filename) {
            const ext = filename.split('.').pop().toLowerCase();
            const icons = {
                pdf: '📄',
                doc: '📝',
                docx: '📝',
                xls: '📊',
                xlsx: '📊',
                ppt: '📽️',
                pptx: '📽️',
                html: '🌐',
                htm: '🌐',
                jpg: '🖼️',
                jpeg: '🖼️',
                png: '🖼️',
                gif: '🖼️',
                svg: '🖼️',
                mp4: '🎥',
                mp3: '🎵',
                zip: '📦',
                rar: '📦',
                txt: '📋',
                csv: '📊',
                json: '{}',
                xml: '</>',
                js: '📜',
                css: '🎨'
            };
            return icons[ext] || '📎';
        }

        function getFileType(filename) {
            const ext = filename.split('.').pop().toLowerCase();
            if (['pdf'].includes(ext)) return 'pdf';
            if (['doc', 'docx', 'txt', 'rtf'].includes(ext)) return 'doc';
            if (['html', 'htm'].includes(ext)) return 'html';
            if (['jpg', 'jpeg', 'png', 'gif', 'svg', 'bmp'].includes(ext)) return 'image';
            return 'other';
        }

        function formatFileSize(bytes) {
            if (bytes === 0) return '0 Bytes';
            const k = 1024;
            const sizes = ['Bytes', 'KB', 'MB', 'GB'];
            const i = Math.floor(Math.log(bytes) / Math.log(k));
            return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + ' ' + sizes[i];
        }

        async function uploadDocuments() {
            const categorySelect = document.getElementById('fileCategory');
            const categoryId = categorySelect.value;
            
            if (selectedFiles.length === 0) {
                alert('Please select files to upload.');
                return;
            }
            
            if (!categoryId) {
                alert('Please select a category for the documents.');
                return;
            }
            
            const uploadProgress = document.getElementById('uploadProgress');
            const progressFill = document.getElementById('progressFill');
            const uploadStatus = document.getElementById('uploadStatus');
            
            uploadProgress.style.display = 'block';
            
            let uploadedCount = 0;
            const totalFiles = selectedFiles.length;
            
            for (const file of selectedFiles) {
                // Check file size (50MB limit)
                if (file.size > 50 * 1024 * 1024) {
                    alert(`File "${file.name}" exceeds 50MB limit and will be skipped.`);
                    continue;
                }
                
                uploadStatus.textContent = `Uploading ${uploadedCount + 1} of ${totalFiles}: ${file.name}`;
                
                try {
                    const content = await readFileAsBase64(file);
                    
                    const doc = {
                        id: Date.now() + Math.random(), // Ensure unique ID
                        name: file.name,
                        size: file.size,
                        type: getFileType(file.name),
                        category: categoryId,
                        uploadDate: new Date().toISOString(),
                        content: content
                    };
                    
                    documents.push(doc);
                    uploadedCount++;
                    
                    // Update progress
                    const progress = (uploadedCount / totalFiles) * 100;
                    progressFill.style.width = progress + '%';
                    
                } catch (error) {
                    console.error(`Error uploading ${file.name}:`, error);
                    alert(`Failed to upload ${file.name}`);
                }
            }
            
            saveDocuments();
            renderDocuments();
            updateDocumentCount();
            
            uploadStatus.textContent = `Upload complete! ${uploadedCount} of ${totalFiles} files uploaded.`;
            
            setTimeout(() => {
                selectedFiles = [];
                categorySelect.value = '';
                resetUploadArea();
            }, 2000);
        }

        function readFileAsBase64(file) {
            return new Promise((resolve, reject) => {
                const reader = new FileReader();
                reader.onload = (e) => resolve(e.target.result);
                reader.onerror = reject;
                reader.readAsDataURL(file);
            });
        }

        function filterDocuments(filter, event) {
            currentDocFilter = filter;
            
            // Update active button
            document.querySelectorAll('.vault-filters .filter-btn').forEach(btn => {
                btn.classList.remove('active');
            });
            
            if (event && event.target) {
                event.target.classList.add('active');
            } else {
                // For programmatic calls, find and activate the right button
                document.querySelector('.vault-filters .filter-btn').classList.add('active');
            }
            
            renderDocuments();
        }

        function renderDocuments() {
            const list = document.getElementById('documentList');
            list.innerHTML = '';
            
            let filteredDocs = documents.filter(doc => {
                if (currentDocFilter === 'all') return true;
                return doc.category === currentDocFilter;
            });
            
            if (filteredDocs.length === 0) {
                list.innerHTML = '<p style="text-align: center; color: #64748b; padding: 2rem;">No documents found in this category.</p>';
                return;
            }
            
            // Sort by upload date (newest first)
            filteredDocs.sort((a, b) => new Date(b.uploadDate) - new Date(a.uploadDate));
            
            filteredDocs.forEach(doc => {
                const item = document.createElement('div');
                item.className = 'document-item';
                
                const category = getDocumentCategoryById(doc.category);
                const categoryBadge = category ? 
                    `<span class="document-category-badge" style="background: ${category.color}20; color: ${category.color};">${category.name}</span>` : 
                    '';
                
                item.innerHTML = `
                    <div class="document-icon">${getFileIcon(doc.name)}</div>
                    <div class="document-info">
                        <div class="document-name">${doc.name}</div>
                        <div class="document-meta">
                            ${categoryBadge}
                            ${formatFileSize(doc.size)} • Uploaded ${new Date(doc.uploadDate).toLocaleDateString()}
                        </div>
                    </div>
                    <div class="document-actions">
                        <button class="doc-btn" onclick="viewDocument(${doc.id})">View</button>
                        <button class="doc-btn" onclick="downloadDocument(${doc.id})">Download</button>
                        <button class="doc-btn danger" onclick="deleteDocument(${doc.id})">Delete</button>
                    </div>
                `;
                
                list.appendChild(item);
            });
        }

        function viewDocument(id) {
            const doc = documents.find(d => d.id === id);
            if (doc) {
                const fileType = doc.name.split('.').pop().toLowerCase();
                
                if (fileType === 'html' || fileType === 'htm') {
                    // For HTML files, extract the base64 content and decode it
                    const base64Content = doc.content.split(',')[1];
                    const decodedContent = atob(base64Content);
                    
                    // Open in new window and write the HTML content
                    const win = window.open('', '_blank');
                    win.document.open();
                    win.document.write(decodedContent);
                    win.document.close();
                } else if (['jpg', 'jpeg', 'png', 'gif', 'svg', 'bmp'].includes(fileType)) {
                    // For images, open in new window with img tag
                    const win = window.open('', '_blank');
                    win.document.write(`
                        <html>
                        <head><title>${doc.name}</title></head>
                        <body style="margin: 0; display: flex; justify-content: center; align-items: center; min-height: 100vh; background: #000;">
                            <img src="${doc.content}" style="max-width: 100%; max-height: 100vh; object-fit: contain;">
                        </body>
                        </html>
                    `);
                } else if (fileType === 'txt') {
                    // For text files, decode and display
                    const base64Content = doc.content.split(',')[1];
                    const decodedContent = atob(base64Content);
                    const win = window.open('', '_blank');
                    win.document.write(`
                        <html>
                        <head><title>${doc.name}</title></head>
                        <body style="font-family: monospace; white-space: pre-wrap; padding: 20px;">
                            ${decodedContent.replace(/</g, '&lt;').replace(/>/g, '&gt;')}
                        </body>
                        </html>
                    `);
                } else {
                    // For other files, open in new tab
                    window.open(doc.content, '_blank');
                }
            }
        }

        function downloadDocument(id) {
            const doc = documents.find(d => d.id === id);
            if (doc) {
                const a = document.createElement('a');
                a.href = doc.content;
                a.download = doc.name;
                document.body.appendChild(a);
                a.click();
                document.body.removeChild(a);
            }
        }

        function deleteDocument(id) {
            const doc = documents.find(d => d.id === id);
            if (doc && confirm(`Are you sure you want to delete "${doc.name}"?`)) {
                documents = documents.filter(d => d.id !== id);
                saveDocuments();
                renderDocuments();
                updateDocumentCount();
            }
        }

        // Link Management Functions
        function updateDocumentSelect() {
            const select = document.getElementById('documentSelect');
            select.innerHTML = '<option value="">Select a document from vault...</option>';
            
            // Group documents by category
            const docsByCategory = {};
            documents.forEach(doc => {
                if (!docsByCategory[doc.category]) {
                    docsByCategory[doc.category] = [];
                }
                docsByCategory[doc.category].push(doc);
            });
            
            // Add documents grouped by category
            Object.keys(docsByCategory).forEach(catId => {
                const category = getDocumentCategoryById(catId);
                if (category && docsByCategory[catId].length > 0) {
                    const optgroup = document.createElement('optgroup');
                    optgroup.label = category.name;
                    
                    docsByCategory[catId].forEach(doc => {
                        const option = document.createElement('option');
                        option.value = doc.id;
                        option.textContent = doc.name;
                        option.dataset.docName = doc.name;
                        option.dataset.docType = doc.type;
                        optgroup.appendChild(option);
                    });
                    
                    select.appendChild(optgroup);
                }
            });
        }

        function addLink() {
            const titleInput = document.getElementById('linkTitle');
            const urlInput = document.getElementById('linkUrl');
            
            const title = titleInput.value.trim();
            const url = urlInput.value.trim();
            
            if (!title || !url) {
                alert('Please enter both title and URL');
                return;
            }
            
            // Allow http(s), file://, data:, mailto:, tel:, and relative paths
            (function() {
                const ok = /^(https?:\/\/|file:\/\/|data:|mailto:|tel:)/i.test(url)
                         || url.startsWith('/') || url.startsWith('./') || url.startsWith('../')
                         || /^[\w\-./#?%=&]+$/.test(url);
                if (!ok) {
                    alert('Please enter a URL or path (http(s), file://, or a relative path like /docs/page.html).');
                    return;
                }
            })();

            
            policyLinks.push({
                id: Date.now(),
                title: title,
                url: url,
                type: 'external',
                icon: '🔗'
            });
            
            titleInput.value = '';
            urlInput.value = '';
            updateLinksList();
        }

        function addDocumentLink() {
            const select = document.getElementById('documentSelect');
            const selectedOption = select.options[select.selectedIndex];
            
            if (!select.value) {
                alert('Please select a document');
                return;
            }
            
            const doc = documents.find(d => d.id == select.value);
            if (!doc) return;
            
            // Check if already added
            if (policyLinks.find(link => link.docId === doc.id)) {
                alert('This document is already linked to this policy');
                return;
            }
            
            policyLinks.push({
                id: Date.now(),
                title: doc.name,
                url: '#', // Will be handled by viewLinkedDocument
                type: 'document',
                docId: doc.id,
                icon: getFileIcon(doc.name)
            });
            
            select.value = '';
            updateLinksList();
        }

        function removeLink(linkId) {
            policyLinks = policyLinks.filter(link => link.id !== linkId);
            updateLinksList();
        }

        function updateLinksList() {
            const container = document.getElementById('linksList');
            container.innerHTML = '';
            
            if (policyLinks.length === 0) {
                container.innerHTML = '<p style="color: #64748b; text-align: center; padding: 1rem;">No links added yet</p>';
                return;
            }
            
            policyLinks.forEach(link => {
                const item = document.createElement('div');
                item.className = 'link-item';
                
                if (link.type === 'external') {
                    item.innerHTML = `
                        <span class="link-icon">${link.icon}</span>
                        <div class="link-info">
                            <a href="${link.url}" target="_blank" class="link-title">${link.title}</a>
                            <div class="link-url">${link.url}</div>
                        </div>
                        <button class="link-remove" onclick="removeLink(${link.id})">×</button>
                    `;
                } else {
                    item.innerHTML = `
                        <span class="link-icon">${link.icon}</span>
                        <div class="link-info">
                            <a href="javascript:void(0)" onclick="viewLinkedDocument(${link.docId})" class="link-title">${link.title}</a>
                            <div class="link-url">Document from vault</div>
                        </div>
                        <button class="link-remove" onclick="removeLink(${link.id})">×</button>
                    `;
                }
                
                container.appendChild(item);
            });
        }

        function viewLinkedDocument(docId) {
            // Find the document and view it
            viewDocument(docId);
        }

        // Update form submission to include links
        document.getElementById('policyForm').addEventListener('submit', function(e) {
            e.preventDefault();
            
            const formData = {
                title: document.getElementById('policyTitle').value,
                category: document.getElementById('policyCategory').value,
                description: document.getElementById('policyDescription').value,
                details: document.getElementById('policyDetails').value,
                date: new Date().toISOString().split('T')[0],
                status: 'Active',
                links: policyLinks
            };
            
            if (editingPolicyId) {
                // Update existing policy
                const index = policies.findIndex(p => p.id === editingPolicyId);
                policies[index] = { ...policies[index], ...formData };
            } else {
                // Add new policy
                formData.id = Date.now();
                policies.push(formData);
            }
            
            savePolicies();
            renderPolicies();
            closeModal();
        });

        // Close modal when clicking outside
        document.getElementById('policyModal').addEventListener('click', function(e) {
            if (e.target === this) {
                closeModal();
            }
        });

        document.getElementById('categoryModal').addEventListener('click', function(e) {
            if (e.target === this) {
                closeCategoryModal();
            }
        });

        document.getElementById('vaultModal').addEventListener('click', function(e) {
            if (e.target === this) {
                closeVaultModal();
            }
        });

        // Handle enter key for adding categories
        document.getElementById('newCategoryName').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                addCategory();
            }
        });

        document.getElementById('newDocCategoryName').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                addDocumentCategory();
            }
        });

        // Initialize on load
        init();
    </script>
</body>
</html>
</body>
</html>
