<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">

<title>Queue Simulator Game</title>

<style>
    * {
        box-sizing: border-box;
        margin: 0;
        padding: 0;
    }

    body {
        font-family: Arial, sans-serif;
        min-height: 100vh;
        background: linear-gradient(135deg, #141e30, #243b55);
        color: gray;
        overflow-x: hidden;
    }

    /* Header */
    header {
        text-align: center;
        padding: 25px;
        background: rgba(0, 0, 0, 0.25);
        box-shadow: 0 5px 20px rgba(0,0,0,0.3);
    }

    header h1 {
        font-size: 38px;
        color: white;
        text-shadow: 0 0 15px #00eaff;
    }

    header p {
        margin-top: 8px;
        color: #ddd;
    }

    /* Main container */
    .game-container {
        width: 90%;
        max-width: 1100px;
        margin: 30px auto;
    }

    /* Dashboard */
    .dashboard {
        display: flex;
        justify-content: space-around;
        gap: 15px;
        flex-wrap: wrap;
        margin-bottom: 25px;
    }

    .card {
        background: rgba(255,255,255,0.1);
        border: 1px solid rgba(255,255,255,0.2);
        border-radius: 15px;
        padding: 18px 35px;
        text-align: center;
        min-width: 180px;
        backdrop-filter: blur(10px);
        box-shadow: 0 8px 20px rgba(0,0,0,0.25);
    }

    .card h3 {
        color: #aaa;
        font-size: 15px;
        margin-bottom: 8px;
    }

    .card span {
        font-size: 28px;
        font-weight: bold;
        color: #00ff99;
    }

    /* Queue area */
    .queue-area {
        background: rgba(0,0,0,0.3);
        border-radius: 20px;
        padding: 30px;
        min-height: 280px;
        box-shadow: inset 0 0 30px rgba(0,0,0,0.4);
    }

    .queue-title {
        text-align: center;
        margin-bottom: 20px;
        font-size: 22px;
        color: #ffcc00;
    }

    .queue {
        min-height: 130px;
        display: flex;
        align-items: center;
        gap: 15px;
        padding: 25px;
        overflow-x: auto;
        border: 3px dashed #555;
        border-radius: 15px;
        background: rgba(255,255,255,0.04);
    }

    /* Queue person/item */
    .person {
        min-width: 90px;
        height: 90px;
        border-radius: 15px;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        background: linear-gradient(145deg, #00c6ff, #0072ff);
        box-shadow: 0 0 15px rgba(0,198,255,0.5);
        animation: enter 0.5s ease;
        position: relative;
        flex-shrink: 0;
    }

    .person.front {
        background: linear-gradient(145deg, #ff512f, #dd2476);
        box-shadow: 0 0 20px rgba(255,80,80,0.8);
        transform: scale(1.08);
    }

    .person .emoji {
        font-size: 35px;
    }

    .person .number {
        font-size: 13px;
        font-weight: bold;
    }

    @keyframes enter {
        from {
            transform: translateX(100px) scale(0.5);
            opacity: 0;
        }

        to {
            transform: translateX(0) scale(1);
            opacity: 1;
        }
    }

    /* Labels */
    .queue-labels {
        display: flex;
        justify-content: space-between;
        margin-top: 15px;
        color: #aaa;
        font-weight: bold;
    }

    .front-label {
        color: #ff5555;
    }

    .rear-label {
        color: #00eaff;
    }

    /* Controls */
    .controls {
        margin-top: 25px;
        text-align: center;
    }

    .input-box {
        display: flex;
        justify-content: center;
        gap: 10px;
        margin-bottom: 20px;
        flex-wrap: wrap;
    }

    input {
        width: 230px;
        padding: 14px;
        border: none;
        border-radius: 10px;
        outline: none;
        font-size: 16px;
    }

    button {
        border: none;
        padding: 13px 22px;
        border-radius: 10px;
        font-size: 15px;
        font-weight: bold;
        cursor: pointer;
        color: white;
        transition: 0.2s;
        margin: 5px;
    }

    button:hover {
        transform: translateY(-3px);
        filter: brightness(1.15);
    }

    .enqueue {
        background: linear-gradient(135deg, #00c853, #64dd17);
    }

    .dequeue {
        background: linear-gradient(135deg, #ff1744, #d50000);
    }

    .peek {
        background: linear-gradient(135deg, #ff9100, #ff6d00);
    }

    .reset {
        background: linear-gradient(135deg, #7c4dff, #6200ea);
    }

    /* Message */
    .message {
        margin: 20px auto 0;
        max-width: 600px;
        padding: 15px;
        border-radius: 10px;
        background: rgba(255,255,255,0.1);
        text-align: center;
        min-height: 50px;
        color: #fff;
    }

    /* Queue explanation */
    .info {
        margin-top: 25px;
        padding: 20px;
        background: rgba(255,255,255,0.08);
        border-radius: 15px;
    }

    .info h2 {
        color: #00eaff;
        margin-bottom: 10px;
    }

    .info p {
        line-height: 1.7;
        color: #ddd;
    }

    /* Mobile */
    @media(max-width:600px) {
        header h1 {
            font-size: 28px;
        }

        .queue-area {
            padding: 15px;
        }

        .queue {
            padding: 15px;
        }
    }
</style>
</head>

<body>

<header>
    <h1>🎮 QUEUE SIMULATOR</h1>
    <p>Learn Data Structures by Playing!</p>
</header>

<div class="game-container">

    <!-- Dashboard -->
    <div class="dashboard">

        <div class="card">
            <h3>QUEUE SIZE</h3>
            <span id="size">0</span>
        </div>

        <div class="card">
            <h3>SCORE</h3>
            <span id="score">0</span>
        </div>

        <div class="card">
            <h3>OPERATIONS</h3>
            <span id="operations">0</span>
        </div>

    </div>

    <!-- Queue -->
    <div class="queue-area">

        <div class="queue-title">
            🚶 People Waiting in Queue
        </div>

        <div class="queue" id="queue">
            <p id="emptyMessage" style="color:#777;">
                Queue is Empty... Add someone! 👇
            </p>
        </div>

        <div class="queue-labels">
            <span class="front-label">⬅ FRONT</span>
            <span class="rear-label">REAR ➡</span>
        </div>

    </div>

    <!-- Controls -->
    <div class="controls">

        <div class="input-box">
            <input
                type="text"
                id="personName"
                placeholder="Enter person's name..."
            >

            <button class="enqueue" onclick="enqueue()">
                ➕ ENQUEUE
            </button>
        </div>

        <button class="dequeue" onclick="dequeue()">
            🚪 DEQUEUE
        </button>

        <button class="peek" onclick="peek()">
            👀 PEEK
        </button>

        <button class="reset" onclick="resetQueue()">
            🔄 RESET
        </button>

        <div class="message" id="message">
            Welcome! Start by adding someone to the queue.
        </div>

    </div>

    <!-- Information -->
    <div class="info">

        <h2>📚 Queue Data Structure</h2>

        <p>
            A Queue follows the <b>FIFO</b> principle:
            <b>First In, First Out</b>.
            The element that enters the queue first is removed first.
        </p>

        <p>
            <b>Enqueue:</b> Add an element at the rear.<br>
            <b>Dequeue:</b> Remove an element from the front.<br>
            <b>Peek:</b> View the front element without removing it.
        </p>

    </div>

</div>


<script>

    // Queue array
    let queue = [];

    // Game statistics
    let score = 0;
    let operations = 0;

    // Emoji collection
    const emojis = [
        "👨",
        "👩",
        "👦",
        "👧",
        "🧑",
        "👴",
        "👵",
        "🧔"
    ];

    // ENQUEUE
    function enqueue() {

        const input = document.getElementById("personName");
        const name = input.value.trim();

        if (name === "") {
            showMessage("⚠️ Please enter a name first!");
            return;
        }

        if (queue.length >= 10) {
            showMessage("🚫 Queue is full! Maximum size is 10.");
            return;
        }

        // Create object
        const person = {
            name: name,
            emoji: emojis[Math.floor(Math.random() * emojis.length)]
        };

        // Add to rear
        queue.push(person);

        score += 10;
        operations++;

        input.value = "";

        updateQueue();

        showMessage(
            "✅ " + name + " joined the queue!"
        );
    }


    // DEQUEUE
    function dequeue() {

        if (queue.length === 0) {
            showMessage("🚫 Queue is empty! Nothing to dequeue.");
            return;
        }

        // Remove first element
        const removed = queue.shift();

        score += 20;
        operations++;

        updateQueue();

        showMessage(
            "🚪 " + removed.name + " left the queue!"
        );
    }


    // PEEK
    function peek() {

        if (queue.length === 0) {
            showMessage("👀 Queue is empty!");
            return;
        }

        const first = queue[0];

        showMessage(
            "👑 FRONT ELEMENT: " +
            first.emoji +
            " " +
            first.name
        );
    }


    // RESET
    function resetQueue() {

        queue = [];
        score = 0;
        operations = 0;

        updateQueue();

        showMessage(
            "🔄 Queue has been reset!"
        );
    }


    // Update screen
    function updateQueue() {

        const queueElement =
            document.getElementById("queue");

        const sizeElement =
            document.getElementById("size");

        const scoreElement =
            document.getElementById("score");

        const operationElement =
            document.getElementById("operations");

        queueElement.innerHTML = "";

        if (queue.length === 0) {

            queueElement.innerHTML =
                `<p style="color:#777;">
                    Queue is Empty... Add someone! 👇
                </p>`;

        } else {

            queue.forEach((person, index) => {

                const div =
                    document.createElement("div");

                div.classList.add("person");

                // Highlight FRONT
                if (index === 0) {
                    div.classList.add("front");
                }

                div.innerHTML = `
                    <div class="emoji">
                        ${person.emoji}
                    </div>

                    <div class="number">
                        ${person.name}
                    </div>
                `;

                queueElement.appendChild(div);

            });
        }

        // Update dashboard
        sizeElement.innerText = queue.length;
        scoreElement.innerText = score;
        operationElement.innerText = operations;
    }


    // Message function
    function showMessage(text) {

        const message =
            document.getElementById("message");

        message.innerHTML = text;

    }


    // Enter key support
    document
        .getElementById("personName")
        .addEventListener("keypress", function(event) {

            if (event.key === "Enter") {
                enqueue();
            }

        });

</script>

</body>
</html>
