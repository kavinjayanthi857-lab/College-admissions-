# College-admissions-<!DOCTYPE html>
<html>
<head>
    <title>AI Admission Chatbot</title>
    <style>
        body {
            font-family: Arial;
            background: #f4f6f9;
            padding: 20px;
        }

        .container {
            background: white;
            padding: 20px;
            max-width: 500px;
            margin: auto;
            border-radius: 10px;
            box-shadow: 0 0 10px #ccc;
        }

        input, select, button {
            width: 100%;
            padding: 10px;
            margin: 8px 0;
        }

        button {
            background: #007bff;
            color: white;
            border: none;
            cursor: pointer;
        }

        button:hover {
            background: #0056b3;
        }

        .result {
            margin-top: 15px;
            background: #f0f0f0;
            padding: 10px;
            border-radius: 5px;
        }
    </style>
</head>
<body>

<div class="container">
    <h2>AI Admission Chatbot</h2>

    <input type="text" id="name" placeholder="Enter your name">
    <input type="number" id="mark" placeholder="Enter 12th mark (%)">

    <select id="course">
        <option>BSC</option>
        <option>BCA</option>
        <option>BCOM</option>
        <option>BA</option>
        <option>MBBS</option>
        <option>BTECH</option>
    </select>

    <input type="number" id="fee" placeholder="Enter maximum fee limit">

    <button onclick="searchCollege()">Search</button>

    <div class="result" id="output"></div>
</div>

<script>
function searchCollege() {

    let name = document.getElementById("name").value;
    let mark = parseInt(document.getElementById("mark").value);
    let course = document.getElementById("course").value;
    let fee_limit = parseInt(document.getElementById("fee").value);

    let colleges = [
        {name:"Nehru Arts and Science College", course:["BSC","BCA","BCOM","BA"], fee:40000, last_date:"30-06-2026", min_mark:50},
        {name:"Krishna College of Arts and Science", course:["BSC","BCOM","BA"], fee:35000, last_date:"25-06-2026", min_mark:50},
        {name:"Ramakrishna College of Arts and Science", course:["BSC","BCA"], fee:60000, last_date:"28-06-2026", min_mark:60},
        {name:"Nehru Institution of Technology", course:["BCA","BSC","BTECH"], fee:75000, last_date:"20-06-2026", min_mark:60},
        {name:"Kovai Medical College", course:["MBBS"], fee:500000, last_date:"15-06-2026", min_mark:85},

        {name:"PSG College of Arts and Science", course:["BSC","BCA","BCOM","BA"], fee:55000, last_date:"30-06-2026", min_mark:60},
        {name:"Hindusthan College of Arts and Science", course:["BSC","BCA","BCOM","BA"], fee:45000, last_date:"27-06-2026", min_mark:55},
        {name:"Sri Ramakrishna Engineering College", course:["BTECH"], fee:120000, last_date:"22-06-2026", min_mark:70},
        {name:"Dr NGP Arts and Science College", course:["BSC","BCA","BCOM"], fee:48000, last_date:"29-06-2026", min_mark:55},
        {name:"Government Arts College Coimbatore", course:["BSC","BCOM","BA"], fee:12000, last_date:"10-07-2026", min_mark:50},
        {name:"Karpagam Academy of Higher Education", course:["BSC","BCA","BTECH"], fee:85000, last_date:"24-06-2026", min_mark:65},
        {name:"Amrita School of Engineering", course:["BTECH"], fee:150000, last_date:"18-06-2026", min_mark:75},
        {name:"KMCH College of Allied Health Sciences", course:["BSC"], fee:70000, last_date:"26-06-2026", min_mark:60},
        {name:"SNS College of Technology", course:["BTECH"], fee:90000, last_date:"23-06-2026", min_mark:65},
        {name:"Sri Krishna Arts and Science College", course:["BSC","BCA","BCOM"], fee:42000, last_date:"28-06-2026", min_mark:55}
    ];

    let result = "";
    let found = false;

    for(let college of colleges) {
        if(college.course.includes(course) && mark >= college.min_mark && college.fee <= fee_limit) {
            result += `<b>${college.name}</b><br>
                       Fees: ₹${college.fee}<br>
                       Last Date: ${college.last_date}<br><hr>`;
            found = true;
        }
    }

    if(!found) {
        result = "No colleges found within your fee limit.";
    }

    document.getElementById("output").innerHTML = result;
}
</script>

</body>
</html>
