# College-admissions-<!DOCTYPE html>
<!DOCTYPE html>
<html>
<head>
    <title>AI Admission Portal</title>

    <style>
        body {
            font-family: Arial, sans-serif;
            background: url("bg.jpg") no-repeat center center fixed;
            background-size: cover;
            margin: 0;
            padding: 20px;
        }

        .container {
            background: rgba(255,255,255,0.93);
            max-width: 550px;
            margin: auto;
            padding: 25px;
            border-radius: 15px;
            box-shadow: 0 0 20px #000;
        }

        h2 { text-align: center; }

        input, select, button {
            width: 100%;
            padding: 10px;
            margin: 8px 0;
            border-radius: 6px;
            border: 1px solid #ccc;
        }

        button {
            background: #007bff;
            color: white;
            font-size: 16px;
            border: none;
            cursor: pointer;
        }

        button:hover { background: #0056b3; }

        .result {
            margin-top: 15px;
            background: #f2f2f2;
            padding: 10px;
            border-radius: 6px;
        }

        .hidden { display: none; }

        hr { border: 0.5px solid #ccc; }
    </style>
</head>

<body>

<!-- LOGIN PAGE -->
<div class="container" id="loginPage">
    <h2>Login</h2>

    <select id="role">
        <option value="student">Student</option>
        <option value="staff">Staff</option>
    </select>

    <input type="text" id="username" placeholder="Username">
    <input type="password" id="password" placeholder="Password">

    <button onclick="login()">Login</button>

    <div class="result" id="loginMsg"></div>
</div>

<!-- STUDENT PAGE -->
<div class="container hidden" id="studentPage">
    <h2>Student Admission Search</h2>

    <input type="text" id="sname" placeholder="Student Name">
    <input type="number" id="mark" placeholder="12th Mark (%)">

    <select id="course">
        <option>BSC</option>
        <option>BCA</option>
        <option>BCOM</option>
        <option>BA</option>
        <option>BTECH</option>
    </select>

    <input type="number" id="fee" placeholder="Max Fee Limit">
    <input type="text" id="district" placeholder="District">

    <button onclick="searchCollege()">Search Colleges</button>
    <button onclick="logout()">Logout</button>

    <div class="result" id="output"></div>
</div>

<!-- STAFF PAGE -->
<div class="container hidden" id="staffPage">
    <h2>Staff – Add College</h2>

    <input type="text" id="cname" placeholder="College Name">
    <input type="text" id="cdistrict" placeholder="District">
    <input type="text" id="ccourse" placeholder="Courses (BSC,BCA)">
    <input type="number" id="cfee" placeholder="Annual Fee">
    <input type="number" id="cmark" placeholder="Minimum Mark">

    <button onclick="addCollege()">Add College</button>
    <button onclick="logout()">Logout</button>

    <div class="result" id="staffMsg"></div>
</div>

<script>
let colleges = [
    {name:"PSG College of Arts and Science", district:"coimbatore", course:["BSC","BCA","BCOM"], fee:55000, min:60},
    {name:"Loyola College", district:"chennai", course:["BA","BSC","BCOM"], fee:65000, min:65},
    {name:"Government Arts College Salem", district:"salem", course:["BA","BCOM","BSC"], fee:12000, min:50}
];

function login() {
    let role = document.getElementById("role").value;
    let user = document.getElementById("username").value;
    let pass = document.getElementById("password").value;

    if (role === "student" && user === "student" && pass === "student123") {
        showPage("studentPage");
    } 
    else if (role === "staff" && user === "staff" && pass === "staff123") {
        showPage("staffPage");
    } 
    else {
        document.getElementById("loginMsg").innerHTML = "Invalid login details!";
    }
}

function showPage(pageId) {
    document.getElementById("loginPage").classList.add("hidden");
    document.getElementById("studentPage").classList.add("hidden");
    document.getElementById("staffPage").classList.add("hidden");
    document.getElementById(pageId).classList.remove("hidden");
}

function logout() {
    showPage("loginPage");
}

function searchCollege() {
    let mark = parseInt(document.getElementById("mark").value);
    let course = document.getElementById("course").value;
    let fee = parseInt(document.getElementById("fee").value);
    let district = document.getElementById("district").value.toLowerCase();

    let result = "";
    let found = false;

    for (let c of colleges) {
        if (c.district === district && c.course.includes(course) && mark >= c.min && fee >= c.fee) {
            result += `<b>${c.name}</b><br>Fee: ₹${c.fee}<br><hr>`;
            found = true;
        }
    }

    if (!found) result = "No colleges found.";

    document.getElementById("output").innerHTML = result;
}

function addCollege() {
    let name = document.getElementById("cname").value;
    let district = document.getElementById("cdistrict").value.toLowerCase();
    let course = document.getElementById("ccourse").value.split(",");
    let fee = parseInt(document.getElementById("cfee").value);
    let min = parseInt(document.getElementById("cmark").value);

    colleges.push({name:name, district:district, course:course, fee:fee, min:min});
    document.getElementById("staffMsg").innerHTML = "College added successfully!";
}
</script>

</body>
</html>
