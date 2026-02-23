# College-admissions-<!DOCTYPE html>
<!DOCTYPE html>
<html>
<head>
    <title>AI Arts & Science Admission Portal</title>
    <style>
        body {
            font-family: Arial;
            background: #f4f6f9;
            padding: 20px;
        }
        .container {
            background: white;
            padding: 20px;
            max-width: 600px;
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
            max-height:300px;
            overflow-y:auto;
        }
    </style>
</head>
<body>

<div class="container">
    <h2>AI Arts & Science Admission Portal</h2>

    <input type="text" id="name" placeholder="Enter your name">
    <input type="number" id="mark" placeholder="Enter 12th mark (%)">

    <select id="course">
        <option>BSC</option>
        <option>BCA</option>
        <option>BCOM</option>
        <option>BA</option>
    </select>

    <input type="number" id="fee" placeholder="Enter maximum fee limit">

    <button onclick="searchCollege()">Search Colleges</button>

    <div class="result" id="output"></div>
</div>

<script>

function searchCollege() {

    let mark = parseInt(document.getElementById("mark").value);
    let course = document.getElementById("course").value;
    let fee_limit = parseInt(document.getElementById("fee").value);

    let colleges = [

    {name:"PSG College of Arts and Science", course:["BSC","BCA","BCOM","BA"], fee:55000, min_mark:60},
    {name:"Nehru Arts and Science College", course:["BSC","BCA","BCOM","BA"], fee:40000, min_mark:50},
    {name:"Hindusthan College of Arts and Science", course:["BSC","BCA","BCOM","BA"], fee:45000, min_mark:55},
    {name:"Dr NGP Arts and Science College", course:["BSC","BCA","BCOM"], fee:48000, min_mark:55},
    {name:"Sri Krishna Arts and Science College", course:["BSC","BCA","BCOM"], fee:42000, min_mark:55},
    {name:"Government Arts College Coimbatore", course:["BSC","BCOM","BA"], fee:12000, min_mark:50},
    {name:"Loyola College Chennai", course:["BSC","BCOM","BA"], fee:60000, min_mark:65},
    {name:"Madras Christian College", course:["BSC","BCA","BCOM","BA"], fee:65000, min_mark:70},
    {name:"Women’s Christian College", course:["BSC","BCOM","BA"], fee:50000, min_mark:60},
    {name:"Ethiraj College for Women", course:["BSC","BCA","BCOM"], fee:52000, min_mark:60},
    {name:"Guru Nanak College Chennai", course:["BSC","BCOM","BA"], fee:45000, min_mark:55},
    {name:"Bishop Heber College", course:["BSC","BCA","BCOM","BA"], fee:50000, min_mark:60},
    {name:"St Joseph’s College Trichy", course:["BSC","BCOM","BA"], fee:58000, min_mark:65},
    {name:"National College Trichy", course:["BSC","BCOM","BA"], fee:45000, min_mark:55},
    {name:"Fatima College Madurai", course:["BSC","BCOM","BA"], fee:48000, min_mark:55},
    {name:"The American College Madurai", course:["BSC","BCA","BCOM"], fee:53000, min_mark:60},
    {name:"Lady Doak College", course:["BSC","BCOM","BA"], fee:50000, min_mark:60},
    {name:"AVS College of Arts and Science", course:["BSC","BCOM","BA"], fee:35000, min_mark:50},
    {name:"Sona College of Arts and Science", course:["BSC","BCA","BCOM"], fee:47000, min_mark:55},
    {name:"Kongu Arts and Science College", course:["BSC","BCOM","BA"], fee:38000, min_mark:50},
    {name:"Vellalar College for Women", course:["BSC","BCOM","BA"], fee:36000, min_mark:50},
    {name:"Sarah Tucker College", course:["BSC","BCOM","BA"], fee:42000, min_mark:55},
    {name:"St John’s College Palayamkottai", course:["BSC","BCOM"], fee:40000, min_mark:55},
    {name:"Voorhees College", course:["BSC","BCOM","BA"], fee:37000, min_mark:50},
    {name:"Government Arts College Thanjavur", course:["BSC","BCOM","BA"], fee:15000, min_mark:50},
    {name:"GTN Arts College", course:["BSC","BCOM","BA"], fee:34000, min_mark:50},
    {name:"Scott Christian College", course:["BSC","BCOM","BA"], fee:46000, min_mark:55},
    {name:"St Joseph’s College Cuddalore", course:["BSC","BCOM","BA"], fee:39000, min_mark:50},
    {name:"Government Arts College Karur", course:["BSC","BCOM","BA"], fee:14000, min_mark:50},
    {name:"Sengunthar Arts and Science College", course:["BSC","BCOM","BA"], fee:33000, min_mark:50},
    {name:"VHNSN College", course:["BSC","BCOM","BA"], fee:36000, min_mark:50},
    {name:"V.O. Chidambaram College", course:["BSC","BCOM","BA"], fee:37000, min_mark:50},
    {name:"Roever College of Arts and Science", course:["BSC","BCOM","BA"], fee:35000, min_mark:50}

    ];

    let result = "";
    let found = false;

    for(let college of colleges) {
        if(college.course.includes(course) && mark >= college.min_mark && college.fee <= fee_limit) {
            result += `<b>${college.name}</b><br>
                       Fees: ₹${college.fee}<br>
                       Minimum Mark: ${college.min_mark}%<br><hr>`;
            found = true;
        }
    }

    if(!found) {
        result = "No eligible Arts & Science colleges found.";
    }

    document.getElementById("output").innerHTML = result;
}

</script>

</body>
</html>
