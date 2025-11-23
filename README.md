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

        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'IBM Plex Sans', 'Segoe UI', system-ui, sans-serif;
        }

        body {
            background: linear-gradient(135deg, var(--ibm-gray-90) 0%, var(--ibm-gray-80) 100%);
            color: white;
            min-height: 100vh;
            line-height: 1.6;
        }

        .container {
            max-width: 1400px;
            margin: 0 auto;
            padding: 20px;
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
            max-width: 1400px;
            margin: 0 auto;
            padding: 0 20px;
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
            margin-bottom: 40px;
        }

        /* Panel Styles */
        .panel {
            background: rgba(255, 255, 255, 0.05);
            backdrop-filter: blur(10px);
            border-radius: var(--radius);
            border: 1px solid rgba(255, 255, 255, 0.1);
            padding: 25px;
            overflow: hidden;
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
            height: 600px;
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

        .message.casey .message-bubble {
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
            max-width: 1400px;
            margin: 0 auto;
            padding: 0 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
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

    <script>
        // State management
        let isListening = false;
        let tasksCompleted = 47;
        let timeSaved = 18.5;
        let currentSkill = 'workflow-orchestration';
        let conversationHistory = [];

        // Search configuration
        const SEARCH_CONFIG = {
            apiKey: '136dd37c758449132a404a6384a803c3242d5dce93961f5d3e89583fed8119f1',
            endpoint: 'https://serpapi.com/search',
            provider: 'serpapi'
        };

        // Initialize the application
        function init() {
            updateCounters();
            
            // Add welcome message
            setTimeout(() => {
                addMessage('cora', `Try describing a repetitive task you'd like to automate. For example:<br><br>
• "Process monthly sales reports from our CRM and database"<br>
• "Onboard new employees by setting up accounts across systems"<br>
• "Generate weekly performance dashboards automatically"<br>
• "Route customer support tickets based on complexity"`);
            }, 2000);
        }

        // Configure search API
        function configureSearchApi() {
            const mode = prompt('Configure search: type "serpapi" to use current API key, or "proxy" to set a custom endpoint:', 'serpapi');
            if (!mode) return;
            
            if (mode.toLowerCase() === 'serpapi') {
                alert('Using SerpAPI with existing key. Search functionality is ready.');
            } else if (mode.toLowerCase() === 'proxy') {
                const url = prompt('Enter search proxy endpoint:', SEARCH_CONFIG.endpoint);
                if (url) {
                    SEARCH_CONFIG.endpoint = url;
                    SEARCH_CONFIG.provider = 'proxy';
                    alert('Search proxy endpoint configured.');
                }
            }
        }

        // Connect to Watsonx
        function configureWatsonx() {
            const apiKey = prompt('');
            if (!apiKey) return;
            
            // Simulate API connection
            setTimeout(() => {
                document.getElementById('watsonxStatus').textContent = 'Watsonx: connected';
                alert('Successfully connected to Watsonx!');
            }, 1000);
        }

        // Activate a skill
        function activateSkill(card, skillId) {
            // Remove active class from all cards
            document.querySelectorAll('.skill-card').forEach(c => c.classList.remove('active'));
            // Add active class to clicked card
            card.classList.add('active');
            
            currentSkill = skillId;
            const skill = digitalSkills[skillId];
            
            addMessage('cora', `🔄 <strong>${skill.name} Activated</strong><br>${skill.description}<br><br>${skill.useCase}`);
        }

        // Digital Skills definitions
        const digitalSkills = {
            'workflow-orchestration': {
                name: 'Workflow Orchestration',
                description: 'Coordinate tasks across multiple systems and tools',
                useCase: 'I can automate multi-step processes like monthly reporting, data synchronization between systems, or cross-departmental approval workflows.'
            },
            'document-automation': {
                name: 'Document Automation',
                description: 'Generate reports, summaries, and process documents',
                useCase: 'I can automatically create weekly performance reports, contract summaries, or compliance documentation from your data sources.'
            },
            'data-processing': {
                name: 'Data Processing',
                description: 'Analyze and transform complex datasets automatically',
                useCase: 'I can process sales data, customer feedback, or operational metrics to provide actionable insights without manual intervention.'
            },
            'customer-service': {
                name: 'Customer Service',
                description: 'Automate responses and ticket routing',
                useCase: 'I can automatically categorize support tickets, provide initial responses, and route complex issues to the right team members.'
            },
            'hr-onboarding': {
                name: 'HR Onboarding',
                description: 'Streamline employee onboarding processes',
                useCase: 'I can coordinate account creation, system access, training scheduling, and documentation across HR, IT, and department systems.'
            },
            'it-support': {
                name: 'IT Support',
                description: 'Automate ticket resolution and system monitoring',
                useCase: 'I can automatically handle common IT requests, monitor system health, and escalate issues when needed.'
            }
        };

        // Add a message to the chat
        function addMessage(sender, text) {
            const chatMessages = document.getElementById('chatMessages');
            const messageDiv = document.createElement('div');
            messageDiv.className = `message ${sender}`;
            
            const time = new Date().toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'});
            
            messageDiv.innerHTML = `
                <div class="message-bubble">${text}</div>
                <div class="message-time">${time}</div>
            `;
            
            chatMessages.appendChild(messageDiv);
            chatMessages.scrollTop = chatMessages.scrollHeight;
            
    // Update counters for automated tasks
    if (sender === 'cora' && (text.includes('automated') || text.includes('completed') || text.includes('generated'))) {
        tasksCompleted++;
        timeSaved += 0.5;
        updateCounters();
    }

    // Save plain-text conversation entry for reporting
    try {
        conversationHistory.push({
            sender,
            text: stripHtml(text),
            time: new Date().toISOString()
        });
        // keep history reasonably sized (last 200 messages)
        if (conversationHistory.length > 200) conversationHistory.shift();
    } catch (e) {
        console.warn('Could not save conversation history', e);
    }
}

// Helper to strip HTML when saving messages for the report
function stripHtml(html) {
    const tmp = document.createElement('div');
    tmp.innerHTML = html;
    return tmp.textContent || tmp.innerText || '';
}

        // Update counters
        function updateCounters() {
            document.getElementById('tasksCompleted').textContent = tasksCompleted;
            document.getElementById('timeSaved').textContent = timeSaved.toFixed(1) + ' hours';
        }

        // Send a message
        function sendMessage() {
            const input = document.getElementById('chatInput');
            const text = input.value.trim();
            
            if (text) {
                addMessage('user', text);
                input.value = '';
                
                // Process the user input
                setTimeout(() => {
                    processUserInput(text);
                }, 1000);
            }
        }

        // Toggle voice input
        function toggleVoice() {
            const voiceBtn = document.getElementById('voiceBtn');
            isListening = !isListening;
            
            if (isListening) {
                voiceBtn.innerHTML = '🔴';
                voiceBtn.classList.add('pulse');
                addMessage('cora', '🎤 <strong>Voice Input Active</strong><br>I\'m listening... Describe the repetitive task you\'d like to automate.');
                
                // Simulate voice input for demo
                setTimeout(() => {
                    if (isListening) {
                        const demoCommands = [
                            "Process monthly sales reports from our CRM and generate summaries",
                            "Onboard new employees by setting up accounts in all our systems",
                            "Create a workflow for customer support ticket routing",
                            "Automate our weekly performance dashboard generation"
                        ];
                        const randomCommand = demoCommands[Math.floor(Math.random() * demoCommands.length)];
                        document.getElementById('chatInput').value = randomCommand;
                        sendMessage();
                        toggleVoice(); // Turn off listening
                    }
                }, 3000);
            } else {
                voiceBtn.innerHTML = '🎤';
                voiceBtn.classList.remove('pulse');
                addMessage('cora', 'Voice input stopped.');
            }
        }

        // Process user input and generate responses
        function processUserInput(input) {
            const lowerInput = input.toLowerCase();
            let response = '';
            
            // Handle search requests
            if (lowerInput.includes('search') || lowerInput.includes('find') || lowerInput.includes('look up')) {
                const query = input.replace(/search|find|look up/gi, '').trim();
                response = `🔍 <strong>Searching for:</strong> ${query}<br><br>I'll search the web for the most relevant information about this topic.`;
                
                // Simulate search results
                setTimeout(() => {
                    const searchResults = performWebSearch(query);
                    addMessage('cora', searchResults);
                }, 2000);
            }
            // Handle workflow automation requests
            else if (lowerInput.includes('process') || lowerInput.includes('automate') || lowerInput.includes('workflow')) {
                if (lowerInput.includes('report') || lowerInput.includes('dashboard')) {
                    response = `📊 <strong>Document Automation Workflow Designed</strong><br><br>
I'll create an automated reporting workflow using Watsonx Orchestrate:<br><br>
1. <strong>Data Collection</strong>: Connect to your CRM, database, and analytics tools<br>
2. <strong>Processing</strong>: Use Watsonx.ai to analyze and summarize the data<br>
3. <strong>Document Generation</strong>: Create formatted reports with insights<br>
4. <strong>Distribution</strong>: Automatically email to stakeholders<br><br>
This will save approximately 6-8 hours of manual work per report. Would you like me to implement this workflow?`;
                    
                    setTimeout(() => {
                        tasksCompleted += 5;
                        timeSaved += 4;
                        updateCounters();
                    }, 3000);
                }
                else if (lowerInput.includes('onboard') || lowerInput.includes('employee')) {
                    response = `👥 <strong>HR Onboarding Workflow Designed</strong><br><br>
I'll orchestrate an automated employee onboarding process:<br><br>
1. <strong>Trigger</strong>: New hire data from HR system<br>
2. <strong>Account Creation</strong>: Automatically create accounts in Active Directory, email, and other systems<br>
3. <strong>Equipment Setup</strong>: Coordinate with IT for hardware provisioning<br>
4. <strong>Training Schedule</strong>: Automatically enroll in required training<br>
5. <strong>Documentation</strong>: Generate and route onboarding documents<br><br>
This eliminates 15+ manual steps and reduces onboarding time by 70%.`;
                }
                else if (lowerInput.includes('customer') || lowerInput.includes('support') || lowerInput.includes('ticket')) {
                    response = `💬 <strong>Customer Service Automation Designed</strong><br><br>
I'll create an intelligent ticket routing and response system:<br><br>
1. <strong>Ticket Analysis</strong>: Use Watsonx NLP to understand customer issues<br>
2. <strong>Automated Responses</strong>: Handle common queries instantly<br>
3. <strong>Smart Routing</strong>: Route complex issues to appropriate specialists<br>
4. <strong>Escalation</strong>: Automatically escalate urgent matters<br>
5. <strong>Follow-up</strong>: Schedule and send follow-up communications<br><br>
This reduces response time by 85% and frees agents for high-value interactions.`;
                }
                else {
                    response = `🔄 <strong>Workflow Analysis Complete</strong><br><br>
Based on your request, I can design a Watsonx Orchestrate workflow to automate this process. The workflow will:<br><br>
• Connect to your existing systems and tools<br>
• Use AI to process and analyze information<br>
• Coordinate tasks across departments if needed<br>
• Provide real-time status updates<br>
• Deliver completed work automatically<br><br>
This typically saves 4-6 hours per week in manual effort. Should I proceed with implementation?`;
                }
            }
            // Handle data processing requests
            else if (lowerInput.includes('data') || lowerInput.includes('analyze') || lowerInput.includes('process')) {
                response = `📈 <strong>Data Processing Pipeline Created</strong><br><br>
I'll set up an automated data processing workflow:<br><br>
1. <strong>Data Ingestion</strong>: Connect to your data sources (SQL, APIs, files)<br>
2. <strong>Cleaning & Transformation</strong>: Use Watsonx to standardize and prepare data<br>
3. <strong>Analysis</strong>: Apply AI models to identify patterns and insights<br>
4. <strong>Visualization</strong>: Generate dashboards and reports<br>
5. <strong>Alerting</strong>: Notify stakeholders of key findings<br><br>
This eliminates manual data manipulation and provides real-time insights.`;
            }
            // Handle capability questions
            else if (lowerInput.includes('what can you do') || lowerInput.includes('capabilities')) {
                response = `🤖 <strong>Watsonx Orchestrate Capabilities</strong><br><br>
I'm powered by IBM Watsonx Orchestrate to transform how you work:<br><br>
• <strong>Automate Repetitive Tasks</strong>: Handle routine work across systems<br>
• <strong>Orchestrate Workflows</strong>: Coordinate multi-step processes seamlessly<br>
• <strong>Integrate Tools</strong>: Connect CRM, ERP, HR systems, and more<br>
• <strong>Intelligent Processing</strong>: Use AI to understand and process information<br>
• <strong>Scale Operations</strong>: Handle increasing workload without adding staff<br><br>
I help employees focus on high-value work by eliminating manual, repetitive tasks.`;
            }
            // Handle business value questions
            else if (lowerInput.includes('save') || lowerInput.includes('time') || lowerInput.includes('efficiency')) {
                response = `💰 <strong>Business Impact Analysis</strong><br><br>
Based on similar implementations, Watsonx Orchestrate typically delivers:<br><br>
• <strong>65-80% reduction</strong> in time spent on repetitive tasks<br>
• <strong>3-5x faster</strong> process completion<br>
• <strong>50-70% cost reduction</strong> for automated processes<br>
• <strong>10-15 hours weekly</strong> regained per employee for strategic work<br>
• <strong>Improved accuracy</strong> with elimination of manual errors<br><br>
These efficiencies directly impact your bottom line and employee satisfaction.`;
            }
            // Default response
            else {
                response = `🔍 <strong>Analyzing Your Request</strong><br><br>
I understand you're looking to "${input}". Using Watsonx Orchestrate, I can design an automated solution that:<br><br>
1. Identifies the repetitive elements of this task<br>
2. Connects to relevant systems and data sources<br>
3. Applies AI to handle decision points<br>
4. Coordinates the workflow across tools if needed<br>
5. Delivers completed work automatically<br><br>
This typically saves 70-80% of the time currently spent on manual work. Would you like me to design this automation?`;
            }
            
            addMessage('cora', response);
        }

        // Perform web search (simulated with SerpAPI)
        function performWebSearch(query) {
            // In a real implementation, this would call the SerpAPI
            // For demo purposes, we'll return simulated results
            return `🔍 <strong>Search Results for "${query}"</strong><br><br>
<strong>1. Comprehensive Guide to ${query}</strong><br>
• Source: Industry Publication<br>
• Summary: Latest trends and best practices in ${query}<br><br>

<strong>2. Recent Developments in ${query}</strong><br>
• Source: Research Journal<br>
• Summary: Analysis of recent advancements and market changes<br><br>

<strong>3. Practical Applications</strong><br>
• Source: Professional Association<br>
• Summary: Case studies and implementation examples<br><br>

<em>These results are simulated. In production, I would use SerpAPI with your API key to fetch real-time search results.</em>`;
        }

        // Generate email draft
        function generateEmailDraft() {
            const subject = prompt('Enter email subject:', 'Meeting Update');
            if (!subject) return;
            
            const recipients = prompt('Enter recipients:', 'team@company.com');
            const body = prompt('Enter email body:', 'Hello team,\n\nI wanted to provide an update on our recent discussions.\n\nBest regards,\nCORA Assistant');
            
            const emailContent = `📧 <strong>Email Draft Generated</strong><br><br>
<strong>To:</strong> ${recipients}<br>
<strong>Subject:</strong> ${subject}<br><br>
${body.replace(/\n/g, '<br>')}<br><br>
<em>Ready to send? Copy this content to your email client.</em>`;
            
            addMessage('cora', emailContent);
        }

        // Generate weekly performance report and download as PDF
        async function generateWeeklyReport() {
            // Build a simple report HTML
            const reportTitle = `Weekly Performance Report — CORA`;
            const now = new Date();
            const weekRange = `${new Date(now.getFullYear(), now.getMonth(), now.getDate()-6).toLocaleDateString()} - ${now.toLocaleDateString()}`;
            
            // Aggregate some stats
            const totalTasks = tasksCompleted;
            const totalHoursSaved = timeSaved.toFixed(1);
            const activeSkill = (digitalSkills[currentSkill] && digitalSkills[currentSkill].name) || currentSkill;
            
            // Build conversation snippet (last 30)
            const convo = conversationHistory.slice(-30).map(c => {
                const when = new Date(c.time).toLocaleString();
                return `<tr><td style="vertical-align:top;padding:6px 8px;border-bottom:1px solid #eee;"><strong>${escapeHtml(c.sender)}</strong><br><small style="color:#666">${when}</small></td><td style="padding:6px 8px;border-bottom:1px solid #eee;">${escapeHtml(c.text)}</td></tr>`;
            }).join('');
            
            const reportHtml = `
                <div style="font-family:Arial, Helvetica, sans-serif;color:#222;padding:20px;">
                    <h1 style="color:#0062FF;margin-bottom:4px;">${escapeHtml(reportTitle)}</h1>
                    <div style="color:#555;margin-bottom:18px;">Period: <strong>${escapeHtml(weekRange)}</strong></div>
                    <section style="margin-bottom:18px;">
                        <h2 style="font-size:18px;color:#333;margin-bottom:8px;">Key Metrics</h2>
                        <table style="width:100%;border-collapse:collapse;">
                            <tr><td style="padding:8px 0;font-weight:600;width:40%;">Active Skill</td><td style="padding:8px 0;">${escapeHtml(activeSkill)}</td></tr>
                            <tr><td style="padding:8px 0;font-weight:600;">Tasks Automated (total)</td><td style="padding:8px 0;">${escapeHtml(String(totalTasks))}</td></tr>
                            <tr><td style="padding:8px 0;font-weight:600;">Estimated Hours Saved</td><td style="padding:8px 0;">${escapeHtml(String(totalHoursSaved))} hours</td></tr>
                        </table>
                    </section>
                    <section style="margin-bottom:18px;">
                        <h2 style="font-size:18px;color:#333;margin-bottom:8px;">Recent Conversation (last ${Math.min(30, conversationHistory.length)} messages)</h2>
                        <table style="width:100%;border-collapse:collapse;border:1px solid #e6e6e6;">
                            ${convo || '<tr><td style="padding:10px;">No conversation recorded.</td></tr>'}
                        </table>
                    </section>
                    <footer style="color:#888;font-size:12px;margin-top:20px;">
                        Generated by CORA — Watsonx Orchestrate UI
                    </footer>
                </div>
            `;
            
            // Create a container element for html2pdf to render
            let container = document.getElementById('reportContent');
            if (!container) {
                container = document.createElement('div');
                container.id = 'reportContent';
                container.style.display = 'none';
                document.body.appendChild(container);
            }
            container.innerHTML = reportHtml;
            
            // Use html2pdf to generate PDF and trigger download
            try {
                const opt = {
                    margin:       10,
                    filename:     `CORA-weekly-report-${now.toISOString().slice(0,10)}.pdf`,
                    image:        { type: 'jpeg', quality: 0.98 },
                    html2canvas:  { scale: 2, useCORS: true },
                    jsPDF:        { unit: 'mm', format: 'a4', orientation: 'portrait' }
                };
                // html2pdf requires the global function to be available (loaded via CDN)
                if (typeof html2pdf === 'undefined') {
                    alert('PDF library not loaded. Please check your connection and try again.');
                    return;
                }
                await html2pdf().set(opt).from(container).save();
            } catch (err) {
                console.error('generateWeeklyReport error', err);
                alert('Failed to generate PDF: ' + (err.message || err));
            }
        }
        
        // utility to escape text for HTML
        function escapeHtml(str) {
            return String(str)
                .replace(/&/g, '&amp;')
                .replace(/</g, '&lt;')
                .replace(/>/g, '&gt;')
                .replace(/"/g, '&quot;')
                .replace(/'/g, '&#39;');
        }

        // Show business impact
        function showBusinessImpact() {
            addMessage('cora', `📈 <strong>Business Value of Watsonx Orchestrate</strong><br><br>
<strong>Quantifiable Benefits:</strong><br>
• 65-80% reduction in manual task time<br>
• 3-5x faster process completion<br>
• 50-70% lower process costs<br>
• 10-15 hours weekly focus time per employee<br>
• 90%+ reduction in manual errors<br><br>
<strong>Strategic Impact:</strong><br>
• Employees focus on innovation and customer value<br>
• Faster response to business opportunities<br>
• Consistent process execution<br>
• Scalable operations without proportional headcount growth<br>
• Enhanced compliance through automated auditing<br><br>
These improvements typically deliver 200-400% ROI in the first year.`);
        }

        // Show use cases
        function showUseCases() {
            addMessage('cora', `🏢 <strong>Enterprise Use Cases</strong><br><br>
<strong>Sales & Marketing:</strong><br>
• Automated lead scoring and routing<br>
• Campaign performance reporting<br>
• Customer segmentation updates<br>
• Sales forecast generation<br><br>
<strong>Customer Service:</strong><br>
• Intelligent ticket routing<br>
• Automated response generation<br>
• Customer sentiment analysis<br>
• Service level monitoring<br><br>
<strong>HR & Operations:</strong><br>
• Employee onboarding workflows<br>
• Performance review automation<br>
• Compliance reporting<br>
• Vendor management processes<br><br>
<strong>Finance:</strong><br>
• Invoice processing automation<br>
• Expense report validation<br>
• Financial reporting<br>
• Audit preparation<br><br>
Each use case typically saves 5-20 hours per week in manual effort.`);
        }

        // Show integration options
        function showIntegration() {
            addMessage('cora', `🔗 <strong>System Integration</strong><br><br>
Watsonx Orchestrate connects seamlessly with your existing tools:<br><br>
<strong>CRM Systems:</strong> Salesforce, HubSpot, Dynamics 365<br>
<strong>ERP Systems:</strong> SAP, Oracle, Microsoft Dynamics<br>
<strong>Communication:</strong> Slack, Microsoft Teams, Email<br>
<strong>Productivity:</strong> Microsoft 365, Google Workspace<br>
<strong>Databases:</strong> SQL Server, Oracle, MongoDB<br>
<strong>APIs:</strong> REST, SOAP, GraphQL<br>
<strong>Cloud Services:</strong> AWS, Azure, Google Cloud<br><br>
I can orchestrate workflows across these systems without requiring custom coding for each integration.`);
        }

        // Show security information
        function showSecurity() {
            addMessage('cora', `🔒 <strong>Security & Compliance</strong><br><br>
Watsonx Orchestrate is enterprise-ready with:<br><br>
• <strong>End-to-end encryption</strong> for all data<br>
• <strong>Role-based access control</strong> for workflows<br>
• <strong>Compliance with regulations</strong> like GDPR, HIPAA, SOC2<br>
• <strong>Audit trails</strong> for all automated actions<br>
• <strong>Data residency</strong> options for global deployments<br>
• <strong>Private cloud</strong> deployment options available<br><br>
Your business processes remain secure while achieving automation benefits.`);
        }

        // Show demo workflow
        function showDemoWorkflow() {
            addMessage('cora', `🎬 <strong>Demo: Monthly Sales Report Automation</strong><br><br>
Let me show how Watsonx Orchestrate transforms a common business process:<br><br>
<strong>BEFORE (Manual Process):</strong><br>
1. Export data from CRM (30 min)<br>
2. Download sales figures from database (15 min)<br>
3. Combine and clean data in Excel (45 min)<br>
4. Create charts and analysis (60 min)<br>
5. Write summary and insights (30 min)<br>
6. Format report and distribute (15 min)<br>
<strong>Total: 3+ hours monthly</strong><br><br>
<strong>AFTER (Watsonx Orchestrate):</strong><br>
1. Workflow triggers automatically on schedule<br>
2. Data gathered from all sources simultaneously<br>
3. AI analyzes trends and generates insights<br>
4. Professional report created automatically<br>
5. Distributed to stakeholders via email<br>
<strong>Total: 5 minutes (98% time savings)</strong><br><br>
This is just one example of how I transform repetitive work.`);
        }

        // Show automation ideas
        function showAutomationIdeas() {
            addMessage('cora', `💡 <strong>Automation Opportunities</strong><br><br>
Based on common business processes, here are tasks I can automate:<br><br>
<strong>High-Impact Opportunities:</strong><br>
• Monthly/quarterly reporting processes<br>
• Customer onboarding workflows<br>
• Employee offboarding checklists<br>
• Data synchronization between systems<br>
• Invoice processing and approval<br><br>
<strong>Quick Wins:</strong><br>
• Meeting scheduling and follow-ups<br>
• Social media posting and monitoring<br>
• Expense report validation<br>
• Newsletter creation and distribution<br>
• Database cleanup and maintenance<br><br>
Which of these would have the biggest impact on your team's productivity?`);
        }

        // Clear chat history
        function clearChat() {
            const chatMessages = document.getElementById('chatMessages');
            chatMessages.innerHTML = '';
            addMessage('cora', `🔄 <strong>Chat Cleared</strong><br><br>
Welcome back! I'm CORA, powered by IBM Watsonx Orchestrate. I'm here to help automate repetitive tasks and orchestrate workflows across your tools.<br><br>
What repetitive process would you like me to transform today?`);
        }

        // Handle Enter key in chat input
        document.getElementById('chatInput').addEventListener('keypress', function(e) {
            if (e.key === 'Enter' && !e.shiftKey) {
                e.preventDefault();
                sendMessage();
            }
        });

        // Initialize the application
        window.onload = init;
    </script>

    <!-- html2pdf bundle: converts HTML to PDF client-side -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.9.3/html2pdf.bundle.min.js"></script>
</body>
</html>
