


<img width="1348" height="595" alt="WhatsApp Image 2026-05-20 at 20 56 44" src="https://github.com/user-attachments/assets/100f47cb-125d-4050-a6a6-270974714757" />


---

<img width="1347" height="593" alt="WhatsApp Image 2026-05-20 at 20 56 44 (1)" src="https://github.com/user-attachments/assets/6481cd40-cebc-440b-b580-585237c7efdc" />

[projeto_Gerenciador de Tarefas.html](https://github.com/user-attachments/files/28079437/projeto_estudo_de_caso.1.html)
<!-- 
Prompt: Faça um app de gerenciamento de tarefas (estilo Trello simplificado), com:
Frontend Interface responsiva com HTML, CSS e JavaScript e com Dashboard com estatísticas visuais das tarefas
-->

<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Gerenciador de Tarefas com Dashboard</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        :root {
            --primary: #667eea;
            --secondary: #764ba2;
            --success: #48bb78;
            --warning: #ed8936;
            --danger: #f56565;
            --light: #f7fafc;
            --dark: #2d3748;
            --border: #e2e8f0;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, var(--primary) 0%, var(--secondary) 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
        }

        header {
            text-align: center;
            color: white;
            margin-bottom: 40px;
        }

        header h1 {
            font-size: 2.5rem;
            margin-bottom: 10px;
        }

        header p {
            font-size: 1.1rem;
            opacity: 0.9;
        }

        nav {
            display: flex;
            gap: 10px;
            margin-bottom: 30px;
            background: white;
            padding: 15px;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            flex-wrap: wrap;
        }

        .nav-btn {
            padding: 10px 20px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 1rem;
            font-weight: 600;
            transition: all 0.3s ease;
            background: var(--light);
            color: var(--dark);
        }

        .nav-btn:hover,
        .nav-btn.active {
            background: var(--primary);
            color: white;
        }

        .section {
            display: none;
            animation: fadeIn 0.3s ease;
        }

        .section.active {
            display: block;
        }

        @keyframes fadeIn {
            from {
                opacity: 0;
                transform: translateY(10px);
            }
            to {
                opacity: 1;
                transform: translateY(0);
            }
        }

        /* Dashboard */
        .dashboard {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .card {
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            transition: transform 0.3s ease;
        }

        .card:hover {
            transform: translateY(-5px);
        }

        .card-header {
            display: flex;
            align-items: center;
            gap: 10px;
            margin-bottom: 10px;
        }

        .card-icon {
            font-size: 2rem;
        }

        .card-title {
            font-size: 0.9rem;
            color: #718096;
            text-transform: uppercase;
        }

        .card-value {
            font-size: 2.5rem;
            font-weight: bold;
            color: var(--primary);
        }

        .charts-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .chart-container {
            background: white;
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            position: relative;
            height: 300px;
        }

        /* Tarefas */
        .tasks-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
            flex-wrap: wrap;
            gap: 10px;
        }

        .btn {
            padding: 10px 20px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 1rem;
            font-weight: 600;
            transition: all 0.3s ease;
        }

        .btn-primary {
            background: var(--primary);
            color: white;
        }

        .btn-primary:hover {
            background: #5568d3;
        }

        .btn-success {
            background: var(--success);
            color: white;
        }

        .btn-danger {
            background: var(--danger);
            color: white;
        }

        .btn-warning {
            background: var(--warning);
            color: white;
        }

        .filter-group {
            display: flex;
            gap: 10px;
            flex-wrap: wrap;
        }

        select {
            padding: 10px;
            border: 1px solid var(--border);
            border-radius: 8px;
            background: white;
            cursor: pointer;
        }

        .tasks-list {
            display: grid;
            gap: 15px;
        }

        .task-item {
            background: white;
            padding: 15px;
            border-radius: 8px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
            border-left: 5px solid var(--primary);
            transition: all 0.3s ease;
        }

        .task-item.completed {
            opacity: 0.7;
            border-left-color: var(--success);
        }

        .task-item.high-priority {
            border-left-color: var(--danger);
        }

        .task-item.medium-priority {
            border-left-color: var(--warning);
        }

        .task-item.low-priority {
            border-left-color: var(--success);
        }

        .task-info {
            flex: 1;
        }

        .task-title {
            font-size: 1.1rem;
            font-weight: 600;
            color: var(--dark);
            margin-bottom: 5px;
        }

        .task-item.completed .task-title {
            text-decoration: line-through;
            color: #a0aec0;
        }

        .task-meta {
            display: flex;
            gap: 15px;
            font-size: 0.9rem;
            color: #718096;
        }

        .badge {
            display: inline-block;
            padding: 4px 12px;
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: 600;
        }

        .badge-high {
            background: #fed7d7;
            color: #742a2a;
        }

        .badge-medium {
            background: #feebc8;
            color: #7c2d12;
        }

        .badge-low {
            background: #c6f6d5;
            color: #22543d;
        }

        .badge-pending {
            background: #bee3f8;
            color: #2c5282;
        }

        .badge-completed {
            background: #c6f6d5;
            color: #22543d;
        }

        .task-actions {
            display: flex;
            gap: 8px;
        }

        .btn-small {
            padding: 6px 12px;
            font-size: 0.9rem;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            transition: all 0.3s ease;
        }

        .btn-edit {
            background: var(--primary);
            color: white;
        }

        .btn-complete {
            background: var(--success);
            color: white;
        }

        .btn-delete {
            background: var(--danger);
            color: white;
        }

        /* Modal */
        .modal {
            display: none;
            position: fixed;
            z-index: 1000;
            left: 0;
            top: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            animation: fadeIn 0.3s ease;
        }

        .modal.active {
            display: flex;
            align-items: center;
            justify-content: center;
        }

        .modal-content {
            background: white;
            padding: 30px;
            border-radius: 10px;
            width: 90%;
            max-width: 500px;
            box-shadow: 0 10px 40px rgba(0, 0, 0, 0.2);
        }

        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .modal-title {
            font-size: 1.5rem;
            font-weight: bold;
            color: var(--dark);
        }

        .close-btn {
            background: none;
            border: none;
            font-size: 2rem;
            cursor: pointer;
            color: var(--dark);
        }

        .form-group {
            margin-bottom: 15px;
        }

        label {
            display: block;
            margin-bottom: 5px;
            font-weight: 600;
            color: var(--dark);
        }

        input,
        textarea,
        select {
            width: 100%;
            padding: 10px;
            border: 1px solid var(--border);
            border-radius: 8px;
            font-size: 1rem;
            font-family: inherit;
        }

        textarea {
            resize: vertical;
            min-height: 100px;
        }

        input:focus,
        textarea:focus,
        select:focus {
            outline: none;
            border-color: var(--primary);
            box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
        }

        .modal-footer {
            display: flex;
            gap: 10px;
            justify-content: flex-end;
            margin-top: 20px;
        }

        .btn-cancel {
            background: var(--light);
            color: var(--dark);
        }

        .btn-cancel:hover {
            background: var(--border);
        }

        /* Empty State */
        .empty-state {
            text-align: center;
            padding: 40px 20px;
            color: #718096;
        }

        .empty-state-icon {
            font-size: 3rem;
            margin-bottom: 10px;
        }

        .empty-state-title {
            font-size: 1.3rem;
            font-weight: 600;
            margin-bottom: 10px;
        }

        /* Responsivo */
        @media (max-width: 768px) {
            header h1 {
                font-size: 1.8rem;
            }

            .dashboard {
                grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
            }

            .task-item {
                flex-direction: column;
                align-items: flex-start;
            }

            .task-actions {
                width: 100%;
                margin-top: 10px;
            }

            .task-meta {
                flex-wrap: wrap;
            }

            .tasks-header {
                flex-direction: column;
                align-items: flex-start;
            }

            .filter-group {
                width: 100%;
            }

            select {
                width: 100%;
            }

            .btn-small {
                flex: 1;
            }
        }

        .success-message {
            position: fixed;
            top: 20px;
            right: 20px;
            background: var(--success);
            color: white;
            padding: 15px 20px;
            border-radius: 8px;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
            z-index: 2000;
            animation: slideIn 0.3s ease;
        }

        @keyframes slideIn {
            from {
                transform: translateX(400px);
            }
            to {
                transform: translateX(0);
            }
        }

        /* Footer */
        footer {
            text-align: center;
            color: white;
            margin-top: 40px;
            padding: 20px;
            opacity: 0.85;
            font-size: 0.95rem;
        }

        footer p {
            margin-bottom: 4px;
        }

        footer a {
            color: white;
            text-decoration: underline;
            text-underline-offset: 3px;
        }

        footer a:hover {
            opacity: 0.7;
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>📊 Gerenciador de Tarefas</h1>
            <p>Organize suas atividades e acompanhe sua produtividade</p>
        </header>

        <nav>
            <button class="nav-btn active" onclick="showSection('dashboard')">📈 Dashboard</button>
            <button class="nav-btn" onclick="showSection('tasks')">📋 Minhas Tarefas</button>
        </nav>

        <!-- DASHBOARD -->
        <section id="dashboard" class="section active">
            <div class="dashboard">
                <div class="card">
                    <div class="card-header">
                        <span class="card-icon">📝</span>
                    </div>
                    <div class="card-title">Total de Tarefas</div>
                    <div class="card-value" id="totalTasks">0</div>
                </div>

                <div class="card">
                    <div class="card-header">
                        <span class="card-icon">✅</span>
                    </div>
                    <div class="card-title">Concluídas</div>
                    <div class="card-value" id="completedTasks">0</div>
                </div>

                <div class="card">
                    <div class="card-header">
                        <span class="card-icon">⏳</span>
                    </div>
                    <div class="card-title">Pendentes</div>
                    <div class="card-value" id="pendingTasks">0</div>
                </div>

                <div class="card">
                    <div class="card-header">
                        <span class="card-icon">🔥</span>
                    </div>
                    <div class="card-title">Taxa de Conclusão</div>
                    <div class="card-value" id="completionRate">0%</div>
                </div>
            </div>

            <div class="charts-grid">
                <div class="chart-container">
                    <canvas id="priorityChart"></canvas>
                </div>
                <div class="chart-container">
                    <canvas id="statusChart"></canvas>
                </div>
            </div>
        </section>

        <!-- TAREFAS -->
        <section id="tasks" class="section">
            <div class="tasks-header">
                <button class="btn btn-primary" onclick="openModal()">➕ Nova Tarefa</button>
                <div class="filter-group">
                    <select id="filterStatus" onchange="filterTasks()">
                        <option value="">Status: Todos</option>
                        <option value="pending">Pendentes</option>
                        <option value="completed">Concluídas</option>
                    </select>
                    <select id="filterPriority" onchange="filterTasks()">
                        <option value="">Prioridade: Todas</option>
                        <option value="high">Alta</option>
                        <option value="medium">Média</option>
                        <option value="low">Baixa</option>
                    </select>
                </div>
            </div>

            <div id="tasksList" class="tasks-list"></div>
        </section>
    </div>

    <!-- MODAL -->
    <div id="taskModal" class="modal">
        <div class="modal-content">
            <div class="modal-header">
                <h2 class="modal-title" id="modalTitle">Nova Tarefa</h2>
                <button class="close-btn" onclick="closeModal()">&times;</button>
            </div>

            <form onsubmit="saveTask(event)">
                <div class="form-group">
                    <label for="taskTitle">Título *</label>
                    <input type="text" id="taskTitle" required>
                </div>

                <div class="form-group">
                    <label for="taskDescription">Descrição</label>
                    <textarea id="taskDescription"></textarea>
                </div>

                <div class="form-group">
                    <label for="taskPriority">Prioridade *</label>
                    <select id="taskPriority" required>
                        <option value="low">Baixa</option>
                        <option value="medium" selected>Média</option>
                        <option value="high">Alta</option>
                    </select>
                </div>

                <div class="form-group">
                    <label for="taskDate">Data de Vencimento</label>
                    <input type="date" id="taskDate">
                </div>

                <div class="modal-footer">
                    <button type="button" class="btn btn-cancel" onclick="closeModal()">Cancelar</button>
                    <button type="submit" class="btn btn-primary">Salvar Tarefa</button>
                </div>
            </form>
        </div>
    </div>

    <footer>
        <p>📊 Gerenciador de Tarefas &mdash; Estudo de Caso</p>
        <p>Desenvolvido com HTML, CSS &amp; JavaScript</p>
        <p style="margin-top: 10px;">Feito por
            <a href="https://github.com/Mauricioortiga/Engenharia-de-Prompt-e-Aplicacoes-em-IA/tree/main/Projeto" target="_blank">Maurício Cunha Ortiga Ferreira</a>
            e
            <a href="https://github.com/ezequiel-lindoso/Engenharia_de_Prompt_Aplicacoes_AI/tree/main/ProjetoModulo3" target="_blank">Ezequiel Ferreira Lindoso</a>
        </p>
    </footer>

    <script>
        // Database
        let tasks = JSON.parse(localStorage.getItem('tasks')) || [];
        let editingTaskId = null;
        let priorityChart = null;
        let statusChart = null;

        // Inicializar
        document.addEventListener('DOMContentLoaded', function() {
            updateDashboard();
            renderTasks();
            initCharts();
        });

        // Navegação
        function showSection(sectionId) {
            document.querySelectorAll('.section').forEach(s => s.classList.remove('active'));
            document.getElementById(sectionId).classList.add('active');

            document.querySelectorAll('.nav-btn').forEach(btn => btn.classList.remove('active'));
            event.target.classList.add('active');

            if (sectionId === 'dashboard') {
                updateDashboard();
                updateCharts();
            }
        }

        // Modal
        function openModal(taskId = null) {
            editingTaskId = taskId;
            const modal = document.getElementById('taskModal');
            const modalTitle = document.getElementById('modalTitle');

            if (taskId) {
                const task = tasks.find(t => t.id === taskId);
                document.getElementById('taskTitle').value = task.title;
                document.getElementById('taskDescription').value = task.description;
                document.getElementById('taskPriority').value = task.priority;
                document.getElementById('taskDate').value = task.dueDate || '';
                modalTitle.textContent = 'Editar Tarefa';
            } else {
                document.getElementById('taskTitle').value = '';
                document.getElementById('taskDescription').value = '';
                document.getElementById('taskPriority').value = 'medium';
                document.getElementById('taskDate').value = '';
                modalTitle.textContent = 'Nova Tarefa';
            }

            modal.classList.add('active');
        }

        function closeModal() {
            document.getElementById('taskModal').classList.remove('active');
            editingTaskId = null;
        }

        window.onclick = function(event) {
            const modal = document.getElementById('taskModal');
            if (event.target === modal) {
                closeModal();
            }
        };

        // Tarefas
        function saveTask(event) {
            event.preventDefault();

            const title = document.getElementById('taskTitle').value;
            const description = document.getElementById('taskDescription').value;
            const priority = document.getElementById('taskPriority').value;
            const dueDate = document.getElementById('taskDate').value;

            if (editingTaskId) {
                const task = tasks.find(t => t.id === editingTaskId);
                task.title = title;
                task.description = description;
                task.priority = priority;
                task.dueDate = dueDate;
                showNotification('Tarefa atualizada com sucesso!');
            } else {
                const newTask = {
                    id: Date.now(),
                    title,
                    description,
                    priority,
                    dueDate,
                    status: 'pending',
                    createdAt: new Date().toLocaleDateString('pt-BR')
                };
                tasks.push(newTask);
                showNotification('Tarefa criada com sucesso!');
            }

            saveTasks();
            closeModal();
            renderTasks();
            updateDashboard();
            updateCharts();
        }

        function deleteTask(taskId) {
            if (confirm('Tem certeza que deseja deletar essa tarefa?')) {
                tasks = tasks.filter(t => t.id !== taskId);
                saveTasks();
                renderTasks();
                updateDashboard();
                updateCharts();
                showNotification('Tarefa deletada!');
            }
        }

        function completeTask(taskId) {
            const task = tasks.find(t => t.id === taskId);
            task.status = task.status === 'pending' ? 'completed' : 'pending';
            saveTasks();
            renderTasks();
            updateDashboard();
            updateCharts();
            showNotification(task.status === 'completed' ? 'Tarefa concluída!' : 'Tarefa reaberta!');
        }

        function renderTasks() {
            const tasksList = document.getElementById('tasksList');
            let filteredTasks = [...tasks];

            const statusFilter = document.getElementById('filterStatus')?.value;
            const priorityFilter = document.getElementById('filterPriority')?.value;

            if (statusFilter) {
                filteredTasks = filteredTasks.filter(t => t.status === statusFilter);
            }
            if (priorityFilter) {
                filteredTasks = filteredTasks.filter(t => t.priority === priorityFilter);
            }

            if (filteredTasks.length === 0) {
                tasksList.innerHTML = `
                    <div class="empty-state">
                        <div class="empty-state-icon">📭</div>
                        <div class="empty-state-title">Nenhuma tarefa encontrada</div>
                        <p>Crie uma nova tarefa para começar!</p>
                    </div>
                `;
                return;
            }

            tasksList.innerHTML = filteredTasks.map(task => `
                <div class="task-item ${task.status === 'completed' ? 'completed' : ''} ${task.priority}-priority">
                    <div class="task-info">
                        <div class="task-title">${task.title}</div>
                        <div class="task-meta">
                            <span class="badge badge-${task.priority}">
                                ${task.priority === 'high' ? '🔴' : task.priority === 'medium' ? '🟡' : '🟢'} 
                                ${task.priority.charAt(0).toUpperCase() + task.priority.slice(1)}
                            </span>
                            <span class="badge badge-${task.status}">
                                ${task.status === 'completed' ? '✅ Concluída' : '⏳ Pendente'}
                            </span>
                            ${task.dueDate ? `<span>📅 ${new Date(task.dueDate).toLocaleDateString('pt-BR')}</span>` : ''}
                            <span>📍 ${task.createdAt}</span>
                        </div>
                        ${task.description ? `<p style="margin-top: 10px; color: #4a5568;">${task.description}</p>` : ''}
                    </div>
                    <div class="task-actions">
                        <button class="btn-small btn-complete" onclick="completeTask(${task.id})">
                            ${task.status === 'completed' ? '↩️ Reabrir' : '✓ Concluir'}
                        </button>
                        <button class="btn-small btn-edit" onclick="openModal(${task.id})">✏️ Editar</button>
                        <button class="btn-small btn-delete" onclick="deleteTask(${task.id})">🗑️ Deletar</button>
                    </div>
                </div>
            `).join('');
        }

        function filterTasks() {
            renderTasks();
        }

        // Dashboard
        function updateDashboard() {
            const total = tasks.length;
            const completed = tasks.filter(t => t.status === 'completed').length;
            const pending = tasks.filter(t => t.status === 'pending').length;
            const completionRate = total === 0 ? 0 : Math.round((completed / total) * 100);

            document.getElementById('totalTasks').textContent = total;
            document.getElementById('completedTasks').textContent = completed;
            document.getElementById('pendingTasks').textContent = pending;
            document.getElementById('completionRate').textContent = completionRate + '%';
        }

        // Gráficos
        function initCharts() {
            const priorityCtx = document.getElementById('priorityChart').getContext('2d');
            const statusCtx = document.getElementById('statusChart').getContext('2d');

            priorityChart = new Chart(priorityCtx, {
                type: 'doughnut',
                data: {
                    labels: ['Alta', 'Média', 'Baixa'],
                    datasets: [{
                        data: [0, 0, 0],
                        backgroundColor: ['#f56565', '#ed8936', '#48bb78'],
                        borderColor: 'white',
                        borderWidth: 2
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            position: 'bottom'
                        },
                        title: {
                            display: true,
                            text: 'Tarefas por Prioridade'
                        }
                    }
                }
            });

            statusChart = new Chart(statusCtx, {
                type: 'doughnut',
                data: {
                    labels: ['Pendentes', 'Concluídas'],
                    datasets: [{
                        data: [0, 0],
                        backgroundColor: ['#667eea', '#48bb78'],
                        borderColor: 'white',
                        borderWidth: 2
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: {
                            position: 'bottom'
                        },
                        title: {
                            display: true,
                            text: 'Tarefas por Status'
                        }
                    }
                }
            });
        }

        function updateCharts() {
            if (!priorityChart || !statusChart) return;

            const highPriority = tasks.filter(t => t.priority === 'high').length;
            const mediumPriority = tasks.filter(t => t.priority === 'medium').length;
            const lowPriority = tasks.filter(t => t.priority === 'low').length;

            const pending = tasks.filter(t => t.status === 'pending').length;
            const completed = tasks.filter(t => t.status === 'completed').length;

            priorityChart.data.datasets[0].data = [highPriority, mediumPriority, lowPriority];
            priorityChart.update();

            statusChart.data.datasets[0].data = [pending, completed];
            statusChart.update();
        }

        // Storage
        function saveTasks() {
            localStorage.setItem('tasks', JSON.stringify(tasks));
        }

        // Notificação
        function showNotification(message) {
            const notification = document.createElement('div');
            notification.className = 'success-message';
            notification.textContent = message;
            document.body.appendChild(notification);

            setTimeout(() => {
                notification.remove();
            }, 3000);
        }
    </script>
</body>
</html>
