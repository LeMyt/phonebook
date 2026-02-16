<!DOCTYPE html>
<html lang="ru">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <meta name="theme-color" content="#1a1a2e">
    <meta name="apple-mobile-web-app-capable" content="yes">
    <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
    <title>Телефонный справочник</title>
    <link rel="manifest" href="data:application/json;base64,eyJuYW1lIjoi0KLQtdGB0YLQvtCy0YvQuSDRgdGA0L7QutC40Y8iLCJzaG9ydF9uYW1lIjoi0JzQsNGI0LjQvdCwIiwic3RhcnRfdXJsIjoiLiIsImRpc3BsYXkiOiJzdGFuZGFsb25lIiwiYmFja2dyb3VuZF9jb2xvciI6IiMxYTFhMmUiLCJ0aGVtZV9jb2xvciI6IiMxYTFhMmUiLCJpY29ucyI6W3sic3JjIjoiaHR0cHM6Ly9jZG4uanNkZWxpdnIubmV0L2doL3R3ZW1vamkvdHdlbW9qaUAxNS4wLjMvYXNzZXRzL3N2Zy8xZjRkZC5zdmciLCJzaXplcyI6IjE5MngxOTIiLCJ0eXBlIjoiaW1hZ2Uvc3ZnIn1dfQ==">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            -webkit-tap-highlight-color: transparent;
        }

        body {
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, sans-serif;
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
            min-height: 100vh;
            color: #eaeaea;
            overflow-x: hidden;
        }

        .hidden { display: none !important; }

        /* Auth Screen */
        .auth-screen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 20px;
            z-index: 1000;
        }

        .auth-logo {
            width: 120px;
            height: 120px;
            background: rgba(255,255,255,0.1);
            border-radius: 30px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 60px;
            margin-bottom: 30px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255,255,255,0.2);
            animation: float 3s ease-in-out infinite;
        }

        @keyframes float {
            0%, 100% { transform: translateY(0px); }
            50% { transform: translateY(-10px); }
        }

        .auth-title {
            font-size: 24px;
            font-weight: 600;
            margin-bottom: 10px;
            text-align: center;
        }

        .auth-subtitle {
            font-size: 14px;
            color: #a0a0a0;
            margin-bottom: 40px;
            text-align: center;
        }

        .auth-input {
            width: 100%;
            max-width: 320px;
            padding: 16px 20px;
            border: none;
            border-radius: 12px;
            background: rgba(255,255,255,0.1);
            color: #fff;
            font-size: 16px;
            margin-bottom: 16px;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255,255,255,0.1);
            transition: all 0.3s;
        }

        .auth-input:focus {
            outline: none;
            border-color: #e94560;
            background: rgba(255,255,255,0.15);
        }

        .auth-btn {
            width: 100%;
            max-width: 320px;
            padding: 16px;
            border: none;
            border-radius: 12px;
            background: linear-gradient(135deg, #e94560 0%, #ff6b6b 100%);
            color: #fff;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: transform 0.2s, box-shadow 0.2s;
            margin-top: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }

        .auth-btn:active {
            transform: scale(0.98);
        }

        .auth-btn:disabled {
            opacity: 0.6;
            cursor: not-allowed;
        }

        .auth-btn.secondary {
            background: rgba(255,255,255,0.1);
            color: #fff;
            margin-top: 10px;
        }

        .auth-status {
            margin-top: 20px;
            padding: 12px 20px;
            border-radius: 8px;
            font-size: 14px;
            max-width: 320px;
            text-align: center;
            display: none;
        }

        .auth-status.error {
            background: rgba(233, 69, 96, 0.2);
            color: #ff6b6b;
            display: block;
        }

        .auth-status.success {
            background: rgba(76, 175, 80, 0.2);
            color: #4caf50;
            display: block;
        }

        .auth-status.info {
            background: rgba(33, 150, 243, 0.2);
            color: #2196f3;
            display: block;
        }

        .auth-status.warning {
            background: rgba(255, 152, 0, 0.2);
            color: #ff9800;
            display: block;
        }

        .settings-link {
            margin-top: 30px;
            color: #666;
            font-size: 14px;
            cursor: pointer;
            text-decoration: underline;
        }

        .warning-box {
            background: rgba(255, 152, 0, 0.1);
            border: 1px solid rgba(255, 152, 0, 0.3);
            border-radius: 12px;
            padding: 16px;
            margin: 20px 0;
            max-width: 320px;
            font-size: 13px;
            color: #ff9800;
            line-height: 1.5;
        }

        /* Settings Modal */
        .settings-modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.95);
            z-index: 2000;
            display: none;
            align-items: center;
            justify-content: center;
            padding: 20px;
            overflow-y: auto;
        }

        .settings-modal.active {
            display: flex;
        }

        .settings-content {
            background: linear-gradient(180deg, #1a1a2e 0%, #16213e 100%);
            width: 100%;
            max-width: 450px;
            border-radius: 20px;
            padding: 30px;
            max-height: 90vh;
            overflow-y: auto;
        }

        .settings-title {
            font-size: 20px;
            margin-bottom: 20px;
            color: #e94560;
        }

        .settings-field {
            margin-bottom: 20px;
        }

        .settings-label {
            display: block;
            font-size: 12px;
            color: #a0a0a0;
            margin-bottom: 8px;
            text-transform: uppercase;
        }

        .settings-input {
            width: 100%;
            padding: 12px;
            border: 1px solid rgba(255,255,255,0.1);
            border-radius: 8px;
            background: rgba(255,255,255,0.05);
            color: #fff;
            font-size: 14px;
            font-family: monospace;
        }

        .settings-hint {
            font-size: 12px;
            color: #666;
            margin-top: 5px;
            line-height: 1.4;
        }

        .method-selector {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }

        .method-btn {
            flex: 1;
            min-width: 100px;
            padding: 12px;
            border: 1px solid rgba(255,255,255,0.2);
            background: rgba(255,255,255,0.05);
            color: #fff;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.2s;
            font-size: 13px;
        }

        .method-btn.active {
            background: #e94560;
            border-color: #e94560;
        }

        .info-box {
            background: rgba(76, 175, 80, 0.1);
            border: 1px solid rgba(76, 175, 80, 0.3);
            border-radius: 8px;
            padding: 12px;
            margin: 15px 0;
            font-size: 12px;
            color: #81c784;
            line-height: 1.5;
        }

        .danger-box {
            background: rgba(244, 67, 54, 0.1);
            border: 1px solid rgba(244, 67, 54, 0.3);
            border-radius: 8px;
            padding: 12px;
            margin: 15px 0;
            font-size: 12px;
            color: #e57373;
            line-height: 1.5;
        }

        .test-btn {
            background: rgba(33, 150, 243, 0.2);
            border: 1px solid rgba(33, 150, 243, 0.3);
            color: #64b5f6;
            padding: 10px;
            border-radius: 8px;
            cursor: pointer;
            font-size: 13px;
            margin-top: 10px;
            width: 100%;
        }

        /* Debug Console */
        .debug-console {
            background: rgba(0,0,0,0.8);
            border: 1px solid rgba(255,255,255,0.1);
            border-radius: 8px;
            padding: 10px;
            margin-top: 15px;
            font-family: monospace;
            font-size: 11px;
            color: #aaa;
            max-height: 150px;
            overflow-y: auto;
            display: none;
        }

        .debug-console.active {
            display: block;
        }

        .debug-line {
            margin-bottom: 4px;
            border-bottom: 1px solid rgba(255,255,255,0.05);
            padding-bottom: 2px;
        }

        .debug-error { color: #ff6b6b; }
        .debug-success { color: #4caf50; }
        .debug-info { color: #64b5f6; }

        /* Main App */
        .app-container {
            display: none;
            min-height: 100vh;
            padding-bottom: 80px;
        }

        .app-container.active {
            display: block;
        }

        /* Header */
        .header {
            position: sticky;
            top: 0;
            background: rgba(26, 26, 46, 0.95);
            backdrop-filter: blur(20px);
            padding: 15px 20px;
            border-bottom: 1px solid rgba(255,255,255,0.1);
            z-index: 100;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .header-title {
            font-size: 20px;
            font-weight: 600;
        }

        .menu-btn {
            width: 40px;
            height: 40px;
            border-radius: 10px;
            background: rgba(255,255,255,0.1);
            border: none;
            color: #fff;
            font-size: 20px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: background 0.2s;
        }

        .menu-btn:active {
            background: rgba(255,255,255,0.2);
        }

        /* Search */
        .search-container {
            padding: 15px 20px;
            background: rgba(26, 26, 46, 0.8);
        }

        .search-input {
            width: 100%;
            padding: 12px 20px;
            border: none;
            border-radius: 25px;
            background: rgba(255,255,255,0.1);
            color: #fff;
            font-size: 16px;
            backdrop-filter: blur(10px);
        }

        .search-input:focus {
            outline: none;
            background: rgba(255,255,255,0.15);
        }

        /* Content */
        .content {
            padding: 20px;
        }

        .section-title {
            font-size: 14px;
            text-transform: uppercase;
            letter-spacing: 1px;
            color: #a0a0a0;
            margin-bottom: 15px;
            font-weight: 600;
        }

        /* Contact Card */
        .contact-card {
            background: rgba(255,255,255,0.05);
            border-radius: 16px;
            padding: 16px;
            margin-bottom: 12px;
            display: flex;
            gap: 15px;
            align-items: center;
            cursor: pointer;
            transition: all 0.3s;
            border: 1px solid rgba(255,255,255,0.05);
            position: relative;
            overflow: hidden;
        }

        .contact-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255,255,255,0.05), transparent);
            transform: translateX(-100%);
            transition: transform 0.5s;
        }

        .contact-card:hover::before {
            transform: translateX(100%);
        }

        .contact-card:active {
            transform: scale(0.98);
            background: rgba(255,255,255,0.1);
        }

        .contact-photo {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            object-fit: cover;
            border: 2px solid rgba(233, 69, 96, 0.5);
            background: rgba(255,255,255,0.1);
        }

        .contact-info {
            flex: 1;
        }

        .contact-name {
            font-size: 16px;
            font-weight: 600;
            margin-bottom: 4px;
        }

        .contact-position {
            font-size: 13px;
            color: #a0a0a0;
            margin-bottom: 4px;
        }

        .contact-phone {
            font-size: 14px;
            color: #e94560;
            font-weight: 500;
        }

        .birthday-badge {
            position: absolute;
            top: 10px;
            right: 10px;
            background: linear-gradient(135deg, #e94560, #ff6b6b);
            color: #fff;
            padding: 4px 10px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 600;
        }

        /* Bottom Navigation */
        .bottom-nav {
            position: fixed;
            bottom: 0;
            left: 0;
            right: 0;
            background: rgba(26, 26, 46, 0.98);
            backdrop-filter: blur(20px);
            border-top: 1px solid rgba(255,255,255,0.1);
            display: flex;
            justify-content: space-around;
            padding: 10px 0;
            z-index: 100;
        }

        .nav-item {
            flex: 1;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 4px;
            padding: 8px;
            cursor: pointer;
            transition: all 0.3s;
            border: none;
            background: none;
            color: #a0a0a0;
        }

        .nav-item.active {
            color: #e94560;
        }

        .nav-item:active {
            transform: scale(0.9);
        }

        .nav-icon {
            font-size: 24px;
        }

        .nav-label {
            font-size: 11px;
            font-weight: 500;
        }

        /* Modal */
        .modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.8);
            backdrop-filter: blur(5px);
            z-index: 1000;
            display: none;
            align-items: flex-end;
            justify-content: center;
        }

        .modal.active {
            display: flex;
        }

        .modal-content {
            background: linear-gradient(180deg, #1a1a2e 0%, #16213e 100%);
            width: 100%;
            max-height: 90vh;
            border-radius: 24px 24px 0 0;
            overflow: hidden;
            animation: slideUp 0.3s ease-out;
            display: flex;
            flex-direction: column;
        }

        @keyframes slideUp {
            from { transform: translateY(100%); }
            to { transform: translateY(0); }
        }

        .modal-header {
            padding: 20px;
            border-bottom: 1px solid rgba(255,255,255,0.1);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .modal-title {
            font-size: 18px;
            font-weight: 600;
        }

        .modal-close {
            width: 36px;
            height: 36px;
            border-radius: 50%;
            background: rgba(255,255,255,0.1);
            border: none;
            color: #fff;
            font-size: 20px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .modal-body {
            padding: 20px;
            overflow-y: auto;
            flex: 1;
        }

        .contact-detail-photo {
            width: 120px;
            height: 120px;
            border-radius: 50%;
            object-fit: cover;
            border: 3px solid #e94560;
            margin: 0 auto 20px;
            display: block;
            background: rgba(255,255,255,0.1);
        }

        .detail-field {
            background: rgba(255,255,255,0.05);
            border-radius: 12px;
            padding: 16px;
            margin-bottom: 12px;
            border: 1px solid rgba(255,255,255,0.05);
        }

        .detail-label {
            font-size: 12px;
            color: #a0a0a0;
            text-transform: uppercase;
            letter-spacing: 0.5px;
            margin-bottom: 4px;
        }

        .detail-value {
            font-size: 16px;
            color: #fff;
            font-weight: 500;
        }

        .detail-input {
            width: 100%;
            background: transparent;
            border: none;
            color: #fff;
            font-size: 16px;
            font-family: inherit;
            padding: 0;
        }

        .detail-input:focus {
            outline: none;
        }

        .action-btn {
            width: 100%;
            padding: 16px;
            border: none;
            border-radius: 12px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            margin-top: 10px;
            transition: all 0.2s;
        }

        .action-btn.primary {
            background: linear-gradient(135deg, #e94560 0%, #ff6b6b 100%);
            color: #fff;
        }

        .action-btn.secondary {
            background: rgba(255,255,255,0.1);
            color: #fff;
        }

        .action-btn.danger {
            background: rgba(244, 67, 54, 0.2);
            color: #f44336;
            border: 1px solid rgba(244, 67, 54, 0.3);
        }

        .action-btn:active {
            transform: scale(0.98);
        }

        /* Admin Fields */
        .admin-section {
            margin-top: 20px;
            padding-top: 20px;
            border-top: 2px dashed rgba(233, 69, 96, 0.3);
        }

        .admin-badge {
            display: inline-block;
            background: linear-gradient(135deg, #e94560, #ff6b6b);
            color: #fff;
            padding: 4px 12px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 600;
            margin-bottom: 15px;
        }

        /* Menu Drawer */
        .menu-drawer {
            position: fixed;
            top: 0;
            right: -100%;
            width: 80%;
            max-width: 300px;
            height: 100%;
            background: linear-gradient(180deg, #1a1a2e 0%, #16213e 100%);
            z-index: 2000;
            transition: right 0.3s ease;
            box-shadow: -5px 0 30px rgba(0,0,0,0.5);
        }

        .menu-drawer.active {
            right: 0;
        }

        .menu-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0,0,0,0.5);
            z-index: 1999;
            display: none;
            opacity: 0;
            transition: opacity 0.3s;
        }

        .menu-overlay.active {
            display: block;
            opacity: 1;
        }

        .menu-header {
            padding: 40px 20px 20px;
            border-bottom: 1px solid rgba(255,255,255,0.1);
        }

        .menu-user {
            font-size: 18px;
            font-weight: 600;
            margin-bottom: 5px;
        }

        .menu-email {
            font-size: 14px;
            color: #a0a0a0;
        }

        .menu-items {
            padding: 20px 0;
        }

        .menu-item {
            padding: 16px 20px;
            display: flex;
            align-items: center;
            gap: 15px;
            cursor: pointer;
            transition: background 0.2s;
            border: none;
            background: none;
            color: #fff;
            width: 100%;
            text-align: left;
            font-size: 16px;
        }

        .menu-item:active {
            background: rgba(255,255,255,0.1);
        }

        .menu-item-icon {
            width: 40px;
            height: 40px;
            border-radius: 10px;
            background: rgba(255,255,255,0.1);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 20px;
        }

        .menu-item.active {
            background: rgba(233, 69, 96, 0.2);
            border-left: 3px solid #e94560;
        }

        /* Sync Status */
        .sync-status {
            position: fixed;
            top: 70px;
            right: 20px;
            padding: 8px 16px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 600;
            z-index: 99;
            display: flex;
            align-items: center;
            gap: 8px;
            transition: all 0.3s;
        }

        .sync-status.online {
            background: rgba(76, 175, 80, 0.2);
            color: #4caf50;
        }

        .sync-status.offline {
            background: rgba(255, 152, 0, 0.2);
            color: #ff9800;
        }

        .sync-status.syncing {
            background: rgba(33, 150, 243, 0.2);
            color: #2196f3;
        }

        .sync-status.error {
            background: rgba(244, 67, 54, 0.2);
            color: #f44336;
        }

        /* Loading */
        .loading {
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 40px;
        }

        .spinner {
            width: 40px;
            height: 40px;
            border: 3px solid rgba(255,255,255,0.1);
            border-top-color: #e94560;
            border-radius: 50%;
            animation: spin 1s linear infinite;
        }

        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        /* Empty State */
        .empty-state {
            text-align: center;
            padding: 60px 20px;
            color: #a0a0a0;
        }

        .empty-icon {
            font-size: 64px;
            margin-bottom: 20px;
            opacity: 0.5;
        }

        /* Add Button */
        .fab {
            position: fixed;
            bottom: 100px;
            right: 20px;
            width: 56px;
            height: 56px;
            border-radius: 50%;
            background: linear-gradient(135deg, #e94560 0%, #ff6b6b 100%);
            border: none;
            color: #fff;
            font-size: 24px;
            cursor: pointer;
            box-shadow: 0 4px 15px rgba(233, 69, 96, 0.4);
            display: none;
            align-items: center;
            justify-content: center;
            z-index: 50;
            transition: transform 0.2s;
        }

        .fab:active {
            transform: scale(0.9);
        }

        .fab.visible {
            display: flex;
        }

        /* Date inputs row */
        .date-row {
            display: grid;
            grid-template-columns: 1fr 1fr 1fr;
            gap: 10px;
            margin-bottom: 12px;
        }

        .date-input {
            background: rgba(255,255,255,0.1);
            border: 1px solid rgba(255,255,255,0.1);
            border-radius: 8px;
            padding: 12px;
            color: #fff;
            font-size: 16px;
            text-align: center;
        }

        .date-input:focus {
            outline: none;
            border-color: #e94560;
        }

        /* Pending approval */
        .pending-screen {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
            display: none;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            padding: 20px;
            z-index: 1001;
        }

        .pending-screen.active {
            display: flex;
        }

        .pending-icon {
            font-size: 80px;
            margin-bottom: 30px;
            animation: pulse 2s infinite;
        }

        @keyframes pulse {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.5; }
        }

        .pending-text {
            font-size: 20px;
            font-weight: 600;
            margin-bottom: 10px;
            text-align: center;
        }

        .pending-subtext {
            font-size: 14px;
            color: #a0a0a0;
            text-align: center;
            max-width: 300px;
        }

        .logout-btn {
            margin-top: 40px;
            padding: 12px 24px;
            background: rgba(255,255,255,0.1);
            border: 1px solid rgba(255,255,255,0.2);
            border-radius: 8px;
            color: #fff;
            cursor: pointer;
        }

        /* Last Sync Info */
        .last-sync {
            position: fixed;
            bottom: 80px;
            left: 20px;
            font-size: 11px;
            color: #666;
            background: rgba(0,0,0,0.5);
            padding: 4px 8px;
            border-radius: 4px;
        }

        /* Instructions */
        .instructions {
            background: rgba(255,255,255,0.05);
            border-radius: 12px;
            padding: 20px;
            margin: 20px 0;
            font-size: 13px;
            line-height: 1.6;
        }

        .instructions h4 {
            color: #e94560;
            margin-bottom: 10px;
        }

        .instructions ol {
            padding-left: 20px;
            color: #a0a0a0;
        }

        .instructions li {
            margin-bottom: 8px;
        }

        .instructions code {
            background: rgba(255,255,255,0.1);
            padding: 2px 6px;
            border-radius: 4px;
            font-family: monospace;
            color: #fff;
        }

        .url-fix-box {
            background: rgba(255, 193, 7, 0.1);
            border: 1px solid rgba(255, 193, 7, 0.3);
            border-radius: 8px;
            padding: 12px;
            margin: 10px 0;
            font-size: 12px;
            color: #ffc107;
        }
    </style>
</head>
<body>

    <!-- Auth Screen -->
    <div class="auth-screen" id="authScreen">
        <div class="auth-logo">📞</div>
        <h1 class="auth-title">Телефонный справочник</h1>
        <p class="auth-subtitle">Корпоративный справочник с синхронизацией</p>
        
        <button class="auth-btn" id="loginBtn" onclick="app.login()">
            <span>📧</span> Войти по Email
        </button>
        
        <div class="auth-status" id="authStatus"></div>
        
        <div class="warning-box" id="warningBox" style="display: none;">
            <strong>⚠️ Внимание:</strong> Для синхронизации с Google Sheets необходимо настроить подключение. Нажмите "Настройки" ниже.
        </div>

        <button class="auth-btn secondary" onclick="app.openSettings()">
            <span>⚙️</span> Настройки подключения
        </button>

        <div style="margin-top: 20px; font-size: 12px; color: #666; text-align: center; max-width: 320px;">
            <p>💡 Совет: Используйте метод "Публичный CSV"</p>
        </div>
    </div>

    <!-- Settings Modal -->
    <div class="settings-modal" id="settingsModal">
        <div class="settings-content">
            <h2 class="settings-title">Настройки подключения</h2>
            
            <div class="method-selector">
                <button class="method-btn active" onclick="app.selectMethod('csv')" id="btnCsv">📄 Публичный CSV</button>
                <button class="method-btn" onclick="app.selectMethod('manual')" id="btnManual">✋ Локальный режим</button>
            </div>

            <!-- CSV Method -->
            <div id="csvMethod">
                <div class="info-box">
                    <strong>✅ Рекомендуемый метод!</strong> Быстрая настройка без API ключей.
                </div>

                <div class="instructions">
                    <h4>📋 Пошаговая инструкция:</h4>
                    <ol>
                        <li>Откройте вашу Google Таблицу</li>
                        <li>Убедитесь, что первый лист называется <code>Contacts</code></li>
                        <li>Нажмите <code>Файл</code> → <code>Опубликовать в интернете</code></li>
                        <li>В разделе "Ссылка" выберите:
                            <br>• Лист: <code>Contacts</code>
                            <br>• Формат: <code>CSV</code>
                        </li>
                        <li>Нажмите <code>Опубликовать</code></li>
                        <li>Скопируйте ссылку и вставьте ниже</li>
                    </ol>
                </div>

                <div class="url-fix-box">
                    <strong>⚠️ Важно:</strong> Если ваша ссылка содержит <code>single=true</code> или <code>gid=</code>, удалите эти параметры или используйте кнопку "Исправить URL" ниже.
                </div>

                <div class="settings-field">
                    <label class="settings-label">URL публичного CSV</label>
                    <input type="text" class="settings-input" id="csvUrlInput" placeholder="https://docs.google.com/spreadsheets/d/e/2PACX-.../pub?output=csv">
                    <div class="settings-hint">Должен заканчиваться на /pub?output=csv</div>
                </div>

                <button class="test-btn" onclick="app.testConnection()">🧪 Проверить подключение</button>
                <button class="test-btn" onclick="app.fixUrl()" style="margin-top: 5px;">🔧 Исправить URL (убрать ограничения)</button>
                
                <div class="debug-console" id="debugConsole"></div>
            </div>

            <!-- Manual Method -->
            <div id="manualMethod" style="display: none;">
                <div class="info-box">
                    <strong>💾 Локальный режим:</strong> Данные хранятся только на устройстве.
                </div>

                <div class="settings-field">
                    <label class="settings-label">Экспорт/Импорт данных</label>
                    <button class="auth-btn secondary" onclick="app.exportData()" style="margin-bottom: 10px;">📤 Экспорт JSON</button>
                    <input type="file" id="importFile" accept=".json" onchange="app.importData(this)" style="display: none;">
                    <button class="auth-btn secondary" onclick="document.getElementById('importFile').click()">📥 Импорт JSON</button>
                </div>
            </div>

            <button class="auth-btn" onclick="app.saveSettings()" style="margin-top: 20px;">💾 Сохранить настройки</button>
            <button class="auth-btn secondary" onclick="app.closeSettings()" style="margin-top: 10px;">Отмена</button>
        </div>
    </div>

    <!-- Pending Approval Screen -->
    <div class="pending-screen" id="pendingScreen">
        <div class="pending-icon">⏳</div>
        <div class="pending-text">Ожидание подтверждения</div>
        <div class="pending-subtext">Администратор должен подтвердить ваш доступ к базе данных</div>
        <button class="logout-btn" onclick="app.logout()">Выйти</button>
    </div>

    <!-- Main App -->
    <div class="app-container" id="mainApp">
        <div class="sync-status offline" id="syncStatus">
            <span>●</span> <span id="syncText">Офлайн</span>
        </div>

        <header class="header">
            <h1 class="header-title" id="pageTitle">ФРЭС</h1>
            <button class="menu-btn" onclick="app.toggleMenu()">☰</button>
        </header>

        <div class="search-container">
            <input type="text" class="search-input" id="searchInput" placeholder="Поиск по ФИО, должности..." oninput="app.search()">
        </div>

        <div class="content" id="contentArea">
            <div class="loading">
                <div class="spinner"></div>
            </div>
        </div>

        <div class="last-sync" id="lastSync"></div>

        <button class="fab" id="addBtn" onclick="app.openAddModal()">+</button>

        <nav class="bottom-nav">
            <button class="nav-item active" onclick="app.switchTab('fres')">
                <span class="nav-icon">⚡</span>
                <span class="nav-label">ФРЭС</span>
            </button>
            <button class="nav-item" onclick="app.switchTab('ods')">
                <span class="nav-icon">🔧</span>
                <span class="nav-label">ОДС</span>
            </button>
            <button class="nav-item" onclick="app.switchTab('bres')">
                <span class="nav-icon">🏗️</span>
                <span class="nav-label">БРЭС</span>
            </button>
            <button class="nav-item" onclick="app.switchTab('smit')">
                <span class="nav-icon">📡</span>
                <span class="nav-label">СМиТ</span>
            </button>
        </nav>
    </div>

    <!-- Menu Drawer -->
    <div class="menu-overlay" id="menuOverlay" onclick="app.toggleMenu()"></div>
    <div class="menu-drawer" id="menuDrawer">
        <div class="menu-header">
            <div class="menu-user" id="menuUserName">Пользователь</div>
            <div class="menu-email" id="menuUserEmail">email@example.com</div>
        </div>
        <div class="menu-items">
            <button class="menu-item" onclick="app.switchSection('birthdays')">
                <div class="menu-item-icon">🎂</div>
                <span>Дни рождения</span>
            </button>
            <button class="menu-item admin-only hidden" onclick="app.switchSection('archive')">
                <div class="menu-item-icon">📦</div>
                <span>Архив</span>
            </button>
            <button class="menu-item" onclick="app.switchSection('about')">
                <div class="menu-item-icon">ℹ️</div>
                <span>О приложении</span>
            </button>
            <button class="menu-item" onclick="app.syncData()" id="syncMenuItem">
                <div class="menu-item-icon">🔄</div>
                <span>Синхронизировать</span>
            </button>
            <button class="menu-item" onclick="app.logout()">
                <div class="menu-item-icon">🚪</div>
                <span>Выйти</span>
            </button>
        </div>
    </div>

    <!-- Contact Detail Modal -->
    <div class="modal" id="contactModal">
        <div class="modal-content">
            <div class="modal-header">
                <h2 class="modal-title">Карточка контакта</h2>
                <button class="modal-close" onclick="app.closeModal()">×</button>
            </div>
            <div class="modal-body" id="modalBody">
                <!-- Dynamic content -->
            </div>
        </div>
    </div>

    <script>
        // Configuration
        const CONFIG = {
            SYNC_METHOD: localStorage.getItem('pb_sync_method') || 'csv',
            CSV_URL: localStorage.getItem('pb_csv_url') || '',
            DEBUG_MODE: true // Enable debug logging
        };

        // Debug logger
        const debug = {
            log: (msg, type = 'info') => {
                if (!CONFIG.DEBUG_MODE) return;
                console.log(`[${type.toUpperCase()}] ${msg}`);
                const consoleEl = document.getElementById('debugConsole');
                if (consoleEl) {
                    const line = document.createElement('div');
                    line.className = `debug-line debug-${type}`;
                    line.textContent = `[${new Date().toLocaleTimeString()}] ${msg}`;
                    consoleEl.appendChild(line);
                    consoleEl.scrollTop = consoleEl.scrollHeight;
                }
            },
            clear: () => {
                const consoleEl = document.getElementById('debugConsole');
                if (consoleEl) consoleEl.innerHTML = '';
            },
            show: () => {
                const consoleEl = document.getElementById('debugConsole');
                if (consoleEl) consoleEl.classList.add('active');
            }
        };

        // Application State
        const app = {
            currentUser: null,
            isAdmin: false,
            isApproved: false,
            contacts: [],
            currentTab: 'fres',
            currentSection: 'main',
            db: null,
            isOnline: navigator.onLine,
            lastSync: null,

            // Initialize
            init() {
                this.initDB();
                this.checkAuth();
                this.setupEventListeners();
                this.updateOnlineStatus();
                this.checkConfig();
            },

            checkConfig() {
                if (!CONFIG.CSV_URL) {
                    document.getElementById('warningBox').style.display = 'block';
                }
            },

            // IndexedDB Setup
            initDB() {
                const request = indexedDB.open('PhoneBookDB', 5);
                
                request.onerror = (e) => {
                    debug.log('Database error: ' + e.target.error, 'error');
                };
                
                request.onupgradeneeded = (event) => {
                    const db = event.target.result;
                    
                    if (!db.objectStoreNames.contains('contacts')) {
                        const store = db.createObjectStore('contacts', { keyPath: 'id' });
                        store.createIndex('department', 'department', { unique: false });
                        store.createIndex('archived', 'archived', { unique: false });
                    }
                    
                    if (!db.objectStoreNames.contains('users')) {
                        db.createObjectStore('users', { keyPath: 'email' });
                    }
                    
                    if (!db.objectStoreNames.contains('pending')) {
                        db.createObjectStore('pending', { keyPath: 'email' });
                    }
                };
                
                request.onsuccess = (event) => {
                    this.db = event.target.result;
                    debug.log('Database initialized successfully');
                    this.loadContacts();
                };
            },

            // Event Listeners
            setupEventListeners() {
                window.addEventListener('online', () => {
                    this.isOnline = true;
                    this.updateOnlineStatus();
                    debug.log('Connection restored', 'success');
                });
                
                window.addEventListener('offline', () => {
                    this.isOnline = false;
                    this.updateOnlineStatus();
                    debug.log('Connection lost', 'warning');
                });
                
                setInterval(() => this.checkPendingApprovals(), 5000);
            },

            updateOnlineStatus() {
                const status = document.getElementById('syncStatus');
                const text = document.getElementById('syncText');
                
                if (this.isOnline) {
                    status.className = 'sync-status online';
                    text.textContent = 'Онлайн';
                } else {
                    status.className = 'sync-status offline';
                    text.textContent = 'Офлайн режим';
                }
            },

            // Settings
            selectMethod(method) {
                CONFIG.SYNC_METHOD = method;
                document.getElementById('btnCsv').classList.toggle('active', method === 'csv');
                document.getElementById('btnManual').classList.toggle('active', method === 'manual');
                
                document.getElementById('csvMethod').style.display = method === 'csv' ? 'block' : 'none';
                document.getElementById('manualMethod').style.display = method === 'manual' ? 'block' : 'none';
            },

            openSettings() {
                debug.clear();
                this.selectMethod(CONFIG.SYNC_METHOD);
                document.getElementById('csvUrlInput').value = CONFIG.CSV_URL;
                document.getElementById('settingsModal').classList.add('active');
            },

            closeSettings() {
                document.getElementById('settingsModal').classList.remove('active');
            },

            fixUrl() {
                let url = document.getElementById('csvUrlInput').value.trim();
                
                // Remove problematic parameters
                url = url.replace(/[?&]single=true/gi, '');
                url = url.replace(/[?&]gid=\d+/gi, '');
                
                // Ensure it ends with ?output=csv or &output=csv
                if (!url.includes('output=csv')) {
                    if (url.includes('?')) {
                        url += '&output=csv';
                    } else {
                        url += '?output=csv';
                    }
                }
                
                // Clean up double parameters
                url = url.replace(/&&/g, '&');
                url = url.replace(/\?&/g, '?');
                
                document.getElementById('csvUrlInput').value = url;
                debug.log('URL fixed: ' + url, 'success');
                alert('URL исправлен! Теперь нажмите "Проверить подключение"');
            },

            async testConnection() {
                debug.show();
                debug.clear();
                debug.log('Starting connection test...');
                
                const url = document.getElementById('csvUrlInput').value.trim();
                
                if (!url) {
                    debug.log('URL is empty!', 'error');
                    alert('Введите URL');
                    return;
                }

                try {
                    debug.log('Fetching: ' + url);
                    
                    const response = await fetch(url, {
                        method: 'GET',
                        mode: 'cors',
                        cache: 'no-cache'
                    });
                    
                    debug.log('Response status: ' + response.status, response.ok ? 'success' : 'error');
                    
                    if (!response.ok) {
                        throw new Error('HTTP ' + response.status + ': ' + response.statusText);
                    }
                    
                    const text = await response.text();
                    debug.log('Received ' + text.length + ' bytes', 'success');
                    debug.log('First 200 chars: ' + text.substring(0, 200).replace(/\n/g, '\\n'), 'info');
                    
                    const lines = text.split('\n').filter(l => l.trim());
                    debug.log('Found ' + lines.length + ' lines', 'success');
                    
                    if (lines.length > 0) {
                        debug.log('Header: ' + lines[0], 'info');
                    }
                    
                    if (lines.length > 1) {
                        debug.log('First data row: ' + lines[1], 'info');
                        alert('✅ Подключение успешно! Найдено ' + (lines.length - 1) + ' контактов.');
                    } else {
                        debug.log('No data rows found!', 'warning');
                        alert('⚠️ Подключение есть, но данные не найдены. Проверьте структуру таблицы.');
                    }
                    
                } catch (error) {
                    debug.log('Error: ' + error.message, 'error');
                    
                    if (error.message.includes('Failed to fetch')) {
                        debug.log('CORS error or network issue. Try opening URL in browser first.', 'error');
                        alert('❌ Ошибка CORS. Попробуйте:\n1. Открыть URL в браузере\n2. Убедиться что таблица опубликована\n3. Проверить подключение к интернету');
                    } else {
                        alert('❌ Ошибка: ' + error.message);
                    }
                }
            },

            saveSettings() {
                const method = CONFIG.SYNC_METHOD;
                
                if (method === 'csv') {
                    let csvUrl = document.getElementById('csvUrlInput').value.trim();
                    
                    if (!csvUrl) {
                        alert('Введите URL CSV');
                        return;
                    }
                    
                    // Auto-fix URL if needed
                    if (csvUrl.includes('single=true') || csvUrl.includes('gid=')) {
                        csvUrl = csvUrl.replace(/[?&]single=true/gi, '');
                        csvUrl = csvUrl.replace(/[?&]gid=\d+/gi, '');
                        csvUrl = csvUrl.replace(/&&/g, '&');
                        csvUrl = csvUrl.replace(/\?&/g, '?');
                    }
                    
                    if (!csvUrl.includes('output=csv')) {
                        if (csvUrl.includes('?')) {
                            csvUrl += '&output=csv';
                        } else {
                            csvUrl += '?output=csv';
                        }
                    }
                    
                    localStorage.setItem('pb_csv_url', csvUrl);
                    localStorage.setItem('pb_sync_method', 'csv');
                    CONFIG.CSV_URL = csvUrl;
                    
                    debug.log('Settings saved. URL: ' + csvUrl, 'success');
                } else {
                    localStorage.setItem('pb_sync_method', 'manual');
                }
                
                this.closeSettings();
                this.showAuthStatus('Настройки сохранены!', 'success');
                document.getElementById('warningBox').style.display = 'none';
                
                if (this.isOnline && CONFIG.CSV_URL) {
                    this.syncWithGoogleSheets();
                }
            },

            // Data Export/Import
            exportData() {
                const data = {
                    contacts: this.contacts,
                    exportedAt: new Date().toISOString(),
                    version: '2.1'
                };
                
                const blob = new Blob([JSON.stringify(data, null, 2)], { type: 'application/json' });
                const url = URL.createObjectURL(blob);
                const a = document.createElement('a');
                a.href = url;
                a.download = `phonebook_backup_${new Date().toISOString().split('T')[0]}.json`;
                a.click();
                URL.revokeObjectURL(url);
                
                debug.log('Data exported: ' + this.contacts.length + ' contacts', 'success');
            },

            async importData(input) {
                const file = input.files[0];
                if (!file) return;
                
                try {
                    const text = await file.text();
                    const data = JSON.parse(text);
                    
                    if (data.contacts && Array.isArray(data.contacts)) {
                        const tx = this.db.transaction(['contacts'], 'readwrite');
                        const store = tx.objectStore('contacts');
                        
                        const existing = await store.getAll();
                        existing.forEach(c => store.delete(c.id));
                        
                        data.contacts.forEach(c => store.put(c));
                        
                        tx.oncomplete = () => {
                            this.loadContacts();
                            debug.log('Imported ' + data.contacts.length + ' contacts', 'success');
                            alert(`✅ Импортировано ${data.contacts.length} контактов`);
                        };
                    }
                } catch (e) {
                    debug.log('Import error: ' + e.message, 'error');
                    alert('❌ Ошибка импорта: ' + e.message);
                }
                
                input.value = '';
            },

            // Authentication
            async login() {
                const email = prompt('Введите ваш email:', 'user@company.com');
                if (!email || !email.includes('@')) {
                    this.showAuthStatus('Введите корректный email', 'error');
                    return;
                }
                this.completeLogin(email.toLowerCase().trim(), email.split('@')[0]);
            },

            async completeLogin(email, name) {
                this.showAuthStatus('Проверка...', 'info');

                await new Promise(r => setTimeout(r, 500));

                const user = await this.getUser(email);
                const pending = await this.getPendingUser(email);

                if (user) {
                    if (user.approved) {
                        this.currentUser = user;
                        this.isAdmin = user.isAdmin;
                        this.isApproved = true;
                        this.saveSession();
                        this.showMainApp();
                    } else {
                        this.showPendingScreen();
                    }
                } else if (pending) {
                    this.showPendingScreen();
                } else {
                    const isFirstUser = await this.isFirstUser();
                    const newUser = {
                        email: email,
                        name: name,
                        isAdmin: isFirstUser || email === 'admin@company.com',
                        approved: false,
                        createdAt: new Date().toISOString()
                    };
                    
                    await this.savePendingUser(newUser);
                    this.showPendingScreen();
                    
                    if (newUser.isAdmin) {
                        setTimeout(() => this.approveUser(email), 1500);
                    }
                }
            },

            async approveUser(email) {
                const pending = await this.getPendingUser(email);
                if (pending) {
                    pending.approved = true;
                    await this.saveUser(pending);
                    await this.removePendingUser(email);
                    
                    if (document.getElementById('pendingScreen').classList.contains('active')) {
                        this.currentUser = pending;
                        this.isAdmin = pending.isAdmin;
                        this.isApproved = true;
                        this.showMainApp();
                    }
                }
            },

            async checkPendingApprovals() {
                if (!this.currentUser && document.getElementById('pendingScreen').classList.contains('active')) {
                    const email = localStorage.getItem('pending_email');
                    if (email) {
                        const user = await this.getUser(email);
                        if (user && user.approved) {
                            this.currentUser = user;
                            this.isAdmin = user.isAdmin;
                            this.isApproved = true;
                            this.showMainApp();
                        }
                    }
                }
            },

            logout() {
                localStorage.removeItem('phonebook_session');
                location.reload();
            },

            checkAuth() {
                const session = localStorage.getItem('phonebook_session');
                if (session) {
                    const data = JSON.parse(session);
                    this.currentUser = data.user;
                    this.isAdmin = data.user.isAdmin;
                    this.isApproved = true;
                    this.showMainApp();
                }
            },

            saveSession() {
                localStorage.setItem('phonebook_session', JSON.stringify({
                    user: this.currentUser,
                    timestamp: new Date().toISOString()
                }));
            },

            // UI Methods
            showAuthStatus(msg, type) {
                const el = document.getElementById('authStatus');
                el.textContent = msg;
                el.className = `auth-status ${type}`;
            },

            showPendingScreen() {
                document.getElementById('authScreen').style.display = 'none';
                document.getElementById('pendingScreen').classList.add('active');
            },

            showMainApp() {
                document.getElementById('authScreen').style.display = 'none';
                document.getElementById('pendingScreen').classList.remove('active');
                document.getElementById('mainApp').classList.add('active');
                
                document.getElementById('menuUserEmail').textContent = this.currentUser.email;
                document.getElementById('menuUserName').textContent = this.currentUser.name || (this.isAdmin ? 'Администратор' : 'Пользователь');
                
                if (this.isAdmin) {
                    document.querySelectorAll('.admin-only').forEach(el => el.classList.remove('hidden'));
                    document.getElementById('addBtn').classList.add('visible');
                }
                
                this.loadContacts();
            },

            // Database Operations
            async getUser(email) {
                return new Promise((resolve) => {
                    if (!this.db) return resolve(null);
                    const tx = this.db.transaction(['users'], 'readonly');
                    const store = tx.objectStore('users');
                    const request = store.get(email);
                    request.onsuccess = () => resolve(request.result);
                    request.onerror = () => resolve(null);
                });
            },

            async saveUser(user) {
                return new Promise((resolve) => {
                    const tx = this.db.transaction(['users'], 'readwrite');
                    const store = tx.objectStore('users');
                    store.put(user);
                    tx.oncomplete = resolve;
                });
            },

            async getPendingUser(email) {
                return new Promise((resolve) => {
                    if (!this.db) return resolve(null);
                    const tx = this.db.transaction(['pending'], 'readonly');
                    const store = tx.objectStore('pending');
                    const request = store.get(email);
                    request.onsuccess = () => resolve(request.result);
                });
            },

            async savePendingUser(user) {
                localStorage.setItem('pending_email', user.email);
                return new Promise((resolve) => {
                    const tx = this.db.transaction(['pending'], 'readwrite');
                    const store = tx.objectStore('pending');
                    store.put(user);
                    tx.oncomplete = resolve;
                });
            },

            async removePendingUser(email) {
                return new Promise((resolve) => {
                    const tx = this.db.transaction(['pending'], 'readwrite');
                    const store = tx.objectStore('pending');
                    store.delete(email);
                    tx.oncomplete = resolve;
                });
            },

            async isFirstUser() {
                return new Promise((resolve) => {
                    const tx = this.db.transaction(['users'], 'readonly');
                    const store = tx.objectStore('users');
                    const request = store.getAll();
                    request.onsuccess = () => resolve(request.result.length === 0);
                });
            },

            // Contacts Management
            async loadContacts() {
                if (!this.db) {
                    debug.log('Database not ready', 'error');
                    return;
                }
                
                const tx = this.db.transaction(['contacts'], 'readonly');
                const store = tx.objectStore('contacts');
                const request = store.getAll();
                
                request.onsuccess = () => {
                    this.contacts = request.result;
                    debug.log('Loaded ' + this.contacts.length + ' contacts from DB');
                    this.render();
                    
                    // Auto-sync if configured
                    if (this.isOnline && CONFIG.CSV_URL && CONFIG.SYNC_METHOD === 'csv') {
                        debug.log('Auto-sync triggered');
                        this.syncWithGoogleSheets();
                    }
                };
                
                request.onerror = () => {
                    debug.log('Error loading contacts from DB', 'error');
                };
            },

            async saveContact(contact) {
                return new Promise((resolve) => {
                    const tx = this.db.transaction(['contacts'], 'readwrite');
                    const store = tx.objectStore('contacts');
                    
                    if (!contact.id) {
                        contact.id = 'local_' + Date.now();
                    }
                    
                    store.put(contact);
                    
                    tx.oncomplete = () => {
                        debug.log('Contact saved: ' + contact.id, 'success');
                        this.loadContacts();
                        resolve();
                    };
                    
                    tx.onerror = () => {
                        debug.log('Error saving contact', 'error');
                    };
                });
            },

            async archiveContact(id) {
                const contact = this.contacts.find(c => c.id === id);
                if (contact) {
                    contact.archived = true;
                    contact.archivedAt = new Date().toISOString();
                    await this.saveContact(contact);
                    this.closeModal();
                    this.render();
                }
            },

            async deleteContact(id) {
                return new Promise((resolve) => {
                    const tx = this.db.transaction(['contacts'], 'readwrite');
                    const store = tx.objectStore('contacts');
                    store.delete(id);
                    tx.oncomplete = () => {
                        this.loadContacts();
                        resolve();
                    };
                });
            },

            // CSV Sync with improved error handling
            async syncWithGoogleSheets() {
                if (CONFIG.SYNC_METHOD === 'manual') {
                    debug.log('Manual mode - skipping sync');
                    return;
                }

                if (!CONFIG.CSV_URL) {
                    debug.log('No CSV URL configured', 'warning');
                    return;
                }

                const status = document.getElementById('syncStatus');
                status.className = 'sync-status syncing';
                document.getElementById('syncText').textContent = 'Синхронизация...';

                try {
                    debug.log('Fetching CSV from: ' + CONFIG.CSV_URL);
                    
                    // Use no-cors mode as fallback if CORS fails
                    let response;
                    try {
                        response = await fetch(CONFIG.CSV_URL, {
                            method: 'GET',
                            cache: 'no-cache',
                            headers: {
                                'Accept': 'text/csv,text/plain,*/*'
                            }
                        });
                    } catch (corsError) {
                        debug.log('CORS error, trying no-cors mode...', 'warning');
                        response = await fetch(CONFIG.CSV_URL, {
                            method: 'GET',
                            mode: 'no-cors',
                            cache: 'no-cache'
                        });
                        // In no-cors mode we can't read the response properly
                        // This is a limitation for GitHub Pages hosting
                        throw new Error('CORS policy blocked. Try hosting app on same domain or use manual import.');
                    }
                    
                    if (!response.ok) {
                        throw new Error('HTTP ' + response.status);
                    }
                    
                    const csvText = await response.text();
                    debug.log('Received ' + csvText.length + ' bytes');
                    
                    if (!csvText || csvText.trim().length === 0) {
                        throw new Error('Empty response');
                    }

                    const contacts = this.parseCSV(csvText);
                    debug.log('Parsed ' + contacts.length + ' contacts', 'success');
                    
                    if (contacts.length === 0) {
                        throw new Error('No valid contacts found in CSV');
                    }

                    await this.processSheetData(contacts);
                    
                    this.lastSync = new Date();
                    this.updateLastSyncDisplay();
                    
                    status.className = 'sync-status online';
                    document.getElementById('syncText').textContent = `✓ ${contacts.length} контактов`;
                    
                    debug.log('Sync completed successfully', 'success');
                    
                } catch (error) {
                    debug.log('Sync error: ' + error.message, 'error');
                    console.error('Sync error:', error);
                    
                    status.className = 'sync-status error';
                    document.getElementById('syncText').textContent = 'Ошибка синхронизации';
                    
                    // Don't show alert on auto-sync, only on manual
                    if (this.manualSync) {
                        alert('❌ Ошибка синхронизации:\n' + error.message + '\n\nПопробуйте:\n1. Проверить URL в настройках\n2. Нажать "Исправить URL"\n3. Использовать локальный режим');
                        this.manualSync = false;
                    }
                }
            },

            parseCSV(csvText) {
                const lines = csvText.trim().split(/\r?\n/);
                const contacts = [];
                
                debug.log('CSV lines: ' + lines.length);
                
                if (lines.length === 0) return contacts;
                
                // Detect header
                const firstLine = lines[0].toLowerCase();
                const hasHeader = firstLine.includes('фамилия') || 
                                  firstLine.includes('last') || 
                                  firstLine.includes('имя') ||
                                  firstLine.includes('name') ||
                                  firstLine.includes('фио');
                
                const startIndex = hasHeader ? 1 : 0;
                
                debug.log('Header detected: ' + hasHeader + ', starting from line ' + startIndex);
                
                for (let i = startIndex; i < lines.length; i++) {
                    const line = lines[i].trim();
                    if (!line) continue;
                    
                    // Parse CSV line (handle quoted values)
                    const values = this.parseCSVLine(line);
                    
                    if (values.length < 2) {
                        debug.log('Skipping line ' + i + ': too few columns', 'warning');
                        continue;
                    }
                    
                    // Skip if looks like header row accidentally included
                    if (values[0].toLowerCase().includes('фамилия') || 
                        values[0].toLowerCase().includes('last')) {
                        continue;
                    }
                    
                    const contact = {
                        id: 'sheet_' + (i + 1),
                        lastName: values[0] || '',
                        firstName: values[1] || '',
                        middleName: values[2] || '',
                        position: values[3] || '',
                        department: (values[4] || 'fres').toString().toLowerCase().trim(),
                        phoneMobile: values[5] || '',
                        phoneWork: values[6] || '',
                        birthday: values[7] || '',
                        photo: values[8] || '',
                        isAdmin: this.parseBoolean(values[9]),
                        archived: this.parseBoolean(values[10]),
                        birthDay: parseInt(values[11]) || null,
                        birthMonth: parseInt(values[12]) || null,
                        birthYear: parseInt(values[13]) || null,
                        email: values[14] || ''
                    };
                    
                    // Validate department
                    const validDepts = ['fres', 'ods', 'bres', 'smit'];
                    if (!validDepts.includes(contact.department)) {
                        contact.department = 'fres';
                    }
                    
                    contacts.push(contact);
                }
                
                return contacts;
            },

            parseCSVLine(line) {
                const result = [];
                let current = '';
                let inQuotes = false;
                
                for (let i = 0; i < line.length; i++) {
                    const char = line[i];
                    const nextChar = line[i + 1];
                    
                    if (char === '"') {
                        if (inQuotes && nextChar === '"') {
                            current += '"';
                            i++; // Skip next quote
                        } else {
                            inQuotes = !inQuotes;
                        }
                    } else if (char === ',' && !inQuotes) {
                        result.push(current.trim());
                        current = '';
                    } else {
                        current += char;
                    }
                }
                
                result.push(current.trim());
                return result;
            },

            parseBoolean(value) {
                if (!value) return false;
                const str = value.toString().toLowerCase().trim();
                return str === 'true' || str === '1' || str === 'yes' || str === 'да';
            },

            async processSheetData(contacts) {
                return new Promise((resolve, reject) => {
                    const tx = this.db.transaction(['contacts'], 'readwrite');
                    const store = tx.objectStore('contacts');
                    
                    // Get existing local contacts
                    const getRequest = store.getAll();
                    
                    getRequest.onsuccess = () => {
                        const existing = getRequest.result;
                        const localContacts = existing.filter(c => c.id.toString().startsWith('local_'));
                        
                        debug.log('Preserving ' + localContacts.length + ' local contacts');
                        
                        // Clear all
                        existing.forEach(c => store.delete(c.id));
                        
                        // Add sheet contacts
                        contacts.forEach(contact => {
                            store.put(contact);
                        });
                        
                        // Restore local contacts
                        localContacts.forEach(c => store.put(c));
                        
                        tx.oncomplete = () => {
                            debug.log('Database updated with ' + contacts.length + ' contacts', 'success');
                            this.loadContacts();
                            resolve();
                        };
                        
                        tx.onerror = () => {
                            debug.log('Transaction error', 'error');
                            reject(new Error('Database transaction failed'));
                        };
                    };
                });
            },

            updateLastSyncDisplay() {
                if (this.lastSync) {
                    const time = this.lastSync.toLocaleTimeString('ru-RU', { hour: '2-digit', minute: '2-digit' });
                    document.getElementById('lastSync').textContent = `Синхр.: ${time}`;
                }
            },

            // Rendering
            render() {
                const container = document.getElementById('contentArea');
                const searchTerm = document.getElementById('searchInput').value.toLowerCase();
                
                let filtered = this.contacts.filter(c => {
                    if (c.archived && this.currentSection !== 'archive') return false;
                    if (!c.archived && this.currentSection === 'archive') return false;
                    
                    if (searchTerm) {
                        const fullName = `${c.lastName} ${c.firstName} ${c.middleName}`.toLowerCase();
                        return fullName.includes(searchTerm) || 
                               (c.position && c.position.toLowerCase().includes(searchTerm)) ||
                               (c.phoneMobile && c.phoneMobile.includes(searchTerm));
                    }
                    
                    if (this.currentSection === 'birthdays') return !c.archived && c.birthday;
                    if (this.currentSection === 'archive') return true;
                    if (this.currentSection === 'about') return false;
                    
                    return c.department === this.currentTab && !c.archived;
                });

                if (this.currentSection === 'birthdays') {
                    filtered = this.sortByBirthday(filtered);
                }

                if (this.currentSection === 'about') {
                    this.renderAbout(container);
                    return;
                }

                if (filtered.length === 0) {
                    container.innerHTML = `
                        <div class="empty-state">
                            <div class="empty-icon">📭</div>
                            <p>Нет контактов</p>
                            ${!this.isOnline ? '<p style="font-size: 14px; margin-top: 10px;">Проверьте подключение к интернету</p>' : ''}
                            ${this.currentSection === 'main' ? '<p style="font-size: 12px; margin-top: 10px; color: #666;">Проверьте настройки синхронизации</p>' : ''}
                        </div>
                    `;
                    return;
                }

                container.innerHTML = filtered.map(c => this.renderContactCard(c)).join('');
            },

            renderContactCard(contact) {
                const daysToBirthday = this.getDaysToBirthday(contact.birthday);
                const birthdayBadge = daysToBirthday !== null && daysToBirthday <= 7 ? 
                    `<div class="birthday-badge">🎂 ${daysToBirthday} дн.</div>` : '';
                
                const photo = contact.photo || `https://ui-avatars.com/api/?name=${encodeURIComponent(contact.firstName + ' ' + contact.lastName)}&background=e94560&color=fff&size=128`;
                
                return `
                    <div class="contact-card" onclick="app.openContact('${contact.id}')">
                        <img src="${photo}" class="contact-photo" alt="" 
                             onerror="this.src='https://ui-avatars.com/api/?name=${encodeURIComponent(contact.firstName || 'U')}&background=e94560&color=fff&size=128'">
                        <div class="contact-info">
                            <div class="contact-name">${contact.lastName} ${contact.firstName}</div>
                            <div class="contact-position">${contact.position || 'Сотрудник'}</div>
                            <div class="contact-phone">${contact.phoneMobile || 'Нет телефона'}</div>
                        </div>
                        ${birthdayBadge}
                    </div>
                `;
            },

            renderAbout(container) {
                container.innerHTML = `
                    <div style="text-align: center; padding: 40px 20px;">
                        <div style="font-size: 64px; margin-bottom: 20px;">📞</div>
                        <h2 style="margin-bottom: 10px;">Телефонный справочник</h2>
                        <p style="color: #a0a0a0; margin-bottom: 30px;">Версия 2.1</p>
                        
                        <div style="background: rgba(255,255,255,0.05); padding: 20px; border-radius: 12px; margin-bottom: 20px; text-align: left;">
                            <h3 style="margin-bottom: 15px; color: #e94560;">Статус системы</h3>
                            <p style="color: #a0a0a0; line-height: 1.6; margin-bottom: 10px;">
                                Метод: <strong style="color: #fff;">${CONFIG.SYNC_METHOD === 'csv' ? 'CSV' : 'Локальный'}</strong>
                            </p>
                            <p style="color: #a0a0a0; line-height: 1.6; margin-bottom: 10px;">
                                Всего контактов: <strong style="color: #fff;">${this.contacts.length}</strong>
                            </p>
                            <p style="color: #a0a0a0; line-height: 1.6;">
                                Последняя синхронизация: ${this.lastSync ? this.lastSync.toLocaleString('ru-RU') : 'Никогда'}
                            </p>
                        </div>

                        <div style="background: rgba(255,255,255,0.05); padding: 20px; border-radius: 12px; text-align: left;">
                            <h3 style="margin-bottom: 15px; color: #e94560;">Возможности</h3>
                            <ul style="color: #a0a0a0; line-height: 1.8; padding-left: 20px;">
                                <li>✅ Работа без интернета</li>
                                <li>✅ Синхронизация с Google Sheets</li>
                                <li>✅ Быстрый поиск</li>
                                <li>✅ Уведомления о ДР</li>
                                <li>✅ Архивирование</li>
                            </ul>
                        </div>
                        
                        <button class="action-btn secondary" onclick="app.openSettings()" style="margin-top: 20px;">
                            ⚙️ Настройки
                        </button>
                    </div>
                `;
            },

            // Navigation
            switchTab(tab) {
                this.currentTab = tab;
                this.currentSection = 'main';
                
                document.querySelectorAll('.nav-item').forEach(el => el.classList.remove('active'));
                event.currentTarget.classList.add('active');
                
                const titles = {
                    'fres': 'ФРЭС',
                    'ods': 'ОДС',
                    'bres': 'БРЭС',
                    'smit': 'СМиТ'
                };
                
                document.getElementById('pageTitle').textContent = titles[tab];
                document.getElementById('addBtn').classList.toggle('visible', this.isAdmin);
                
                this.render();
            },

            switchSection(section) {
                this.currentSection = section;
                this.toggleMenu();
                
                const titles = {
                    'birthdays': 'Дни рождения',
                    'archive': 'Архив',
                    'about': 'О приложении'
                };
                
                document.getElementById('pageTitle').textContent = titles[section];
                document.querySelectorAll('.nav-item').forEach(el => el.classList.remove('active'));
                document.getElementById('addBtn').classList.remove('visible');
                
                this.render();
            },

            toggleMenu() {
                document.getElementById('menuDrawer').classList.toggle('active');
                document.getElementById('menuOverlay').classList.toggle('active');
            },

            // Contact Details
            openContact(id) {
                const contact = this.contacts.find(c => c.id === id);
                if (!contact) return;
                
                const daysToBirthday = this.getDaysToBirthday(contact.birthday);
                const birthdayText = daysToBirthday !== null ? 
                    `${contact.birthday} (через ${daysToBirthday} дней)` : (contact.birthday || 'Не указана');
                
                const photo = contact.photo || `https://ui-avatars.com/api/?name=${encodeURIComponent(contact.firstName + ' ' + contact.lastName)}&background=e94560&color=fff&size=256`;
                
                let html = `
                    <img src="${photo}" class="contact-detail-photo" alt="" 
                         onerror="this.src='https://ui-avatars.com/api/?name=${encodeURIComponent(contact.firstName || 'U')}&background=e94560&color=fff&size=256'">
                    
                    <div class="detail-field">
                        <div class="detail-label">ФИО</div>
                        <div class="detail-value">${contact.lastName} ${contact.firstName} ${contact.middleName}</div>
                    </div>
                    
                    <div class="detail-field">
                        <div class="detail-label">Должность</div>
                        <div class="detail-value">${contact.position || 'Сотрудник'}</div>
                    </div>
                    
                    <div class="detail-field">
                        <div class="detail-label">Подразделение</div>
                        <div class="detail-value">${this.getDepartmentName(contact.department)}</div>
                    </div>
                    
                    <div class="detail-field">
                        <div class="detail-label">Мобильный телефон</div>
                        <div class="detail-value">
                            ${contact.phoneMobile ? `<a href="tel:${contact.phoneMobile}" style="color: #e94560; text-decoration: none;">${contact.phoneMobile}</a>` : 'Не указан'}
                        </div>
                    </div>
                    
                    <div class="detail-field">
                        <div class="detail-label">Рабочий телефон</div>
                        <div class="detail-value">${contact.phoneWork || 'Не указан'}</div>
                    </div>
                    
                    <div class="detail-field">
                        <div class="detail-label">Дата рождения</div>
                        <div class="detail-value">${birthdayText}</div>
                    </div>
                `;
                
                if (this.isAdmin && !contact.archived) {
                    html += `
                        <div class="admin-section">
                            <span class="admin-badge">👑 Администрирование</span>
                            
                            <div class="detail-field">
                                <div class="detail-label">Дата рождения (редактирование)</div>
                                <div class="date-row">
                                    <input type="number" class="date-input" placeholder="ДД" value="${contact.birthDay || ''}" id="editDay" min="1" max="31">
                                    <input type="number" class="date-input" placeholder="ММ" value="${contact.birthMonth || ''}" id="editMonth" min="1" max="12">
                                    <input type="number" class="date-input" placeholder="ГГГГ" value="${contact.birthYear || ''}" id="editYear" min="1900" max="2024">
                                </div>
                            </div>
                            
                            <button class="action-btn secondary" onclick="app.saveContactDetails('${contact.id}')">💾 Сохранить изменения</button>
                            <button class="action-btn danger" onclick="app.archiveContact('${contact.id}')">📦 В архив</button>
                        </div>
                    `;
                }
                
                if (contact.archived && this.isAdmin) {
                    html += `
                        <div class="admin-section">
                            <span class="admin-badge">📦 Архив</span>
                            <button class="action-btn secondary" onclick="app.restoreContact('${contact.id}')">Восстановить из архива</button>
                            <button class="action-btn danger" onclick="app.deleteContactPermanently('${contact.id}')">🗑️ Удалить навсегда</button>
                        </div>
                    `;
                }
                
                document.getElementById('modalBody').innerHTML = html;
                document.getElementById('contactModal').classList.add('active');
            },

            closeModal() {
                document.getElementById('contactModal').classList.remove('active');
            },

            async saveContactDetails(id) {
                const contact = this.contacts.find(c => c.id === id);
                if (!contact) return;
                
                const day = document.getElementById('editDay').value;
                const month = document.getElementById('editMonth').value;
                const year = document.getElementById('editYear').value;
                
                if (day && month && year) {
                    contact.birthDay = parseInt(day);
                    contact.birthMonth = parseInt(month);
                    contact.birthYear = parseInt(year);
                    contact.birthday = `${year}-${String(month).padStart(2, '0')}-${String(day).padStart(2, '0')}`;
                    
                    await this.saveContact(contact);
                    this.closeModal();
                    this.render();
                }
            },

            async restoreContact(id) {
                const contact = this.contacts.find(c => c.id === id);
                if (contact) {
                    contact.archived = false;
                    delete contact.archivedAt;
                    await this.saveContact(contact);
                    this.closeModal();
                    this.render();
                }
            },

            async deleteContactPermanently(id) {
                if (confirm('Удалить контакт навсегда?')) {
                    await this.deleteContact(id);
                    this.closeModal();
                    this.render();
                }
            },

            // Add Contact
            openAddModal() {
                if (!this.isAdmin) return;
                
                const html = `
                    <div class="detail-field">
                        <div class="detail-label">Фамилия *</div>
                        <input type="text" class="detail-input" id="newLastName" placeholder="Введите фамилию">
                    </div>
                    
                    <div class="detail-field">
                        <div class="detail-label">Имя *</div>
                        <input type="text" class="detail-input" id="newFirstName" placeholder="Введите имя">
                    </div>
                    
                    <div class="detail-field">
                        <div class="detail-label">Отчество</div>
                        <input type="text" class="detail-input" id="newMiddleName" placeholder="Введите отчество">
                    </div>
                    
                    <div class="detail-field">
                        <div class="detail-label">Должность</div>
                        <input type="text" class="detail-input" id="newPosition" placeholder="Введите должность">
                    </div>
                    
                    <div class="detail-field">
                        <div class="detail-label">Подразделение</div>
                        <select class="detail-input" id="newDepartment" style="background: transparent; border: none; color: #fff; width: 100%;">
                            <option value="fres" style="background: #1a1a2e;">ФРЭС</option>
                            <option value="ods" style="background: #1a1a2e;">ОДС</option>
                            <option value="bres" style="background: #1a1a2e;">БРЭС</option>
                            <option value="smit" style="background: #1a1a2e;">СМиТ</option>
                        </select>
                    </div>
                    
                    <div class="detail-field">
                        <div class="detail-label">Мобильный телефон</div>
                        <input type="tel" class="detail-input" id="newPhoneMobile" placeholder="+7 (900) 000-00-00">
                    </div>
                    
                    <div class="detail-field">
                        <div class="detail-label">Рабочий телефон</div>
                        <input type="tel" class="detail-input" id="newPhoneWork" placeholder="0000">
                    </div>
                    
                    <div class="detail-field">
                        <div class="detail-label">Email</div>
                        <input type="email" class="detail-input" id="newEmail" placeholder="email@company.com">
                    </div>
                    
                    <div class="detail-field">
                        <div class="detail-label">Дата рождения</div>
                        <div class="date-row">
                            <input type="number" class="date-input" placeholder="ДД" id="newDay" min="1" max="31">
                            <input type="number" class="date-input" placeholder="ММ" id="newMonth" min="1" max="12">
                            <input type="number" class="date-input" placeholder="ГГГГ" id="newYear" min="1900" max="2024">
                        </div>
                    </div>
                    
                    <button class="action-btn primary" onclick="app.addContact()">➕ Добавить контакт</button>
                `;
                
                document.getElementById('modalBody').innerHTML = html;
                document.getElementById('contactModal').classList.add('active');
            },

            async addContact() {
                const lastName = document.getElementById('newLastName').value.trim();
                const firstName = document.getElementById('newFirstName').value.trim();
                
                if (!lastName || !firstName) {
                    alert('Введите фамилию и имя');
                    return;
                }
                
                const contact = {
                    lastName,
                    firstName,
                    middleName: document.getElementById('newMiddleName').value.trim(),
                    position: document.getElementById('newPosition').value.trim(),
                    department: document.getElementById('newDepartment').value,
                    phoneMobile: document.getElementById('newPhoneMobile').value.trim(),
                    phoneWork: document.getElementById('newPhoneWork').value.trim(),
                    email: document.getElementById('newEmail').value.trim(),
                    birthDay: parseInt(document.getElementById('newDay').value) || null,
                    birthMonth: parseInt(document.getElementById('newMonth').value) || null,
                    birthYear: parseInt(document.getElementById('newYear').value) || null,
                    birthday: '',
                    photo: '',
                    archived: false,
                    isAdmin: false
                };

                if (contact.birthDay && contact.birthMonth && contact.birthYear) {
                    contact.birthday = `${contact.birthYear}-${String(contact.birthMonth).padStart(2, '0')}-${String(contact.birthDay).padStart(2, '0')}`;
                }
                
                await this.saveContact(contact);
                this.closeModal();
                this.render();
            },

            syncData() {
                this.toggleMenu();
                if (this.isOnline) {
                    this.manualSync = true;
                    this.syncWithGoogleSheets();
                } else {
                    alert('Нет подключения к интернету');
                }
            },

            // Helpers
            getDepartmentName(code) {
                const names = {
                    'fres': 'ФРЭС',
                    'ods': 'ОДС',
                    'bres': 'БРЭС',
                    'smit': 'СМиТ'
                };
                return names[code] || code.toUpperCase();
            },

            getDaysToBirthday(birthdayStr) {
                if (!birthdayStr) return null;
                
                const today = new Date();
                const birthDate = new Date(birthdayStr);
                if (isNaN(birthDate.getTime())) return null;
                
                const currentYear = today.getFullYear();
                
                let nextBirthday = new Date(currentYear, birthDate.getMonth(), birthDate.getDate());
                
                if (nextBirthday < today) {
                    nextBirthday.setFullYear(currentYear + 1);
                }
                
                const diffTime = nextBirthday - today;
                const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24));
                
                return diffDays;
            },

            sortByBirthday(contacts) {
                return contacts.filter(c => c.birthday).sort((a, b) => {
                    const daysA = this.getDaysToBirthday(a.birthday);
                    const daysB = this.getDaysToBirthday(b.birthday);
                    if (daysA === null) return 1;
                    if (daysB === null) return -1;
                    return daysA - daysB;
                });
            },

            search() {
                this.render();
            }
        };

        // Initialize
        document.addEventListener('DOMContentLoaded', () => {
            app.init();
        });

        // Close modal on outside click
        document.getElementById('contactModal').addEventListener('click', (e) => {
            if (e.target === e.currentTarget) {
                app.closeModal();
            }
        });
    </script>
</body>
</html>
