let chart;

window.onload = function () {
    loadTasks();
    updateStats();
    renderChart();
};

function addTask() {
    const text = document.getElementById("taskInput").value.trim();
    const deadline = document.getElementById("deadline").value;
    const category = document.getElementById("category").value;

    if (!text) return;

    const task = { text, deadline, category, completed: false };

    saveTask(task);
    createTaskElement(task);

    document.getElementById("taskInput").value = "";
    updateStats();
    renderChart();
}

function createTaskElement(task) {
    const li = document.createElement("li");
    const today = new Date().toISOString().split("T")[0];

    li.innerHTML = `
        <div>
            <strong>${task.text}</strong><br>
            <small class="category ${task.category.toLowerCase()}">
                ${task.category} | ${task.deadline || "No deadline"}
            </small>
        </div>
        <div>
            <button onclick="toggleComplete(this)">✔</button>
            <button onclick="editTask(this)">✏️</button>
            <button class="delete-btn" onclick="deleteTask(this)">X</button>
        </div>
    `;

    if (task.completed) li.classList.add("completed");
    if (task.deadline && task.deadline < today) li.classList.add("urgent");

    document.getElementById("taskList").appendChild(li);
}

function toggleComplete(btn) {
    const li = btn.closest("li");
    li.classList.toggle("completed");
    updateStorage();
    updateStats();
    renderChart();
}

function editTask(btn) {
    const li = btn.closest("li");
    const text = prompt("Edit task:");
    if (text) {
        li.querySelector("strong").textContent = text;
        updateStorage();
    }
}

function deleteTask(btn) {
    btn.closest("li").remove();
    updateStorage();
    updateStats();
    renderChart();
}

function saveTask(task) {
    let tasks = JSON.parse(localStorage.getItem("tasks")) || [];
    tasks.push(task);
    localStorage.setItem("tasks", JSON.stringify(tasks));
}

function loadTasks() {
    let tasks = JSON.parse(localStorage.getItem("tasks")) || [];
    tasks.forEach(task => createTaskElement(task));
}

function updateStorage() {
    const listItems = document.querySelectorAll("#taskList li");
    let tasks = [];

    listItems.forEach(li => {
        tasks.push({
            text: li.querySelector("strong").textContent,
            category: li.querySelector(".category").textContent.split(" | ")[0],
            deadline: li.querySelector(".category").textContent.split(" | ")[1],
            completed: li.classList.contains("completed")
        });
    });

    localStorage.setItem("tasks", JSON.stringify(tasks));
}

function updateStats() {
    const tasks = JSON.parse(localStorage.getItem("tasks")) || [];

    const total = tasks.length;
    const completed = tasks.filter(t => t.completed).length;

    document.getElementById("totalTasks").textContent = total;
    document.getElementById("completedTasks").textContent = completed;

    const percent = total === 0 ? 0 : (completed / total) * 100;
    document.getElementById("progress").style.width = percent + "%";
}

function renderChart() {
    const tasks = JSON.parse(localStorage.getItem("tasks")) || [];

    const completed = tasks.filter(t => t.completed).length;
    const pending = tasks.length - completed;

    const ctx = document.getElementById("taskChart");

    if (chart) chart.destroy();

    chart = new Chart(ctx, {
        type: "doughnut",
        data: {
            labels: ["Completed", "Pending"],
            datasets: [{
                data: [completed, pending]
            }]
        }
    });
}
