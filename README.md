# College-admissions-<!DO
<!DOCTYPE html>
<html>
<head>
<title>Tamil Nadu Mega College Finder</title>

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
    padding:25px;
    max-width:750px;
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
    border:none;
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

<h1>🎓 Tamil Nadu 500+ College Admission System</h1>

<div class="container">

<label>Enter Your Mark (%)</label>
<input type="number" id="mark" placeholder="Enter your mark">

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
<input type="number" id="fee" placeholder="Enter maximum fee">

<button onclick="searchCollege()">Search Colleges</button>

<div id="output"></div>

</div>

<script>

// =====================
// AUTO GENERATE 500 COLLEGES
// =====================

let colleges = [];

let districts = [
"Coimbatore","Chennai","Madurai","Trichy","Salem",
"Erode","Tirunelveli","Vellore","Thanjavur","Dharmapuri",
"Namakkal","Cuddalore","Dindigul","Karur","Tiruppur",
"Kanyakumari","Virudhunagar"
];

let coursesList = ["BSC","BCA","BCOM","BA"];

for(let i=1; i<=500; i++){

    let districtName = districts[i % districts.length];

    colleges.push({
        name: districtName + " Arts & Science College " + i,
        street: "Main Road " + i,
        village: "Village " + ((i % 30) + 1),
        district: districtName,
        course: coursesList,
        sem_fee: 5000 + Math.floor(Math.random()*30000),
        last_date: "30-06-2026",
        min_mark: 40 + Math.floor(Math.random()*30)
    });
}

// =====================
// SEARCH FUNCTION
// =====================

function searchCollege(){

let mark = parseInt(document.getElementById("mark").value);
let course = document.getElementById("course").value;
let district = document.getElementById("district").value;
let fee_limit = parseInt(document.getElementById("fee").value);

if(!course || !district){
    document.getElementById("output").innerHTML="Please select course and district.";
    return;
}

let result="";
let found=false;

for(let college of colleges){

if(college.district === district &&
college.course.includes(course) &&
mark >= college.min_mark &&
college.sem_fee <= fee_limit){

result += "<b>"+college.name+"</b><br>";
result += "📍 Location: "+college.street+", "+college.village+", "+college.district+"<br>";
result += "💰 Semester Fee: ₹"+college.sem_fee+"<br>";
result += "📅 Last Date: "+college.last_date+"<br>";
result += "<hr>";

found=true;
}
}

if(!found){
result="No colleges found based on your selection.";
}

document.getElementById("output").innerHTML=result;

}

</script>

</body>
</html>
