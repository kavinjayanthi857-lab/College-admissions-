# College-admissions-<!DO
<!DOCTYPE html>
<html>
<head>
    <title>Tamil Nadu College Finder</title>
    <style>
        body{
            font-family: Arial;
            background:#f4f6f9;
            margin:0;
            padding:20px;
        }

        h1{
            text-align:center;
            color:#2c3e50;
        }

        .container{
            background:white;
            padding:20px;
            max-width:650px;
            margin:auto;
            border-radius:10px;
            box-shadow:0 0 10px rgba(0,0,0,0.1);
        }

        input, select, button{
            width:100%;
            padding:10px;
            margin-top:10px;
            border-radius:5px;
            border:1px solid #ccc;
        }

        button{
            background:#2c3e50;
            color:white;
            cursor:pointer;
        }

        button:hover{
            background:#1a252f;
        }

        #output{
            margin-top:20px;
        }

        hr{
            border:0;
            height:1px;
            background:#ddd;
        }
    </style>
</head>

<body>

<h1>🎓 Tamil Nadu College Finder</h1>

<div class="container">

    <label>Enter Your Mark (%)</label>
    <input type="number" id="mark" placeholder="Enter mark">

    <label>Select Course</label>
    <select id="course">
        <option value="">Select Course</option>
        <option>BSC</option>
        <option>BCA</option>
        <option>BCOM</option>
        <option>BA</option>
    </select>

    <label>Select District</label>
    <select id="district">
        <option value="">Select District</option>
        <option>Coimbatore</option>
        <option>Chennai</option>
        <option>Madurai</option>
        <option>Trichy</option>
        <option>Salem</option>
        <option>Erode</option>
        <option>Tirunelveli</option>
        <option>Vellore</option>
        <option>Thanjavur</option>
        <option>Dharmapuri</option>
        <option>Namakkal</option>
        <option>Cuddalore</option>
        <option>Dindigul</option>
        <option>Karur</option>
        <option>Tiruppur</option>
        <option>Kanyakumari</option>
        <option>Virudhunagar</option>
    </select>

    <label>Maximum Semester Fee (₹)</label>
    <input type="number" id="fee" placeholder="Enter max fee">

    <button onclick="searchCollege()">Search College</button>

    <div id="output"></div>

</div>

<script>
function searchCollege(){

    let mark = parseInt(document.getElementById("mark").value);
    let course = document.getElementById("course").value;
    let district = document.getElementById("district").value;
    let fee_limit = parseInt(document.getElementById("fee").value);

    if(!course || !district){
        document.getElementById("output").innerHTML = "Please select course and district.";
        return;
    }

    let colleges = [

        {name:"PSG College of Arts and Science", street:"Avinashi Road", village:"Peelamedu", district:"Coimbatore", course:["BSC","BCA","BCOM","BA"], sem_fee:27500, last_date:"30-06-2026", min_mark:60},
        {name:"Government Arts College", street:"Town Hall Road", village:"RS Puram", district:"Coimbatore", course:["BSC","BCOM","BA"], sem_fee:6000, last_date:"10-07-2026", min_mark:50},
        {name:"Loyola College", street:"Sterling Road", village:"Nungambakkam", district:"Chennai", course:["BSC","BCOM","BA"], sem_fee:30000, last_date:"25-06-2026", min_mark:65},
        {name:"Madras Christian College", street:"Velachery Main Road", village:"Tambaram", district:"Chennai", course:["BSC","BCOM","BA"], sem_fee:32000, last_date:"27-06-2026", min_mark:65},
        {name:"The American College", street:"Alagar Koil Road", village:"Tallakulam", district:"Madurai", course:["BSC","BCOM","BA"], sem_fee:25000, last_date:"29-06-2026", min_mark:55},
        {name:"Fatima College", street:"Mary Land", village:"Kochadai", district:"Madurai", course:["BSC","BCOM","BA"], sem_fee:22000, last_date:"30-06-2026", min_mark:55},
        {name:"Bishop Heber College", street:"Vayalur Road", village:"Puthur", district:"Trichy", course:["BSC","BCA","BCOM","BA"], sem_fee:25000, last_date:"28-06-2026", min_mark:55},
        {name:"St. Joseph's College", street:"College Road", village:"Tennur", district:"Trichy", course:["BSC","BCOM","BA"], sem_fee:27000, last_date:"27-06-2026", min_mark:60},
        {name:"Sri Sarada College", street:"Fairlands Road", village:"Fairlands", district:"Salem", course:["BSC","BCOM","BA"], sem_fee:18000, last_date:"05-07-2026", min_mark:55},
        {name:"Vellalar College", street:"Thindal Road", village:"Thindal", district:"Erode", course:["BSC","BCOM","BA"], sem_fee:20000, last_date:"03-07-2026", min_mark:55},
        {name:"St. Xavier's College", street:"High Ground Road", village:"Palayamkottai", district:"Tirunelveli", course:["BSC","BCOM","BA"], sem_fee:23000, last_date:"01-07-2026", min_mark:55},
        {name:"Voorhees College", street:"Officers Line", village:"Gandhi Nagar", district:"Vellore", course:["BSC","BCOM","BA"], sem_fee:19000, last_date:"03-07-2026", min_mark:55},
        {name:"Rajah Serfoji Govt College", street:"Medical College Road", village:"Thanjavur Town", district:"Thanjavur", course:["BSC","BCOM","BA"], sem_fee:7000, last_date:"10-07-2026", min_mark:50},
        {name:"Scott Christian College", street:"Court Road", village:"Nagercoil", district:"Kanyakumari", course:["BSC","BCOM","BA"], sem_fee:22000, last_date:"04-07-2026", min_mark:55},
        {name:"VHNSN College", street:"Virudhunagar Main Road", village:"Virudhunagar Town", district:"Virudhunagar", course:["BSC","BCOM","BA"], sem_fee:20000, last_date:"06-07-2026", min_mark:55}
    ];

    let result = "";
    let found = false;

    for(let college of colleges){

        if(
            college.district === district &&
            college.course.includes(course) &&
            mark >= college.min_mark &&
            college.sem_fee <= fee_limit
        ){

            result += `
                <b>${college.name}</b><br>
                📍 Location: ${college.street}, ${college.village}, ${college.district}<br>
                💰 Semester Fees: ₹${college.sem_fee}<br>
                📅 Last Date: ${college.last_date}<br>
                <hr>
            `;

            found = true;
        }
    }

    if(!found){
        result = "No colleges found based on your selection.";
    }

    document.getElementById("output").innerHTML = result;
}
</script>

</body>
</html>
