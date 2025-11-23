<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CORA - Watsonx Orchestrate Virtual Assistant</title>
    <style>
        :root {
            --ibm-blue: #0062FF;
            --ibm-blue-dark: #0051D4;
            --ibm-teal: #00D4AA;
            --ibm-purple: #8A3FFC;
            --ibm-gray-10: #F4F4F4;
            --ibm-gray-20: #E0E0E0;
            --ibm-gray-80: #393939;
            --ibm-gray-90: #262626;
            --radius: 8px;
            --shadow: 0 4px 16px rgba(0, 0, 0, 0.1);
        }

        html, body {
            height: 100%;
            width: 100%;
            min-height: 100vh;
            margin: 0;
            padding: 0;
        }

        body {
            background: linear-gradient(135deg, var(--ibm-gray-90) 0%, var(--ibm-gray-80) 100%);
            color: white;
            min-height: 100vh;
            min-width: 100vw;
            line-height: 1.6;
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        .container {
            width: 100vw;
            height: 100vh;
            max-width: none;
            min-height: 100vh;
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        /* Header */
        header {
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
            padding: 20px 0;
            margin-bottom: 30px;
        }

        .header-content {
            display: flex;
            justify-content: space-between;
            align-items: center;
            width: 100vw;
            margin: 0 auto;
            padding: 0 20px;
            box-sizing: border-box;
        }

        .logo {
            display: flex;
            align-items: center;
            gap: 15px;
        }

        .logo-icon {
            width: 50px;
            height: 50px;
            background: var(--ibm-blue);
            border-radius: 12px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            font-size: 24px;
        }

        .logo-text {
            font-size: 28px;
            font-weight: 600;
        }

        .logo-subtitle {
            font-size: 14px;
            color: var(--ibm-teal);
            margin-top: -5px;
        }

        .watsonx-badge {
            background: var(--ibm-purple);
            color: white;
            padding: 5px 12px;
            border-radius: 20px;
            font-size: 12px;
            font-weight: 600;
        }

        /* Main Grid */
        .main-grid {
            display: grid;
            grid-template-columns: 320px 1fr 350px;
            gap: 25px;
            width: 100vw;
            height: calc(100vh - 120px); /* Adjust if header/footer height differs */
            margin: 0;
            min-height: 0;
            box-sizing: border-box;
        }

        /* Panel Styles */
        .panel {
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            border-radius: var(--radius);
            border: 1px solid rgba(255, 255, 255, 0.1);
            padding: 25px;
            overflow: hidden;
            height: 100%;
            box-sizing: border-box;
            min-width: 0;
        }

        .panel-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            padding-bottom: 15px;
            border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        }

        .panel-title {
            font-size: 18px;
            font-weight: 600;
            color: white;
        }

        /* Skills Panel */
        .skills-grid {
            display: grid;
            gap: 15px;
        }

        .skill-card {
            background: rgba(255, 255, 255, 0.08);
            border-radius: var(--radius);
            padding: 20px;
            border-left: 4px solid var(--ibm-blue);
            transition: all 0.3s ease;
            cursor: pointer;
        }

        .skill-card:hover {
            background: rgba(255, 255, 255, 0.12);
            transform: translateY(-2px);
        }

        .skill-card.active {
            background: rgba(0, 98, 255, 0.15);
            border-left-color: var(--ibm-teal);
        }

        .skill-icon {
            width: 40px;
            height: 40px;
            background: rgba(0, 98, 255, 0.2);
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-bottom: 10px;
            font-size: 18px;
        }

        .skill-name {
            font-weight: 600;
            margin-bottom: 5px;
            font-size: 16px;
        }

        .skill-desc {
            font-size: 13px;
            color: #8C94A7;
            line-height: 1.4;
        }

        /* Chat Panel */
        .chat-container {
            display: flex;
            flex-direction: column;
            height: 100%;
        }

        .chat-messages {
            flex: 1;
            overflow-y: auto;
            padding: 20px;
            background: rgba(255, 255, 255, 0.02);
            border-radius: var(--radius);
            margin-bottom: 20px;
        }

        .message {
            margin-bottom: 20px;
            animation: fadeIn 0.3s ease;
        }

        .message.user {
            text-align: right;
        }

        .message-bubble {
            display: inline-block;
            padding: 15px 20px;
            border-radius: 20px;
            max-width: 80%;
            text-align: left;
        }

        .message.user .message-bubble {
            background: var(--ibm-blue);
            border-bottom-right-radius: 5px;
        }

        .message.cora .message-bubble {
            background: rgba(255, 255, 255, 0.1);
            border-bottom-left-radius: 5px;
        }

        .message-time {
            font-size: 11px;
            color: #8C94A7;
            margin-top: 5px;
        }

        .chat-input-container {
            display: flex;
            gap: 10px;
        }

        .chat-input {
            flex: 1;
            background: rgba(255, 255, 255, 0.08);
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: var(--radius);
            padding: 15px 20px;
            color: white;
            font-size: 14px;
            resize: none;
            height: 60px;
        }

        .chat-input:focus {
            outline: none;
            border-color: var(--ibm-blue);
        }

        /* Controls */
        .controls {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
        }

        .btn {
            padding: 12px 24px;
            border: none;
            border-radius: var(--radius);
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            gap: 8px;
            font-size: 14px;
        }

        .btn-primary {
            background: var(--ibm-blue);
            color: white;
        }

        .btn-primary:hover {
            background: var(--ibm-blue-dark);
            transform: translateY(-2px);
        }

        .btn-secondary {
            background: rgba(255, 255, 255, 0.1);
            color: white;
        }

        .btn-secondary:hover {
            background: rgba(255, 255, 255, 0.15);
        }

        .btn-teal {
            background: var(--ibm-teal);
            color: white;
        }

        .btn-icon {
            width: 45px;
            height: 45px;
            padding: 0;
            justify-content: center;
            border-radius: 50%;
        }

        /* Status Panel */
        .status-item {
            background: rgba(255, 255, 255, 0.05);
            border-radius: var(--radius);
            padding: 20px;
            margin-bottom: 15px;
            border-left: 4px solid var(--ibm-teal);
        }

        .status-label {
            font-size: 12px;
            color: #8C94A7;
            margin-bottom: 5px;
        }

        .status-value {
            font-size: 16px;
            font-weight: 600;
        }

        .workflow-visual {
            background: rgba(255, 255, 255, 0.03);
            border-radius: var(--radius);
            padding: 20px;
            margin-top: 20px;
        }

        .workflow-step {
            display: flex;
            align-items: center;
            gap: 10px;
            padding: 10px;
            margin-bottom: 10px;
            background: rgba(255, 255, 255, 0.05);
            border-radius: 8px;
        }

        .step-number {
            width: 25px;
            height: 25px;
            background: var(--ibm-blue);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 12px;
            font-weight: bold;
        }

        /* Business Impact Section */
        .impact-section {
            background: rgba(255, 255, 255, 0.05);
            border-radius: var(--radius);
            padding: 30px;
            margin-top: 40px;
        }

        .impact-title {
            font-size: 24px;
            font-weight: 600;
            margin-bottom: 20px;
            text-align: center;
            color: var(--ibm-teal);
        }

        .impact-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
        }

        .impact-card {
            background: rgba(255, 255, 255, 0.08);
            border-radius: var(--radius);
            padding: 25px;
            text-align: center;
            transition: transform 0.3s ease;
        }

        .impact-card:hover {
            transform: translateY(-5px);
        }

        .impact-icon {
            font-size: 40px;
            margin-bottom: 15px;
        }

        .impact-number {
            font-size: 32px;
            font-weight: 700;
            color: var(--ibm-teal);
            margin-bottom: 10px;
        }

        .impact-text {
            font-size: 16px;
            color: #8C94A7;
        }

        /* Footer */
        footer {
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            border-top: 1px solid rgba(255, 255, 255, 0.1);
            padding: 30px 0;
            margin-top: 50px;
        }

        .footer-content {
            width: 100vw;
            margin: 0 auto;
            padding: 0 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-sizing: border-box;
        }

        .footer-links {
            display: flex;
            gap: 20px;
        }

        .footer-link {
            color: #8C94A7;
            text-decoration: none;
            font-size: 14px;
            transition: color 0.3s;
        }

        .footer-link:hover {
            color: white;
        }

        /* Animations */
        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }
        .pulse {
            animation: pulse 2s infinite;
        }

        /* Responsive */
        @media (max-width: 1200px) {
            .main-grid {
                grid-template-columns: 1fr;
                height: auto;
            }
            .skills-panel, .status-panel {
                display: none;
            }
        }

        @media (max-width: 768px) {
            .header-content {
                flex-direction: column;
                gap: 15px;
            }

            .footer-content {
                flex-direction: column;
                gap: 15px;
                text-align: center;
            }

            .impact-grid {
                grid-template-columns: 1fr;
            }
        }
    </style>
</head>
<body>
    <header>
        <div class="header-content">
            <div class="logo">
                <div class="logo-icon">C</div>
                <div>
                    <div class="logo-text">CORA</div>
                    <div class="logo-subtitle">Powered by Watsonx Orchestrate</div>
                </div>
                <div class="watsonx-badge">watsonx</div>
            </div>
            <div class="nav-buttons">
                <button class="btn btn-secondary" onclick="showBusinessImpact()">Business Impact</button>
                <button class="btn btn-primary" onclick="showUseCases()">Use Cases</button>
                <button class="btn btn-teal" onclick="configureSearchApi()">Configure Search</button>
                <button class="btn btn-teal" onclick="configureWatsonx()">Connect Watsonx</button>
                <span id="watsonxStatus" style="margin-left:12px;font-size:13px;color:#8C94A7">Watsonx: disconnected</span>
            </div>
        </div>
    </header>
    <div class="container">
        <!-- Main Content Grid -->
        <div class="main-grid">
            <!-- Skills Panel -->
            <div class="panel skills-panel">
                <div class="panel-header">
                    <div class="panel-title">Digital Skills</div>
                </div>
                <div class="skills-grid">
                    <div class="skill-card active" onclick="activateSkill(this, 'workflow-orchestration')">
                        <div class="skill-icon">🔄</div>
                        <div class="skill-name">Workflow Orchestration</div>
                        <div class="skill-desc">Coordinate tasks across multiple systems and tools</div>
                    </div>
                    <div class="skill-card" onclick="activateSkill(this, 'document-automation')">
                        <div class="skill-icon">📄</div>
                        <div class="skill-name">Document Automation</div>
                        <div class="skill-desc">Generate reports, summaries, and process documents</div>
                    </div>
                    <div class="skill-card" onclick="activateSkill(this, 'data-processing')">
                        <div class="skill-icon">📊</div>
                        <div class="skill-name">Data Processing</div>
                        <div class="skill-desc">Analyze and transform complex datasets automatically</div>
                    </div>
                    <div class="skill-card" onclick="activateSkill(this, 'customer-service')">
                        <div class="skill-icon">💬</div>
                        <div class="skill-name">Customer Service</div>
                        <div class="skill-desc">Automate responses and ticket routing</div>
                    </div>
                    <div class="skill-card" onclick="activateSkill(this, 'hr-onboarding')">
                        <div class="skill-icon">👥</div>
                        <div class="skill-name">HR Onboarding</div>
                        <div class="skill-desc">Streamline employee onboarding processes</div>
                    </div>
                    <div class="skill-card" onclick="activateSkill(this, 'it-support')">
                        <div class="skill-icon">🔧</div>
                        <div class="skill-name">IT Support</div>
                        <div class="skill-desc">Automate ticket resolution and system monitoring</div>
                    </div>
                </div>
            </div>
            <!-- Chat Panel -->
            <div class="panel chat-panel">
                <div class="chat-container">
                    <div class="chat-messages" id="chatMessages">
                        <div class="message cora">
                            <div class="message-bubble">
                                <strong>Welcome to CORA powered by Watsonx Orchestrate!</strong><br><br>
                                I'm designed to transform how you work by automating repetitive tasks and orchestrating workflows across your tools. I can help you:<br><br>
                                • 🤖 Automate multi-step business processes<br>
                                • 📊 Generate intelligent reports and summaries<br>
                                • 🔄 Coordinate workflows across systems<br>
                                • 💼 Handle routine customer service tasks<br>
                                • 👥 Streamline HR and onboarding processes<br><br>
                                What repetitive task would you like me to automate today?
                            </div>
                            <div class="message-time">Just now</div>
                        </div>
                    </div>
                    <div class="chat-input-container">
                        <textarea class="chat-input" id="chatInput" placeholder="Describe a repetitive task you'd like to automate (e.g., 'Process monthly sales reports')"></textarea>
                        <div class="controls">
                            <button class="btn btn-primary btn-icon" onclick="sendMessage()">➤</button>
                            <button class="btn btn-secondary btn-icon" id="voiceBtn" onclick="toggleVoice()">🎤</button>
                        </div>
                    </div>
                    <div class="controls" style="margin-top: 15px;">
                        <button class="btn btn-secondary" onclick="clearChat()">Clear Chat</button>
                        <button class="btn btn-teal" onclick="showDemoWorkflow()">Demo Workflow</button>
                        <button class="btn btn-primary" onclick="showAutomationIdeas()">Get Automation Ideas</button>
                        <button class="btn btn-primary" onclick="generateEmailDraft()">Draft Email</button>
                        <button class="btn btn-teal" onclick="generateWeeklyReport()">Weekly Report (PDF)</button>
                    </div>
                </div>
            </div>
            <!-- Status Panel -->
            <div class="panel status-panel">
                <div class="panel-header">
                    <div class="panel-title">Orchestration Status</div>
                </div>
                <div class="status-item">
                    <div class="status-label">WORKFLOWS ACTIVE</div>
                    <div class="status-value">12/15</div>
                </div>
                <div class="status-item">
                    <div class="status-label">TASKS AUTOMATED TODAY</div>
                    <div class="status-value" id="tasksCompleted">47</div>
                </div>
                <div class="status-item">
                    <div class="status-label">TIME SAVED</div>
                    <div class="status-value" id="timeSaved">18.5 hours</div>
                </div>
                <div class="status-item">
                    <div class="status-label">PROCESS EFFICIENCY</div>
                    <div class="status-value">+42%</div>
                </div>
                <div class="workflow-visual">
                    <div class="panel-title" style="margin-bottom: 15px;">Active Workflow</div>
                    <div class="workflow-step">
                        <div class="step-number">1</div>
                        <div>Receive Task Request</div>
                    </div>
                    <div class="workflow-step">
                        <div class="step-number">2</div>
                        <div>Analyze with Watsonx.ai</div>
                    </div>
                    <div class="workflow-step">
                        <div class="step-number">3</div>
                        <div>Orchestrate Tools & Systems</div>
                    </div>
                    <div class="workflow-step">
                        <div class="step-number">4</div>
                        <div>Execute Digital Skills</div>
                    </div>
                    <div class="workflow-step">
                        <div class="step-number">5</div>
                        <div>Deliver Results & Notify</div>
                    </div>
                </div>
            </div>
        </div>
        <!-- Business Impact Section -->
        <div class="impact-section">
            <div class="impact-title">Business Impact with Watsonx Orchestrate</div>
            <div class="impact-grid">
                <div class="impact-card">
                    <div class="impact-icon">⏱️</div>
                    <div class="impact-number">65%</div>
                    <div class="impact-text">Reduction in manual task time</div>
                </div>
                <div class="impact-card">
                    <div class="impact-icon">💰</div>
                    <div class="impact-number">$2.3M</div>
                    <div class="impact-text">Annual cost savings</div>
                </div>
                <div class="impact-card">
                    <div class="impact-icon">🚀</div>
                    <div class="impact-number">3.2x</div>
                    <div class="impact-text">Faster process completion</div>
                </div>
                <div class="impact-card">
                    <div class="impact-icon">👨‍💼</div>
                    <div class="impact-number">15 hours</div>
                    <div class="impact-text">Weekly focus time per employee</div>
                </div>
            </div>
        </div>
    </div>
    <footer>
        <div class="footer-content">
            <div class="footer-links">
                <a href="#" class="footer-link" onclick="showBusinessImpact()">Business Value</a>
                <a href="#" class="footer-link" onclick="showUseCases()">Use Cases</a>
                <a href="#" class="footer-link" onclick="showIntegration()">Integration</a>
                <a href="#" class="footer-link" onclick="showSecurity()">Security & Compliance</a>
            </div>
            <div class="footer-text">
                <span>CORA &mdash; Transforming work through AI automation</span>
            </div>
        </div>
    </footer>
    <!-- JavaScript unchanged from your prior code: paste here as is -->
    <script>
        // (Insert your script)
        // For brevity and so this answer fits, reuse your existing <script> from your post
        
        // No JavaScript changes are needed for fullscreen/layout.
    </script>
    <!-- html2pdf bundle: converts HTML to PDF client-side -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.9.3/html2pdf.bundle.min.js"></script>
</body>
</html>
